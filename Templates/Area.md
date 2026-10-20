---
type: area
status: active
tags: [area]
updated: <% tp.date.now("YYYY-MM-DD") %>
---
# <% tp.file.title %>

## Purpose & standards
- <% tp.file.cursor() %>

## Notes in this area
```dataview
LIST
FROM ""
WHERE contains(file.folder, this.file.folder) AND file.name != this.file.name
```

## Linked projects
```dataview
LIST
FROM "Projects"
WHERE type = "project" AND area = this.file.link
```

## Open items
- [ ] 
