# Module 2 — Cryptography for Humans

> **Audience:** 🟡 🔴 (🟢-friendly sections marked) · **Time:** ~90 min · **Prereqs:** Modules 0–1

## Why this matters

Crypto is the single biggest source of interview traps ("is Base64 encryption?") and audit questions ("what's your key management?"). You don't need to invent crypto — in fact, **never invent crypto**. But you must understand what the tools do, why they're used, and how they fail.

By the end you'll know:
- The three operations people confuse: **encoding, hashing, encryption**.
- **Symmetric vs asymmetric** crypto and when each is used.
- How **TLS** actually secures a connection.
- **Hashing**, **MACs**, and **digital signatures** — each solves a different problem.
- **Key management** — the part where real-world crypto actually fails.

---

## 2.1 The three operations people confuse (🟢 read this at least)

| Operation | Reversible? | Needs a key? | Purpose |
|-----------|------------|--------------|---------|
| **Encoding** (e.g., Base64) | ✅ Yes, by anyone | ❌ No | Represent binary data as text. *Not security.* |
| **Hashing** (e.g., SHA-256) | ❌ No (one-way) | ❌ No (plain hash) | Fingerprint a message; integrity check. |
| **Encryption** (e.g., AES, RSA) | ✅ Yes, with the key | ✅ Yes | Confidentiality — only key-holders read. |

### The one-sentence test

- **Encoding** protects *nothing*. It's a format change. Base64 is as "secure" as uppercase.
- **Hashing** does *not* hide data; it proves data hasn't changed. A hash can't be "decrypted."
- **Encryption** hides data but requires careful key management — which is 90% of the difficulty.

**Interview trap:** "We encode passwords in Base64 before storage." This person just told you their system is broken. Passwords must be **hashed** (one-way) with a slow, salted algorithm (bcrypt, scrypt, Argon2).

---

## 2.2 Hashing — the data fingerprint

A hash function takes any input and returns a fixed-size output ("digest"). Two guarantees:

1. **Deterministic** — same input always yields same output.
2. **One-way** — given the output, you can't practically recover the input.

Plus in cryptographic hashes: tiny input changes produce wildly different outputs (avalanche effect), and it's infeasible to find two inputs with the same hash (collision resistance).

### Examples of hash functions

- **MD5** — ❌ broken (collisions found in 2004). Never use for security.
- **SHA-1** — ❌ broken (collisions in 2017). Phase out.
- **SHA-256 / SHA-3** — ✅ modern, currently safe.
- **bcrypt / scrypt / Argon2** — ✅ *password* hashing (deliberately slow, salted, memory-hard).

### Use cases

- **File integrity** — "does the download match what was published?" Compare hashes.
- **Password storage** — store only the salted hash; never the password itself.
- **Blockchain** — chains of hashes make tampering detectable.
- **Git commits** — each commit is a SHA-1 (Git plans SHA-256 migration).

### Why passwords need special hashes

A fast hash (SHA-256) can be computed *billions* of times per second on a GPU. If someone steals your password-hash database, they can try billions of candidate passwords. **bcrypt/Argon2** are *deliberately slow* and can be tuned to take, say, 250 ms per guess — turning a days-long attack into millennia. They also include a **salt** — a random per-user value appended before hashing — so precomputed "rainbow table" attacks don't work.

### Worked example

User signs up with password `p@ssw0rd!`. System generates a random salt `a91f...`. Stored in DB:
```
argon2id$v=19$m=65536,t=3,p=4$a91f...$<hash>
```
At login, the server re-hashes the entered password with the same salt and compares. Even if the entire DB leaks, attacking each password individually at 250 ms/guess is infeasible for strong passwords.

### Common beginner mistakes

- Using plain SHA-256 for passwords. Too fast; attackable.
- Rolling your own salting scheme. Use the library's defaults.
- Comparing hashes with `==` in code — opens timing attacks. Use constant-time compare functions.

### Mini-exercise

Use `openssl` on your terminal:
```
echo -n "hello" | openssl dgst -sha256
echo -n "hello " | openssl dgst -sha256   # note the trailing space
```
Observe how dramatically the output changes. This is the avalanche effect.

---

## 2.3 Symmetric encryption — one shared key

Both sender and receiver share the **same** secret key. Fast, ideal for bulk data.

- **AES (Advanced Encryption Standard)** — the universal standard. Common sizes: AES-128, AES-256. AES-256 is overkill for most uses but standard for regulated industries.
- **Modes of operation** — AES alone only encrypts one block (16 bytes). Modes chain blocks:
  - **ECB** — ❌ never use. Identical blocks produce identical ciphertext (the famous "ECB penguin" image).
  - **CBC** — okay but fragile (padding oracle attacks).
  - **GCM** — ✅ authenticated encryption. **Use this** unless you have reason not to. Provides confidentiality **and** integrity.
  - **ChaCha20-Poly1305** — modern alternative to AES-GCM, especially on mobile.

### The key-distribution problem

If Alice and Bob share a symmetric key, *how did they agree on the key* in the first place without an eavesdropper catching it? This is the problem asymmetric crypto solves.

---

## 2.4 Asymmetric (public-key) crypto — two keys, one secret

Each party has a **key pair**:
- A **public key** — share freely.
- A **private key** — never share.

What one key encrypts, the other decrypts. This one magical property enables:

- **Encrypting to someone** — anyone can encrypt *to* Bob using his public key; only Bob can decrypt with his private key.
- **Digital signatures** — Alice signs a message with her *private* key; anyone can verify with her *public* key. Proves authenticity + integrity + non-repudiation.
- **Key agreement** — Diffie-Hellman, ECDH — derive a shared secret over a public channel.

Common algorithms: **RSA** (older, large keys), **ECC / ECDSA / Ed25519** (modern, smaller keys, faster).

### Cost

Asymmetric is ~1000× slower than symmetric. So in practice: **use asymmetric crypto only to agree on a symmetric key, then use symmetric for the bulk data.** This is exactly how TLS works.

---

## 2.5 TLS — putting it all together (🟢 read slowly, this repays)

When your browser connects to an HTTPS site, here's the simplified handshake (TLS 1.3):

1. **Client Hello** — browser says: "Here are the cipher suites I support, here's a random number."
2. **Server Hello** — server picks a cipher suite, sends its **certificate** (containing its public key, signed by a trusted CA), sends its random number, and a key-agreement share.
3. **Client verifies the certificate** — is it signed by a trusted CA? Is it for the right domain? Is it expired? Is it revoked?
4. **Key agreement** — both sides compute a **shared symmetric key** from the exchanged shares (using ECDHE — ephemeral, so even if the server's private key leaks later, past sessions stay safe: *forward secrecy*).
5. **Encrypted HTTP** — from here on, everything is symmetrically encrypted (AES-GCM or ChaCha20-Poly1305) and authenticated.

### Certificates and the CA chain

A certificate says "this public key belongs to *example.com* and is valid until *date*." It's signed by a **Certificate Authority** your browser trusts. The CA's own certificate is signed by a root CA, which your OS/browser ships with pre-trusted. That chain of signatures is the **chain of trust**.

**Let's Encrypt** made certificates free and automated. No excuse for HTTP in 2026.

### Why "the padlock" doesn't mean safe

The padlock means the *connection* is encrypted. The **site** could still be phishing `amaz0n.com` with a perfectly valid cert. Encryption ≠ trustworthy.

### Common beginner mistakes

- Confusing TLS version with cipher suite. Use **TLS 1.2 or 1.3** with modern ciphers; disable TLS 1.0/1.1/SSL.
- Self-signing certs for production. Use a public CA (Let's Encrypt) or an internal CA for internal systems.
- Ignoring certificate expiry. An expired cert breaks the site instantly. Automate renewal (cert-manager, acme.sh).

### Interview trap

- **Q:** "If HTTPS uses public-key crypto, why do we need symmetric keys at all?"
  **A:** Public-key is too slow for bulk data. It's used only for the handshake to agree on a symmetric key, then the bulk traffic uses symmetric crypto for speed.

---

## 2.6 MACs and digital signatures — proving who and what

Encryption alone doesn't guarantee *who* sent a message or that it wasn't altered.

- **MAC (Message Authentication Code)** — symmetric. A shared key proves integrity and authenticity *between parties who share the key*. Example: HMAC-SHA256.
- **Digital signature** — asymmetric. Signed with private key; verified by anyone with the public key. Adds **non-repudiation** (the signer can't deny signing).

Use MACs when you have a shared secret already (API HMAC signing). Use signatures when a third party needs to verify (code signing, document signing, TLS certs).

### Authenticated encryption (AEAD)

Modern modes like **AES-GCM** combine encryption + MAC in one step, preventing whole classes of bugs. Use AEAD unless you have a specific reason not to.

---

## 2.7 Key management — the part where real systems fail

Crypto algorithms rarely break; **key management** breaks all the time.

### The lifecycle of a key

1. **Generation** — in a random, secure way (proper RNG, not `rand()`).
2. **Distribution** — get it to the right place without leaking.
3. **Storage** — in a **Key Management System (KMS)**, **Hardware Security Module (HSM)**, or cloud KMS (AWS KMS, Azure Key Vault, GCP KMS).
4. **Use** — ideally the key never leaves the KMS; the KMS performs the operation and returns the result.
5. **Rotation** — periodically replace the key.
6. **Revocation / destruction** — securely retire when compromised or past its life.

### Key concepts you must know

- **KEK vs DEK** — Key-Encrypting-Key vs Data-Encrypting-Key. Cheap small KEK protects many DEKs — standard envelope-encryption pattern.
- **BYOK (Bring Your Own Key)** — customer supplies their own master key to the cloud KMS. Common in enterprise SaaS.
- **HYOK (Hold Your Own Key)** — stronger: customer holds the key; cloud cannot access it without explicit approval.
- **Customer-Managed Keys (CMK)** vs provider-managed.
- **Key ceremony** — a formal, witnessed process to generate root keys in HSMs (yes, people wear suits to these).

### Common real-world failures

- Hardcoded API keys in GitHub repos (→ use secret scanning, Git-leaks).
- Secrets committed in `.env.example` that was never meant to go live.
- Same key used across dev/staging/prod.
- No rotation; keys 5 years old.
- Passwords (not random keys) used to encrypt critical data.
- "We encrypted it" but the encryption key lived next to the ciphertext.

### Mini-exercise (15 min)

For an app you know, list:
1. Where encryption keys are stored.
2. Who can access them.
3. When they were last rotated.
4. What happens if they're compromised tomorrow.
If you can't answer any of these, you've found your first improvement project.

---

## 2.8 A quick word on post-quantum cryptography

Classical asymmetric algorithms (RSA, ECC) rely on math problems that a sufficiently powerful **quantum computer** could solve. That computer doesn't exist yet at useful scale, but encrypted data stolen today can be decrypted later ("harvest now, decrypt later"). NIST finalised its first **post-quantum algorithms** in 2024 (ML-KEM / CRYSTALS-Kyber for key-encapsulation, ML-DSA / CRYSTALS-Dilithium for signatures). Your organisation should have a **crypto-agility** plan — the ability to swap algorithms when standards change. You do not need to migrate tomorrow; you do need to know the words.

---

## 2.9 Go deeper (curated free)

- 📘 [Crypto 101 — free online book by lvh](https://www.crypto101.io/) — the most accessible deep dive.
- 📘 [Khan Academy — Journey into Cryptography](https://www.khanacademy.org/computing/computer-science/cryptography)
- 🧪 [Cryptopals Challenges](https://cryptopals.com/) — learn by breaking weak crypto; the single best hands-on resource.
- 📰 [Cloudflare — How TLS works](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)
- 📰 [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- 🏛 [NIST FIPS 140-3](https://csrc.nist.gov/projects/cryptographic-module-validation-program) — the crypto-module validation standard (know the name).
- 🏛 [NIST Post-Quantum Cryptography project](https://csrc.nist.gov/projects/post-quantum-cryptography)
- 🎥 [Computerphile cryptography playlist (YouTube)](https://www.youtube.com/@Computerphile)

---

## Module 2 — Glossary recap

Encoding, Base64, Hashing, Salt, Pepper, MD5, SHA-256, bcrypt, scrypt, Argon2, Symmetric encryption, AES, AES-GCM, ChaCha20-Poly1305, ECB/CBC/GCM modes, Asymmetric encryption, Public key, Private key, RSA, ECC, Ed25519, Key agreement, Diffie-Hellman, ECDHE, Forward secrecy, TLS handshake, Certificate, CA, Chain of trust, Let's Encrypt, MAC, HMAC, Digital signature, AEAD, KMS, HSM, KEK, DEK, BYOK, HYOK, CMK, Key rotation, Key ceremony, Post-quantum cryptography, Crypto-agility.

→ Next: [Module 3 — Identity, Access & Authentication](03-identity-and-access.md)
