# Changelog

*[Français](CHANGELOG.fr.md) · [中文](CHANGELOG.zh-CN.md)*

## 1.0.0-beta.5 — pre-release

- Fix live UI recomposition (UIR) changes being ignored while Frame Generation is already running ([#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)). This affects enabling and disabling UIR, including **Force UIR** and game/automatic requests, with qualified production Streamline 2.11.1, 2.12.0 and 2.14.1. Turning Force UIR off still allows the game or automatic mode to request UIR.
- Keep the compatibility scope of beta.4. Prefer the latest qualified Streamline 2.14.1 and DLSS-G 310.9.1.0 for the fullest feature coverage; see [Compatibility](docs/COMPATIBILITY.md#streamline-versions).

The UIR correction passes automated checks outside the game. Confirmation of the final beta.5 build in Starfield and other games remains pending; no performance improvement is claimed.

### Thanks

- [@Avictuu](https://github.com/Avictuu) — Starfield report, component comparisons and logs that helped identify the live UIR problem ([#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)).

## 1.0.0-beta.4 — pre-release

- Extend native Dynamic MFG and UI recomposition to the qualified production Streamline 2.11.1 and 2.12.0 files, alongside 2.14.1. Add Encore adaptive X2–X4 for qualified older DX12 Streamline 2.7.2–2.10.3 files. Vulkan keeps its adaptive path; D3D11 remains Smooth Motion X2 only. See [Compatibility](docs/COMPATIBILITY.md#streamline-versions) for exact versions and limits.
- Add **System → Component compatibility**: loaded component versions, available features and explanations. Grey out unsupported choices while retaining the option to disable saved overrides. Prefer the latest qualified Streamline 2.14.1 and DLSS-G 310.9.1.0 for the fullest coverage.
- Correct an invalid memory access identified with Dynamic FG in The Witcher 3 ([#4](https://github.com/SilyNoMeta/rtx-encore/issues/4)) and a Streamline 2.12 shutdown problem in Starfield ([#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)). The Starfield exit fix was confirmed with the earlier test build; final beta.4 retesting and confirmation of the Witcher crash resolution remain pending.
- Correct Reflex plugin lifetime across Streamline restarts, addressing the investigated Cyberpunk startup problems ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)). In-game confirmation remains pending.
- Correct delayed Reflex cap updates and restoration across game threads. Add direct FPS entry, **Use game settings** and **For this display** (refresh rate minus 3 FPS). FG counting depends on the game/driver; Dynamic MFG suspends this override. Extend Reflex compatibility to qualified DLL versions 2.1.1–2.14.1; no official 2.13 SDK was available for verification.
- Add independent SR, RR and FG preset controls, applied after restarting the game. FG A/B requests support individually qualified production DLSS-G 310.6.0.0, 310.7.0.0, 310.7.128.0, 310.7.129.0, 310.8.0.0, 310.9.0.0 and 310.9.1.0. SR/RR support depends on the loaded DLL and driver overrides; the effective model is not guaranteed. Defaults preserve the game’s choices.
- Automatically record system and loaded component versions, DLSS quality/resolutions and requested SR/RR presets. Optional performance logs now include GPU temperature, power, clocks, reported limits and gaps longer than one second. These additions improve diagnostics; they do not establish fixes for [#12](https://github.com/SilyNoMeta/rtx-encore/issues/12) or [#19](https://github.com/SilyNoMeta/rtx-encore/issues/19).

Automated qualification is not a substitute for testing each game, GPU and driver combination. The new adaptive path, compatibility display and preset changes still need in-game feedback.

### Thanks

- [@Avictuu](https://github.com/Avictuu) — repeated Starfield tests, multiplier checks, detailed Reflex/exit logs and confirmation of normal shutdown in the test build ([#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)).
- [@wisecase2](https://github.com/wisecase2) — matching Witcher 3 session logs and Cyberpunk follow-up evidence ([#4](https://github.com/SilyNoMeta/rtx-encore/issues/4), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)).
- [@hunan-NRT](https://github.com/hunan-NRT) and [@naruto490610-alt](https://github.com/naruto490610-alt) — Cyberpunk reports, logs and follow-up tests ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)).
- [@yannduwooz83](https://github.com/yannduwooz83) and [@PrincessofCelery](https://github.com/PrincessofCelery) — reports that helped improve automatic support information and freeze diagnostics ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12), [#19](https://github.com/SilyNoMeta/rtx-encore/issues/19)).

Thanks to everyone who shares logs and takes time to test the builds.


## 1.0.0-beta.3 — pre-release

- Address a startup crash identified in The Last of Us when using RTX Encore with OptiScaler and the game's older Streamline runtime. In-game confirmation is pending; Dead Space remains unconfirmed.
- Detect DLSS Ray Reconstruction activity correctly. NR reports when its required depth input is unavailable instead of hiding DLSS activity; compatibility still depends on the game's inputs.
- Keep separate exception logs for each process and launch, so an error reporter cannot replace the game's diagnostic data. This improves investigation of the Cyberpunk startup report; it does not establish a fix for that crash.

- Fix a Starfield crash when enabling Frame Generation with the built-in menu present.
  The initial test build was confirmed working in-game, including FG off/on and opening the menu.
  The final variant adds lifetime and diagnostic corrections and passes automated Windows tests; it has not received a separate in-game retest.
- Bound the optional graphics trace and avoid diagnostic exceptions while recording loaded modules.

### Thanks

- [@mennogreg](https://github.com/mennogreg) for the Starfield reports, logs and repeated in-game tests ([#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)), and the OptiScaler compatibility report and logs ([#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)).
- [@hunan-NRT](https://github.com/hunan-NRT), with confirmation from @mennogreg, for reporting the Ray Reconstruction detection problem ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)).
- [@naruto490610-alt](https://github.com/naruto490610-alt) for the Cyberpunk report and logs that exposed the exception-log isolation problem ([#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)).

Thanks also to everyone sharing reports and logs to help investigate the remaining issues.

## 1.0.0-beta.2 — pre-release

First release under the name **RTX Encore**. Version numbers start again at 1.0.0: this release follows
`v1.4.5.3-rtx20-30-40`, published as DLSSG-Transfusion, and replaces it.

### New

- **Built-in menu and overlay.** Press **Insert** in the game: every setting, the state of each feature, and an
  overlay with frame rate, multiplier, frame pacing, GPU and VRAM. It works in DirectX 11, DirectX 12 and Vulkan
  games. ReShade is no longer needed, and the former ReShade add-on is no longer shipped.
- **Neural Rendering** on RTX 20, RTX 30 and RTX 40, applied to the image DLSS produces: one to four passes with a
  style for each, HDR support, and a choice of NVIDIA or experimental Open engine. Experimental, off by default;
  needs NVIDIA's `nvngx_dlssnr.dll` 310.8.0. Open does not support RTX 20.
- **Our NR performance work for Ampere (RTX 30).** Faster processing with identical output in local NVIDIA-engine
  reference tests; NVIDIA Fast precision with about 17 % less NR time measured on RTX 30; independent NR resolution,
  including Ultra Performance; and Open's Fast precision and optional speed/VRAM controls. Our **Ultra-fast
  mode** reduced NR time from **18.7 to 9.8 ms (−48 %)** and raised displayed FPS from **73.8 to
  94.9 (+29 %)** in the Wukong benchmark on an RTX 3070 Ti Laptop with FG X3, one run per configuration. One NR pass,
  possible motion artifacts; results are not guaranteed. [Measurements and tradeoffs](docs/NEURAL-RENDERING.md).
- **Smooth Motion on RTX 30** now accepts NVIDIA drivers 617.42, 616.92 and 616.64 in addition to 617.14, and is
  set from the menu. Only 617.14 has been verified in a game.
- **One universal file.** `rtx-encore.dll` now accepts nineteen file names, so it can sit beside the mods that
  already use the usual ones.
- **New controls**: a limit on rendered frames through NVIDIA Reflex, a request to present without V-Sync, and
  compatible ray-traced hair in The Witcher 3.
- **Optional panel for Cyber Engine Tweaks** in Cyberpunk 2077 (experimental).
- **Session logs** are kept in a `rtx-encore-logs` folder, the three most recent by default.

### Changed

- The files are named `rtx-encore.*`. Settings, shortcuts and an earlier plugin are taken over automatically: see
  [Upgrading from an earlier name](docs/INSTALLATION.md#upgrading-from-an-earlier-name).
- The settings file is `rtx-encore.jsonc`, organized in sections, with a comment on each key.
- Several settings and menu entries have clearer names. A settings file from an earlier version is taken over with
  its values.
- Frame generation costs 2 to 3 % less GPU time on RTX 40 cards.
- The overlay text is more compact, with a size setting.

### Known limits

- RTX 20 support is experimental and has not been validated on a physical RTX 20 card.
- Neural Rendering with several passes, the open engine, the Cyber Engine Tweaks panel and ray-traced hair on
  RTX 20 and RTX 30 have had little or no testing in games.
- X5 and X6 are experimental and need plenty of VRAM.

## Earlier releases

The releases up to `v1.4.5.3-rtx20-30-40` were published under the name DLSSG-Transfusion. They are no longer distributed.
