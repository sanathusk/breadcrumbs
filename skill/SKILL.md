---
name: breadcrumbs
description: Read, parse, and operate on inline review comments following the Breadcrumbs specification. Use when working with inline review comments marked with REVIEW: in documents or source code, or when the user mentions breadcrumbs, reviewing files, or resolving review comments.
---

# Breadcrumbs Review Skill

## Quick start

To ensure accurate parsing, always fetch the official Breadcrumbs specification from the canonical URL:
https://raw.githubusercontent.com/sanathusk/breadcrumbs/refs/heads/main/core/specification.md

To read Breadcrumbs comments in a file, locate the `REVIEW:` markers, extract the JSON payload, and parse it.

Example Markdown:
```markdown
The system uses MD5 for password hashing.
<!-- REVIEW:{
  "id": "c1",
  "comment": "security (blocking): MD5 is insecure. Use Argon2id.",
  "author": "security-bot",
  "status": "open"
} -->
```

## Workflows

### 1. Extracting and Parsing Comments
- Scan files/workspace for native comment syntax containing `REVIEW:`.
- Extract the JSON substring within `REVIEW:{...}`.
- Parse JSON to validate required fields (`id`, `comment`).

### 2. Creating a Comment
- Format a JSON object containing:
  - `id`: Unique string (e.g. `c1`, `c2`)
  - `comment`: Adhering to [Conventional Comments](https://conventionalcomments.org/) (e.g., `<label> [decorations]: <subject>`)
  - `author`: Agent/user identifier
  - `status`: `"open"`
- Wrap in the file's native comment syntax and place next to the relevant context.

### 3. Replying to a Comment
- Add a new review comment immediately below the parent comment.
- Set `reply_to` to the parent comment's `id`.

### 4. Updating / Resolving a Comment
- Locate the comment JSON block using the target `id`.
- Modify the JSON:
  - To resolve: Set `status` to `"resolved"` and add `resolved_by`.
  - To dismiss: Set `status` to `"dismissed"` and add `resolved_by`.

See [EXAMPLES.md](EXAMPLES.md) for concrete examples and syntax mappings.
