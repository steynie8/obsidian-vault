---
type: area
status: active
tags: [area, claude]
updated: 2026-11-14
---
# Dev Environment

Index for the **Dev Environment & Claude Tooling** area.

## Hardware (desktop PC)
See also [[Devices]].

| Component | Spec |
|---|---|
| GPU | AMD RX 7800 XT (16 GB VRAM) |
| RAM | 32 GB |

## Local LLM evaluation
| | |
|---|---|
| **Model** | Qwen3.6-27B UD-Q3_K_XL (~15 GB) |
| **Runtime** | llama.cpp on ROCm 6.4.2 (gfx1101) |
| **Speed-up** | MTP speculative decoding (1.4–2×) |
| **Fallback** | bf16 KV-cache flags |

## Accounts
- Personal Claude account + work Enterprise account (Claude in Chrome disabled on work).
- Goal: tooling that works across both.

## Notes
- [[Design System]]: shared design tokens and enforcement
- [[Claude Setup]]: plugins, skills, custom instructions, memory pruning
- [[Claude Projects]]: which claude.ai Projects use which vault notes
- [[Ideas]]: agent building, advisor council, capture system
- [[Prompt Library]]

Back to [[Areas]]
