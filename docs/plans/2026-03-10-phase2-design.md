# Phase 2 Design — Three-Column Layout + Threat Timeline

## Layout

Target: 1024x768 CSS pixels (iPad 5th gen landscape Safari fullscreen)

Three columns using CSS Grid:
- **Left (~35%)**: Active alerts sorted by severity, with polygon badges. All-clear if none.
- **Center (~35%)**: Home coordinates, NWS grid info, general status. Sparse now, becomes home for Phase 3/4 content.
- **Right (~30%)**: 24-hour threat timeline, vertical, now at top, +24h at bottom.

Header spans all columns: app name, phase label, last-checked timestamp.

## Threat Timeline

Data sources:
1. NWS Hourly Forecast (`/gridpoints/GRR/41,19/forecast/hourly`) — temp, conditions, precip probability
2. Active alerts — mapped onto hour blocks by effective/expires time range

Color coding (highest severity wins per hour):
- Red (#ef4444): Warning active
- Orange (#f59e0b): Watch active
- Blue (#3b82f6): Advisory active
- Green (#22c55e): Clear, precip < 30%
- Slate (#475569): No alert but precip >= 30%

Each hour block: hour label, temperature, short condition, color bar.
"Now" indicator: highlighted border on current hour.

## Constraints

- Single index.html file, no framework, no build step
- Hourly forecast joins existing Promise.all in init()
- If hourly forecast fails, timeline renders with alert data only
- If alerts fail, timeline renders with forecast data only (green bars)
- No hover states (iPad touch only)
- 12-14px monospace fonts
- No scrolling within columns — 24 blocks at ~28px fits in viewport
- 60-second refresh cycle

## Not included (later phases)

- Radar map (Phase 3)
- SPC outlooks, mesoscale discussions (Phase 4)
- Preparation checklists (Phase 5)
- Push notifications (Phase 6)
