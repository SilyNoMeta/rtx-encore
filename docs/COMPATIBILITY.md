# Compatibility

*[Français](COMPATIBILITY.fr.md) · [中文](COMPATIBILITY.zh-CN.md)*

These notes record what was tested and observed. They are not a certification: a game update, another driver or
another mod can change the result.

## Graphics cards

| GPU | Frame generation | Smooth Motion | Neural Rendering |
| :--- | :--- | :--- | :--- |
| RTX 40 | Supported. DirectX 12 and Vulkan exercised on an RTX 4090. | Provided by NVIDIA | Supported |
| RTX 30 | Supported. DirectX 12 exercised on an RTX 3070 Ti Laptop. | Supported with the drivers listed below | Supported |
| RTX 20 | Experimental: not validated on a physical RTX 20 card yet. | Not supported | Experimental |

RTX Encore is made for RTX 20, 30 and 40 cards. GTX cards and cards from other vendors are not supported.

## Graphics APIs

| Feature | DirectX 11 | DirectX 12 | Vulkan |
| :--- | :---: | :---: | :---: |
| Frame generation X2 to X6 | — | ✅ | ✅ (experimental on RTX 20 and RTX 30) |
| UI assist | — | ✅ | — |
| Smooth Motion (RTX 30) | ✅ | ✅ | ✅ |
| Neural Rendering | 🧪 | 🧪 | 🧪 |
| Menu and overlay | ✅ | ✅ | ✅ |

DirectX 9 and OpenGL games are not supported.

## DLSS Frame Generation versions

Frame generation works with these versions of the game's `nvngx_dlssg.dll`:

`310.1.0` · `310.2.0` · `310.2.1` · `310.3.0` · `310.4.0` · `310.5.0` · `310.5.2` · `310.5.3` · `310.6.0` ·
`310.7.0` · `310.7.128` · `310.7.129` · `310.8.0` · `310.9.0` · `310.9.1` · `310.9.16`

- Any other version is left untouched: the game keeps its own frame generation.
- **310.9.0 and 310.9.1** retain their quality and Fast frame generation support. RTX 20 users should use a
  qualified 310.9.0, 310.9.1 or 310.9.16 file.
- **310.9.16** is the file the NVIDIA app beta downloads; it is the only one with frame generation preset C.
  **A/B use Normal; selecting C automatically enables Lite after restarting the game.** Fast frame generation
  stays independent for every preset. Normal supports the anti-tearing policies. Lite keeps NVIDIA's native image policy:
  Anti-tearing and Protection tuning have no effect, UI recomposition is unavailable, and Fast frame generation uses only optimizations relevant
  to Lite. These additions were checked locally and are not yet confirmed in game; no Lite performance gain is measured.
- The other DLSS files of the game (Super Resolution, Ray Reconstruction, ...) can have any version.

## Streamline versions

Streamline support depends on the feature. The available Windows x64 SDKs from 1.0.0 to 2.14.1 were
examined offline, including production and development files where published. This does not establish
in-game compatibility for every SDK/game/driver combination.

| Feature | Examined coverage and limits |
| :--- | :--- |
| Streamline controls | Require Streamline 2.x. The Streamline 1.x interface is not supported. |
| Fixed MFG above X2 | Recognized in the published 2.7.2, 2.7.30, 2.7.32, 2.8.0, 2.9.0, 2.10.0, 2.10.3, 2.11.1, 2.12.0 and 2.14.1 SDK files, and in the 2.14.16 files the NVIDIA app beta downloads. Earlier SDKs are not qualified for these multipliers. A supported DLSS Frame Generation file is also required. |
| Native Dynamic MFG extension | Official production Streamline 2.11.1, 2.12.0, 2.14.1 and 2.14.16 files qualified offline. Pacing differs by version; per-game confirmation remains necessary. Other builds retain their native capabilities. |
| Encore adaptive MFG (DX12) | Official production and development files: 2.7.2, 2.7.30, 2.7.32, 2.8.0, 2.9.0, 2.10.0, 2.10.3. X2–X4 only, including when experimental X5/X6 is enabled. Requires a compatible game integration; older integrations may remain unsupported. Offline qualified, not yet confirmed in game. |
| UI recomposition override | Production 2.11.1, 2.12.0, 2.14.1 and 2.14.16 are qualified locally. Explicit on/off requests remain authoritative; an unspecified request retains the existing choice. Still needs usable UI inputs from the game and in-game validation for 2.14.16. Other builds retain their own UI handling. This is separate from UI assist. |
| Reflex frame limit | DLL versions 2.1.1–2.14.1, and 2.14.16; see [Menu and overlay](MENU-AND-OVERLAY.md#suggested-reflex-cap). |

There is no public SDK tag for 2.13 or 2.14.0 in this review. Intermediate version numbers alone do not
establish support for every feature. Keep the game's Streamline files together; these notes are not a
recommendation to replace individual files.

**Check availability per feature in the menu.** The Streamline 2.14.16 and DLSS-G 310.9.16 additions await in-game validation. Older versions expose fewer features; a newer, unreviewed file is not automatically supported. Keep a matched Streamline set compatible with the game. SR/RR preset availability still depends on their own DLLs and the driver; Neural Rendering still specifically requires 310.8.0.

**DirectX 11:** DLSS MFG and its Dynamic mode are unavailable. The separate Smooth Motion path provides X2 only; it cannot switch to X3/X4. Its GPU and driver requirements still apply.

## Smooth Motion drivers

Smooth Motion on RTX 30 accepts NVIDIA drivers **620.12**, **617.42**, **617.14**, **616.92** and
**616.64**, and no other.
Only **617.14** has been verified in a game. See [Smooth Motion](SMOOTH-MOTION.md).

## Neural Rendering file

Neural Rendering needs NVIDIA's `nvngx_dlssnr.dll` **310.8.0**, supplied by you or fetched by [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub). See
[Neural Rendering](NEURAL-RENDERING.md).

## Game notes

| Game | API | Notes |
| :--- | :---: | :--- |
| Cyberpunk 2077 | DX12 | Frame generation tested on an RTX 3070 Ti Laptop. Works as `version.dll` or as an `.asi` plugin; see [Installation](INSTALLATION.md#cyberpunk-2077). |
| Black Myth: Wukong | DX12 | Frame generation tested on an RTX 4090. Neural Rendering confirmed by users. |
| No Man's Sky | Vulkan | Frame generation tested on an RTX 4090. **Turn Automatic UI recomposition off**: it causes doubled outlines in camera motion in this game. Neural Rendering with frame generation X2 confirmed at 1080p and 1440p. |
| DOOM: The Dark Ages | Vulkan | Frame generation tested on an RTX 4090. The game keeps its own choice for the interface handling, whatever the setting. |
| Starfield | DX12 | Works with UI assist on; a crash when loading a save was fixed after a user's report. |
| Star Wars Outlaws | DX12 | The full game (Ubisoft Connect) closes at launch when the RTX Encore menu is attached. **Set `"disableInGameMenu": true`** in `rtx-encore.jsonc`, or apply the game's community profile in [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub). Without the menu, frame generation up to X4 starts, as reported by two users; change settings in the file or with the Hub. The demo runs with the menu. |
| The Witcher 3 | DX12 | Compatible ray-traced hair on game builds 25575366 and 25646871; see [Menu and overlay](MENU-AND-OVERLAY.md#other-controls). |
| Manor Lords | DX12 | Smooth Motion observed working on RTX 30 (driver 617.14). |
| Shadows of Doubt | DX11 | Smooth Motion observed working on RTX 30 (driver 617.14). Neural Rendering confirmed by users. |
| Enshrouded | Vulkan | Smooth Motion observed working on RTX 30 (driver 617.14). Neural Rendering confirmed by users. |
| Bodycam, Palworld, Portal with RTX, Star Wars Zero Company | — | Neural Rendering confirmed by users. |

Tell us what you observe in other games by opening an
[issue](https://github.com/SilyNoMeta/rtx-encore/issues): game, graphics API, card, driver, and what works or not.

## Other tools

| Tool | Notes |
| :--- | :--- |
| ReShade, Special K | Leave them their `dxgi.dll` and give RTX Encore another name. ReShade is not needed for the menu. |
| DXVK | Do not name RTX Encore `dxgi.dll`: DXVK provides that file. The menu is drawn in its Vulkan output. |
| OptiScaler | A dedicated option exists for setups that need it: see [Frame generation](FRAME-GENERATION.md#compatibility-options). |
| Cyber Engine Tweaks, RED4ext | Keep their files under their own names. An optional, experimental panel exists for Cyber Engine Tweaks. |
| NVIDIA Profile Inspector | In **Game decides** mode, the multiplier it sets is followed. |

## Anti-cheat and online games

RTX Encore loads inside the game. Games protected by an anti-cheat can refuse to start, or flag the account. Do not
use it there.
