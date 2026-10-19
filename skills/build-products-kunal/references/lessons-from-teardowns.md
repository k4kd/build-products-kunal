# Lessons from teardowns

Rules learnt by taking apart real products with product-teardowns. Each rule is something a shipped product got right, or got wrong in a way worth avoiding. Apply them when writing the non-functional requirements (Step 2) and choosing the stack (Step 3). The products each rule came from are in brackets.

## Accounts and access
- Let people browse and start a purchase as a guest, then log in with a one-time code sent to their phone. No password is needed before they see value. (from srimandir)
- Use a ready-made login service. Never ship a single shared admin password with unlimited guesses. (from onion-link-list)
- When a local helper program takes commands, generate a random secret on each run and reject any message without it. (from rea)

## Data
- Make the database table the single source of truth, and keep timed workers stateless, so a crash mid-job loses nothing. (from onion-link-list)
- Let data clean itself up: delete or archive rows that have been dead past a fixed age, on a schedule. (from onion-link-list)
- Stop duplicates with a unique constraint in the database, not with checks in app code. (from onion-link-list)
- Never print personal data (email, phone) to the browser console or logs on a live site. (from srimandir)

## Payments
- Compute the price on the server. The browser sends what was chosen plus a signed price token, never a price it could edit. (from srimandir)
- Use hosted checkout (for example Razorpay or Stripe), so card details never touch your servers. (from srimandir)
- Ask only what payment needs before paying. Collect the rest (address, preferences) after it succeeds, because every extra field before paying loses buyers. (from srimandir)

## Errors
- Never silently cut a response short when it is too big. Return a clear error that says how to fetch it in pages. (from rea)
- When more than one option fits, ask, rather than picking one silently. (from rea)

## Cost control
- Cache expensive results, and write down exactly what makes each cached result go stale. Reuse is only safe when invalidation is clear. (from rea)
- Process work in small fair batches (the oldest-checked items first), so a cheap server keeps up with thousands of items. (from onion-link-list)

## Secrets
- No new rules yet.

## Backups and recovery
- No new rules yet.

## Performance and scale
- Give built files content-fingerprinted names and cache them for a year. Cache HTML at the CDN for minutes, never personal pages. (from srimandir)
- Feed a fragile, single-threaded resource one job at a time through a first-in-first-out queue. (from rea)

## Stack
- Pick a framework and host that give you a CDN with no setup (for example Next.js on Vercel). (from srimandir)
- Send app events to one tag manager or analytics tool through a data layer from day one, so marketing tools can be added later without code changes. (from srimandir)
- Mark any outside text you pass to an AI as untrusted data, never instructions. (from rea)
- Only follow links or addresses from a list you approved, and never turn off HTTPS certificate checks. (from onion-link-list)

## Keep it minimal
- A useful product can ship as one table, one timed job and one page. (from onion-link-list)
- Start with one analytics tool. Every extra tracking vendor slows the page and adds risk. (from srimandir)
- Keep throwaway scripts out of the code you ship. (from onion-link-list)
- Cover one use case deeply before going broad: one engine and five tools beats every engine and 250 tools. (from rea)
