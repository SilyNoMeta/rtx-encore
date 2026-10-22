# Changelog

*[Français](CHANGELOG.fr.md) · [中文](CHANGELOG.zh-CN.md)*

## 1.2.0

**Intel XeSS and XeFG are new and experimental.** Expect bugs, crashes, visual artifacts and differences
between games and graphics APIs. Please report problems: in RTX Encore Hub, open the game's **Health** page,
choose **Prepare a diagnostic**, review the archive for personal information, then attach it to an
[RTX Encore issue](https://github.com/SilyNoMeta/rtx-encore/issues/new/choose). Include your engine choices,
graphics API and requested multiplier. The Hub does not upload the archive for you.

### New

- **Intel XeSS and XeFG (experimental).** Choose the reconstruction engine (DLSS or XeSS Super Resolution) and
  the frame-generation engine (DLSS-G or XeFG with XeLL). Leave **Game** selected to keep the game's normal engine.
  Intel modes are off by default and require compatible game inputs; choosing an engine does not prove it became
  active. Install the Intel runtimes through RTX Encore Hub. See
  [Intel XeSS and XeFG (experimental)](docs/INTEL-ENGINES.md).
- **Ray Reconstruction + Neural Rendering (Direct3D 12).** Compatible linear-depth games can now run NR after RR.
  Direct3D 11 and Vulkan still do not support this linear-depth case.
- **Per-pass Neural Rendering settings.** Each of up to four NR passes can override intensity, local tone, local
  structure, skin structure and the character mask, or inherit the global value. Numeric overrides are 0 to 2;
  the mask is `inherit`, `true` or `false`. Passes after the first inherit their tone from `nrLaterPassLocalTone`.
  An intensity of zero still evaluates the pass and does not remove its GPU/VRAM cost.
- **HDR input and auto exposure** use the common `srHdr` and `srAutoExposure` controls for the selected
  reconstruction engine. Both default to `game`; restart after changing them. They do not turn on display HDR or
  convert SDR to HDR, and incorrect input assumptions can damage image quality. DLSS models may impose auto
  exposure; XeSS auto exposure has a GPU cost and is not recommended for LDR input. XeSS Vulkan LDR with manual
  exposure remains unsupported.
- **`providerPatchPolicy`** (local setting, default `heuristic`). `exact` restricts NVIDIA provider compatibility
  to reviewed files; `heuristic` also permits guarded compatibility with other eligible signed NVIDIA files, an
  admission reported as unqualified. Restart after changing it. The Hub keeps it out of shared profiles.
- Existing JSONC settings files are reordered by feature, with Intel options near the engine selectors. Selected
  values, comments and custom settings are kept.
- **DLSS presets and the reconstruction engine change while the game runs.** The Super Resolution and Ray
  Reconstruction presets, and the choice between DLSS and XeSS Super Resolution, now apply without a restart in
  Direct3D 12, Direct3D 11 and Vulkan games. Frame Generation presets and the frame-generation engine still need a
  restart. In Vulkan games, XeSS must already be selected when the game starts.
- **Smooth Motion on NVIDIA driver 621.01** (RTX 30). Verified in Enshrouded (Vulkan) on the maintainer's PC.
- **Smooth Motion in Vulkan games** now always runs where the game's NVIDIA profile excludes it. The
  **Force Smooth Motion** setting is gone; a `smoothMotionForce` line left in `rtx-encore.jsonc` is ignored. NVIDIA
  may exclude a game for a reason: unexpected results remain possible there. See [Smooth Motion](docs/SMOOTH-MOTION.md).
- **The menu names the engine and the preset.** The Upscaling chip shows DLSS SR, DLSS RR or XeSS-SR and, for DLSS,
  the preset requested when the feature was created; the Frame Generation chip shows DLSS-G and the FG preset RTX
  Encore supplies. The driver does not report the model it actually runs.
- DLSS models delivered by the NVIDIA driver (for example through the NVIDIA app's DLSS override) are now recognized
  by the menu.

### Fixed

- **Smooth Motion could stay off in Vulkan games**: it now starts reliably when the driver is reloaded during the
  game's startup.
- **XeFG**: Cyberpunk 2077 window resizing, borderless window size, game-controlled multi-frame generation
  (X4 checked in GTA V Enhanced), the menu drawn over XeFG's presentation, and XeFG created before the game's
  Streamline setup.
- **XeSS Super Resolution**: Direct3D 12 games recording on their own command lists, the game's compute state
  restored after XeSS, legacy Vulkan integrations routed to XeSS, and the DLSS fallback presets updated live.
- The menu warns about XeSS with Ray Reconstruction only while the game actually runs Ray Reconstruction.

Checked by the maintainer: Smooth Motion in Enshrouded on driver 621.01, XeFG X2 and X4 in Cyberpunk 2077, XeFG X4 in
GTA V Enhanced, XeSS SR in 007 First Light, XeFG X2 in No Man's Sky and DOOM: The Dark Ages (Vulkan, borderless,
NR off), NR after RR in 007 First Light with path tracing (image quality and cost not measured). Live preset
switching, HDR/exposure overrides and provider admissions have automated checks only; other games, GPUs and
drivers were not tested. On an Intel route the menu can be unavailable: use the Hub to change settings.

### Known issue

- **007 First Light** can still crash intermittently at startup; another launch may succeed.
- **SCSKiller's recorder is not compatible with RTX Encore.** When SCSKiller records a game, its recorder takes over
  the game's swap chain creation before RTX Encore, so XeFG is refused and the game's DLSS-G stays in use (the Frame
  Generation chip shows DLSS-G). In The Witcher 3 the game could also freeze while loading, with a GPU reset when frame
  generation started. You can keep using SCSKiller: in its **Settings**, turn off **Record in all compatible games**
  (and recording on the game's own page if you turned it on there), then restart the game. Other tools that hook
  DXGI's swap chain creation can have the same effect.

Thanks to [@SanyaGames227](https://github.com/SanyaGames227) for requesting HDR-input and auto-exposure controls
([#63](https://github.com/SilyNoMeta/rtx-encore/issues/63)), and to [@Theoryedz](https://github.com/Theoryedz) for
the report in [#58](https://github.com/SilyNoMeta/rtx-encore/issues/58). The reporters have not tested this release.

Thanks to [@dashdogy](https://github.com/dashdogy) (MFG Unlocker) for the idea of changing DLSS presets and the reconstruction engine while the game runs.

## 1.2.0-beta.1

**XeSS / XeFG: VERY EXPERIMENTAL.** Getting this integration working was a particularly difficult and frustrating part of development :) Expect bugs, crashes, visual artifacts and differences between games and graphics APIs. Please report problems: in RTX Encore Hub, open the game’s **Health** page, choose **Prepare a diagnostic**, review the archive for personal information, then attach it to an [RTX Encore issue](https://github.com/SilyNoMeta/rtx-encore/issues/new/choose). Include your engine choices, graphics API and requested multiplier. The Hub does not upload the archive for you.

- Existing JSONC settings files are reordered by feature, with Intel options near the engine selectors. Selected values, comments and custom settings are kept.
- **Ray Reconstruction + Neural Rendering (Direct3D 12)** — Compatible linear-depth games can now run NR after RR. Checked locally in 007 First Light with path tracing on an RTX 3070 Ti Laptop: NR runs; image quality and cost have not been measured. Direct3D 11 and Vulkan still do not support this linear-depth case.

- **Intel XeSS and XeFG (experimental)** — RTX Encore 1.2.0-beta.1 adds a choice of reconstruction and frame-generation engines. Leave **Game** selected to keep the game’s normal engine. Intel modes are experimental, off by default and require compatible game inputs; choosing an engine does not prove it became active.
- Each of up to four NR passes can override intensity, local tone, local structure, skin structure and the character mask, or inherit the global value. Numeric overrides are 0 to 2; the mask is `inherit`, `true` or `false`. Passes after the first inherit their tone from `nrLaterPassLocalTone`. An intensity of zero still evaluates the pass and does not remove its GPU/VRAM cost.
- HDR input and auto exposure use the common `srHdr` and `srAutoExposure` controls for the selected reconstruction engine. Both default to `game`; restart after changing them. They do not turn on display HDR or convert SDR to HDR. Incorrect input assumptions can damage image quality. DLSS models may impose auto exposure; XeSS auto exposure has a GPU cost and is not recommended for LDR input. XeSS Vulkan LDR with manual exposure remains unsupported. RR and frame generation are unchanged.
- The local `providerPatchPolicy` setting defaults to `heuristic`. `exact` restricts NVIDIA provider compatibility to reviewed files; `heuristic` also permits guarded compatibility with other eligible signed NVIDIA files. That admission remains unqualified and is reported as such. Restart after changing the policy. The Hub keeps it out of shared profiles; it is not a promise of game compatibility.

Limited local Vulkan trials in No Man’s Sky and DOOM: The Dark Ages support the XeFG X2 route; the DOOM test was accepted by the maintainer in borderless mode with NR off. This does not qualify exclusive fullscreen, DirectX 11, XeFG with NR, all higher multipliers or other games. The menu/overlay can be unavailable on an Intel route; use the Hub to change settings and review its session findings.

007 First Light’s intermittent startup crash remains unresolved. New HDR/exposure overrides and provider admissions have static/local automated checks; this is not broad visual or in-game validation.

Thanks to [@SanyaGames227](https://github.com/SanyaGames227) for requesting HDR-input and auto-exposure controls ([#63](https://github.com/SilyNoMeta/rtx-encore/issues/63)). This is a feature request, not reporter validation of the final beta.

[Intel XeSS and XeFG (experimental)](docs/INTEL-ENGINES.md)

Thanks to [@Theoryedz](https://github.com/Theoryedz) for reporting the issue in [#58](https://github.com/SilyNoMeta/rtx-encore/issues/58). The reporter has not tested the updated build.

## 1.1.4

Includes everything from 1.1.3.

### Fixed

- **Neural Rendering flickered with two or more passes** ([#62](https://github.com/SilyNoMeta/rtx-encore/issues/62)). In Aion 2, the picture flickered, mostly in busy menus such as character creation, while the Neural Rendering indicator turned yellow and then green. With two or more passes, a frame could be left without Neural Rendering when the game was submitting its own work at the same moment. That no longer happens.
- **Aion 2: the menu cursor stayed stuck in the middle of the screen**, so the settings could not be changed. The game no longer pulls the cursor back while the menu is open.

Checked on the maintainer's PC in Aion 2 with two passes: the flicker no longer appears on the character creation screen. The reporters have not tested this release. The maintainer has since confirmed the cursor fix in Aion 2. Other games, other GPUs and three or four passes were not tested.

### Known issue

- **007 First Light** can still crash intermittently at startup. This release does not fix that issue; another launch may succeed.

### Thanks

- [@unsungloser](https://github.com/unsungloser) — reported the flicker with several Neural Rendering passes in Aion 2 and supplied the logs and a video ([#62](https://github.com/SilyNoMeta/rtx-encore/issues/62)).

## 1.1.3

### Fixed

- **The Witcher 3: Wild Hunt (DirectX 12) crashed at startup with 1.1.2** ([#60](https://github.com/SilyNoMeta/rtx-encore/issues/60)). This release fixes that regression while keeping the menu improvements from 1.1.2.

Checked locally on the maintainer's GOG copy: startup succeeds with the menu active, installed as either `version.dll` or `dxgi.dll`. These are automated startup checks, not extended gameplay tests. The Steam reporter has not yet tested the fix.

### Known issue

- **007 First Light** can still crash intermittently at startup. This release does not fix that issue; another launch may succeed.

### Thanks

- [@Herror7](https://github.com/Herror7) — reported the Witcher 3 startup regression and supplied the logs that helped identify it ([#60](https://github.com/SilyNoMeta/rtx-encore/issues/60)).

## 1.1.2

Everything from the 1.1.2 pre-releases, and the fixes that came after 1.1.2-beta.3.

### New

- **Smooth out X3 / X4** ([#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)). In some games, X3 and X4 showed the generated frames in bursts, so motion looked choppier than at X2: Baldur's Gate 3 with bg3fgvk, for example. With this setting on, Streamline spaces the generated frames itself instead of leaving it to the NVIDIA driver, now in Vulkan games as well as DirectX 12. It stays off by default: turn it on in the menu, in the Frame generation section under the multiplier, in RTX Encore Hub, or with `flipMeteringBypass` in the `frameGeneration` section, then restart the game. An existing setting is kept. See [Frame generation](docs/FRAME-GENERATION.md#smooth-out-x3--x4).
- **Versions line of the overlay**: it now lists every NVIDIA DLL the game loaded, Ray Reconstruction and Neural Rendering included. DLLs of the same version share a line (`SR-RR-FG-NR 310.9.16`), and a DLL that is not loaded is not shown.

### Fixed

- **Neural Rendering crashed some DirectX 12 games** ([#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)). Turning Neural Rendering on could crash the game a moment later on RTX 20, 30 and 40 GPUs, whatever the driver: Resident Evil Requiem, Pragmata and 007 First Light, for example. Neural Rendering now leaves the game's rendering state as it found it. NVIDIA's on-screen DLSS indicator could also make it fail; Neural Rendering now has everything the indicator needs. If a step still fails when Neural Rendering starts, the log says which one.
- **The RTX Encore menu did not appear** (Insert did nothing) in more games:
  - **Pillars of Eternity II: Deadfire** and other DirectX 11 games that run their graphics on a single thread, whatever the name RTX Encore is installed under ([#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)).
  - **With RTX Encore installed as `dxgi.dll`**, in games that take their display from it ([#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)).
  - **Games with an older version of NVIDIA Streamline**, such as The Last of Us Part I ([#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)) and Dying Light 2. In Dying Light 2, install RTX Encore as `winmm.dll`.
  - **DirectX 11 games** that create their display in ways the menu did not follow before, including games whose graphics are handled by a separate part of the game loaded after startup.
- **Overlays** (RivaTuner Statistics Server, Steam, Epic, Rockstar Social Club, Overwolf, GOG Galaxy, Discord, Ubisoft Connect): the RTX Encore menu no longer attaches to their own windows or tiny helper surfaces, only to the game's.

Checked in game on the maintainer's PC (RTX 3070 Ti Laptop): Resident Evil Requiem crashed as soon as Neural Rendering started and now runs with it; Black Myth: Wukong still works, with and without the DLSS indicator; the menu opens in Dying Light 2, Rise of the Tomb Raider and The Blood of Dawnwalker; in No Man's Sky the bursts at X4 are gone with Smooth out X3 / X4 on. Not yet confirmed in game: Pragmata, 007 First Light, Pillars of Eternity II, The Last of Us Part I and Baldur's Gate 3.

Windows Release build, all six distribution variants; the automated regression tests pass outside the game.

### Thanks

- [@Theoryedz](https://github.com/Theoryedz) — the Resident Evil Requiem report, then tests in Pragmata and 007 First Light with and without the DLSS indicator, with the logs and crash records that showed where the game stopped when Neural Rendering started ([#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)).
- [@sewer86](https://github.com/sewer86) — the Pillars of Eternity II report, with logs from 1.1.2-beta.1 and 1.1.2-beta.3 that showed where the menu was lost ([#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)).
- [@deusimperatork](https://github.com/deusimperatork) — the Baldur's Gate 3 report, with logs and PresentMon captures that showed the generated frames arriving in bursts at X4 ([#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)).
- [@SanyaGames227](https://github.com/SanyaGames227) — The Last of Us Part I logs on driver 591.86 that showed the menu never attaching ([#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)).

## 1.1.2-beta.3 — pre-release

### Fixed

- **Neural Rendering with NVIDIA's DLSS indicator on** ([#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)). With NVIDIA's on-screen DLSS indicator enabled, turning Neural Rendering on could crash the game about two seconds later on RTX 20, 30 and 40 GPUs (reported in Resident Evil Requiem on an RTX 3090 Ti). Neural Rendering now has everything the indicator needs. If a step still fails when Neural Rendering starts, the log now says which one and how to turn the indicator off.

  Checked in game on the maintainer's PC (RTX 3070 Ti Laptop, Black Myth: Wukong, DirectX 12) with the indicator on: Neural Rendering starts and runs. Version 1.1.1 did not crash on that PC either, so the fix is not yet confirmed in Resident Evil Requiem.

- **The RTX Encore menu did not appear with RTX Encore installed as `dxgi.dll`** in games that take their display from it, such as Pillars of Eternity II: Deadfire (DirectX 11, [#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)). Not yet confirmed in game: Pillars of Eternity II could not be tested on the maintainer's PC.

Windows Release build, all six distribution variants; the automated regression tests pass outside the game.

### Thanks

- [@Theoryedz](https://github.com/Theoryedz) — the Resident Evil Requiem report, with the logs and crash records that showed where the game stopped when Neural Rendering started ([#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)).
- [@sewer86](https://github.com/sewer86) — the Pillars of Eternity II report, with logs from 1.1.2-beta.1 that showed where the menu was lost ([#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)).

## 1.1.2-beta.2 — pre-release

### New

- **Flip metering bypass now works in Vulkan games too** ([#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)). In some Vulkan games, X3 and X4 showed the generated frames in bursts, so motion looked choppier than at X2: Baldur's Gate 3 with bg3fgvk, for example. With the setting on, Streamline spaces the generated frames itself instead of leaving it to the NVIDIA driver. It stays off by default: turn it on in the menu (**Compatibility** > **Flip metering bypass**) or with `flipMeteringBypass`, then restart the game. See [Frame generation](docs/FRAME-GENERATION.md#compatibility-options).

  Checked in game on the maintainer's PC (RTX 3070 Ti Laptop, X4): in No Man's Sky the bursts are gone with the setting on; in DOOM: The Dark Ages the driver already spaced the frames well and the setting makes no visible difference. Baldur's Gate 3 is not yet confirmed.

### Changed

- The setting is now called **Flip metering bypass**, and its description says when to try it.
- **Versions line of the overlay**: it now lists every NVIDIA DLL the game loaded, Ray Reconstruction and Neural Rendering included. DLLs of the same version share a line (`SR-RR-FG-NR 310.9.16`), and a DLL that is not loaded is not shown.

Windows Release build, all six distribution variants; the automated regression tests pass outside the game.

### Thanks

- [@deusimperatork](https://github.com/deusimperatork) — the Baldur's Gate 3 report, with logs and PresentMon captures that showed the generated frames arriving in bursts at X4 ([#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)).

## 1.1.2-beta.1 — pre-release

### Fixed

- **The RTX Encore menu did not appear** (Insert did nothing) in more games:
  - **Games with an older version of NVIDIA Streamline**, such as The Last of Us Part I ([#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)) and Dying Light 2. Smooth Motion and DLSS already worked there; only the menu was missing. In Dying Light 2, install RTX Encore as `winmm.dll`.
  - **DirectX 11 games** that create their display in ways the menu did not follow before, including games whose graphics are handled by a separate part of the game loaded after startup.
- **Overlays** (RivaTuner Statistics Server, Steam, Epic, Rockstar Social Club, Overwolf, GOG Galaxy, Discord, Ubisoft Connect): the RTX Encore menu no longer attaches to their own windows or tiny helper surfaces, only to the game's.

Checked in game on the maintainer's PC (RTX 30): the menu now opens in Dying Light 2 (DirectX 11); it still opens in Rise of the Tomb Raider (Epic Games Store) and The Blood of Dawnwalker. The Last of Us Part I could not be tested here and is not yet confirmed. Windows Release build, all six distribution variants; the automated regression tests pass outside the game.

### Thanks

- [@SanyaGames227](https://github.com/SanyaGames227) — The Last of Us Part I logs on driver 591.86 that showed the menu never attaching ([#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)).

## 1.1.1

### Fixed

- **Games with the Epic Online Services overlay**: this overlay comes with games built on Epic's online services, whatever the store you bought them from. Two problems with it are fixed:
  - **Smooth Motion on RTX 30 in DirectX 12**: the game could stop with "Device removed" (`0x887A002B`) shortly after starting, as soon as the overlay appeared. Rise of the Tomb Raider (Epic Games Store) quit after the terms of use; it now reaches the menu with Smooth Motion on.
  - **The RTX Encore menu did not appear** (Insert did nothing and the first-launch message never showed) in games that set up their graphics in a certain way, such as Rise of the Tomb Raider, with or without Smooth Motion ([#50](https://github.com/SilyNoMeta/rtx-encore/issues/50), thanks to @kakaroto-cmd for pointing out the problem and for the logs).

  Both fixes were checked in Rise of the Tomb Raider (Epic Games Store, DirectX 12, RTX 3070 Ti Laptop). The menu was checked with Smooth Motion off; other games with this overlay are not yet confirmed.

## 1.1.0

This release arrives with RTX Encore's **first sponsorship**. Thank you, sincerely: it means a lot for a project made on free evenings.

### New

- **Fast super resolution (RTX 30)**, off by default: on RTX 30 cards in DirectX 12, DLSS Super Resolution presets **M** and **L** run a faster version of NVIDIA's model. On an RTX 3070 Ti Laptop, the upscaling took about **18 % less GPU time with M and 15 % less with L** on our bench, and **22 % less in The Witcher 3 and 25 % less in Black Myth: Wukong** in game (both with M). The image stays very close to NVIDIA's, with no difference visible in our tests. These are first figures, from one laptop and two games: your gain will depend on your card and the game. See [Super Resolution](docs/SUPER-RESOLUTION.md#fast-super-resolution-rtx-30).
- **The SR model preset chosen in RTX Encore now applies even when the game's NVIDIA driver profile forces another one** (the DLSS override of the NVIDIA app), with DLSS Super Resolution 310.9.1 and 310.9.16. With other versions, the driver still wins.
- **Neural Rendering accepts NVIDIA's `nvngx_dlssnr.dll` 310.9.16** as well as 310.8.0. This file has two networks, chosen with the new **NR model preset**, beside the SR and RR model presets: **B** (the default), smaller and much faster, and **A**, the network 310.8.0 runs. In a local test at 1080p on an RTX 3070 Ti Laptop, B took about 15 ms of NR time and A about 27 ms. RTX Encore's optimizations for RTX 20 and 30 now apply to both networks of this file as well, with the same image. With 310.8.0, nothing changes. Support for 310.9.16 is experimental and lightly tested. See [Neural Rendering](docs/NEURAL-RENDERING.md).
- The DLSS-G 310.9.16 of the Streamline SDK 2.14.16 is accepted as well, with the same features as the one the NVIDIA app downloads: presets A, B and C, Lite, Fast frame generation and UI recomposition.
- The Super Resolution 310.9.16 of the Streamline SDK 2.14.16, the same file as on NVIDIA's DLSS GitHub, is recognized as well: the SR model preset chosen in RTX Encore applies even when the driver profile forces another one. This file runs the code of 310.9.1, so it has no presets A to D; Fast super resolution works with it.
- **A shortcut to turn Neural Rendering on and off**: **Ctrl+Alt+N** by default, changeable in the menu's shortcut editor ([#47](https://github.com/SilyNoMeta/rtx-encore/issues/47), requested by @SiTWulf).
- **Smooth Motion on RTX 30 also accepts the NVIDIA 591.86 driver** ([#52](https://github.com/SilyNoMeta/rtx-encore/issues/52), requested by @SanyaGames227). Checked outside a game only: not yet confirmed in game.

- The `rtx-encore-logs` folder also holds `rtx-encore-status.json`, the current session's state, which RTX Encore Hub reads.

### Changed

- `nrPreset` becomes `dlssNrPreset`, in the `dlssPresets` section beside the other model presets, and takes `"auto"`, `"a"` or `"b"` (default `"b"`). Settings files of earlier versions keep working: their `nrPreset` is renamed with its value, and `0` reads as auto, `1` as a, `2` as b, `3` as auto.
- The menu's **Component compatibility** block is folded into **Session details**, which now also lists the Streamline Frame Generation, SR and RR versions and any feature the loaded files do not allow, with the reason.

### Fixed

- **Two copies of RTX Encore in the same game**, under two file names (for example `version.dll` and `dxgi.dll`), both changed it; with Ray Reconstruction and Neural Rendering on, the image was badly degraded. The first copy now runs alone and the log names the other one, to remove.
- **DLSS Super Resolution and Ray Reconstruction sizes**: the input and output sizes are now read as the game submits them, before DLSS creates the feature; in Starfield the log could show them swapped ([#51](https://github.com/SilyNoMeta/rtx-encore/issues/51), log provided by @Avictuu). The higher power draw reported in that issue is not addressed by this change.
- **Smooth Motion on RTX 30 with the 616.64 and 620.12 drivers**: part of the work Smooth Motion needs was refused with these drivers. This may explain [#35](https://github.com/SilyNoMeta/rtx-encore/issues/35) (reported by @CorwinMaximus); not yet confirmed in game.
- The documentation now says that, with Ray Reconstruction, Neural Rendering works on the image RR has denoised, never on the raw ray-traced image ([#48](https://github.com/SilyNoMeta/rtx-encore/issues/48), raised by @wisecase2).

## 1.0.0

The first stable release of **RTX Encore**. It follows `v1.4.5.3-rtx20-30-40`, published as DLSSG-Transfusion, and replaces it; version numbers start again at 1.0.0. This entry gathers everything since then, including what the betas and release candidates brought.

### New

- **Built-in menu and overlay.** Press **Insert** in the game: every setting, the state of each feature, and an overlay with frame rate, multiplier, frame pacing, GPU, VRAM and the loaded DLSS, DLSS-G and Streamline versions. It works in DirectX 11, DirectX 12 and Vulkan games; ReShade is no longer needed. **System → Component compatibility** lists the loaded components, which features they allow and why a choice is unavailable. See [Menu and overlay](docs/MENU-AND-OVERLAY.md).
- **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)**, the Windows companion: it installs RTX Encore in your games with the latest qualified DLSS and Streamline files and NVIDIA's Neural Rendering file, updates every game in one click, and can always put a game back as it was. Installing by hand works as before; see [Installation](docs/INSTALLATION.md#the-easy-way-rtx-encore-hub).
- **Neural Rendering** on RTX 20, RTX 30 and RTX 40, applied to the image DLSS produces: one to four passes with a style for each, HDR support, and a choice of the NVIDIA engine or the experimental Open engine. Off by default; needs NVIDIA's `nvngx_dlssnr.dll` 310.8.0. Open does not support RTX 20.
- **Our NR performance work for Ampere (RTX 30):** faster processing with identical output in local NVIDIA-engine reference tests; NVIDIA Fast precision with about 17 % less NR time measured on RTX 30; independent NR resolution, including Ultra Performance; Open's Fast precision and optional speed/VRAM controls. Our **Ultra-fast mode** reduced NR time from **18.7 to 9.8 ms (−48 %)** and raised displayed FPS from **73.8 to 94.9 (+29 %)** in the Wukong benchmark on an RTX 3070 Ti Laptop with FG X3, one run per configuration; one NR pass, possible motion artifacts, results not guaranteed. See [Neural Rendering](docs/NEURAL-RENDERING.md).
- **More Streamline versions.** Native Dynamic MFG and UI recomposition with the qualified production Streamline 2.11.1, 2.12.0, 2.14.1 and 2.14.16; Encore adaptive X2–X4 for qualified older DirectX 12 Streamline 2.7.2–2.10.3; support for the files the NVIDIA app beta of 13 October 2026 downloads (Streamline 2.14.16, DLSS-G 310.9.16). Vulkan keeps its adaptive path; DirectX 11 keeps Smooth Motion X2. See [Compatibility](docs/COMPATIBILITY.md#streamline-versions).
- **DLSS presets:** independent Super Resolution, Ray Reconstruction and Frame Generation preset choices, applied after a restart, including frame generation preset **C (Lite)**, NVIDIA's faster performance mode, with the qualified DLSS-G 310.9.16.0. A/B keep Normal, and **Fast frame generation** stays independent of the preset. Defaults keep the game's choices.
- **Frame-rate controls:** a frame limit through NVIDIA Reflex with direct FPS entry, **Use game settings** and **For this display** (a margin that grows with the refresh rate: 116 FPS at 120 Hz, 138 at 144 Hz, 224 at 240 Hz), a request to present without V-Sync, and compatible ray-traced hair in The Witcher 3.
- **Smooth Motion on RTX 30**, set from the menu, accepts NVIDIA drivers 620.12, 617.42, 617.14, 616.92 and 616.64 (617.14 verified in games). **Force Smooth Motion** (off by default) covers Vulkan games whose NVIDIA profile keeps it off, and the overlay and menu say when Smooth Motion is not actually running. See [Smooth Motion](docs/SMOOTH-MOTION.md).
- **One universal file:** `rtx-encore.dll` accepts nineteen file names, so it can sit beside the mods that already use the usual ones.
- **Optional panel for Cyber Engine Tweaks** in Cyberpunk 2077 (experimental).
- **`disableInGameMenu`** leaves the menu out, for games that reject it (Star Wars Outlaws); frame generation keeps working.
- **Diagnostics:** session logs in a `rtx-encore-logs` folder; an automatic record of the system, the loaded component versions, DLSS quality and resolutions, and normal Streamline/NGX messages with feature rejection reasons; separate exception logs for each process, with the thread that ended the process. Optional: `logPerformance` (frame times, GPU temperature, power, clocks and limits), `logGraphics`, `logStreamline`, `logNr`, `profileNrGpu`, `profileFrameGpu` (GPU time of each DLSS pass on DirectX 12), and for crash reports `logDxgiWrappers` and `crashDetails` (memory details and a `.dmp` snapshot). See [Settings](docs/SETTINGS.md).
- A warning when the earlier dlssg_for_sm86 add-on is still installed next to RTX Encore: with both active, frame generation may show no generated frame.

### Changed

- The files are named `rtx-encore.*`. Settings, shortcuts and an earlier plugin are taken over automatically: see [Upgrading from an earlier name](docs/INSTALLATION.md#upgrading-from-an-earlier-name).
- The settings file is `rtx-encore.jsonc`, organized in sections, with a comment on each key; several settings and menu entries have clearer names, and an earlier settings file is taken over with its values.
- Frame generation costs 2 to 3 % less GPU time on RTX 40 cards.
- The overlay text is more compact, with a size setting; when the game's Streamline plugins are newer than its Streamline core, the overlay shows both versions.
- The former ReShade add-on is no longer shipped.

### Fixed

- **Star Wars Outlaws** ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)): the full game (Ubisoft Connect) closes at launch when the menu is attached to it; set `"disableInGameMenu": true`, or apply the game's community profile in the Hub, and frame generation up to X4 starts, as two users confirmed with test builds. Also fixed: a crash when the video settings show the frame generation options, and a crash after an alt-tab from the main menu.
- **Starfield:** a crash when enabling frame generation with the menu present ([#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)), a shutdown problem with Streamline 2.12 ([#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)), and live UI recomposition changes being ignored while frame generation runs ([#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)).
- **Forza Horizon 6** closing at its startup logos with the menu ([#11](https://github.com/SilyNoMeta/rtx-encore/issues/11), [#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)).
- **Final Fantasy VII Rebirth** hiding frame generation on RTX 20 and 30 cards ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)).
- **Where Winds Meet** failing to start (0xc0000142) with `version.dll` or `winmm.dll` ([#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)); use `version.dll` in that game. Also a crash of the `dinput8.dll` name with the Epic overlay.
- **Euro Truck Simulator 2:** the menu did not appear ([#22](https://github.com/SilyNoMeta/rtx-encore/issues/22)); install RTX Encore as `winmm.dll` in that game.
- **Cyberpunk 2077** startup problems linked to the Reflex plugin across Streamline restarts ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)), and **The Witcher 3** with dynamic frame generation ([#4](https://github.com/SilyNoMeta/rtx-encore/issues/4)): in-game confirmation pending.
- **The Last of Us** startup crash with OptiScaler and the game's older Streamline ([#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)): in-game confirmation pending.
- The menu refusing to attach in DirectX 12 with **Special K** ([#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)).
- DLSS **Ray Reconstruction** activity not detected ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)); Neural Rendering now says when its depth input is unavailable.
- Reflex frame-limit changes applied late or not restored across game threads.
- The first launch after an update from an older settings file, where some startup settings only took effect at the second launch; and a game's crash reporter writing its own session logs over the real ones.

### Known limits

- RTX 20 support is experimental and has not been validated on a physical RTX 20 card; a frame generation failure on RTX 20 ([#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)) is still under investigation.
- In Star Wars Outlaws (full game), the in-game menu is not available.
- Neural Rendering with several passes, the Open engine, the Cyber Engine Tweaks panel and ray-traced hair on RTX 20 and RTX 30 have had little or no testing in games. X5 and X6 are experimental and need plenty of VRAM.
- Several fixes above were confirmed with test builds or local runs, not with this final build; where a reporter has not retested, it is said above. Automated checks are no substitute for testing each game, card and driver.

Windows Release build with Open NR included, default diagnostics and all six distribution variants, built and tested on the maintainer's PC: the automated regression tests pass outside the game. The final 1.0.0 build was not run in game before publication, and no performance improvement is claimed beyond the measurements named above.

### Thanks

- [@ivanmestre](https://github.com/ivanmestre) — the Star Wars Outlaws report, logs, isolation and trace tests and crash snapshots ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)).
- [@yannduwooz83](https://github.com/yannduwooz83) — the Star Wars Outlaws tests and the comparison with the earlier project ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)), the Final Fantasy VII Rebirth report and logs ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)).
- [@Avictuu](https://github.com/Avictuu) — repeated Starfield tests, component comparisons, Reflex and exit logs ([#21](https://github.com/SilyNoMeta/rtx-encore/issues/21), [#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)).
- [@mennogreg](https://github.com/mennogreg) — Starfield reports, logs and repeated tests ([#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)), the OptiScaler report ([#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)), the Ray Reconstruction confirmation ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)).
- [@hunan-NRT](https://github.com/hunan-NRT) — the Ray Reconstruction report and Cyberpunk tests ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)).
- [@naruto490610-alt](https://github.com/naruto490610-alt) — Cyberpunk reports and logs ([#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)), the Euro Truck Simulator 2 report ([#22](https://github.com/SilyNoMeta/rtx-encore/issues/22)).
- [@wisecase2](https://github.com/wisecase2) — Witcher 3 session logs and Cyberpunk evidence ([#4](https://github.com/SilyNoMeta/rtx-encore/issues/4), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)).
- [@avikpal693](https://github.com/avikpal693) and [@wer2357666](https://github.com/wer2357666) — Forza Horizon 6 reports and logs ([#11](https://github.com/SilyNoMeta/rtx-encore/issues/11), [#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)).
- [@lovebabyx](https://github.com/lovebabyx) — the RTX 2070 Super logs that showed what the session log was missing ([#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)).
- [@wd3dmlys](https://github.com/wd3dmlys) — the GPU-architecture pointer ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)).
- [@ranmo-1](https://github.com/ranmo-1) — the Where Winds Meet report ([#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)).
- [@PhantomGamers](https://github.com/PhantomGamers) — the refresh-rate margin formula ([#23](https://github.com/SilyNoMeta/rtx-encore/issues/23)) and the Special K report ([#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)).
- [@PrincessofCelery](https://github.com/PrincessofCelery) — the reports that improved freeze diagnostics ([#19](https://github.com/SilyNoMeta/rtx-encore/issues/19)).
- [@CorwinMaximus](https://github.com/CorwinMaximus) — the 620.12 driver request ([#35](https://github.com/SilyNoMeta/rtx-encore/issues/35)).
- [@hasnainb237](https://github.com/hasnainb237) — the Star Wars Zero Company logs that showed the leftover add-on ([#1](https://github.com/SilyNoMeta/rtx-encore/issues/1)).

Thanks to everyone who shared logs and tested the betas and release candidates.

## Earlier releases

The releases up to `v1.4.5.3-rtx20-30-40` were published under the name DLSSG-Transfusion. They are no longer distributed.
