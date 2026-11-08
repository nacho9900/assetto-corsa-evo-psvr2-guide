# Sources and applicability

The strongest evidence for this repository is the local configuration, startup/crash logs, and the owner's test reports. Community experiences are useful leads, not guarantees for this hardware. The links below were consulted during the November 6–8, 2026 investigation.

## Official game changes

- [505 Games: EVO 0.9.1 hotfix, September 1, 2026](https://505games-help.freshdesk.com/support/solutions/articles/150000233185-assetto-corsa-evo-0-9-1-hotfix-1st-sept-2026-): lists TrackIR support and several fixes, including crashes on certain AMD GPUs. That does not establish a fix for this NVIDIA/PSVR2 resource-pool crash.
- [505 Games patch-note index](https://support.505games.com/support/solutions/folders/150000543169): release reference. The locally inspected installation was 0.9.1+release.6; do not treat this document as a perpetual statement of the latest available build.
- [Kunos: EVO 0.7 release](https://assettocorsa.gg/assetto-corsa-evo-early-access-0-7-now-available/): background on renderer/physics/multiplayer work and texture-quality controls. Texture-pool tuning is not a demonstrated remedy for the exception observed here.

## Recentring and VR usability

- [Owner report: VR Spawn Height](https://www.reddit.com/r/assettocorsaevo/comments/1u5pjpr/vr_spawn_height/): describes spawning above the car and resolving it by binding recenter to a wheel. Ctrl+Space reportedly did not respond in the menu for that user.
- [Owner report: Camera way too high in VR](https://www.reddit.com/r/assettocorsaevo/comments/1rq0kw0/camera_way_too_high_in_vr_mode/): another report of Ctrl+Space resolving an elevated viewpoint.
- [Traxion firsthand VR test, July 9, 2026](https://traxion.gg/hands-on-assetto-corsa-evos-vr-update-is-a-welcome-improvement/): describes the 0.8 pixel-density/foveated-rendering controls and identifies Camera → Reset View as the VR centering control. Its hardware was different.

## PSVR2-specific research leads

- [PSVR2: much improved VR experience](https://www.reddit.com/r/assettocorsaevo/comments/1qf9yix/psvr2_much_improved_vr_experience/): a community discussion with later update feedback. Different hardware and 120 Hz/reprojection reports cannot be directly transferred to this 3080 Ti configuration. Our headset remained at 90 Hz with motion smoothing off.
- [Community discussion: eye-tracked foveated DLSS in EVO](https://www.reddit.com/r/assettocorsaevo/comments/1w7i4ji/eyetracked_foveated_dlss_is_working_in_assetto/): includes PSVR2 discussion and much faster GPU hardware. Experimental rendering modifications were considered as research leads only; none were installed, and online compatibility was not established here.
- [PSVR2Toolkit project](https://github.com/BnuuySolutions/PSVR2Toolkit): optional community tooling, not part of the tested configuration. Do not attribute this guide's results to eye tracking or Toolkit integration.

## Configuration format

- [ac-evo-data-tools](https://github.com/Breakalien/ac-evo-data-tools): independent project describing EVO's Protocol Buffer content and executable-embedded schemas. Found during research; not used in the local tuning workflow. This repository's readable JSON exports are not directly importable game configuration.

No public source located during the investigation established a verified fix for **“No more free elements in pool” on PSVR2 with EVO 0.9.1**. The proposed car-count/detail reduction remains an empirical mitigation with limited follow-up evidence.
