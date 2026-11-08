# Test history

Dates use the owner's local calendar. Earlier unsuccessful VR sessions predated this investigation. An August session log showed EVO 0.9.0; the November investigation found 0.9.1+release.6 installed.

| Stage | Changes / observations | Reported result |
| --- | --- | --- |
| Initial startup, November 6 | The headset was initially powered off; VR initialization failed. After powering it on and completing the play-area setup, the owner reached the EVO menu. | Headset initialization succeeded. A black desktop game window alone did not establish that the headset was black. |
| Original configuration | 100% EVO density; most expensive effects Medium; MSAA Off; 10 cars ahead and 10 behind; vehicle detail Medium; 60 FPS gameplay/occluded caps. | Baseline configuration, not a controlled benchmark. |
| Profile 1, November 6 | Density 80%; foveated rendering Medium; MSAA 2×; shadows, reflections, clouds, volumetrics, grass, AO, particles and mirror resolution Low; DOF and motion blur Off; caps 120. | Owner reported some stuttering and no crash during that test, but an unusable sideways/above-roof head position. |
| Profile 2, November 6 | Density 80% → 70%; Reset View changed from Ctrl+Space to Space. | Owner confirmed successful recentering and initially good operation, then reported a crash in a nighttime session. |
| Profile 3, applied November 6; feedback received November 8 | Vehicle detail Medium → Low; visible cars 10/10 → 6/4. Density remained 70%. | Owner reported successful Nürburgring laps without a crash, but excessive blur. The report did not explicitly establish that this repeated the same nighttime server conditions. |
| Profile 4, November 8 | Density 70% → 80% only. Preserved 6/4 cars, Low vehicle detail, and Space recentering. | Configuration applied and verified. No subsequent driving feedback yet. |

## Resolution evidence

The same runtime reported a recommended eye buffer of 2924 × 2980. EVO logged these render buffers:

| EVO density | Per-eye buffer | Combined stereo width × height |
| --- | --- | --- |
| 100% | 2924 × 2980 | 5848 × 2980 |
| 80% | 2339 × 2384 | 4678 × 2384 |
| 70% | 2047 × 2086 | 4094 × 2086 |

Rounded dimensions account for small differences from exact percentage calculations. From 100% to 80%, the pixel count falls about 36%; from 80% to 70%, it falls about 23%. These are pixel-count calculations, not FPS improvements.

## Evidence boundaries

- Settings were read from local configuration and corroborated by startup logs where available.
- Head alignment and driving outcomes are the owner's subjective reports.
- There was no repeatable benchmark, long-duration stability test, or controlled online/offline comparison.
- Profile 3's success followed multiple earlier changes; it cannot isolate a single cause.
- Profile 4 has not been validated as equally smooth or stable.
- No GPU driver update, game update, headset firmware update, system tuning, or VR mod installation was performed during these adjustments.
