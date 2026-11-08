# Configuration, recentering, and backups

## Where settings live

The observed Windows setup stored EVO user configuration in:

```text
%USERPROFILE%\Saved Games\ACE\
```

Relevant files:

| File | Purpose in this investigation |
| --- | --- |
| `vr.videosettings` | Separate VR graphics profile |
| `video.videosettings` | Desktop graphics profile |
| `input_keyboard.keyboardinputconfiguration` | Keyboard action bindings |
| `input_devices.inputdeviceconfiguration` | Controller/wheel bindings; may contain device identifiers |
| `camerasettings.camerasettings` | Camera behavior |
| `CarCameras.carcamerausersettings` | Per-car camera data |
| `Logs\` | Human-readable logs, including graphics settings and VR initialization |

SteamVR configuration was found under the Steam installation's `config\steamvr.vrsettings`. Its contents are JSON. Steam's installation directory may differ on your machine.

**In the inspected EVO build, `.videosettings` and the listed input configuration files were binary Protocol Buffers, not plain-text JSON or INI.** Logs contained readable JSON representations. The JSON snapshots in this repository are for inspection and comparison: renaming one to `vr.videosettings` will not make it loadable.

## How the local edits were made

A local Python helper extracted embedded Protocol Buffer descriptors from the installed EVO executable, decoded the relevant settings, and staged narrow field changes. Unchanged fields retained their original bytes. Each stage checked that only the intended decoded settings changed; original files were backed up, source hashes checked before replacement, and destination hashes verified afterward with EVO closed.

The recenter change removed the Ctrl modifier from the existing Reset View command while preserving the remaining keyboard commands. In this build:

- `InputAction_Camera_ResetView` was action 418.
- The bound key was Windows virtual-key 32 (Space).
- The required modifier was virtual-key 17 (Ctrl), serialized in a packed repeated field.
- `InputAction_UI_Vr_Settings_Reset` was a separate action, 535.

These are build-specific observations, not stable public API contracts. The experimental helper was not a complete general-purpose Protobuf implementation: repeated/packed fields, defaults, and signed values require careful handling. It is not published as an automatic end-user config patcher. No executable, extracted descriptor bundle, or game content is redistributed here.

For reproduction, **prefer the game's settings UI** and bind Camera → Reset View yourself. An independent community tool describing EVO's embedded schemas is linked in [sources](sources.md); it was discovered during research and was not used to make these edits.

## Back up before changing anything

Close EVO first so it cannot overwrite restored or edited files on exit. The following PowerShell snippet makes a timestamped private backup under Documents without changing the live configuration:

```powershell
$ErrorActionPreference = 'Stop'
if (Get-Process -Name AssettoCorsaEVO -ErrorAction SilentlyContinue) {
    throw 'Close Assetto Corsa EVO before backing up.'
}
$aceFolder = Join-Path $env:USERPROFILE 'Saved Games\ACE'
$backupFolder = Join-Path ([Environment]::GetFolderPath('MyDocuments')) ('EVO-VR-backup-' + (Get-Date -Format 'yyyyMMdd-HHmmss'))
New-Item -ItemType Directory -Path $backupFolder -ErrorAction Stop | Out-Null
foreach ($fileName in @('vr.videosettings', 'input_keyboard.keyboardinputconfiguration')) {
    Copy-Item -LiteralPath (Join-Path $aceFolder $fileName) -Destination (Join-Path $backupFolder $fileName)
}
Write-Output $backupFolder
```

To restore, close EVO and copy those two backed-up files to the same original ACE directory, replacing their current counterparts. Restore only `vr.videosettings` if you want to retain your latest key bindings. Keep backups outside a public repository.

## Recenter procedure

1. In Controls → Camera, bind **Reset View** to Space or a convenient unused wheel button.
2. Ensure EVO is the active Windows window.
3. Enter the cockpit, sit naturally, and look forward.
4. Press the binding. Recenter again after changing your physical seating position.

The owner confirmed this fixed the above-roof/sideways view. If the command does not respond, check window focus and the binding. If an exterior camera is selected, switch back to cockpit view; the inspected setup used F1 to cycle driving cameras. Do not assume a VR panel/HUD reset centers the driving viewpoint.

## Startup notes

Power on/connect the PSVR2, confirm SteamVR detects it, and complete the play area before testing. The working local launch used Steam's app launch for EVO (app ID 3058630) with `-vr`. A direct executable launch produced a Steam entitlement error during troubleshooting. No permanent Steam launch-option change was made.

A desktop mirror can appear black while the headset is rendering the menu. Verify the actual headset view before diagnosing a startup failure.
