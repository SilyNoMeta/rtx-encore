# Changelog

*[Français](CHANGELOG.fr.md) · [中文](CHANGELOG.zh-CN.md)*

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
