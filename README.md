# Breadcrumbs

**A Lightweight Format for Inline Document Reviews**

Version: 1.0

## Overview

Documents deserve comments that don't get lost.

Breadcrumbs provides a simple way to embed review comments directly inside Markdown and other text-based documents, so feedback stays attached to the content instead of disappearing into a separate system.

Think of it as sticky notes for documents, except they are structured, searchable, and understandable by both humans and machines.

The goals are simple:

* Keep comments close to the content being reviewed.
* Make review discussions easy to follow.
* Support collaboration between humans and AI agents.
* Support threaded discussions.
* Remain lightweight and easy to adopt.
* Avoid becoming an issue tracker or workflow engine.

Breadcrumbs is intended for reviewing any text-based content.

---

## Why Breadcrumbs Exists

Modern software development increasingly relies on collaborative documents (e.g., plans, PRDs, designs, architectures, tasks, research, and implementation plans).

Increasingly, many of these documents are generated or refined by AI and agentic tools.

A common workflow looks like this:

1. An AI agent generates a plan.
2. A human reviews it.
3. Feedback is provided.
4. The AI updates the document.
5. The process repeats until the document is ready.

Breadcrumbs is designed for adding quick review comments, especially to text files. While external tools will always win in terms of features, Breadcrumbs provides a lightweight, portable alternative that keeps the discussion inside the document itself without the complexity of external systems.

Example:

```md
... some document content
...
The system will use PostgreSQL for all persistence.

<!-- REVIEW:{
  "schema_version":"1.0",
  "id":"c1",
  "author":"alice",
  "addressed_to":["planner-agent"],
  "comment":"question: Why was PostgreSQL chosen over the existing MySQL deployment?"
} -->

The application will cache popular items to improve performence.

<!-- REVIEW:{
  "schema_version":"1.0",
  "id":"c2",
  "author":"bob",
  "comment":"typo: 'performence' should be spelled 'performance'."
} -->

We will implement a simple password authentication mechanism.

<!-- REVIEW:{
  "schema_version":"1.0",
  "id":"c3",
  "author":"charlie",
  "comment":"suggestion: We should support OAuth2/OIDC instead of building a custom password login for security compliance."
} -->
...
... other document content
```

This allows:

* Humans to review AI-generated documents.
* AI agents to discover and respond to feedback.
* Discussions to remain attached to the relevant content.
* Reviews to travel with the document.
* Feedback to remain available regardless of platform or tool.

---

## Typical Use Cases

Breadcrumbs supports documentation reviews, human-AI and agent-agent collaboration, and portable reviews.
For detailed scenarios and examples, see the [Typical Use Cases](core/use-cases.md) document.

---

## Reviewing with AI

Once you have annotated a document with review comments, you can pass it to any AI agent or chat interface with a prompt like the following to automate the edits:

> "This document contains inline review comments based on the [Breadcrumbs specification](https://github.com/sanathusk/breadcrumbs) a format that relies on native comment tags marked with the `REVIEW:` prefix followed by a JSON object containing the review comment details. Please address each review comment by editing the surrounding text to implement the feedback, and remove the comment tags once resolved."

---

## Design Principles

Breadcrumbs follows a few simple rules:

1. Keep comments close to the content being reviewed.
2. Prefer simplicity over completeness.
3. Store only information that cannot easily be derived.
4. Let Git or other version control systems handle history, auditing, and versioning.
5. Keep the format readable without special tools.
6. Make the format easy for AI agents and software to process.

If a feature feels more appropriate for an issue tracker, project management tool, or workflow engine, it probably does not belong in Breadcrumbs.

---

## Specification

The complete Breadcrumbs format specification, including field details and examples, is available in [specification.md](specification.md).

---

## Non-Goals

Breadcrumbs is intentionally not:

* An issue tracker
* A project management system
* A workflow engine
* A permissions system
* A replacement for Git history

Features such as:

* Priorities
* Due dates
* Visibility controls
* Audit trails
* Review rounds
* Dependencies
* Revision tracking
* Approval workflows

are intentionally outside the scope of Breadcrumbs.

The goal is simple:

> Keep review conversations close to the content being reviewed, in a format that both humans and machines can understand.

---

## Want to Collaborate?

If you have a suggestion, comment, idea, or something else altogether, please visit the GitLab project to collaborate. Issues and Merge Requests are welcome!
