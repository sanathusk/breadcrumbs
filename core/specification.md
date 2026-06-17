# Breadcrumbs Specification (Version 1)

This document details the comment format, field reference, and rules for the Breadcrumbs inline review format.

## Preface

The Breadcrumbs specification is intentionally lean to maintain simplicity, keep comments close to the content, and ensure ease of adoption by both human developers and agentic workflows. By focusing solely on critical review details and avoiding complex workflow or project management mechanisms, Breadcrumbs remains lightweight, portable, and easy to parse.

---

## Comment Syntax

Review comments are embedded using the native comment syntax of the document format being reviewed. A review comment is identified by the `REVIEW:` marker, followed by a JSON object containing the metadata fields.

| Format | Native Comment Syntax |
| :--- | :--- |
| **Markdown / HTML** | `<!-- REVIEW:{...} -->` |
| **YAML / TOML / Shell** | `# REVIEW:{...}` |
| **JS / TS / JSONC** | `// REVIEW:{...}` or `/* REVIEW:{...} */` |

> [!IMPORTANT]
> Tools should recognize review comments regardless of the underlying comment syntax, and must ignore standard comments that do not begin with the `REVIEW:` marker.

---

## Comment Field Format

To keep feedback clear and actionable, the `comment` field should follow the principles of [Conventional Comments](https://conventionalcomments.org/).

### Format
```text
<label> [decorations]: <subject>

[discussion]
```

- **`<label>`**: Signifies the type of comment (e.g., `issue:`, `suggestion:`, `question:`, `praise:`, `nitpick:`, `todo:`, `thought:`, `chore:`, `note:`, `typo:`).
- **`[decorations]`** *(optional)*: Provides extra context or severity, enclosed in parentheses (e.g., `(blocking)`, `(non-blocking)`, `(security)`).
- **`<subject>`**: A concise summary of the feedback.
- **`[discussion]`** *(optional)*: Supporting context, reasoning, or next steps.

---

## Complete Field Reference

### Required Fields

*   **`id`**
    *   **Description**: Unique identifier for the comment.
    *   **Recommended formats**: Simple auto-incrementing numbers (e.g., `1`, `2` or `c1`, `c2`), hierarchical numbering (e.g., `1.1` for subsection/threaded reply), or arbitrary unique strings.
*   **`comment`**
    *   **Description**: Review comment text, adhering to the Conventional Comments format described above.

### Recommended Fields

*   **`author`**
    *   **Description**: The creator of the comment (person, team, or AI agent).
*   **`anchor`**
    *   **Description**: A hint/text representation of the content being reviewed. Helps tools relocate comments if the document changes.
    *   *Note*: Because comments are embedded inline, the physical placement of the comment is the authoritative location. The `anchor` is merely a fallback hint; if multiple instances of the anchor text exist, tools should prioritize the instance closest to the comment's physical location.
*   **`status`**
    *   **Description**: The current state of the comment.
    *   **Allowed values**:
        *   `open`: The comment still requires attention.
        *   `resolved`: The comment has been addressed.
        *   `dismissed`: No action will be taken and the discussion is closed.
    *   *Best Practice*: It is recommended to resolve comments (updating `status` to `resolved` or `dismissed`) only after all relevant stakeholders agree.

### Optional Fields

*   **`schema_version`**
    *   **Description**: Version of the Breadcrumbs format. If omitted, tools should assume the latest version of the specification.
    *   *Note*: Document/comment history and revision tracking are intended to be handled by version control systems (like Git), rather than within the comment schema itself.
*   **`addressed_to`**
    *   **Description**: Identifies who should respond to the comment (represents ownership of the comment).
*   **`reply_to`**
    *   **Description**: Parent comment ID, used for threaded discussions. A comment without `reply_to` is considered top-level.
*   **`resolved_by`**
    *   **Description**: Records who resolved or dismissed the comment (typically used with `status`).
*   **`mentions`**
    *   **Description**: References additional participants. Unlike `addressed_to`, mentions do not imply ownership.
*   **`labels`**
    *   **Description**: Free-form classification tags.
    *   **Suggested uses**: `docs`, `api`, `security`, `style`, `clarification`, `evidence`. Tools should treat labels as free-form values.
---

## Reviewing with AI

Because Breadcrumbs comments are machine-readable, they are ideal for automated workflows. Once you have annotated a document with review comments, you can pass it to an AI agent or LLM with a prompt like the following to automate the edits:

> "This document contains inline review comments formatted as JSON objects within native comment tags (marked with the `REVIEW:` prefix). Please address each review comment by editing the surrounding text to implement the feedback, and remove the comment tags once resolved."

---

## Example Integration

Here is a comprehensive example of review comments embedded within a Markdown document:

```markdown
... some document content
...
The system will use PostgreSQL for all persistence.

<!-- REVIEW:{
  "schema_version": "1.0",
  "id": "c1",
  "author": "alice",
  "addressed_to": ["planner-agent"],
  "comment": "question: Why was PostgreSQL chosen over the existing MySQL deployment?"
} -->

The application will cache popular items to improve performence.

<!-- REVIEW:{
  "schema_version": "1.0",
  "id": "c2",
  "author": "bob",
  "comment": "typo: 'performence' should be spelled 'performance'."
} -->

We will implement a simple password authentication mechanism.

<!-- REVIEW:{
  "schema_version": "1.0",
  "id": "c3",
  "author": "charlie",
  "comment": "suggestion: We should support OAuth2/OIDC instead of building a custom password login for security compliance."
} -->
...
... other document content
```
