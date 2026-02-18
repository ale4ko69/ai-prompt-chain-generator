---
name: {{agent_name}}
target: {{target_env}}
model: {{model}}
description: '{{agent_description}}'
tools: {{tools_list}}
---

# {{agent_name}}

## Purpose

{{agent_purpose}}


## How to Adapt
1. Replace all {{...}} variables with values relevant to your project.
2. For long project names, use a short identifier (e.g., npap for new-project-ai-prompts) in the agent file name: `{project_id}-dev.agent.md`.
3. At the top of the agent file, specify the full project name and its purpose.
4. Specify actual paths to documentation, templates, and rules (e.g., templates/base/universal-rules.md, chains/, etc.).
5. Make sure all links and recommendations match your repository structure.

## Project Integration
- Core standards: templates/base/universal-rules.md
- Pattern templates: templates/base/languages/
- Analysis chains: chains/
- Documentation: README.md, CONTRIBUTING.md

## Agent Capabilities
- Code generation and refactoring according to local standards
- Following project templates and chains
- Synchronization and adaptation of features between projects
- Documentation review and update
- Use only allowed tools and dependencies

## Usage Scenarios
- New feature development
- Bug fixing
- Refactoring
- Code synchronization between projects
- Documentation audit and update
