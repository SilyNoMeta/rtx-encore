# Troubleshooting

*[Français](TROUBLESHOOTING.fr.md) · [中文](TROUBLESHOOTING.zh-CN.md)*

## Intel XeSS and XeFG (experimental)

RTX Encore 1.2.0-beta.1 adds a choice of reconstruction and frame-generation engines. Leave **Game** selected to keep the game’s normal engine. Intel modes are experimental, off by default and require compatible game inputs; choosing an engine does not prove it became active.

[Intel XeSS and XeFG (experimental)](INTEL-ENGINES.md)

## First checks

1. **Is the mod loaded?** A `rtx-encore-logs` folder appears beside the mod's file when it is, and **Insert** opens
   the menu. If neither happens, the game did not load the file.
2. **Is there only one copy?** Two copies of the mod in the same game, under two names, cause crashes, odd
   behavior and a degraded image with Neural Rendering.
3. **What does the menu say?** The four chips at the top give the state of each feature, and a card names any
   lasting problem with its fix.

## The mod does not load

| Check | What to do |
| :--- | :--- |
| Wrong folder | The file must be beside the executable that really runs the game. For Unreal Engine: `<Game>\Binaries\Win64\`. |
| The name is not loaded by this game | Try the next name: `version.dll`, then `dinput8.dll`, `winmm.dll`, `dxgi.dll`. |
| The name is used by another mod | Leave that mod its file and choose another name for RTX Encore. |
| The renamed file is refused | Use the ready-named copy from `alternative-proxies`. |
| The game has an anti-cheat | It can block the file. RTX Encore must not be used there. |

## Frame generation

| Symptom | First check |
| :--- | :--- |
| The game has no Frame Generation option | RTX Encore extends DLSS Frame Generation in games that have it; it cannot add one. See [Smooth Motion](SMOOTH-MOTION.md) for the other games. |
| The option is there but greyed out | Check that DLSS is on, that the game runs in DirectX 12 or Vulkan, and that the mod is loaded. |
| The multiplier stays at X2 | Turn frame generation off and on in the game, or restart it. Check the mode in the menu: in **Game decides**, the game chooses. |
| X5 or X6 is not reached in Dynamic mode | Turn **Allow 5x and 6x** on. |
| Nothing happens with this game's version | The game's `nvngx_dlssg.dll` may be a version that is not supported: see [Compatibility](COMPATIBILITY.md#dlss-frame-generation-versions). |
| Doubled outlines or distorted frames in motion | Turn **Automatic UI recomposition** off. |
| Artifacts on fences, foliage or shadows | Keep **Anti-tearing / anti-ghosting** on and try the other **Protection tuning**. |
| Stutter or crashes at high multipliers | VRAM is probably full: lower the multiplier or the texture quality. |
| Crash in menus or loading screens | Keep **Disable menu detection** off. |

## Smooth Motion and Neural Rendering

See the end of their pages: [Smooth Motion](SMOOTH-MOTION.md#if-it-does-not-work),
[Neural Rendering](NEURAL-RENDERING.md#when-something-does-not-work).

## Menu and settings

| Symptom | First check |
| :--- | :--- |
| **Insert** does nothing | The mod is not loaded (see above), or the shortcut was changed: look at `menuState` in `rtx-encore.jsonc`. |
| The menu opens in a separate window | Expected in a few games: see [Menu and overlay](MENU-AND-OVERLAY.md#when-the-menu-opens-in-a-separate-window). |
| Clicks do not register | Release every mouse button, then click again. Borderless window mode helps. |
| Settings revert at once | The settings file could not be saved: make sure it is not read-only, nor open and locked in an editor. |
| A setting has no effect | Settings marked with `*` are read when the game starts: restart the game. |
| Shortcuts do nothing | They only act while the game window has focus, and `disableKeybinds` must be `false`. |

## Performance drops after playing for a while

Enable **System → Diagnostics → Log performance**, then play from a cool start until the slowdown occurs.
Keep the same game settings for that capture, and note the approximate time of the drop. Send the game's
session log from `rtx-encore-logs`; the frame-time CSV, if produced, is useful too. Turn the option off afterwards.

The `[GPU-PERF]` entries record temperature, power, frequencies, reported limits, CPU usage and AC/battery
status alongside the game's cadence. They identify the NVIDIA GPU being monitored; check its name if your
machine has several NVIDIA GPUs. Missing readings are marked `NA`, not zero. CPU temperature/power and
laptop fan RPM are not included. The readings do not prove the condition of the thermal paste.

A reported thermal limit helps confirm overheating-related slowdown. A power limit alone does not tell us
whether the cause is a laptop power profile, CPU/GPU power sharing, or another power constraint. Brief events
between samples may be missed. Comparing the same scene before and after the drop is especially useful.

For a separate comparison, try **Frames → Frame pacing → Suggested Reflex cap** when available, or a manual
Reflex request. The suggestion uses a limit below the display rate (224 FPS at 240 Hz); it is not a thermal target. FG counting
depends on the game/driver. If applying it does not reduce the actual render rate, try a lower request.
Compare measured source FPS and GPU readings; `accepted` alone does not prove a benefit. The saved limit
stays fixed until you change it. See [the cap controls](MENU-AND-OVERLAY.md#suggested-reflex-cap).

## Reporting a problem

Normal Streamline/NGX messages, including the feature rejection reasons they report, are recorded automatically
even when diagnostic options are off. Message counts are limited to prevent repeated messages from filling
the log. If support requests more detail, set `diagnostics.logStreamline` to `true` in `rtx-encore.jsonc` and
restart the game. Turn it off and restart after collecting the log; verbose logging can reduce performance.

Basic context is recorded automatically in the session log, even with diagnostic options off:
Windows build, CPU, memory, display adapters and desktop modes, and loaded DLL versions and paths
(including the game, mod, Streamline, DLSS and driver components). Unavailable versions are marked explicitly.
Multiple loaded copies do not prove which one is used. Discovery can miss a very early crash or a DLL
that unloads before it is observed.

Observed DLSS creations also report the graphics API, SR/RR mode, quality, resolutions and requested
presets. **A requested preset is not a confirmed effective preset**: the driver or an override may select
another model. Default, unavailable and unknown values are kept distinct. Reports are bounded and deduplicated
where appropriate; frame-by-frame performance and detailed graphics/NR traces still require their options.
Zip the whole `rtx-encore-logs` folder from the failing launch so the game's log is included alongside any
loader/error-reporter logs. The menu's **Copy** report is a short summary and does not replace that folder.

Open an [issue](https://github.com/SilyNoMeta/rtx-encore/issues) with:

- the game, its version or store, and its graphics API;
- your card and NVIDIA driver version;
- the RTX Encore version and the name you gave the file;
- the report copied from the menu (**System** tab, **Copy**);
- the log of the session where the problem happened, from `rtx-encore-logs`. Logs can contain folder names from your
  PC: read them before posting.

One problem per issue, with what you did and what you saw.
