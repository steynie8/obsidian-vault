---
type: prompt
status: active
use: <% await tp.system.suggester(["claude.ai chat", "Claude Project", "Claude Code", "API"], ["claude.ai chat", "Claude Project", "Claude Code", "API"], false, "Where is this prompt used?") %>
rating: 
tags: [claude]
updated: <% tp.date.now("YYYY-MM-DD") %>
---
# <% tp.file.title %>

## Purpose
- <% tp.file.cursor() %>

## Prompt
```text

```

## Inputs
- 

## Iterations
| Date | Change | Result |
|---|---|---|
| <% tp.date.now("YYYY-MM-DD") %> | v1 | |
