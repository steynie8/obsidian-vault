---
type: area
status: active
area: "[[Smart Home]]"
tags: [smart-home]
updated: 2026-10-20
---
# Humidity Automation

## Working end to end
| Trigger | Action |
|---|---|
| RH > 65% | Aircon → Dry mode |
| RH < 55% | Aircon off |
| 22:30 | Scheduled off |

## Fixed
- Sensor buzzing was caused by ripple from a switch-mode charger. Swapping to the Anker charger fixed it. See [[USB Power]].

## Open issue
Humidity rose to 68% after the door was closed.
- [ ] TODO: Compare the temp + RH graphs to tell apart:

| Hypothesis | What the graph would show |
|---|---|
| (a) RH rising as temperature drops | RH rises while temp falls; absolute moisture unchanged |
| (b) IR one-way blindness | Aircon never received the command (see [[IR Control]]) |
| (c) Coastal moisture ingress | RH rises with temp steady |

Back to [[Smart Home]]
