# Claude Subagents for WordPress Development

This repository contains a set of preconfigured **Claude‑Code subagents** designed to streamline development, review and testing of WordPress plugins and block themes.  These subagents incorporate the [agent‑os WordPress coding standards and instructions](https://github.com/buildermethods/agent-os) so that you can delegate work to specialized assistants while maintaining consistent quality and security.

## What are subagents?

Claude‑Code subagents are specialised AI personas that handle specific tasks.  Each subagent:

- Has a **specific purpose and area of expertise**, such as building a plugin or running tests.
- Operates in its **own context window**, which keeps the main thread focused on high‑level goals.
- Can be given a custom **system prompt** that defines its behaviour, workflows and tone.
- May be restricted to a set of **tools** (e.g., Read, Write, Bash) or inherit all tools if none are specified.

By delegating work to subagents, you can preserve the main conversation context, maintain code quality and enforce best practices consistently.

## Where are subagent files stored?

Subagents are defined in Markdown files with YAML frontmatter.  You can store them in two locations:

- `.claude/agents/` inside a project – used only for that project, with the highest precedence.
- `~/.claude/agents/` in your home directory – available across all projects.

If two subagents have the same name, the project‑level version overrides the user‑level one.

## File structure

Each subagent file follows this format:

```markdown
---
name: my-subagent       # lowercase letters and hyphens
description: |
  When to use this subagent and why.
tools: Read, Bash        # optional – omit to inherit all tools
---

Your subagent’s system prompt goes here.  Use multiple paragraphs to describe the workflow,
include checklists, best practices and any constraints.  The prompt should tell the subagent
what it does and how to do it.
```

### Managing subagents

The easiest way to create, edit or remove subagents is with the `/agents` command inside Claude‑Code.  This command displays all available subagents, guides you through creating new ones and helps you configure their tool access.  Alternatively, you can edit the Markdown files directly under `.claude/agents/` or `~/.claude/agents/`.

To use a subagent explicitly, mention its name in your request (e.g., “Use the `wp-code-reviewer` subagent to review my changes”).  Claude‑Code will also automatically delegate tasks to subagents when the request description matches their `description` field and the tools are appropriate.

## Included subagents

The following subagents are provided in this repository:

### wp-builder

*Purpose:* Create or modify WordPress plugins or block/FSE themes using agent‑os coding standards.  This subagent asks clarifying questions, scaffolds the appropriate project structure (plugin files, block templates, `theme.json`, etc.), applies coding standards across PHP, CSS, HTML and JavaScript, enforces security and accessibility best practices, adds documentation and provides a summary of the generated files.  See [`wp-builder.md`](./wp-builder.md) for the full prompt.

### wp-code-reviewer

*Purpose:* Review WordPress plugin or theme code for quality, security and adherence to standards.  It focuses on naming conventions, file structure, indentation and spacing rules, proper sanitisation and escaping, UI/UX considerations and comprehensive documentation.  It organises feedback into critical issues, warnings and suggestions.  See [`wp-code-reviewer.md`](./wp-code-reviewer.md) for details.

### wp-test-runner

*Purpose:* Automate the execution of unit and integration tests for plugins or themes.  This subagent sets up a WordPress testing environment (e.g., via `wp-env`), runs tests using tools like PHPUnit or Jest, analyses failures, fixes issues while respecting coding standards and summarises the results.  See [`wp-test-runner.md`](./wp-test-runner.md) for the prompt.

## Using these subagents in your project

1. **Add the files:** Copy the `.md` files from this repository into the `.claude/agents/` folder of your WordPress project (create the folder if it doesn’t exist).  For example:

   ```bash
   mkdir -p .claude/agents
   cp wp-builder.md .claude/agents/
   cp wp-code-reviewer.md .claude/agents/
   cp wp-test-runner.md .claude/agents/
   ```

   Alternatively, copy them into `~/.claude/agents/` to make them available across all projects.

2. **Ensure prerequisites:** You’ll need the agent‑os WordPress coding standards (`wp-code-style.md`) and development instructions (`wp-development.md`) in your `agent‑os` installation so that the subagents have the necessary context.

3. **Use the subagents:** In Claude‑Code, ask for tasks and mention the subagent explicitly when necessary.  For example:

   - “Use the `wp-builder` subagent to scaffold a plugin named `my-custom-plugin`.”
   - “Ask the `wp-code-reviewer` subagent to evaluate my latest changes.”
   - “Run tests with the `wp-test-runner` subagent and fix any failures.”

These subagents will help you build, review and test WordPress plugins and themes efficiently while adhering to the high standards set by agent‑os.
