# User Command Reference for Agents

**Purpose:** Centralized list of user commands for all AI agents, including those with subagents and chain execution.

---

## General Agent Commands
- **Help** — display this command reference list
- **Context** — show current work context: active branch, changed files, current task status
- **Re-read docs** — re-read all project documentation (chains/, templates/, specs, instructions)
- **Re-read spec** — re-read feature spec and all related documentation
- **Document rule** — document rules/patterns in instructions and guidelines (MUST update documentation)

## Git Operations
- **Check status** — show `git status` and summarize changes
- **Commit** — create git commit of relevant files with detailed message
- **Push** — push changes to remote repository
- **New branch** — create new branch from main and checkout (ask for name if needed)
- **Revert changes** — revert changes in specified files
- **Show changes** — show list of changed files with brief summary

## Workflow Control
- **Plan** — output brief action plan (3-7 steps) before starting work
- **Start** — start implementation immediately without additional questions
- **Stop** — halt all actions and wait for further instructions
- **Collect context** — list important files and current task state

## Quality & Testing
- **Check errors** — check errors/lint in affected files
- **Run tests** — run tests for affected area
- **Apply style** — strictly follow existing patterns and code style
- **Change only relevant** — change only what's relevant to the task
- **Update instructions** — update documentation rules/instructions

---

## Code Review Agent Commands
- **Review code** — perform code review for specified files, PR, or code changes
- **Check dependencies** — verify all suggested packages exist in project dependencies
- **Find patterns** — search for existing solutions and patterns in the codebase
- **Suggest fix** — provide actionable feedback with code examples and priority

---

## Subagent & Chain Execution Commands
- **Run chain** — execute a predefined chain of agents for complex tasks
- **Delegate to subagent** — assign a specific subtask to a subagent (specify agent and task)
- **Show chain status** — display progress/status of current agent chain execution
- **Sync agents** — synchronize state or data between agents in a chain

---

**For the full set of universal rules and best practices, see:**
[templates/base/universal-rules.md](../base/universal-rules.md)
