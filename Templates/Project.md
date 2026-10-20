---
type: project
status: active
area: 
tags: [project]
updated: <% tp.date.now("YYYY-MM-DD") %>
---
# <% tp.file.title %>

## Overview
- <% tp.file.cursor() %>

## Outcome (what "done" looks like)
- 

## Next actions
- [ ] 

## Notes in this project
```dataview
LIST
FROM ""
WHERE contains(file.folder, this.file.folder) AND file.name != this.file.name
```

## Log
| Date | Update |
|---|---|
| <% tp.date.now("YYYY-MM-DD") %> | Project created |
