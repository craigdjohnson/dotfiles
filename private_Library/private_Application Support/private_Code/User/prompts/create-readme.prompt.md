---
name: Create-README
agent: agent
description: "Intelligent README.md generation prompt."
model: Auto (copilot)
---

# README Generator Prompt

Generate a comprehensive README.md for this repository by analyzing the .github/copilot-instructions.md file.
Format the README with proper Markdown, including:

-   Clear headings and subheadings
-   Code blocks where appropriate
-   Lists for better readability
-   Links to other documentation files
-   Badges for build status, version, etc. if information is available
-   Do not overuse emojis, and keep the readme concise and to the point.
-   Do not include sections like "LICENSE", "CONTRIBUTING", "CHANGELOG", etc. There are dedicated files for those sections.
-   Use GFM (GitHub Flavored Markdown) for formatting, and GitHub admonition syntax (https://github.com/orgs/community/discussions/16925) where appropriate.
-   If you find a logo or icon for the project, use it in the readme's header.

Keep the README concise yet informative, focusing on what new developers or users would need to know about the project.

Follow these steps:

1. Review the copilot-instructions.md file in the .github folder

2. Also review any existing README.md files in the repository (knowing they may be outdated)

3. Create a README.md with the following sections:

## Project Name and Description

-   Extract the project name and primary purpose from the documentation
-   Include a concise description of what the project does

## Technology Stack

-   List the primary technologies, languages, and frameworks used
-   Include version information when available

## Project Architecture

-   Provide a high-level overview of the architecture
-   Consider including a simple diagram if described in the documentation

## Getting Started

-   Include installation instructions based on the technology stack
-   Add setup and configuration steps
-   Include any prerequisites

## Key Features

-   List main functionality and features of the project
-   Extract from various documentation files

## Development Workflow

-   Summarize the development process
-   Include information about branching strategy if available

## Coding Standards

-   Summarize key coding standards and conventions

## Testing

-   Explain testing approach and tools
-   Source from _\_Test._ files
