# Readable configuration snapshots

These JSON files preserve the decoded video settings at each stage. They contain no local paths, controller IDs, account information, or raw game data.

| File | Meaning |
| --- | --- |
| `00-original.json` | Original configuration before the first tuning pass |
| `01-performance-80.json` | Initial lower-quality 80% profile |
| `02-performance-70.json` | 70% profile used when the populated nighttime session crashed |
| `03-tested-70-fewer-cars.json` | 70% with Low vehicle detail and 6/4 visible cars; successful laps reported |
| `04-sharper-80-fewer-cars.json` | Current sharper candidate; driving validation pending |

**Reference data only.** Do not copy these JSON files over EVO's binary `.videosettings` files. Use the settings UI to reproduce the values.

Enums have their observed numeric value and readable label. Omitted fields and empty objects do not necessarily mean “disabled”: Protocol Buffers can omit values that use defaults, and the local decoder did not materialize all defaults. The snapshots are not a universal schema or a general settings API.

Keyboard recentering is documented separately; no keyboard or wheel configuration is distributed.
