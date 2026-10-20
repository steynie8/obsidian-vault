---
name: new-project
description: Scaffold a new project in the vault (folder + MOC note from the Project template, linked into Projects.md). Use when the user says "create a project", "start a new project", or "new project for X".
---

# New Project

## Steps
1. Confirm the project name, a one-line outcome, and its parent area (if any). Ask at most 3 questions in one batch, and only for what's missing.
2. Search the vault for existing notes about it. If any exist, offer to move them into the new project folder.
3. Create `Projects/<Name>/<Name>.md` from `Templates/Project.md`. Fill in every field yourself and leave no `<% %>` tags:
   - `type: project`, `status: active`, `updated: <today>`, `tags: [project]`
   - `area: "[[<Area index note>]]"` if there is a parent area
4. Add a row to the table in `Projects/Projects.md` and to "Active projects" in `Home.md`.
5. If there's a parent area, add a link to the project in that area's index note.
6. Commit: `git commit -m "Add <Name> project"`.
