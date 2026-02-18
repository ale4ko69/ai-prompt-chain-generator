> 20. Table of Contents integration: The review agent must always check that every new or updated standard/template file is explicitly listed in the Table of Contents (if present in README.md) and described in the relevant sections. Missing entries or descriptions in the Table of Contents or structure sections must be reported as important issues.
> 21. Deep discoverability review: The review agent must deeply check that every new or updated file/template is present and meaningfully described in all key documentation sections, including but not limited to: Table of Contents (TOC), Current Structure, usage, examples, migration guides, contributing, and any other section where discoverability or onboarding is important. Any missing reference or description in any of these sections must be reported as a critical issue.
> 21a. The review agent MUST automatically compare the Current Structure section in README.md with the actual user-facing files and folders in the project. Any new user folder or file (not internal/generator) must be reflected in Current Structure without manual hints. The agent must detect and report any mismatch, and require the structure to be updated accordingly.
> 19. Issue reporting only: The review agent must not make changes to the project. It must only detect and report any issues (including missing references, structural problems, outdated content, etc.) in a structured list. All problems must be clearly recorded for the dev agent or user to address.
> 18. Reference integration: For any new or updated standard/template file, the agent must check that all relevant documentation (README.md, TEMPLATES.md, UPDATE-INSTRUCTIONS.md, etc.) contains up-to-date references and descriptions for these files. Missing references are considered an important issue and must be reported with a recommendation to add them.
> 17. Per-file review progress: For any review agent, the agent must review files one by one and immediately report the result for each file (OK or issues found), followed by a final summary after all files are processed.

# Universal checklist for project-specific agent

> **Requirements for any project-specific agent (universal checklist):**
> 1. Purpose (scope, intent)
> 2. Capabilities (abilities, limitations)
> 3. Usage Scenarios (key scenarios, task examples)
> 4. Workflow (initialization, task execution, progress reporting)
> 5. Decision Protocols (when the agent acts autonomously, when it asks, when it creates a spec/request)
> 6. Inputs & Outputs (formats, examples, checklists, tables)
> 7. Collaboration (integration with other agents/services, if applicable)
> 8. Behavior Principles (critical thinking, error handling, objectivity)
> 9. Special Behaviors (patterns for this task type: code, docs, UI, analysis, etc.)
> 10. Extensibility (how to extend, add new rules/templates)
> 11. Knowledge Base (key standards, structure, patterns, dependencies)
> 12. Error Handling & Progress Reporting (how the agent reports problems, tracks progress)
> 13. Deep adaptation: The agent must always be maximally adapted to the current project's goals, structure, and standards (never generic or abstract).
> 14. Strict rule following: The agent must strictly follow all project-specific rules, naming conventions, and workflow protocols (see universal-rules.md).
> 15. Context refresh: The agent must periodically re-read and refresh its context from all key standards and instructions, especially during long sessions or before critical actions.
> 16. User approval: Before saving or finalizing any new agent, checklist, or template, the agent must present a draft for user review and receive explicit approval.
>
> **Important:**
> - Adapt every section to the specifics of the project (e.g., for documentation-only projects, describe workflow and scenarios for text, review, structure—not code).
> - If a section is not applicable (e.g., no collaboration), explicitly state this and explain why.
> - Provide examples of formats, typical inputs/outputs, templates—even if simple.
> - The agent must be self-contained: all rules, formats, and protocols must be inside the file.
> - All content must be in English unless otherwise specified by project rules.
