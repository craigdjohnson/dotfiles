---
applyTo: "**"
description: "General guidelines for all code contributions regardless of language or framework."
---

# General Code Contribution Guidelines

These guidelines apply to all code contributions regardless of language or framework.

## Core Principles

- **Prioritize Minimal Impact:** Understand code context before modifying. Make the smallest change possible while preserving functionality.
- **Targeted Implementation:** Modify only essential code sections. Maintain existing behavior in unrelated areas.
- **Clarify Ambiguities:** Request clarification when requirements are unclear.
- **Document Potential Enhancements:** Note improvements outside immediate scope without implementing them.
- **Ensure Reversibility:** Design changes to be easily reverted if needed.
- **Avoid Premature Optimization:** Focus on clear, maintainable code first. Optimize only when necessary.

## Graduated Change Strategy:

- **Default:** Implement minimal focused changes
- **When necessary:** Propose localized refactoring
- **Only when requested:** Perform comprehensive restructuring
- **Never:** Make sweeping changes without clear justification

## Quality Standards

- **Clarity & Readability:** Use descriptive names and single-purpose functions
- **Consistency:** Follow project conventions unless change is justified
- **Error Handling:** Anticipate failures with appropriate mechanisms
- **Security:** Sanitize inputs and manage secrets securely
- **Testability:** Design for easy testing with adequate coverage
- **Documentation:** Comment complex logic with standard formats

## Commit Standards

- **Version Control:** Use Conventional Commits format: `type(scope): description`
- **Version Control:** Examples: `feat(api):`, `fix(auth):`, `docs(readme):`, `refactor(login):`
