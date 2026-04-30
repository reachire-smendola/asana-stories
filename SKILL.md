---
name: asana-stories
description: Break a product spec into tasks and create them in Asana, with acceptance criteria for each.
category: product
version: 0.2.0
key_capabilities: spec-decomposition, task titles, acceptance criteria, asana-creation
when_to_use: User has a product spec and wants to create Asana tasks with AC's. Keywords: "user story", "tasks", "acceptance criteria", "AC", "Asana", "break up the spec"
---

# Asana Stories Skill

## Purpose

Given a product spec (Google Doc or other source), this skill:
1. Reads and understands the spec (text + images/wireframes)
2. Proposes a decomposition into tasks
3. Iterates on the task structure with the user
4. Writes acceptance criteria (AC) for each task
5. Creates the tasks in Asana under the parent story

## Process

### Step 1 — Read the Spec

Use the `google-docs` skill to read the spec with images:
```
read the spec at <doc_url> with images
```

Summarize what was read back to the user before proceeding.

### Step 2 — Propose Task Titles

Decompose the spec into tasks. Guidelines:
- Tasks typically map to functional chapters of the spec, but not always 1:1
- Each task should represent a coherent, independently deliverable chunk of behavior
- Admin configuration capabilities typically get their own task
- WIP / not-ready-for-dev sections: use judgment — fold into an adjacent task or create a placeholder
- Mobile gets a separate parallel story tree after web is settled; do not include mobile tasks here
- Present titles only first; do not write AC's until titles are approved

Present as a numbered list of titles. Iterate with the user until the structure is approved.

**STOP HERE.** Do not proceed to Step 3 until the user explicitly approves the task list.

### Step 3 — Write Acceptance Criteria

For each task, write AC's following these rules:

**What to capture:**
- All behavioral and functional requirements
- Visibility/access rules (who sees what, under what conditions)
- State transitions and edge cases
- Data persistence and auto-save behavior
- Limits (character counts, time limits, counts)
- One-time vs. repeatable actions
- Error states and empty states
- Navigation/redirect behavior after actions
- Any dynamic text in otherwise static copy

**What NOT to capture:**
- Visual design (colors, layout, spacing) — the wireframes speak for themselves
- Exact copy/labels — the spec text is authoritative
- Things that are obviously true of all features (e.g. "the page loads")

**Format:**
- Each AC is a short, declarative statement of something that must be true
- Use `⬜` checkbox prefix (for pasting into Asana)
- Group under the task title as a heading
- Scope/access rules go first if relevant

**Example style** (from a real story for reference):
```
Task: Opt-in Dialog

⬜ Starting on the day of each team's final session, any participants that log in see this opt-in dialog
⬜ Choosing "I'm in" takes the user to the onboarding UI
⬜ Choosing "Not now" causes the dialog to reappear on next login
⬜ The user must choose one of the two options — there is no way to dismiss the dialog (no X, no click-outside)
⬜ If the user chooses "Not now" twice, the dialog is never shown again
```

### Step 4 — Create in Asana

Once AC's are approved, create the tasks in Asana.

**NOTE:** Asana integration tooling TBD — this step will be detailed once an Asana MCP or CLI is configured.

Each task should be created with:
- Title: the task title
- Description: the AC's as a checklist
- Parent: the parent story task (provided by user)
- Project: same project as parent

## Notes & Conventions

- "Acceptance criteria" in this team's usage means: behavioral assertions that must be true for the feature to be considered correctly implemented. It does NOT mean Gherkin / BDD format.
- Visual specs and copy in the wireframes are not restated in AC's — they are treated as authoritative as-is.
- AC's are written to be verifiable by a QA engineer or the spec owner doing acceptance review.
