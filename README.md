# build-products-kunal

Three Agent Skills for building products with AI and shipping them to real users, each sharpened by lessons from tearing apart real products.

| Skill | For | What it makes the agent do |
|---|---|---|
| **build-products-kunal** | building the software | add what PRDs leave out (security, cost caps, data rules, backups), explain every technical choice in plain English, write decision records, verify its own work, and hold a pre-launch gate before you charge money |
| **build-products-pm-kunal** | making product decisions | ground every decision in sourced, dated data from the internet, find documented decisions by comparable products that worked (and their conditions), estimate impact as honest ranges, and write a decision record with metrics, guardrails and a pre-mortem |
| **build-products-design-kunal** | designing the interface | start from user goals, define tokens before screens, design every state, measure contrast and target sizes, pass a WCAG 2.2 AA gate, write real microcopy, run a heuristic review and a 5-person test, and check for dark patterns |

Each skill has a `references/lessons-from-teardowns.md`: rules learnt from real products by [product-teardowns](https://github.com/k4kd/product-teardowns), each with the products it came from and the conditions under which it holds.

## Includes minimal-code discipline

build-products-kunal has a "ponytail mode" built in: the agent writes the smallest change that fully solves the task, skips unrequested extras, and ends each reply with what it skipped or could not verify. Say "lite", "full" or "ultra" to change how hard it pushes back on scope (default: full).

This part is adapted, in my own words, from [ponytail](https://github.com/DietrichGebert/ponytail) by DietrichGebert (MIT). If you want ponytail's hooks, commands, and review and audit skills, install it alongside this skill.

## Install

```bash
# All three, in every project (user level)
npx skills add k4kd/build-products-kunal -g

# Target a specific agent
npx skills add k4kd/build-products-kunal -g -a claude-code
```

If you use [product-teardowns](https://github.com/k4kd/product-teardowns), its setup installs these for you and keeps them on the latest version.

## Use

- "Use build-products-kunal. Here is my PRD and the design files. Help me build this."
- "Use build-products-pm-kunal: should we add a free tier? Back it with data."
- "Use build-products-design-kunal to review this checkout flow."
- "Run the pre-launch gate on this project."

## How the lessons grow

There is one version of these skills: this repo. People who use product-teardowns can choose, at setup, to contribute what they learn. If they do, each teardown opens a small pull request here that touches only the relevant lessons, and it's merged after review. Everyone gets merged lessons on their next run, or with `npx skills update`.

## Repo layout

```
skills/
  build-products-kunal/          SKILL.md, references/lessons-from-teardowns.md
  build-products-pm-kunal/       SKILL.md, references/lessons-from-teardowns.md
  build-products-design-kunal/   SKILL.md, references/lessons-from-teardowns.md
```

## License

MIT
