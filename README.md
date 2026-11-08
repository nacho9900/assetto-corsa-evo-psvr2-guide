# Assetto Corsa EVO + PSVR2: RTX 3080 Ti settings and troubleshooting

A reproducible account of tuning **Assetto Corsa EVO 0.9.1 + PlayStation VR2 on PC**, including VR recentering, blur, stuttering, and an online resource-pool crash.

**Last updated: November 8, 2026.** This is a single-PC case study, not an official Kunos/Sony guide or a benchmark. The owner reported successful Nürburgring laps without a crash after the third profile. The higher-resolution fourth profile has been applied but has not yet received an in-headset test report.

## Start here

1. Back up your configuration with EVO closed; see [backup and restore](docs/configuration-and-backup.md).
2. Use the **tested baseline** below as a starting point, adjusting settings through EVO's VR graphics menu.
3. Bind **Controls → Camera → Reset View** to an accessible key or wheel button. In this setup, **Space** successfully recentered the headset.
4. Sit in your normal driving position, look straight ahead, select the cockpit camera, and press the assigned key with EVO focused in Windows.
5. Test the same car, circuit, weather, time of day, and traffic before raising resolution. Change one setting at a time.

## Test system

| Component | Observed configuration |
| --- | --- |
| Headset | PlayStation VR2, PC setup |
| GPU | NVIDIA GeForce RTX 3080 Ti |
| CPU | Intel Core i7-14700K, 20 cores / 28 threads |
| RAM | 32 GB |
| Game | EVO 0.9.1+release.6; Steam build 25170311 |
| Runtime | SteamVR/OpenXR 2.18.4 |
| Headset refresh | 90 Hz |
| NVIDIA Windows driver version | 32.0.16.1088, as reported by Windows |
| VR path | Native EVO OpenXR through SteamVR and the PlayStation VR2 driver |
| Additional VR mods | None installed during this tuning session |

SteamVR was already using manual resolution scaling (`supersampleManualOverride: true`, `supersampleScale: 0.74`) with motion smoothing disabled. These values were **observed, not changed**. Do not assume that copying this number produces the same resolution on another system: check the per-eye buffer reported by EVO. No per-application override was found in the inspected SteamVR configuration.

## Tested baseline and sharper candidate

| Setting | Tested baseline, profile 3 | Sharper candidate, profile 4 |
| --- | --- | --- |
| EVO VR pixel density | **70%** | **80%** |
| Per-eye render buffer | 2047 × 2086, observed in startup log | Approximately 2339 × 2384; observed at 80% in an earlier run, not remeasured after profile 4 |
| Foveated rendering | Medium | Medium |
| MSAA | 2× | 2× |
| Visible cars ahead / behind | **6 / 4** | **6 / 4** |
| Vehicle detail | Low | Low |
| Shadows / car reflections / clouds | Low | Low |
| Grass / ambient occlusion / particles / volumetrics | Low | Low |
| Mirror resolution | Low | Low |
| Mirror distance | Medium | Medium |
| Mirror alternate rendering | Enabled | Enabled |
| General level of detail | Medium | Medium |
| Texture pool | Medium | Medium |
| Anisotropic filtering | Medium | Medium |
| Depth of field / motion blur / VSync | Off | Off |
| Gameplay / occluded frame-rate limit | 120 / 120 | 120 / 120 |
| World scale | 1.0 | 1.0 |
| Prefer frame rate over latency | Enabled | Enabled |
| Result | Owner completed Nürburgring laps and reported no crash; image was blurry | Applied and file-verified; driving result pending |

The 120 FPS cap is a ceiling, **not a measured frame rate or a change to the headset's 90 Hz refresh rate**. No controlled FPS, frame-time, reprojection, or in-race VRAM measurements were collected.

Moving from 70% to 80% increases pixel count by about **31%**: `(80 / 70)^2 - 1`. This should recover spatial detail but can reintroduce GPU-related stuttering. Keeping fewer cars visible does not guarantee enough GPU headroom to offset that increase.

## What actually helped

- **Head alignment:** the original binding was Ctrl+Space. Changing Camera → Reset View to Space made recentering work for the owner. The original symptom was a sideways view above the car roof. The separate VR panel reset action is not the same binding.
- **Performance:** lower render resolution and reduced graphics settings produced a usable test, according to the owner. There is no measured attribution to an individual setting.
- **Crash mitigation:** after a resource-pool crash in a populated online session, vehicle detail was reduced from Medium to Low and visible-car limits from 10/10 to 6/4. The next report was successful Nürburgring laps without a crash. This is encouraging but does **not** prove the crash is fixed or identify its cause.

## More detail

- [Chronological test history](docs/test-history.md)
- [Crash analysis and a controlled test plan](docs/crash-analysis.md)
- [Configuration format, backups, and restoration](docs/configuration-and-backup.md)
- [Research sources and their limits](docs/sources.md)
- [Readable profile snapshots](profiles/README.md)

If you reproduce the results, open an issue with your hardware, game/runtime versions, actual per-eye resolution, scenario, and outcome. Please use the [test-report template](.github/ISSUE_TEMPLATE/test-report.md). Remove account IDs, player names, IP addresses, and local usernames from any log excerpts before posting.

This repository contains English documentation and readable settings snapshots. It does not distribute game executables, full logs, crash dumps, controller identifiers, or binary configuration backups. Product names belong to their respective owners; this project is unaffiliated with Kunos, 505 Games, Sony, Valve, and NVIDIA.
