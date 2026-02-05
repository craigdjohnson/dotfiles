---
name: Create-Changie-and-Commit
agent: agent
description: 'Generate changie entries from the most recent commit.'
model: Auto (copilot)
---

## Role

You are a changelog automation agent responsible for converting recent and/or pending git commits into properly formatted changie entries that maintain consistency with the project's changelog standards. Always complete the changie entry creation before committing any changes to git so that the changie files are included in the commit.

## Task

### Prepare git commit
**Follow these steps:**

1. Run `git status` to review changed files.
2. Run `git diff` or `git diff --cached` to inspect changes.
3. Stage your changes with `git add <file>`.
4. Construct your commit message using the following XML structure, BUT DO NOT COMMIT IT YET.

#### Commit Message Structure

```xml
<commit-message>
	<type>feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert</type>
	<scope>()</scope>
	<description>A short, imperative summary of the change</description>
	<body>(optional: more detailed explanation)</body>
	<footer>(optional: e.g. BREAKING CHANGE: details, or issue references)</footer>
</commit-message>
```

#### Examples

```xml
<examples>
	<example>feat(parser): add ability to parse arrays</example>
	<example>fix(ui): correct button alignment</example>
	<example>docs: update README with usage instructions</example>
	<example>refactor: improve performance of data processing</example>
	<example>chore: update dependencies</example>
	<example>feat!: send email on registration (BREAKING CHANGE: email service required)</example>
</examples>
```

#### Validation

```xml
<validation>
	<type>Must be one of the allowed types. See <reference>https://www.conventionalcommits.org/en/v1.0.0/#specification</reference></type>
	<scope>Optional, but recommended for clarity.</scope>
	<description>Required. Use the imperative mood (e.g., "add", not "added").</description>
	<body>Optional. Use for additional context.</body>
	<footer>Use for breaking changes or issue references.</footer>
</validation>
```

### Prepare Changie entries

1. **Use prepared commit or retrieve the most recent commit**

    - Get the latest commit message and metadata using git commands
    - Extract both the subject line and full commit body

2. **Review changie configuration**

    - Search for `.changie.yaml` files in the repository root and common locations
    - Parse the file to identify valid `kind` values (e.g., `added`, `fixed`, `changed`, `deprecated`, `removed`, `security`)
    - Note any custom kind values specific to this project

3. **Analyze the commit message**

    - Determine the most appropriate `kind` based on:
        - The commit type/category (if using conventional commits like `feat:`, `fix:`, `docs:`, etc.)
        - The nature of the changes described in the commit message
        - The project's defined kinds in `.changie.yaml`
    - Extract and reformat the commit message into a user-friendly changelog entry:
        - Remove technical jargon where possible
        - Ensure the message clearly describes what changed and why
        - Keep it concise but informative (1-2 sentences typically)
        - Focus on user-facing or significant changes

4. **Generate the changie entry**

    - Execute the command: `changie new --kind <kind> --body <message>`
    - Replace `<kind>` with the determined kind
    - Replace `<message>` with the formatted changelog message
    - Ensure the message is properly quoted if it contains spaces or special characters

5. **Verify the result**
    - Confirm the changie file was created successfully
    - Review the generated entry to ensure it's properly formatted
    - Report the created changie filename and its content

#### Example Changie Workflow

If the prepared (or most recent commit) is:

```
feat(instructions): add python security guidelines

- Add comprehensive security best practices for Python
- Include OWASP Top 10 related guidance
- Add examples for secure database interactions
```

And `.changie.yaml` defines kinds like `added`, `changed`, `fixed`, `deprecated`, `removed`, `security`:

1. Match commit type `feat` to kind `added`
2. Reformat: "Added Python security guidelines with OWASP Top 10 best practices and examples for secure database interactions"
3. Execute: `changie new --kind added --body "Added Python security guidelines with OWASP Top 10 best practices and examples for secure database interactions"`

#### Changie Notes

-   If the commit type doesn't cleanly map to an available kind, choose the most semantically appropriate one
-   Prioritize clarity and usefulness in the changelog message
-   The changie tool will handle organizing entries into the proper changelog structure
-   Report back with confirmation of the created entry

### Commit prepared commit including the new/updated changie files.

1. Run `git status` to get the new/updated changie files.
2. Run `git add <file>` for the relevant new/updated changie files.
3. Run the below bash command to commit the changes + changie files:
```bash
git commit -m "type(scope): description"
```

#### Final Step example

```xml
<final-step>
	<cmd>git commit -m "type(scope): description"</cmd>
	<note>Replace with your constructed message. Include body and footer if needed.</note>
</final-step>
```
