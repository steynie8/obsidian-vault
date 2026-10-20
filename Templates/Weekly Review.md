---
type: daily
status: done
week: <% tp.date.now("GGGG-[W]WW") %>
tags: [weekly]
updated: <% tp.date.now("YYYY-MM-DD") %>
---
# Weekly Review: <% tp.date.now("GGGG-[W]WW") %>

## 1. Clear
- [ ] Inbox triaged
- [ ] Daily-note captures filed into their notes

## 2. Review
### Open tasks by note
```tasks
not done
path does not include Templates
group by filename
```

### Projects: anything stuck?
```dataview
TABLE status, file.mtime AS "Last touched"
FROM "Projects"
WHERE type = "project" AND status = "active"
SORT file.mtime ASC
```

## 3. Reflect
- **Wins:** <% tp.file.cursor() %>
- **Blockers:**
- **Learned:**

## 4. Next week
- [ ] 
