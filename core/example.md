# Breadcrumbs Examples and Syntax Reference

This guide provides syntax templates and examples for interacting with Breadcrumbs review comments in various files and formats.

---

## 1. Native Comment Syntax Mappings

When inserting review comments, use the syntax appropriate for the file extension:

| File Types / Extensions | Comment Syntax Template |
| :--- | :--- |
| **Markdown / HTML** (`.md`, `.html`, `.xml`) | `<!-- REVIEW:{...} -->` |
| **JS / TS / JSX / TSX / C / C++ / Go / Rust / Swift / Java / CSS** | `// REVIEW:{...}` <br> *or* <br> `/* REVIEW:{...} */` |
| **Python / Ruby / Shell / YAML / TOML / Dockerfile / R** (`.py`, `.rb`, `.sh`, `.yaml`, `.yml`, `.toml`) | `# REVIEW:{...}` |

---

## 2. Creating & Inserting Comments

A comment should be placed immediately after the line of code or text it refers to.

### Example in Markdown (`design.md`)
```markdown
<!-- Before -->
We will use a central database without replication.

<!-- After -->
We will use a central database without replication.
<!-- REVIEW:{
  "schema_version": "1.0",
  "id": "c2",
  "author": "architect-agent",
  "status": "open",
  "comment": "suggestion: Add a replica database for failover redundancy."
} -->
```

---

## 3. Threaded Comments (Replies)

To reply to an existing comment, create a new comment object with the `reply_to` field set to the parent comment's `id`. Place it directly below the parent comment.

```yaml
# parent comment
# REVIEW:{
#   "id": "c3",
#   "author": "lead-dev",
#   "status": "open",
#   "comment": "question: Why are we using inline styles here?"
# }
# REVIEW:{
#   "id": "c3.1",
#   "reply_to": "c3",
#   "author": "ui-designer",
#   "comment": "note: This is a temporary prototype override; it will be moved to index.css in the next task."
# }
```

---

## 4. Resolving or Dismissing Comments

To resolve a comment, edit the comment JSON directly in the source file:
1. Change `"status": "open"` to `"status": "resolved"` (or `"dismissed"`).
2. Add `"resolved_by": "<author_name>"`.

```markdown
<!-- Before -->
<!-- REVIEW:{
  "id": "c2",
  "author": "architect-agent",
  "status": "open",
  "comment": "suggestion: Add a replica database for failover redundancy."
} -->

<!-- After -->
<!-- REVIEW:{
  "id": "c2",
  "author": "architect-agent",
  "status": "resolved",
  "resolved_by": "db-engineer",
  "comment": "suggestion: Add a replica database for failover redundancy."
} -->
```

---

## 5. Regex Matching for Extraction

If you need to programmatically search for breadcrumbs using your tools, you can use the following regular expressions:

- **Regex pattern to match the JSON block**: `REVIEW:(\{[\s\S]*?\})`
- **Ripgrep command (finding files with reviews)**:
  ```bash
  rg "REVIEW:"
  ```

---

## 6. Generating a Summary Report

You can generate a markdown table to present all breadcrumbs in a clean format:

| File | ID | Thread | Author | Status | Comment / Action |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `src/db.js` | `c1` | Top-level | `security-bot` | `open` | `security (blocking): MD5 is weak...` |
| `src/db.js` | `c1.1` | → `c1` | `developer` | `open` | `thought: Will replace with bcrypt.` |
| `docs/design.md` | `c2` | Top-level | `architect-agent` | `resolved` | `suggestion: Add database replication` |
