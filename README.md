# build-products-kunal

An Agent Skill for non-developers who want to build products with AI and ship them to real users.

It makes the agent add the things PRDs usually leave out (security, cost caps, data rules, backups), explain every technical choice in plain English, write short decision records, verify its own work with evidence, and stop you from charging money until a pre-launch checklist is done.

## Includes minimal-code discipline

The skill has a "ponytail mode" built in: the agent writes the smallest change that fully solves the task, skips unrequested extras, and ends each reply with what it skipped or could not verify. Say "lite", "full", or "ultra" to change how hard it pushes back on scope (default: full).

This part is adapted, in my own words, from [ponytail](https://github.com/DietrichGebert/ponytail) by DietrichGebert (MIT). If you want ponytail's hooks, commands, and review and audit skills, install it alongside this skill.

## Install

Replace `<your-github-username>` with the account that hosts this repo.

```bash
# Available in every project (user level)
npx skills add <your-github-username>/build-products-kunal -g

# Only for the current project
npx skills add <your-github-username>/build-products-kunal

# Target a specific agent
npx skills add <your-github-username>/build-products-kunal -g -a claude-code
```

For Claude Code, a user-level install ends up in `~/.claude/skills/build-products-kunal/`.

## Use

Ask your agent something like:

- "Use build-products-kunal. Here is my PRD and the design files. Help me build this."
- "Add non-functional requirements to my PRD."
- "Run the pre-launch gate on this project."

## Repo layout

```
skills/
  build-products-kunal/
    SKILL.md
```

## License

MIT
