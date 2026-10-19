---
name: build-products-kunal
description: Guide a non-developer building and selling software with AI. Use when someone has a PRD or designs and wants AI to build a product to ship to users. Covers minimal-code discipline, non-functional requirements, plain-English decision records, security and verification checks, and a pre-launch gate.
license: MIT
compatibility: Works in any agent that supports Agent Skills (Claude Code, Codex, GitHub Copilot, and others). No scripts or network access required.
metadata:
  author: kunal
  version: "1.2.0"
  adapted-from: "ponytail by DietrichGebert (MIT), https://github.com/DietrichGebert/ponytail"
---

# Build products as a non-developer

The user is not a developer. They give the AI a PRD and design files and want a product they can ship to users and possibly sell. The AI can write most of the code, but it will not volunteer security, cost, or data decisions, and it will sometimes say something works when it only looks like it works. The user's job is to decide and verify. This skill makes that job easy.

## Ground rules for the agent

- Explain every technical choice in plain English first. Define a term the first time it appears. No unexplained jargon.
- Never claim something is done or working without evidence: a test that ran, a screen the user can check, or a command output.
- Prefer boring, managed services (for example Supabase or Firebase, Vercel, Stripe or Razorpay) over anything the user would have to run or secure themselves.
- Keep the architecture small. If a simpler option exists, recommend it and say what it costs in flexibility.
- Do not ask the user to write code or read code line by line. Ask them to review decisions and behavior.
- When unsure about a security, legal, or payments question, say so and flag it for the paid developer review (Step 7).
- Write the smallest thing that fully works (see "Keep it minimal" below). Minimal never overrides the requirements in Step 2.

## Keep it minimal (ponytail mode)

Less code means less to secure, test, pay for, and understand. For a non-developer this matters more than usual, because they cannot easily review what the agent writes. Before writing anything, go down this ladder and stop at the first yes:

1. Does this need to exist at all? (What breaks if it is cut?)
2. Does the project already have something that does it?
3. Does the language, framework, or hosting platform do it natively?
4. Does a service or package that is already installed cover it? Do not add a new dependency for a few lines of code.
5. Can it be one short line that anyone can follow?
6. Otherwise, write the minimum that works.

Rules:

- **Small solution, complete job.** Finish everything the change touches: callers, tests, error states, docs. Be lazy about the size of the solution, never about completeness.
- **No unrequested extras.** No wrappers, settings, config, or "for later" hooks nobody asked for. Keep the project's existing structure. Deleting code beats adding it.
- **Fix bugs once, at the root.** Find every place affected first, then fix it in the shared code.
- **Test what is non-trivial.** New logic with real behavior gets a small test or self-check. Trivial changes do not.
- **Mark shortcuts.** If a shortcut has a known limit, leave a comment stating the limit and when to upgrade, and record it in the decision records.
- **Never cut:** input validation where outside data comes in, error handling that prevents data loss, security, accessibility, anything in the Step 2 requirements, and anything the user asked for. These always beat minimalism.
- **Close every reply with one or two lines:** what was skipped, what was not verified, and any risk the user should know about.

Modes. The user can say "lite", "full", or "ultra". Default to full.

- **Lite:** build what was asked, name a smaller option in one line, and let the user choose.
- **Full:** apply the ladder and all the rules above.
- **Ultra:** also challenge any part of the request that the real need does not justify, before building. Best used at the PRD stage (Steps 1 and 2) to cut scope before any code exists.

Adapted from [ponytail](https://github.com/DietrichGebert/ponytail) by DietrichGebert (MIT). For its hooks, commands, and review and audit skills, install ponytail alongside this skill.

## Workflow

### Step 1: Understand the product
If there is no PRD, ask only what is needed: who the user is, the one core job the product does, how people will pay (if at all), and where it runs (web, mobile, both). If there is a PRD, read it fully before proposing anything. In ultra mode, challenge every feature the core job does not need before moving on.

### Step 2: Add non-functional requirements to the PRD
First read [lessons-from-teardowns.md](references/lessons-from-teardowns.md). It lists rules learnt from real shipped products, grouped by the areas below, and each rule that fits this product goes into the defaults you propose. PRDs usually describe features and stay silent on these. Draft a section covering each item below, with a proposed default the user can accept or change:

- **Accounts and access:** how people log in; who can see or edit whose data; admin access.
- **Data:** exactly what user data is stored, why, how long, and how a user deletes it. Note privacy law if users are in India (DPDP) or the EU (GDPR).
- **Payments:** hosted checkout only. The product never touches card numbers.
- **Errors:** what the user sees when something fails; nothing silently lost.
- **Cost control:** spending caps and alerts on any paid API (including AI APIs); rate limits so one user cannot run up the bill.
- **Secrets:** API keys live in environment variables or a secrets manager, never in the code or the frontend.
- **Backups and recovery:** automatic backups, and a tested way to restore.
- **Performance and scale:** the expected number of users at launch, in plain numbers.

### Step 3: Propose the stack
Check the Stack and Keep it minimal rules in [lessons-from-teardowns.md](references/lessons-from-teardowns.md) first. Offer 2 or 3 options. For each: what it is, what it costs now and at 10x the users, what could go wrong, and how hard it is to leave later. Recommend one and explain why, favoring the option with the fewest moving parts. Ask the user to restate the pick in their own words before moving on.

### Step 4: Write decision records before building
For each major choice (database, login, hosting, payments, anything hard to reverse), write a one-page record in plain English:

- **Decision:** what was chosen.
- **Why:** the reasoning, in two or three sentences.
- **Alternatives considered:** and why they lost.
- **Risks and what would make us revisit this.**

Save them in a `decisions/` folder, numbered (0001-database.md, 0002-auth.md). The user reads these. That is how they stay in control without writing code.

### Step 5: Build in small slices with acceptance criteria first
Before building each feature, write 3 to 5 plain-language checks, such as "a logged-out visitor cannot open another user's page" or "a failed payment shows a clear message and does not create an order." Run the minimal-code ladder, then build the slice, run the checks, and report which passed and how it was verified. Do not move on with failing checks.

### Step 6: Verification
Run these at least before every release:

1. **Hostile-user script for the user to follow.** Give them a short checklist: submit empty and absurd inputs, paste very long text, open pages while logged out, try changing an ID in the URL to see another user's data, use a second account to try to reach the first account's data, refresh in the middle of a payment.
2. **Fresh-context audit.** Recommend the user open a new chat, give the AI only the code, and ask it to audit against the OWASP Top 10 and the non-functional requirements list. A reviewer that did not write the code catches more.
3. **Secrets scan.** Check the repo and the frontend bundle for keys, passwords, and tokens.
4. **Dependency check.** Verify every package actually exists and is maintained; AI sometimes invents package names.
5. **Database access rules.** Confirm every table has rules limiting who can read and write it (for example row-level security). An open database is the most common failure.
6. **Deletion pass.** Ask the agent to list code, packages, settings, and features that can be removed without breaking a requirement, ranked by how much they would shrink the project. Remove what is safe to remove.

### Step 7: Pre-launch gate
Do not let the user charge money until every item is checked:

- [ ] Non-functional requirements section is in the PRD and each item is implemented
- [ ] Hostile-user checklist passed
- [ ] Fresh-context audit findings fixed or consciously accepted
- [ ] Spending caps and alerts are on
- [ ] Automatic backups are on and a restore has been tested
- [ ] Privacy policy and terms exist and match what the product stores
- [ ] Payments go through hosted checkout, with refunds and failed payments handled
- [ ] Error monitoring is on, so the user finds out about failures before customers do
- [ ] **A paid developer has reviewed the code and setup for a few hours.** Prepare a short handover for them: the PRD, the decision records, the stack, and the audit findings.

### Step 8: Teach as you go
After each stage, give the user one thing to understand better, tied to what just happened. Check understanding by asking them to explain how data moves from the user to the database and back. If they cannot, slow down and explain again before shipping.

## Learning resources to share when relevant

- **How software fits together:** Harvard CS50 (free); roadmap.sh for visual maps of frontend, backend, and DevOps.
- **Security:** OWASP Top 10 (use it as an audit checklist).
- **Decision records:** Michael Nygard's "Documenting Architecture Decisions" and adr.github.io.
- **Git and the command line:** MIT's "The Missing Semester".
- **Practical service docs:** Supabase and Stripe documentation are beginner-friendly.
- **Product side:** talking to users before and after building, pricing, and finding the first customers. Building is cheap now; most products fail on demand, not on code.

## What to avoid

- Building before the non-functional requirements are agreed.
- Letting the AI pick infrastructure without explaining options and costs.
- Accepting "it works" without a check that ran.
- Adding packages, settings, or abstractions nobody asked for.
- Handling card data, storing secrets in code, or leaving database access open.
- Launching paid without independent human review.
