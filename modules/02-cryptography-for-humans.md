# Module 2, Cryptography for Humans

> **Audience:** 🟡 🔴 (🟢-friendly sections marked) · **Time:** ~90 min · **Prereqs:** Modules 0–1

## Why this matters

Crypto is the single biggest source of interview traps ("is Base64 encryption?") and audit questions ("what's your key management?"). You don't need to invent crypto. In fact, never invent crypto. But you must understand what the tools do, why they're used and how they fail.

Strong opinion up front: stop trying to memorise algorithm internals. Nobody in a real GRC role draws an AES S-box on a whiteboard. What actually breaks production systems is **key management**, **certificate rotation** and **TLS misconfiguration**. Spend your time there.

By the end you'll know:
- The three operations people confuse: **encoding, hashing, encryption**.
- **Symmetric vs asymmetric** crypto and when each is used.
- How **TLS** actually secures a connection (and how it gets misconfigured).
- **Hashing**, **MACs** and **digital signatures**, each solves a different problem.
- **Key management.** The part where real-world crypto actually fails.

---

## 2.1 The three operations people confuse (🟢 read this at least)

| Operation | Reversible? | Needs a key? | Purpose |
|-----------|------------|--------------|---------|
| **Encoding** (e.g., Base64) | ✅ Yes, by anyone | ❌ No | Represent binary data as text. Not security. |
| **Hashing** (e.g., SHA-256) | ❌ No (one-way) | ❌ No (plain hash) | Fingerprint a message. Integrity check. |
| **Encryption** (e.g., AES, RSA) | ✅ Yes, with the key | ✅ Yes | Confidentiality. Only key-holders read. |

### The one-sentence test

- **Encoding** protects nothing. It's a format change. Base64 is as "secure" as uppercase.
- **Hashing** does not hide data. It proves data hasn't changed. A hash can't be "decrypted".
- **Encryption** hides data but requires careful key management, which is 90% of the difficulty.

**Interview trap:** "We encode passwords in Base64 before storage." This person just told you their system is broken. Passwords must be hashed (one-way) with a slow, salted algorithm (bcrypt, scrypt, Argon2).

---

## 2.2 Hashing, the data fingerprint

A hash function takes any input and returns a fixed-size output ("digest"). Two guarantees:

1. **Deterministic.** Same input always yields same output.
2. **One-way.** Given the output, you can't practically recover the input.

Plus in cryptographic hashes: tiny input changes produce wildly different outputs (the avalanche effect), and it's infeasible to find two inputs with the same hash (collision resistance).

### Examples of hash functions

- **MD5.** ❌ Broken (collisions found in 2004). Never use for security.
- **SHA-1.** ❌ Broken (collisions in 2017). Phase out.
- **SHA-256 / SHA-3.** ✅ Modern, currently safe.
- **bcrypt / scrypt / Argon2.** ✅ Password hashing (deliberately slow, salted, memory-hard).

### Use cases

- **File integrity.** Does the download match what was published? Compare hashes.
- **Password storage.** Store only the salted hash. Never the password.
- **Blockchain.** Chains of hashes make tampering detectable.
- **Git commits.** Each commit is a SHA-1 (Git plans SHA-256 migration).

### Why passwords need special hashes

A fast hash (SHA-256) can be computed billions of times per second on a GPU. If someone steals your password-hash database, they can try billions of candidate passwords. bcrypt and Argon2 are deliberately slow and can be tuned to take, say, 250 ms per guess. That turns a days-long attack into millennia. They also include a **salt**, a random per-user value mixed in before hashing, so precomputed "rainbow table" attacks don't work.

### Worked example

User signs up with password `p@ssw0rd!`. System generates a random salt `a91f...`. Stored in DB:
```
argon2id$v=19$m=65536,t=3,p=4$a91f...$<hash>
```
At login, the server re-hashes the entered password with the same salt and compares. Even if the entire DB leaks, attacking each password individually at 250 ms per guess is infeasible for strong passwords.

### What beginners typically miss

- Using plain SHA-256 for passwords. Too fast. Attackable.
- Rolling your own salting scheme. Use the library's defaults.
- Comparing hashes with `==` in code. Opens timing attacks. Use constant-time compare functions.

### Mini-exercise

Use `openssl` on your terminal:
```
echo -n "hello" | openssl dgst -sha256
echo -n "hello " | openssl dgst -sha256   # note the trailing space
```
Observe how dramatically the output changes. That's the avalanche effect.

---

## 2.3 Symmetric encryption, one shared key

Both sender and receiver share the same secret key. Fast, ideal for bulk data.

- **AES (Advanced Encryption Standard).** The universal standard. Common sizes: AES-128, AES-256. AES-256 is overkill for most uses but standard for regulated industries.
- **Modes of operation.** AES alone only encrypts one 16-byte block. Modes chain blocks together:
  - **ECB.** ❌ Never use. Identical blocks produce identical ciphertext (the famous "ECB penguin" image).
  - **CBC.** Okay but fragile (padding oracle attacks).
  - **GCM.** ✅ Authenticated encryption. Use this unless you have a reason not to. Confidentiality plus integrity in one step.
  - **ChaCha20-Poly1305.** Modern alternative to AES-GCM, especially on mobile.

### The key-distribution problem

If Alice and Bob share a symmetric key, how did they agree on the key in the first place without an eavesdropper catching it? This is the problem asymmetric crypto solves.

---

## 2.4 Asymmetric (public-key) crypto, two keys, one secret

Each party has a key pair:
- A **public key.** Share freely.
- A **private key.** Never share.

What one key encrypts, the other decrypts. That one property enables:

- **Encrypting to someone.** Anyone can encrypt to Bob using his public key. Only Bob can decrypt with his private key.
- **Digital signatures.** Alice signs a message with her private key. Anyone verifies with her public key. Proves authenticity, integrity and non-repudiation.
- **Key agreement.** Diffie-Hellman, ECDH. Derive a shared secret over a public channel.

Common algorithms: **RSA** (older, large keys), **ECC / ECDSA / Ed25519** (modern, smaller keys, faster).

### Cost

Asymmetric is roughly 1000× slower than symmetric. So in practice: use asymmetric crypto only to agree on a symmetric key, then use symmetric for the bulk data. This is exactly how TLS works.

---

## 2.5 TLS, putting it all together (🟢 read slowly, this repays)

When your browser connects to an HTTPS site, here's the simplified handshake (TLS 1.3):

1. **Client Hello.** Browser says: "Here are the cipher suites I support, here's a random number."
2. **Server Hello.** Server picks a cipher suite, sends its certificate (containing its public key, signed by a trusted CA), sends its random number and a key-agreement share.
3. **Client verifies the certificate.** Is it signed by a trusted CA? Is it for the right domain? Is it expired? Is it revoked?
4. **Key agreement.** Both sides compute a shared symmetric key from the exchanged shares (using ECDHE, which is ephemeral, so even if the server's private key leaks later, past sessions stay safe. That's **forward secrecy**).
5. **Encrypted HTTP.** From here on, everything is symmetrically encrypted (AES-GCM or ChaCha20-Poly1305) and authenticated.

### Certificates and the CA chain

A certificate says "this public key belongs to *example.com* and is valid until *date*." It's signed by a **Certificate Authority** your browser trusts. The CA's own certificate is signed by a root CA, which your OS or browser ships with pre-trusted. That chain of signatures is the chain of trust.

**Let's Encrypt** made certificates free and automated. No excuse for HTTP in 2026.

### Where TLS actually breaks in production

Algorithms rarely break. Operations do. Common patterns:

- Cert expires at 02:00 on a Sunday and pages everyone. Always have automated renewal (cert-manager, acme.sh, ACM) and an alert at T-30 days and T-7 days.
- Intermediate certificate not bundled with the leaf. Browsers complain, the API team can't reproduce on their laptop, two days lost.
- An old load balancer still serves TLS 1.0 because nobody dared disable it. Pen test finding next quarter.
- Wildcard cert reused across dev, staging and prod. One leak compromises all environments.
- Mutual TLS works for six months, then a client cert expires and a payment integration silently fails over a weekend.

If you're new to GRC, learn to read SSL Labs output before you learn any algorithm internals. `ssllabs.com/ssltest` is a free crash course.

### Why "the padlock" doesn't mean safe

The padlock means the connection is encrypted. The site could still be phishing `amaz0n.com` with a perfectly valid cert. Encryption is not the same as trustworthy.

### What beginners typically miss

- Confusing TLS version with cipher suite. Use TLS 1.2 or 1.3 with modern ciphers. Disable TLS 1.0, 1.1 and SSL.
- Self-signing certs for production. Use a public CA (Let's Encrypt) or an internal CA for internal systems.
- Ignoring certificate expiry. An expired cert breaks the site instantly. Automate renewal.

### Interview trap

- **Q:** "If HTTPS uses public-key crypto, why do we need symmetric keys at all?"
  **A:** Public-key is too slow for bulk data. It's used only in the handshake to agree on a symmetric key, then the bulk traffic uses symmetric crypto for speed.

---

## 2.6 MACs and digital signatures, proving who and what

Encryption alone doesn't guarantee who sent a message or that it wasn't altered.

- **MAC (Message Authentication Code).** Symmetric. A shared key proves integrity and authenticity between parties who share the key. Example: HMAC-SHA256.
- **Digital signature.** Asymmetric. Signed with the private key. Verified by anyone with the public key. Adds non-repudiation (the signer can't deny signing).

Use MACs when you already have a shared secret (API HMAC signing). Use signatures when a third party needs to verify (code signing, document signing, TLS certs).

### Authenticated encryption (AEAD)

Modern modes like AES-GCM combine encryption and MAC in one step, preventing whole classes of bugs. Use AEAD unless you have a specific reason not to.

---

## 2.7 Key management, the part where real systems fail

Crypto algorithms rarely break. Key management breaks all the time. This is where you should expect to spend most of your crypto-related GRC time.

### The lifecycle of a key

1. **Generation.** In a random, secure way (proper RNG, not `rand()`).
2. **Distribution.** Get it to the right place without leaking.
3. **Storage.** In a **Key Management System (KMS)**, **Hardware Security Module (HSM)**, or cloud KMS (AWS KMS, Azure Key Vault, GCP KMS).
4. **Use.** Ideally the key never leaves the KMS. The KMS performs the operation and returns the result.
5. **Rotation.** Periodically replace the key.
6. **Revocation / destruction.** Securely retire when compromised or past its life.

### Key concepts you must know

- **KEK vs DEK.** Key-Encrypting-Key vs Data-Encrypting-Key. A cheap small KEK protects many DEKs. Standard envelope-encryption pattern.
- **BYOK (Bring Your Own Key).** Customer supplies their own master key to the cloud KMS. Common in enterprise SaaS.
- **HYOK (Hold Your Own Key).** Stronger. Customer holds the key. Cloud cannot access it without explicit approval.
- **Customer-Managed Keys (CMK)** vs provider-managed.
- **Key ceremony.** A formal, witnessed process to generate root keys in HSMs. Yes, people wear suits to these.

### Common real-world failures

- Hardcoded API keys in GitHub repos. Use secret scanning, Gitleaks.
- Secrets committed in `.env.example` that was never meant to go live.
- Same key used across dev, staging and prod.
- No rotation. Keys five years old.
- Passwords (not random keys) used to encrypt critical data.
- "We encrypted it" but the encryption key lived next to the ciphertext in the same database.
- In one SaaS environment I worked in, the rotation policy said "annual" and the runbook didn't exist. Nobody had rotated in three years. The fix was a one-page runbook, not new tooling.

### Mini-exercise (15 min)

For an app you know, list:
1. Where encryption keys are stored.
2. Who can access them.
3. When they were last rotated.
4. What happens if they're compromised tomorrow.

If you can't answer any of these, you've found your first improvement project.

---

## 2.8 A quick word on post-quantum cryptography

Classical asymmetric algorithms (RSA, ECC) rely on math problems a sufficiently powerful quantum computer could solve. That computer doesn't exist yet at useful scale, but encrypted data stolen today can be decrypted later ("harvest now, decrypt later"). NIST finalised its first post-quantum algorithms in 2024: ML-KEM (CRYSTALS-Kyber) for key encapsulation, ML-DSA (CRYSTALS-Dilithium) for signatures.

Your organisation should have a **crypto-agility** plan, meaning the ability to swap algorithms when standards change. You do not need to migrate tomorrow. You do need to know the words and know where in your stack a swap would be hardest (usually long-lived signed artefacts and embedded clients).

---

## 2.9 Go deeper (curated free)

- 📘 [Crypto 101, free online book by lvh](https://www.crypto101.io/). The most accessible deep dive.
- 📘 [Khan Academy, Journey into Cryptography](https://www.khanacademy.org/computing/computer-science/cryptography)
- 🧪 [Cryptopals Challenges](https://cryptopals.com/). Learn by breaking weak crypto. The single best hands-on resource.
- 📰 [Cloudflare, How TLS works](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)
- 📰 [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- 🏛 [NIST FIPS 140-3](https://csrc.nist.gov/projects/cryptographic-module-validation-program). The crypto-module validation standard. Know the name.
- 🏛 [NIST Post-Quantum Cryptography project](https://csrc.nist.gov/projects/post-quantum-cryptography)
- 🎥 [Computerphile cryptography playlist (YouTube)](https://www.youtube.com/@Computerphile)

---

## Module 2, Glossary recap

Encoding, Base64, Hashing, Salt, Pepper, MD5, SHA-256, bcrypt, scrypt, Argon2, Symmetric encryption, AES, AES-GCM, ChaCha20-Poly1305, ECB/CBC/GCM modes, Asymmetric encryption, Public key, Private key, RSA, ECC, Ed25519, Key agreement, Diffie-Hellman, ECDHE, Forward secrecy, TLS handshake, Certificate, CA, Chain of trust, Let's Encrypt, MAC, HMAC, Digital signature, AEAD, KMS, HSM, KEK, DEK, BYOK, HYOK, CMK, Key rotation, Key ceremony, Post-quantum cryptography, Crypto-agility.

→ Next: [Module 3, Identity, Access & Authentication](03-identity-and-access.md)
