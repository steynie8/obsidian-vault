---
type: moc
status: active
tags: [claude]
updated: 2026-10-20
---
# Prompt Library

Reusable prompts. Create new ones with the **Prompt** template.

```dataview
TABLE use AS "Used in", rating AS Rating, file.mtime AS Updated
FROM "Resources/Prompt Library"
WHERE type = "prompt"
SORT rating DESC
```

Back to [[Resources]] · See also [[Claude Setup]]
