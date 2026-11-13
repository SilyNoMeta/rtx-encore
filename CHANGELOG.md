# Changelog

*[Français](CHANGELOG.fr.md) · [中文](CHANGELOG.zh-CN.md)*

## 2.4.1

**RTX Encore 2.4.1 brings per-pass NR placement, spatial anti-aliasing across DirectX and Vulkan with HDR, DLSS custom scales down to 33%, and the fix for game FG menus stuck at X2. Hub 2.4.0 provides mod-version selection and remains the matching Hub.**

### Game frame-generation menu: X3/X4 at startup

When the installed DLSS-G DLL and an NVIDIA-cache model are both loaded, the active provider may not be identified during the game’s first capability query. RTX Encore now preserves the native reported maximum during that unresolved phase instead of replacing it with X2. Higher-factor FG requests remain limited to X2 until provider admission resolves; an identified legacy provider retains its real limit.

The regression test compiles the exact production provider-selection and capability-advertisement functions. It reproduces the startup query before provider selection, the later higher-factor admission, and known X2-only/limited providers, stale identities and game-specific menu bounds. It fails with the 2.4.0 behavior and passes with the fix.

Thanks to the S.T.A.L.K.E.R. 2 reporter for the detailed startup logs and X3/X4 checks. The logs confirm the original regression; the fixed build still needs confirmation in their game. Hub 2.4.0 remains compatible; no Hub update is required.

### Neural Rendering at several stages

Assign each of the one to four NR passes to Pre-SR, Post-SR or Final image, or inherit the global placement. Execution follows the image pipeline, preserving each pass’s appearance settings. Existing configurations retain their behavior. Native game-DLSS integration support compatible DX11, DX12 and Vulkan routes; each stage keeps its existing input, HUD, HDR and FG requirements. An unavailable later stage can be refused even when an earlier stage runs.

### FXAA and SMAA, including HDR

Optional spatial anti-aliasing before or after NIS/FSR 1 now supports compatible DirectX 11, DirectX 12 and Vulkan paths in SDR, HDR10 and scRGB. This extends the DX11 SDR support of 2.3.0. Physical HDR display and individual game results still depend on the setup.

### Custom DLSS render scale: 33–100%

Custom DLSS Super Resolution scale now reaches 33% per axis, with matching Hub bounds. Lower input resolutions trade source detail for rendering cost; results depend on the game and GPU.

### Frame generation and VRAM

Physical VRAM headroom checks avoid double-counting allocations. VRAM budget warnings are informational and no longer disable frame generation by themselves. This does not guarantee enough memory for every configuration.

### Compatibility fixes

FC27 gains integrated native SR/game-motion and Intel routing fixes. KCD2 retains menu engine ownership with NGX client exports. Legacy DLSS-G runtime limits and X2 activity reporting are corrected. NVIDIA 621.24 offline driver profiles are added; admission remains specific to supported runtimes and hardware.

### Horizon and Final image

Correct D3D12 submission inputs, native FG telemetry, RGB10 Pre-SR inputs and Final image scene/HUD composition. Pre-SR/Post-SR/Final operation and telemetry were confirmed on an earlier candidate. The final HUD flicker correction has automated/replay coverage but still needs in-game confirmation; intermittent cinematic Waiting remains under investigation.


D3D12 jittered-motion NR remains an explicit diagnostic trial, disabled by default; this release does not qualify those inputs generally. Thanks to [@OliverBokan](https://github.com/OliverBokan) for the Horizon report ([#126](https://github.com/SilyNoMeta/rtx-encore/issues/126)) and [@deusimperatork](https://github.com/deusimperatork) for the KCD2 report and logs ([#125](https://github.com/SilyNoMeta/rtx-encore/issues/125)).

### Hub version selection and component repair

Select a published mod version in Hub settings to pin it or return to an earlier release. The matching Hub exposes 2.4.0 NR placement controls and 33% custom scales, recognizes NVIDIA 621.24, and repairs/installs the required Streamline SR dependencies. Older mod versions retain their applicable controls.

Neural Rendering remains experimental and off by default. Compatibility depends on the game, API, GPU, driver and runtime. No universal FPS or image-quality gain is promised.

## 2.4.0

**RTX Encore 2.4.0 adds NR placement per pass, spatial anti-aliasing across DirectX and Vulkan with HDR, and DLSS custom scales down to 33%. Hub 2.4.0 lets you choose a mod version and roll back.**

### Neural Rendering at several stages

Assign each of the one to four NR passes to Pre-SR, Post-SR or Final image, or inherit the global placement. Execution follows the image pipeline, preserving each pass’s appearance settings. Existing configurations retain their behavior. Native game-DLSS integration support compatible DX11, DX12 and Vulkan routes; each stage keeps its existing input, HUD, HDR and FG requirements. An unavailable later stage can be refused even when an earlier stage runs.

### FXAA and SMAA, including HDR

Optional spatial anti-aliasing before or after NIS/FSR 1 now supports compatible DirectX 11, DirectX 12 and Vulkan paths in SDR, HDR10 and scRGB. This extends the DX11 SDR support of 2.3.0. Physical HDR display and individual game results still depend on the setup.

### Custom DLSS render scale: 33–100%

Custom DLSS Super Resolution scale now reaches 33% per axis, with matching Hub bounds. Lower input resolutions trade source detail for rendering cost; results depend on the game and GPU.

### Frame generation and VRAM

Physical VRAM headroom checks avoid double-counting allocations. VRAM budget warnings are informational and no longer disable frame generation by themselves. This does not guarantee enough memory for every configuration.

### Compatibility fixes

FC27 gains integrated native SR/game-motion and Intel routing fixes. KCD2 retains menu engine ownership with NGX client exports. Legacy DLSS-G runtime limits and X2 activity reporting are corrected. NVIDIA 621.24 offline driver profiles are added; admission remains specific to supported runtimes and hardware.

### Horizon and Final image

Correct D3D12 submission inputs, native FG telemetry, RGB10 Pre-SR inputs and Final image scene/HUD composition. Pre-SR/Post-SR/Final operation and telemetry were confirmed on an earlier candidate. The final HUD flicker correction has automated/replay coverage but still needs in-game confirmation; intermittent cinematic Waiting remains under investigation.


D3D12 jittered-motion NR remains an explicit diagnostic trial, disabled by default; this release does not qualify those inputs generally. Thanks to [@OliverBokan](https://github.com/OliverBokan) for the Horizon report ([#126](https://github.com/SilyNoMeta/rtx-encore/issues/126)) and [@deusimperatork](https://github.com/deusimperatork) for the KCD2 report and logs ([#125](https://github.com/SilyNoMeta/rtx-encore/issues/125)).

### Hub version selection and component repair

Select a published mod version in Hub settings to pin it or return to an earlier release. The matching Hub exposes 2.4.0 NR placement controls and 33% custom scales, recognizes NVIDIA 621.24, and repairs/installs the required Streamline SR dependencies. Older mod versions retain their applicable controls.

Neural Rendering remains experimental and off by default. Compatibility depends on the game, API, GPU, driver and runtime. No universal FPS or image-quality gain is promised.

## 2.3.0

**RTX Encore 2.3.0 brings Final image Neural Rendering, game motion vectors by default, a redesigned in-game Console and FSR 1 with HDR support.**

### Final image Neural Rendering

Process compatible game images after post-processing, before frame generation and display, in DirectX 12, DirectX 11 and Vulkan. Compatible separate scene/HUD inputs preserve the HUD; a HUD already baked into the scene is processed with it. One to four passes, foveated NR and HDR are supported on compatible routes. Vulkan: select Final image before launch, or restart after switching to it. See the Neural Rendering guide for the supported combinations.

### Game motion vectors, now the default

For added frame generation in games with DLSS Super Resolution but no DLSS Frame Generation, Game motion vectors uses the game’s available motion in DirectX 12, DirectX 11 and Vulkan, with automatic fallback to RTX Encore estimation. It also supplies compatible Final image NR. Existing explicit motion choices are preserved. Earlier DX11/Vulkan game-input routes remain available; 2.3.0 exposes the shared choice and makes it the default. The same five choices are now shown consistently throughout the mod and Hub.

### A new in-game Console

Reorganized navigation, clearer engine cards and a visible image-processing pipeline make it easier to find upscaling, frame generation, Neural Rendering and performance controls. NR exposes global appearance, per-pass controls and complete multipass profiles.

### FSR 1 alongside NIS, including HDR

AMD FidelityFX Super Resolution 1 joins NVIDIA Image Scaling for games without DLSS. Both support compatible DirectX 11, DirectX 12 and Vulkan presentation paths in SDR, HDR10 and scRGB. Adjust spatial sharpness live. Optional FXAA/SMAA before or after scaling remains limited to DirectX 11 SDR. Spatial scaling requires a lower windowed resolution, the same aspect ratio and a strict upscale up to 2x per axis.

### NR looks and exposure

Seven appearance recipes are available in the mod and Hub: Standard, Subtle, Natural, Cinematic, Faces, Soft skin and NVIDIA reference. Applying a look changes appearance and clears per-pass appearance overrides, while preserving pass count, models, resolution and exposure. NR exposure offers Game, Automatic, Measured and Manual for compatible scene-linear DLSS inputs. New configurations use Automatic; existing files without the key keep Game behavior.

### Compare the image and its cost

The Image and performance lab can compare Post-SR, Pre-SR and Final image, with captures, measurements and exports. The Hub displays the selected placement pair and supports the resulting comparisons. Compare matching conditions; NR GPU time covers the NR work, not every transfer or the entire graphics pipeline.

### More selective XeFG pacing

From X3, automatic source pacing looks for persistent uneven intervals before intervening and keeps a correction only when it improves regularity within its source-FPS loss budget. It relaxes the constraint and restores an effective correction when needed. Manual mode exposes a 0–30% loss budget, detection and automatic-adjustment switches, applied live. Automatic uses a 4% budget.

### Compatibility and recovery

Fixes cover The Sims 4 DX11 DLSS-G startup, Baldur’s Gate 3 Vulkan Script Extender/MCM and Final image operation, Cyberpunk 2077 legacy Streamline Final image inputs, Forza Horizon 6 Final image/HUD handling, GTA V Enhanced input tracking, and Red Dead Redemption 2 Vulkan queue/guide handling and DX12 startup after Vulkan discovery. Native SR remains available when Encore initializes FG first. Additional fixes preserve ReShade with spatial/FG routes, buffer formats, window size and cursor behavior; compatible owned DXGI FG can recover after VRAM budget pressure.

### RTX Encore Hub 2.3.0

The matching Hub recognizes stable FSR 1 archives and installed proxy/ASI files, exposes Game motion vectors, NR exposure and XeFG manual pacing, and keeps older mod versions gated correctly. Guided setup supports NIS/FSR 1 and their applicable AA controls. The community schema accepts Final image and the new shared XeFG controls. NIS, Overlay Studio, Tensor Core options and per-file component selection from earlier releases remain available.

Compatibility depends on the game, graphics API, GPU and admitted runtimes. Neural Rendering and the existing experimental routes retain the limits described in their guides. No new universal FPS gain is claimed.

## 2.2.0

**RTX Encore 2.2.0 adds spatial upscaling and a customizable performance overlay.** Games without DLSS can now use NVIDIA Image Scaling (NIS), with optional FXAA or SMAA before or after scaling. The overlay offers configurable styles and metrics. XeFG Tensor Cores reach RTX 20 cards, and XeSS Tensor Cores have new RTX 30/40 optimizations.

### Upscaling and anti-aliasing

- **NVIDIA Image Scaling (NIS)** is a new upscaling choice for games without DLSS, including DirectX 11. Optional **FXAA** and **SMAA** can run before or after NIS, or be disabled. Choose the order that looks best for the game; these are spatial filters and cannot restore detail absent from the source image. The corresponding controls are available in Hub 2.2.0.
- Spatial upscaling now composes correctly with the selected reconstruction engine and handles the game's source geometry more reliably. The Sims 4 source-geometry correction was confirmed in game. NIS window sizing also accounts for Windows display scaling when an engine loads dynamically.
- **XeSS Super Resolution on Tensor Cores** has updated RTX 30 and RTX 40 paths. Output parity was checked on an RTX 3070 Ti and RTX 4090; the RTX 3070 Ti measurements varied by resolution, so this is an optimization rather than a blanket speed claim.

### Frame generation and overlay

- **DLSS-G and game Auto mode:** the new `overrideAutoFrameGeneration` setting, on by default, asks NVIDIA to keep frame generation On when a game requests Auto and NVIDIA would otherwise turn it off during a heavy scene. Turn it off to pass the game's Auto request through unchanged. It applies live and is available in Hub 2.2.0. Thanks to [@avikpal693](https://github.com/avikpal693) for the report ([#116](https://github.com/SilyNoMeta/rtx-encore/issues/116)); the reporter has not confirmed the final build.
- **XeFG on NVIDIA Tensor Cores** now admits RTX 20 (Turing) cards as an experimental option. Compatibility still depends on the driver, runtime, game and graphics API; this release does not claim general in-game validation for RTX 20 XeFG. Thanks to [@MxHassan195](https://github.com/MxHassan195) for requesting it ([#117](https://github.com/SilyNoMeta/rtx-encore/issues/117)).
- **Overlay Studio:** choose which performance metrics appear, and select a visual style. The new classic style restores white text strips; the optional graph and selected metrics remain available. Hub 2.2.0 previews and configures the styles and individual metrics.
- **Baldur's Gate 3:** the Script Extender's ImGui/MCM menu opens again with standalone DLSS Frame Generation in DirectX 11. Confirmed by the maintainer in game with a test build.
- **Kingdom Come: Deliverance:** standalone DLSS Frame Generation no longer attaches to the game's small probe window; its own window runs at X2. The NVIDIA driver Streamline override is also accepted for native DLSS-G. Both were confirmed by the maintainer in game with test builds.

The release archive is built and checked separately from these game tests. Experimental paths remain dependent on each game's integration; report the game, graphics API, GPU and Hub diagnostic when a feature does not activate.


## 2.1.0

**RTX Encore 2.1.0 makes Intel XeFG smooth at X3 and above.** A new setting, **Steady game pacing for XeFG**, on by default, removes the camera judder that high XeFG multipliers could show. Games that ship DLSS but no frame generation now give their own depth and motion to the frame generation RTX Encore adds, XeFG runs on the Tensor Cores in DirectX 11 and Vulkan too, and a long list of fixes follows: Baldur's Gate 3, Final Fantasy VII Rebirth, Forza Horizon 6, Kingdom Come: Deliverance, Islands of Insight and Assassin's Creed Black Flag Resynced.

### Steady game pacing for XeFG (new, on by default)

At X3 and above, XeFG could make camera motion judder even while the frame counter looked fine: the game's own frames alternated between short and long ones, so each group of generated images covered a different slice of time. **Steady game pacing for XeFG** starts the game's frames on a regular schedule, just below the frame rate the game can sustain, and keeps adapting it to the game. Motion stays even across every generated image.

- On by default, in the menu's **Frame rate** tab with the other XeFG settings, and in [RTX Encore Hub 2.1.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.1.0). It applies live: turn it off and on to see the difference.
- It costs a few FPS of the game's own frame rate. It never hurries a late frame and never catches up in a burst.
- It works with XeFG's variable pacing (on by default) from X3; X2 is unchanged.
- XeFG's own output is steadier too: generated images keep their spacing after a hitch and no longer drift over time.

Validated by the maintainer in Assassin's Creed Black Flag Resynced at X4 without a frame limit: the alternation is gone and motion is very smooth. Thanks to **Konan** on the [Discord](https://discord.gg/Zwmq4Xc2Br) for the reports and logs (Black Flag Resynced, Forza Horizon 6, Avatar: Frontiers of Pandora) and for spotting that the judder followed XeFG's variable pacing. Setting `xefgSourcePacing`, see [Settings](docs/SETTINGS.md) and [Intel engines](docs/INTEL-ENGINES.md).

### Frame generation with the game's own depth and motion

- **Games with DLSS but without frame generation** (Baldur's Gate 3, for example): the DLSS Frame Generation or XeFG that RTX Encore adds now uses the depth and motion vectors the game already gives its DLSS, instead of estimating them from the image, in DirectX 11 and Vulkan. When the game's data cannot be used, frame generation falls back to RTX Encore's motion estimation, and the menu then shows **Motion estimation**. The maintainer saw X4 generated in Baldur's Gate 3 in all four combinations (DirectX 11 and Vulkan, DLSS-FG and XeFG) on an RTX 3070 Ti Laptop.
- **Baldur's Gate 3 (DirectX 11): DLSS Frame Generation stayed on "Waiting"** even with the Streamline files in place. Fixed; 10-bit (RGB10) displays are accepted too. Thanks to [@deusimperatork](https://github.com/deusimperatork) for the report ([#109](https://github.com/SilyNoMeta/rtx-encore/issues/109)). The reporter has not retested yet.
- **Baldur's Gate 3 (Vulkan): Neural Rendering and DLSS Frame Generation together**, without any third-party tool. Checked by the maintainer: NR after super resolution and DLSS-FG X2 then X4 at 1440p.

### Tensor Cores in DirectX 11 and Vulkan (experimental)

- **XeFG on NVIDIA Tensor Cores** now also runs in DirectX 11 and Vulkan games without frame generation. In Baldur's Gate 3, DirectX 11 and Vulkan, the maintainer saw the green **Tensor Cores** badge active and XeFG X4. Image parity and the performance gain have not been measured yet.
- **XeSS Super Resolution on Tensor Cores** is now offered in DirectX 11 and Vulkan games without DLSS, and when XeSS replaces DLSS in a DirectX 11 game. Not yet validated in a game. Replacing DLSS in a Vulkan game keeps standard XeSS.

### Fixes

- **Final Fantasy VII Rebirth: frame generation stayed unavailable** with the NVIDIA app's (or NVIDIA Profile Inspector's) DLSS override on: the game then uses the Streamline files the driver downloaded, which RTX Encore did not recognize, so an RTX 30 card was treated as unable to run frame generation. They are recognized now. Thanks to [@yannduwooz83](https://github.com/yannduwooz83) for the report and for finding the setting involved ([#106](https://github.com/SilyNoMeta/rtx-encore/issues/106)).
- **XeFG with RTSS (RivaTuner Statistics Server, MSI Afterburner):** RTSS takes over the creation of the game's swap chain after RTX Encore and skipped XeFG from the second creation on, so XeFG could stay inactive or fail when the game recreated its window. RTX Encore now puts itself back in front of RTSS and hands each creation on to it, so both keep working. Checked with RTSS running in RTX Encore's automated XeFG tests; not yet in a game. Thanks to **Konan** on Discord, whose RTSS reports pointed at it.
- **Kingdom Come: Deliverance (DirectX 11):** the menu did not open when the game was started through Ultimate ASI Loader, and the game crashed when it lost focus (Alt+Tab, minimizing) with DLSS Frame Generation. Both fixed and confirmed in the game by the maintainer. Thanks to **SMN888** on Discord for the report and diagnostic.
- **Assassin's Creed Black Flag Resynced (XeFG):** one-second stalls and black screens are fixed, the menu keeps working with XeFG, and the overlay reports the game's own frame rate separately from the generated output. Gameplay confirmed by the maintainer; the menu and counter changes are not yet confirmed in the game. Thanks to **Konan** on Discord for the report.
- **Forza Horizon 6:** XeSS Super Resolution in DirectX 12 now activates instead of falling back, XeSS on Tensor Cores keeps running after the game resets its graphics device, and a crash with the Steam overlay is avoided. Confirmed in the game by the maintainer. Thanks to **Konan** on Discord for the report.
- **Islands of Insight and other games without DLSS upscaled to the screen's resolution:** the picture, its size and the mouse cursor stay aligned through resizes and the borderless window, frame generation starts reliably in DirectX 11, and **None** really turns the added upscaler off. Confirmed in the game by the maintainer. Thanks to **SMN888** on Discord, whose tests in games without DLSS (with Windows display scaling) led to these fixes. A new guide explains how Windows display scaling can limit the game's resolution and blur text in Unreal Engine games: [Resolution and Windows display scaling](docs/GAMES-WITHOUT-DLSS.md#resolution-and-windows-display-scaling).
- **Menu:** a game's own DLSS is shown as **Game (DLSS)** in the engine choice, and a hint says which Streamline components to install with the Hub when DLSS-FG cannot start; with **None** as the upscaling engine, the XeSS Tensor Cores switch is hidden and **Motion estimation** moves to Frame generation when only frame generation uses it.
- **Manual install:** the [list of runtimes](README.md#runtimes-for-a-manual-install) is now for every game, with Ray Reconstruction (`nvngx_dlssd.dll`) from NVIDIA's DLSS SDK 310.9.16.

### What was checked

The games named above were played by the maintainer with test builds of these changes on an RTX 4090 or an RTX 3070 Ti Laptop. Games were not retested with the final 2.1.0 binaries. Release checks cover compilation, the regression suites of these features and archive integrity.

**Known limit:** RTSS (RivaTuner Statistics Server) is now kept behind XeFG (see Fixes), but its overlay has not been played with XeFG in a game yet. If a game still crashes or XeFG stays inactive with it, turn the RTSS overlay off and please report it.

## 2.0.1

- **Manual install with NVIDIA's own files:** frame generation for games without it now also accepts NVIDIA's official DLSS Frame Generation 310.9.16 (the `nvngx_dlssg.dll` of the Streamline SDK 2.14.16 and of the DLSS SDK), so every file can come from NVIDIA's SDKs. It was refused by a detection error. Thanks to [@Kbx-7](https://github.com/Kbx-7) for the report ([#108](https://github.com/SilyNoMeta/rtx-encore/issues/108)).
- **Intel XeSS files next to RTX Encore:** XeSS-SR, XeFG and XeLL are also found next to RTX Encore's DLL, not only in `rtx-encore-runtimes\intel`.
- Files and download links for a manual install: [README](README.md#runtimes-for-a-manual-install). [RTX Encore Hub 2.0.1](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.0.1) installs NVIDIA's official 310.9.16 files.

## 2.0.0

Engine choices now include None for added reconstruction and frame generation; missing runtimes are hidden and older compatible versions receive a warning. Ultra (75%) joins DLSS SR and XeSS render scales. Added DLSS-FG supports DirectX 11, DirectX 12 and Vulkan.

**RTX Encore 2.0.0 is a major release.** Games that ship no DLSS at all now get DLSS, frame generation and Neural Rendering, driven by a motion estimation engine RTX Encore wrote for them. RTX Encore also has a new visual identity, with a new logo, and its own Discord.

**RTX Encore now has a Discord.** Share your 2.0.0 results, compare settings, or get help with the mod and Hub. [Join the Discord](https://discord.gg/Zwmq4Xc2Br).

RTX Encore 2.0.0 brings **NVIDIA DLSS Super Resolution and DLAA, Intel XeSS, Neural Rendering and frame generation to games that ship no DLSS**, in DirectX 11, DirectX 12 and Vulkan, from the image they display, without ReShade or any other tool. The game needs nothing of its own: no DLSS, no motion vectors, no frame generation. Everything is experimental and off by default; [Games without DLSS](docs/GAMES-WITHOUT-DLSS.md) is the complete guide.

- **Upscaling for games without DLSS:** DLAA at the game's resolution, or a lower game resolution upscaled to the screen's (the game in windowed mode, RTX Encore holding a borderless window over the screen), with **DLSS** or **XeSS**; the engine switches live. XeSS runs on NVIDIA Tensor Cores in DirectX 12 games, with the same qualification as elsewhere, and the **Upscaling** chip shows a **Tensor Cores** badge while they run.
- **Neural Rendering on the displayed image:** in a game without DLSS, the **Neural Rendering** switch starts NR directly on the displayed image, after the upscaler or alone, with every NR control.
- **Frame generation for games without frame generation:** **DLSS Frame Generation** X2 to X6 in DirectX 11, DirectX 12 and Vulkan; **Intel XeFG** X2 to X6 in DirectX 12 (NVIDIA Tensor Cores available), in Vulkan (SDR and HDR10) and in DirectX 11. Game decides gives X2, Fixed gives your multiplier, changed live. In DirectX 11, the game's next frame waits for XeFG's work, X3 is paced evenly, and where the screen allows tearing the output is no longer held to its refresh rate. The HUD is interpolated with the image.
- **Motion estimation, four choices:** **RTX Encore**, the mod's own estimation on the graphics card, by default (one vector per 2×2 pixels, following object edges and using the depth buffer; about 0.8 ms per 4K frame on an RTX 4090); **Shader (vort)**, the method of vortigern11's vort_Motion (about 0.6 ms); **Optical Flow (Quality)** and **Optical Flow (Fast)**, NVIDIA Optical Flow. Switch live and compare.
- **Depth found automatically** when the game draws with a depth buffer, in all three APIs; without one, everything still runs with slightly less stable motion.
- **The Hub installs it all:** [RTX Encore Hub 2.0.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.0.0) puts the DLSS, Neural Rendering, frame generation and Intel files a game without DLSS can use beside the mod when you install or update it there, and takes them away with **Restore game**. Nothing is turned on by that.
- **Manual install:** what is added to games without DLSS or frame generation runs on the exact runtimes it was validated with (DLSS Frame Generation 310.9.16 with Streamline 2.14.16, DLSS 310.9.16, Neural Rendering 310.9.16, XeSS-SR 2.0.2.68, XeFG 1.3.1.78 + XeLL 1.3.2.10). The Hub installs them; by hand, the files and their download links are in the [README](README.md#runtimes-for-a-manual-install).

Validated by the maintainer on an RTX 4090 (NVIDIA 621.01): PEAK in DirectX 12 (DLAA, upscaling 1440p to 4K, NR, DLSS Frame Generation X2 and X4, XeFG X4 with Tensor Cores), DirectX 11 (XeFG X2) and Vulkan (DLAA, upscaling, NR, DLSS Frame Generation X4, XeFG X4); The Sims 4 in DirectX 11 (XeFG X2 and X3, above 144 FPS on a 144 Hz screen); Skyrim Special Edition in DirectX 11 (XeFG X3 to X6); Football Life 2027 in DirectX 11 and HDR (DLAA and NR, 1440p to 4K and NR). Limits to know: the HUD is processed and interpolated with the image; the upscaled image is a little softer than a game's own DLSS, which has the game's camera jitter (DLAA does not have this limit); in Unity games use Windowed, not Fullscreen Window, below the screen's resolution; a game that displays in scRGB (16-bit floating point) keeps its own frames, without frame generation; Tensor Cores apply to DirectX 12 only; turn Smooth Motion off with frame generation. No RTX 20, RTX 30 or RTX 50 card and no other driver have been tested yet.

### Also in 2.0.0

- **Menu in eleven languages:** German, Spanish, Dutch, Polish, Brazilian Portuguese, Vietnamese, Russian and Japanese join English, French and Simplified Chinese. Arabic, a language of the Hub, is shown in English in the menu. The menu is now built on Dear ImGui 1.93. Thanks to [@Matarto](https://github.com/Matarto) for asking for Polish ([Hub issue 12](https://github.com/SilyNoMeta/rtx-encore-hub/issues/12)).
- **Use the NVIDIA App's newest DLSS models:** the former "Force NVIDIA OTA models" option (System tab) now does what its name says, for frame generation, Super Resolution, Ray Reconstruction and Neural Rendering: when the NVIDIA app has downloaded a newer model than the game's, and RTX Encore qualifies it, that model is used, with nothing written to the game's or the NVIDIA app's files. It gives frame generation preset C in a game that ships an older DLSS-G, and a hint under the FG preset says so. Frame generation validated in Cyberpunk 2077 on an RTX 4090 and an RTX 3070 Ti Laptop; Super Resolution, Ray Reconstruction and Neural Rendering not yet checked in a game. Thanks to [@Kbx-7](https://github.com/Kbx-7) for the idea ([#101](https://github.com/SilyNoMeta/rtx-encore/issues/101)).
- **XeFG in Ghost of Tsushima:** XeFG was accepted at X2 but generated nothing: the game hands its depth and motion in a way RTX Encore did not take. It does now; the maintainer saw X2 and X4 generated in the game. Thanks to [@MoDymm-debug](https://github.com/MoDymm-debug) for the report and logs ([#100](https://github.com/SilyNoMeta/rtx-encore/issues/100)).
- **Crash when turning frame generation on or off with XeFG (Crimson Desert):** switching the game's frame generation while XeFG was the engine could close the game. Reproduced on an RTX 3070 Ti Laptop and fixed; several on/off switches in a row then ran without a crash. Thanks to [@pradeepkumar35](https://github.com/pradeepkumar35) for the report ([#103](https://github.com/SilyNoMeta/rtx-encore/issues/103)). The reporter has not retested yet.
- **Tensor Cores in games that ship a DirectX 12 Agility SDK (Crimson Desert and others):** since 1.5.3, XeFG and XeSS Tensor Cores were refused in every such game and ran the standard Intel path. Both run again, checked in Crimson Desert on an RTX 4090 (found while reproducing #103). The XeSS Tensor Cores path also accepts the NVIDIA 621.12 hotfix, identified from the driver package, not yet validated in a game.
- **No Man's Sky does not start (Vulkan):** since 1.2.2, an Intel XeSS runtime found beside the game was prepared at game start even with DLSS selected, and on the reporter's setup the game crashed there. XeSS is now touched only when it is the selected engine when the game starts; live switching to XeSS in Vulkan is no longer offered. Not reproduced on the maintainer's machine; thanks to [@sweddd](https://github.com/sweddd) for the report and logs ([#104](https://github.com/SilyNoMeta/rtx-encore/issues/104)). The reporter has not confirmed yet.
- **XeFG replacing the game's DLSS Frame Generation in No Man's Sky (Vulkan):** HDR10 output is accepted (scRGB is not: the game's DLSS-G stays); a crash at the first frame with ReShade 6.8 installed is fixed; generated frames are shown without waiting for the display's refresh, as the game's own DLSS-G does, so the game's frame rate is no longer held to the refresh rate divided by the multiplier; the multiplier changes live. Validated by the maintainer at X4 in HDR. **Known limit:** with the game at DLAA, XeFG generates nothing (the game's full-resolution motion does not fit XeFG's contract): choose a DLSS quality mode.
- **XeSS Super Resolution in No Man's Sky (Vulkan):** the game's depth buffer was refused and DLSS kept running. XeSS now takes it; when XeSS is selected but DLSS ran instead, the menu says so under **Engine**, with the reason.
- **Smooth out X3 / X4 becomes Automatic:** the setting is now **Automatic** (default), **On** or **Off**. In Automatic, RTX Encore watches how the generated frames reach the screen at X3 and above and learns, for this game, whether Streamline should pace them itself; a notice says what it saw and what it changed, applied from the next start. The Hub shows the learned choice and can reset it. Checked in PEAK on DirectX 12.
- **Football Life 2027 (and other protected executables) closed about 1.5 s after launch** with RTX Encore installed. Fixed; the menu attaches as usual.
- **Text in menus of games without DLSS (Skyrim Special Edition):** still text was deformed with the RTX Encore motion estimation; it now keeps exactly zero motion. Text that scrolls, fades or grows in a menu leaves fewer trails behind DLSS or XeSS. Not yet confirmed in the game. XeFG also keeps working when a DirectX 11 game moves its display to another thread, as Skyrim SE does.
- **Menu:** the first two tabs are now **Frame rate** and **Image quality**, with distinct names in every language; added engines are selected directly under **Engine**, with **None** to disable them; a green **Tensor Cores** badge on the Upscaling and Frame Generation chips while XeSS or XeFG run on the Tensor Cores; the Tensor Cores switches are hidden in DirectX 11 and Vulkan games, where they can do nothing; **Fast super resolution** is shown on RTX 30 cards only, where it applies.

### What was checked

The games named above were played by the maintainer with test builds of these changes on an RTX 4090 (and an RTX 3070 Ti Laptop for #101 and #103). Games were not retested with the final 2.0.0 binaries. Release checks cover compilation, the regression suites of these features and archive integrity; the full test suite was not rerun.

## 1.6.0

1.6.0 pushes frame-generation acceleration even further: each generated frame costs less again on RTX 40 and RTX 30, and generated frames stay identical to NVIDIA's, pixel for pixel.

RTX 4090, DLSS-G 310.9.16, GPU time of each generated frame:

| Case | Optimizations off | 1.5.3 | 1.6.0 | vs 1.5.3 | vs off |
| :--- | ---: | ---: | ---: | ---: | ---: |
| 1080p X2 | 0.781 ms | 0.538 ms | 0.512 ms | −4.8 % | −34.4 % |
| 1080p X6 | 0.330 ms | 0.292 ms | 0.276 ms | −5.5 % | −16.4 % |
| 4K X2 | 1.406 ms | 1.120 ms | 1.100 ms | −1.8 % | −21.8 % |
| 4K X6 | 0.766 ms | 0.687 ms | 0.675 ms | −1.7 % | −11.9 % |

RTX 3070 Ti: the rewritten stages run 4 to 5 % (first network) and 21 to 24 % (second network) faster than in 1.5.3; the gain on the whole frame has not been measured in-game yet, nor on RTX 20.

- **Crash with ReShade and Neural Rendering (Starfield):** with ReShade 6.8 add-ons such as Luma loaded, the game could close with a heap corruption a few minutes after Neural Rendering started or was re-created. Neural Rendering now reuses its descriptor heaps instead of creating and destroying one per frame, which kept exercising a fragile path in the ReShade proxy. Reproduced and fixed on the maintainer's Starfield install with the reporter's mod set; the maintainer then played 35 minutes with the fix and that mod set (ReShade and Luma) without a crash. Thanks to the reporter of [issue 98](https://github.com/SilyNoMeta/rtx-encore/issues/98) for the detailed crash logs.
- **Menu cursor in Starfield and other Raw Input games:** in games that read the mouse without moving the Windows cursor, the menu cursor stayed stuck in the middle of the screen until the game's own pause menu released it. The menu now moves its cursor from the mouse packets themselves. Checked in Starfield; with the ReShade overlay open, ReShade still keeps the mouse for itself, as it does for every other overlay.

### Known issue

- **Starfield with Luma 2.0 (native HDR):** generated frames look embossed, with red and cyan fringes, while the camera moves. It comes from Luma's HDR output together with DLSS Frame Generation, not from this release: it also appears with earlier versions and goes away when Luma is turned off. Turn off Luma or its HDR output.

## 1.5.3

- **Microsoft Flight Simulator 2024 menu:** fixes menu attachment in compatible Streamline configurations where the menu did not open. Thanks to [@jtsorlinis](https://github.com/jtsorlinis) for the report and detailed logs ([#94](https://github.com/SilyNoMeta/rtx-encore/issues/94)). Reporter confirmation of this fix is still pending.

- **XeFG with ReShade add-ons and hand-installed games:** the XeFG engine refused to start when the game's device reached Streamline through a ReShade proxy (RenoDX and other add-ons), and when the mod was installed by hand without the Hub, even though the game shipped the qualified Intel runtimes. Both cases now proceed; the Hub-installed runtimes keep priority when present. Reported in Crimson Desert ([issue 93](https://github.com/SilyNoMeta/rtx-encore/issues/93)); not yet confirmed in-game by the reporter.
- **NVIDIA 621.12 hotfix:** the XeFG and XeSS Tensor Cores options, previously refused on any driver other than 621.01, also accept the 621.12 hotfix of 27 October. Its driver file was identified from the package itself and has not been validated in-game; if the driver does not behave exactly as expected, the options fall back to the standard Intel path. Driver Smooth Motion on RTX 20/30 still requires one of its listed drivers.
- **Ray Reconstruction preset F:** the RR model preset now offers F, the second-generation transformer (RR2) shipped with Ray Reconstruction DLL 310.9.1 and later. Older DLLs ignore it and keep their default ([issue 95](https://github.com/SilyNoMeta/rtx-encore/issues/95)).
- **XeFG and XeSS Tensor Cores on RTX 30, every RTX 40 and RTX 50:** both options were admitted on the RTX 4090 only. They now accept every NVIDIA RTX 30, RTX 40 and RTX 50 adapter on NVIDIA 621.01, with the same acceleration built for each architecture. RTX 20 remains refused and keeps the standard Intel path. Only the RTX 4090 has been validated in-game so far; please report your GPU, driver and game when you try it ([issue 99](https://github.com/SilyNoMeta/rtx-encore/issues/99)).

Thanks also to [@pradeepkumar35](https://github.com/pradeepkumar35) (#93), [@dm1z0r](https://github.com/dm1z0r) (#95) and [@Edward24RD](https://github.com/Edward24RD) (#99) for their reports and requests. The new Tensor admissions and 621.12 remain experimental; in-game qualification is still limited to the earlier RTX 4090 / 621.01 setup. No new performance gain is claimed. Final 1.5.3 binaries have no separate game retest. Release checks cover compilation, focused regressions and archive integrity; the full suite was not rerun.

## 1.5.2

- **V-Sync On:** the V-Sync setting in Frames → Frame pacing now offers Game, Off and On. On asks DirectX games to present with V-Sync, for games that gray out their V-Sync with frame generation and laptops with hybrid graphics where the NVIDIA control panel V-Sync has no effect. A setting forced in the NVIDIA control panel keeps priority; no effect in Vulkan games or with XeSS-FG; On can add latency with high multipliers on a 60 Hz screen. A saved "V-Sync off" choice is kept. Thanks to [@pradeepkumar35](https://github.com/pradeepkumar35) for the request ([#93](https://github.com/SilyNoMeta/rtx-encore/issues/93)).

The maintainer checked live switching in Crimson Desert on an RTX 3070 Ti Laptop with hybrid graphics, NVIDIA 621.01, D3D12 and DLSS-G 310.9.16. The game accepted the requested V-Sync presentation changes without a Present failure in the observed sessions. Visible tearing removal, displayed-frame pacing and added latency were not measured, and the reporter has not tested this fix. These observations used a test build; final 1.5.2 binaries have no separate in-game retest.

Use the in-game menu for the new three-way control: Hub 1.5.2 does not yet expose it. The full automated suite was not rerun; compilation, focused checks and distribution integrity were verified.

## 1.5.1

- **Menu and startup compatibility:** fixes the missing menu in compatible games that create their display through Streamline, and a startup failure with the `dxgi.dll` proxy in MechWarrior 5. Thanks to [@kisoo8](https://github.com/kisoo8) for the report and logs ([#92](https://github.com/SilyNoMeta/rtx-encore/issues/92)). The maintainer confirmed the menu and frame-generation controls in A Plague Tale: Requiem, and D3D11/D3D12 operation in MechWarrior 5, using test builds.
- **RTX 20 Smooth Motion:** V2 is now always used. The V1/V2 selector and unused V1 files are removed; older saved variant choices are ignored. This feature remains experimental, with the same graphics API and driver requirements. Hub 1.5.2 hides the retired selector for mod 1.5.1 and later.
- **Project naming:** aligns menu and diagnostic identifiers with RTX Encore.

The final release binaries have no separate in-game retest. Release validation is limited to compilation, focused checks and distribution integrity; the full automated suite was not rerun.

- **Support:** thank you to [@Scarhead666](https://github.com/Scarhead666) for sponsoring the development of RTX Encore. Public sponsor acknowledgements are shown in the About tab and the project READMEs.

## 1.5.0

- **XeSS Tensor Cores:** new independent, default-off NVIDIA Tensor option for XeSS Super Resolution, with live switching and an optimized final reconstruction pass. Includes corrections for automatic exposure and fractional render sizes.
- **XeFG Tensor Cores:** new independent, default-off NVIDIA Tensor option for XeFG, with live fixed factors X2–X6 within admitted capacity and live On/Off changes. Improves recovery after Ray Reconstruction changes in compatible configurations.
- **Hub 1.5.0 and profiles:** exposes both switches, explains their initial hardware scope, and preserves them in community profiles. Install the new mod with the game closed; changing the FG engine still requires restarting.

Initial qualification is RTX 4090, NVIDIA 621.01, DirectX 12 and the compatible Intel runtimes supplied by Hub. Unsupported preparation retains the standard Intel path; a Tensor execution fault requires a restart. Both options remain experimental.

Earlier TW3 XeSS measurements showed about 30.7% lower XeSS GPU time and 2.8–3.2% more rendered-source FPS in the measured scene. Later final-pass changes were qualified in deterministic replays, not a new game comparison. No general XeFG FPS or latency improvement is established. The maintainer confirmed live XeFG changes and RR recovery on test builds; final 1.5.0 binaries have no separate in-game retest. [Requirements and controls](docs/INTEL-ENGINES.md).

## 1.4.7

- **The Witcher 3:** fixes persistent NR shadow flickering after fast travel that remained in 1.4.6. The maintainer tested the corrected build without flickering, then reproduced the problem again after reinstalling the original 1.4.6. Thanks to [@Scarhead666](https://github.com/Scarhead666) for the report and version comparisons ([#88](https://github.com/SilyNoMeta/rtx-encore/issues/88)). The reporter has not yet tested this new fix.

- **Documentation:** updates the default shortcut tables and marks RTX 20 Smooth Motion as experimental, with its DirectX 12 and driver requirements.

Local automated tests cover the corrected paths. The maintainer’s game comparison used a test build; the final 1.4.7 release binaries have no separate in-game retest.

## 1.4.6

- **Cleaner settings menu:** advanced controls appear only when their parent feature is enabled, including Foveated NR, overlay details and capture controls. Their saved values are preserved.
- **Mirage and Shadows:** fixes the missing Mirage menu and multiplier reporting in Shadows. In compatible DirectX 12 games such as Mirage, choose **DLSS-FG** in Encore and enable the game's **Intel FG** to use NVIDIA frame generation with qualified fixed multipliers. Native FSR and games already using DLSS FG retain their original engine. Thanks to [@kisoo8](https://github.com/kisoo8) for the report and logs ([#89](https://github.com/SilyNoMeta/rtx-encore/issues/89)). The maintainer confirmed the final test build in Mirage, including the corrected motion artifacts; release binaries have no separate in-game retest.
- **Reflex controls and latency:** adds Game / On / On+Boost and the latency report in the menu and overlay. Overrides do not lower the game's choice and are suspended in Dynamic mode or while XeLL owns latency. They do not guarantee lower latency. Thanks to [@kisoo8](https://github.com/kisoo8) for the logs ([#87](https://github.com/SilyNoMeta/rtx-encore/issues/87)); the maintainer tested the controls in game.
- **REFramework:** improves NR runtime recognition when the same compatible runtime is loaded through a redirected path. Thanks to [@aziznasrimay45-sys](https://github.com/aziznasrimay45-sys) for the report ([#90](https://github.com/SilyNoMeta/rtx-encore/issues/90)). Automated checks cover this fix; no separate in-game validation is claimed.

- **The Witcher 3:** attempted to fix NR shadow flickering after fast travel, reported by [@Scarhead666](https://github.com/Scarhead666) from 1.4.3 onward ([#88](https://github.com/SilyNoMeta/rtx-encore/issues/88)). This attempt was insufficient: both the reporter and the maintainer subsequently confirmed flickering with 1.4.6.

## 1.4.5

- **Native Smooth Motion on RTX 40 / RTX 50:** the existing switch can activate the NVIDIA driver's native path with a supported driver. Select the game's actual graphics API and restart. RTX 20/30 behavior and exact driver requirements remain unchanged. [Requirements](docs/SMOOTH-MOTION.md).
- **Counter and status:** uses the requested simple 2x FPS estimate, including on native hardware. This is not a measurement of displayed or interpolated frames. Status reporting distinguishes a prepared driver from a mismatched graphics API.
- **DirectX 11 menu:** fixes the integrated menu not appearing in some DirectX 11 games, including Men of War II.
- **Live settings and stability:** improves live DLSS preset changes, delayed graphics work, graphics-resource handling and unavailable completion tracking. Some DirectX 12 configurations permit at most four live preset replacements; restart when the menu asks before making more changes. Retained resources can have a memory cost that has not been measured in games.
- **HUD and controls:** retries transient HUD texture-allocation failures within a bounded window and fixes pending frame-generation controls not being reapplied.

The maintainer confirmed the menu in Men of War II and native Smooth Motion preparation/presentation on RTX 4090 with driver 621.01 using test builds. This does not establish interpolated-frame counts, image quality, latency or long-session stability. RTX 50 and native Vulkan have no corresponding hardware qualification. Local automated tests cover the corrected paths; the final 1.4.5 binaries have no separate in-game retest. Intel XeSS / XeFG, Pre-SR and experimental RTX 20 Smooth Motion retain their limits. [Hub 1.4.4](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.4) adds matching native Smooth Motion controls.

## 1.4.4

- **Avatar: Frontiers of Pandora:** fixes colored grain and green artifacts from Neural Rendering in very dark HDR scenes. The NR effect is intentionally reduced in the darkest shadows to preserve the original image. Thanks to [@DangerousBerries](https://github.com/DangerousBerries) for the report ([#80](https://github.com/SilyNoMeta/rtx-encore/issues/80)).
- **Where Winds Meet and Direct3D NR compatibility:** fixes Neural Rendering failing to start when a game includes an older shader compiler. Keep the game's original files; renaming or replacing its compiler is not required. Thanks to [@mikotorz](https://github.com/mikotorz) for the report ([#85](https://github.com/SilyNoMeta/rtx-encore/issues/85)).
- [Hub 1.4.2](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.2) detects Where Winds Meet's game executable and requires uncertain installation destinations to be confirmed.

The maintainer confirmed the Avatar improvement in the tested scene and NR working in Where Winds Meet on test builds. These confirmations do not cover every scene or configuration, and the final 1.4.4 binaries have not been separately retested in game. Local automated tests cover HDR shadow reconstruction and Direct3D shader compatibility. Image quality, performance and temporal stability are not generally qualified. Intel XeSS / XeFG and Pre-SR remain experimental.

## 1.4.3

- **Zenless Zone Zero:** fixes XeFG compatibility with the game's original Streamline files and improves the integrated menu and Neural Rendering startup. Keep the game's original files installed. Thanks to [@WBW333](https://github.com/WBW333) for the report ([#83](https://github.com/SilyNoMeta/rtx-encore/issues/83)).
- **Intel runtimes:** recognizes compatible Intel components already installed in the game's native folders. [Hub 1.4.1](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.1) adds the matching installation and repair changes.
- **Neural Rendering:** accepts floating-point R16 depth, including the format reported in Alan Wake 2, plus additional normalized motion and floating-point inputs across supported graphics APIs. Thanks to [@MrMicko](https://github.com/MrMicko) for the Alan Wake 2 report and log ([#76](https://github.com/SilyNoMeta/rtx-encore/issues/76)).

The ZZZ fixes were checked locally in game with XeFG, the integrated menu and NR on a test build. The final startup cleanup and final 1.4.3 binaries have no separate in-game validation. NR input conversion has local automated checks, including WARP and Vulkan on RTX 4090; these do not validate NVIDIA NR image quality or performance. No confirmation of the Alan Wake 2 fix from its reporter is claimed. Intel XeSS / XeFG and Pre-SR remain experimental.

## 1.4.2

- **Pre-SR / Post-SR comparison:** fixes the heavily pixelated in-game capture preview. Fit the complete image to the panel, or turn off **Fit to panel** to inspect at **1x**, with one captured pixel per screen pixel, then zoom and pan for detail.
- **Capture inspection:** refreshes the preview after a new capture and shows captures of different dimensions side by side instead of overlaying mismatched images.

Pixel mapping, display scaling and capture refresh have local automated CPU coverage, with synthetic panel previews. The final 1.4.2 binaries have not been validated in game; image quality, performance, memory costs and latency remain unmeasured. Captures are sequential frames, and HDR is displayed as an SDR preview. The existing Hub settings and comparison reports remain compatible.

## 1.4.1

- **Stability:** improves handling of resolution changes, reconstruction settings, HUD capture and frame generation when graphics operations fail.
- **Error handling:** fixes resource ownership, loading and memory-allocation failures that could leave stale state or interrupt the game.
- **Settings and diagnostics:** improves configuration validation, menu state and the reliability of optional diagnostics.
- **Archives:** strengthens checks when preparing installation packages.

Local compilation and automated CPU tests cover the corrective changes. The final 1.4.1 binaries have not been validated in game; image quality, performance, latency and memory costs remain unmeasured. Some synchronization and runtime-lifetime risks remain unresolved. Intel XeSS / XeFG, Pre-SR and Smooth Motion on RTX 20 remain experimental.

## 1.4.0

- **Experimental NR before reconstruction (Pre-SR):** apply Neural Rendering before ordinary DLSS Super Resolution, with one to four passes and an independent model profile and appearance for each pass. Post-SR remains the default. Pre-SR requires uniform render-size inputs; RR, DLAA, foveation, padding-aware sizing and jittered motion inputs are unavailable. Unsupported inputs keep the original SR image.
- **Image and performance lab:** collect separate Post-SR and Pre-SR series, inspect captures with zoom or an A/B wipe, and export JSON, CSV and a local image viewer. Missing measurements stay unavailable. Memory counters retain their separate scopes; they do not establish savings caused by Pre-SR. A pair is comparable only when the observed SR/FG conditions and viewport association are qualified. Current routes without that association can show individual measurements and captures, but no relative gain.
- **NR pass editor:** responsive pass cards, processing-order display and independent per-pass model and appearance controls. [Requirements and comparison limits](docs/NEURAL-RENDERING.md).
- **DLSS-G controls:** fixes Encore's ownership of Fixed, Dynamic and Game modes and applies Encore's requested dynamic target instead of retaining the game's target. A target is not a strict limit on displayed FPS. Thanks to [@wer2357666](https://github.com/wer2357666) for the report and logs ([Hub #4](https://github.com/SilyNoMeta/rtx-encore-hub/issues/4)).
- **In-game menu languages:** English, French and Simplified Chinese, with a saved per-game choice and English fallback for untranslated messages. Thanks to [@pandaligx](https://github.com/pandaligx) for the request ([#78](https://github.com/SilyNoMeta/rtx-encore/issues/78)).
- **About tab:** version, project and Hub links, GitHub Sponsors, Ko-fi and public sponsor acknowledgements, including [@mennogreg](https://github.com/mennogreg).

The new Pre-SR paths, multipass changes, comparison instrumentation and DLSS-G controls have local compilation and CPU test coverage; GPU, in-game image quality, performance and physical latency remain unvalidated for this release. Sequential captures are different frames and cannot establish temporal stability. Intel XeSS / XeFG and Smooth Motion on RTX 20 remain experimental. [Hub 1.4.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.0) adds the matching settings and report reader.

## 1.3.1

- **Alan Wake 2:** fixes Neural Rendering failing to start with the game's exposure input and improves mouse handling while the menu is open. Thanks to [@SymbioticTortoise](https://github.com/SymbioticTortoise) for the report and logs ([#76](https://github.com/SilyNoMeta/rtx-encore/issues/76)).
- **Neural Rendering:** accepts additional floating-point exposure formats (RG16, RG32 and RGB32) across DirectX 11, DirectX 12 and Vulkan, where the API supports the format.
- **Vulkan Neural Rendering:** enables the RTX 20, RTX 40 and RTX 50 paths alongside RTX 30. Thanks to [@Yukk1No](https://github.com/Yukk1No) for the RTX 4070 SUPER report ([#74](https://github.com/SilyNoMeta/rtx-encore/issues/74)). These additional generations remain experimental and have not been validated on physical cards with this release.
- **Vulkan compatibility:** improves compatibility with other graphics mods, following the Baldur's Gate 3 / bg3fgvk reports from [@deusimperatork](https://github.com/deusimperatork) ([#72](https://github.com/SilyNoMeta/rtx-encore/issues/72)). Combinations with OptiScaler remain unsupported.
- **WRC Generations:** supports the game's original Streamline 1.3.3 integration and fixes the Frame Generation menu staying on Waiting when the game enables FG. Keep the original Streamline files together; mixing old and new components is unsupported. Thanks to [@dm1z0r](https://github.com/dm1z0r) for the report ([#75](https://github.com/SilyNoMeta/rtx-encore/issues/75)).

The maintainer confirmed Alan Wake 2 working and WRC's FG menu status on test builds. WRC's menu reports the enabled mode; it does not measure the displayed multiplier. The final 1.3.1 binaries have not been retested in game. Additional exposure formats have local automated coverage, without visual or performance validation on every API. Intel XeSS / XeFG and Smooth Motion on RTX 20 remain experimental.

## 1.3.0

- Foveated Neural Rendering (experimental, off by default): NR runs at full strength in the middle of the
  image and lighter towards the edges, for more FPS with a softer NR effect near the edges. In the menu,
  turn on **Foveated NR** and pick a preset (Light, Balanced, Strong); the values behind them are under
  **Foveated NR: custom values**. Strong gave +27% average FPS in Cyberpunk 2077 on an RTX 3070 Ti Laptop.
  Thanks to [maximedgoulet](https://github.com/maximedgoulet) for the idea
  ([FoveatedDLSS5](https://github.com/maximedgoulet/FoveatedDLSS5)).

- Neural Rendering follows motion correctly in games whose motion vectors are at output resolution
  (Unreal Engine 5, Black Myth: Wukong): it used to see only part of the motion.

- **Baldur's Gate 3 (Vulkan) with bg3fgvk:** no longer crashes at startup, and Neural Rendering, DLSS and DLSS Frame Generation now work there together. Thanks to [@deusimperatork](https://github.com/deusimperatork) for the detailed reports and logs ([#72](https://github.com/SilyNoMeta/rtx-encore/issues/72)). Using OptiScaler at the same time is not supported in this game.

- Removed the optional Cyber Engine Tweaks panel. Use the built-in menu (Insert) or the Hub. Existing panel settings are copied beside the mod on first launch when no current file exists; the original is kept.

- Removed the experimental OpenNR engine and its settings. NVIDIA Neural Rendering remains available. Older OpenNR configurations upgrade with NR off; enable it again to use NVIDIA NR.

- Smooth Motion on RTX 20 with NVIDIA 621.01 and DirectX 12. Enable Smooth Motion;
  RTX Encore selects the card automatically. Choose **V1 (test 1, default)** or
  **V2 (test 2)** in the Hub or the RTX 20 in-game menu, then restart the game.
  Thanks to [@aziznasrimay45-sys](https://github.com/aziznasrimay45-sys) for RTX 2070
  testing in five games and the V1 preference in Resident Evil Requiem, and
  [@MxHassan195](https://github.com/MxHassan195) for testing both on RTX 2060 in
  The Witcher 3 and preferring V2's smoothness
  ([#53](https://github.com/SilyNoMeta/rtx-encore/issues/53)). Performance depends on
  the card and game; these reports concern experimental builds, not the final DLL.

Foveated NR was measured in Cyberpunk 2077 and the Baldur's Gate 3 fix was checked in that game, both on an RTX 3070 Ti Laptop. Smooth Motion on RTX 20 and Intel XeSS / XeFG remain experimental. RTX Encore installed alone through the Hub is the supported setup; combinations with OptiScaler are not supported.

## 1.2.2

- **Sharpening after reconstruction.** Choose Off, Game or Custom for DLSS, DLAA, Ray Reconstruction and replacement XeSS. Off by default; Custom defaults to 30% and changes apply live. High strengths can create halos and amplify noise. Original DLSS older than 2.5.1 keeps its own sharpening. Thanks to [@aziznasrimay45-sys](https://github.com/aziznasrimay45-sys) for the request ([#70](https://github.com/SilyNoMeta/rtx-encore/issues/70)).
- **Smooth Motion starts correctly in SurrounDead (DirectX 12).** The maintainer confirmed it working in game on an RTX 3070 Ti Laptop with NVIDIA driver 621.01.
- **Baldur's Gate 3 (Vulkan):** fixes a case where the integrated menu fell back to a separate window with bg3fgvk and OptiScaler F5. Thanks to [@deusimperatork](https://github.com/deusimperatork) for the report ([#72](https://github.com/SilyNoMeta/rtx-encore/issues/72)).
- **DLSS Frame Generation compatibility with OptiScaler F5:** fixes a case where Frame Generation became unavailable on RTX 20/30, including Final Fantasy VII Rebirth. Thanks to [@lovebabyx](https://github.com/lovebabyx) for the report ([#73](https://github.com/SilyNoMeta/rtx-encore/issues/73)).

Sharpening has local automated checks; Vulkan operation, real-game appearance and performance remain unvalidated. The fixes for #72 and #73 have not been confirmed by their reporters on this release. Intel XeSS and XeFG remain experimental. [Sharpening settings](docs/SUPER-RESOLUTION.md).

## 1.2.1

- Switch between DLSS and XeSS without restarting in No Man's Sky, with compatible Intel runtimes installed before launch.

- **Elden Ring with ERSS-FG:** improved startup and menu compatibility. Thanks to
  [@mennogreg](https://github.com/mennogreg) for the [report](https://github.com/SilyNoMeta/rtx-encore/issues/64).
- **Menu compatibility** with OptiScaler in Quarantine Zone and ReShade in HumanitZ. Thanks to
  [@PhantomGamers](https://github.com/PhantomGamers) ([#54](https://github.com/SilyNoMeta/rtx-encore/issues/54)) and
  [@sweddd](https://github.com/sweddd) ([#68](https://github.com/SilyNoMeta/rtx-encore/issues/68)) for their reports.
- **Neural Rendering** keeps the configured passes when the render resolution changes. Thanks to
  [@amernet91](https://github.com/amernet91) for the [report](https://github.com/SilyNoMeta/rtx-encore/issues/65).
- Updated Intel runtime version compatibility.
- **NVIDIA runtime compatibility** no longer requires a file to appear in a fixed list. An old
  `providerPatchPolicy` entry is ignored. Intel runtime checks remain unchanged.

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
  (and recording on the game's own page if you turned it on there), then restart the game. Other recording tools
  and overlays can have the same effect.

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
