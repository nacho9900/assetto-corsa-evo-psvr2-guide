# Online crash: resource-pool exhaustion

During the November 6 nighttime online test, EVO was connected to a Nordschleife Tourist road-car practice server. The log recorded **47 `SpawnVehicle(NetCar)` calls in one second**. Roughly nine seconds later, repeated exceptions appeared:

```text
Exception thrown: No more free elements in pool
Exception Detected: 0xE06D7363 (C++)
Thread: Resource Manager Worker 3
```

Other resource-manager workers subsequently reported the same exception, and EVO created a crash dump. The preceding log activity included repeated `CarRenderer::CommitMesh` entries.

## What this establishes

An internal pool ran out of free elements during resource loading. It is a stronger clue than the generic Windows exception module or the observation that the session was at night.

## What this does not establish

- The message does not identify the exhausted pool.
- It does not prove that the GPU's physical VRAM was exhausted.
- It does not establish a PSVR2 driver failure, a nighttime lighting defect, or a network fault.
- Forty-seven spawn calls are not proof that 47 cars were visible simultaneously.
- Symbolized stack labels near an exported SDK function do not identify that SDK as the cause. Detailed game symbols were unavailable.

Startup logging showed a local GPU memory budget of about 11.3 GB and a texture pool resized to 2048 MB. Those were startup observations, not measurements of memory at the moment of the crash. Other UI Automation exception entries appeared earlier in the session while the game continued running; they should not be conflated with this terminal resource-pool failure.

## Applied mitigation

Vehicle detail was lowered from Medium to Low; visible-car limits changed from 10 ahead / 10 behind to 6 ahead / 4 behind. This reduces rendering work. It may not prevent initial loading of all server vehicles or resolve an internal resource leak/capacity limit.

The owner subsequently reported successful Nürburgring laps without a crash. **Treat this as a useful test outcome, not a confirmed fix.** A public search did not uncover a verified remedy for this exact error on this headset/build.

## Next controlled tests

1. Start with profile 3 (70%, Low vehicle detail, 6/4 cars).
2. Run the same car and Nürburgring layout at night in solo practice.
3. Repeat on a lightly populated server, then on the previously failing populated server if practical.
4. Compare daytime and nighttime while holding other conditions as constant as possible.
5. Compare profile 4's 80% density only after establishing the baseline.
6. Record elapsed driving time, actual rendered resolution, CPU/GPU frame times, reprojection, and the first substantive error before a failure.

For 90 Hz, a full refresh interval is approximately 11.1 ms. That is a timing reference, not a guarantee that staying below one displayed metric eliminates stutter. Avoid changing resolution, runtime, mods, and traffic simultaneously if the goal is to identify a cause.

Keep full logs/dumps locally for a developer support report. Publish only minimal sanitized excerpts; logs can include player names, server addresses, account identifiers, and local file paths.
