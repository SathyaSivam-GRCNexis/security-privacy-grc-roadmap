# Practice Scenarios and Capstones

> Three finished capstones from this folder beat five certifications on a CV. Hiring managers will tell you this over coffee, not on a job spec.

Modules 0 to 16 are where you pick up the vocabulary. Module 17 and this folder are where you prove you can actually use it. The split matters because most candidates can recite definitions and then fall apart the moment someone asks them to draft a real artefact under time pressure.

## What lives here

- The scenarios sit in [`modules/17-practice-scenarios.md`](../modules/17-practice-scenarios.md). Ten messy, under-specified situations of the kind you actually run into in a GRC, security or privacy role. Each one has stakeholders, constraints, deliverables, a rubric, and hints you should not read until you have had a proper go.
- The templates in [`templates/`](templates/) are the scaffolds: risk register, DPIA, Statement of Applicability, IR runbook, vendor assessment, policy template, and audit evidence pack.

Scenarios tell you what to produce. Templates show you what good looks like when you produce it.

## Which scenarios mirror real work, and which are exam-flavoured

Being honest about this, since it is the question I wish someone had answered for me:

- Closest to real day-to-day work: the vendor assessment scramble, the DPIA for a new feature, the access-review evidence pack, the breach notification call. These are the ones I have done some version of in actual sprints. The deliverables match the artefacts on my drive.
- Useful but more exam-flavoured: the full ISMS bootstrap from zero, the cross-framework mapping exercises, the board memo at the end of a fictional incident. Real organisations rarely hand a junior the keys to bootstrap an ISMS in eight weeks. You will see slices of this work, not the whole thing. Treat it as a forcing function to make you read the standards properly.
- Worth doing once even if it feels artificial: the incident tabletop. The artificial bit is the time compression. The skill it builds, holding a structure under pressure while making notification calls, transfers exactly.

If you only have time for three, do one vendor assessment, one DPIA, and one IR scenario. That combination covers the questions you will actually face in the first six months of a role.

## Why this folder exists separately

People read Module 17, nod, and never sit down to do a scenario. The deliverables look like a lot of work. They are. That is the point.

The second failure mode is more subtle. People produce deliverables that are technically correct and look nothing like a real workplace artefact. A risk register that is a bullet list is not a risk register. A DPIA without a lawful basis is not a DPIA. The templates here exist so you produce something an auditor or DPO would actually recognise.

## How to use this folder

Pick a scenario from Module 17 that matches the role you are going for.

- GRC analyst, privacy associate, compliance roles: start with Scenarios 1, 2, 4, 5, 6, 9.
- Security engineer, security architect, AppSec: start with Scenarios 3, 7, 8, 10.
- Career shifters going for breadth: one from each cluster.

Then:

1. Read the scenario in full. Do not skim.
2. Identify which templates from [`templates/`](templates/) apply. Most scenarios use two or three.
3. Open a private repo (or a folder on your laptop if you are not ready to publish). Copy the templates across. Rename them for the scenario.
4. Fill them in like the job depended on the output. Time-box it. A real evidence scramble does not give you three weeks.
5. Write a 2-page README for the scenario covering the problem, your approach, and what you would do differently with more time. The README is the file a hiring manager opens first.
6. Compare your output against the rubric in Module 17. Be honest about the gaps.

Do not read the hints in Module 17 until you have had a proper attempt. The hints describe what strong answers cover and what weak ones miss. Reading them first is the same as reading the answer key before sitting the exam. You will feel clever and learn nothing.

## Turning these into portfolio work

Once you have three scenarios you are not embarrassed by:

1. Sanitise. Strip anything that looks like real client data, even if you invented it. The modules use a fictional EdTech platform as the running example. Extend that, or invent your own clearly fictional company. Either is fine, as long as nothing reads like a real organisation.
2. Make the repo public. Pin it on your GitHub profile.
3. Add it to LinkedIn under Featured. Use the word capstone, not project. Project sounds like coursework.
4. Bring two of them ready to discuss in interviews. Not recite. Discuss. Be ready to defend the trade-offs and name what you would do differently.

A senior interviewer will not test whether your DPIA is perfect. They will test whether you can talk about why you made the calls you made. That is the entire game.

## What good submissions look like

What a hiring manager is actually scanning for, in order:

1. You separated what you knew from what you assumed. Real work is full of unknowns. Strong submissions name them.
2. You prioritised. You did not try to fix everything. You picked the two or three things that mattered and defended the choice.
3. You wrote up. The board memo, the email to the CTO, the customer-facing letter. These get graded harder than the technical artefacts. Most candidates are passable at the technical work and poor at the writing. Be the candidate who is not.
4. You wrote like a person. Plain English. Short sentences. No jargon used to sound clever. If a non-technical executive cannot follow your exec summary, it is not an exec summary.

## A note on AI

Use an AI assistant to help you draft if you want. Do not use it to think for you. The point of these scenarios is to build judgement, and judgement does not transfer when something else makes the calls.

A useful test: after you finish, close the laptop, walk away for an hour, come back, and explain your top three decisions out loud to an empty room. If you cannot defend them without re-reading what you wrote, you do not know your own work, and you will fail the interview discussion.

---

Start with [`modules/17-practice-scenarios.md`](../modules/17-practice-scenarios.md), then come back to [`templates/`](templates/) when you are ready to produce deliverables.
