<div align="center">

<h1>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/wordmark-dark-theme.png">
  <img src="assets/wordmark-light-theme.png" alt="RTX Encore" width="440">
</picture>
</h1>

**Frame generation up to X6, Smooth Motion and Neural Rendering\
for GeForce RTX 20, RTX 30 and RTX 40 — now in games that ship no DLSS too.**

A built-in menu, with RTX Encore Hub to install the mod and its required runtimes.

[![Latest release](https://img.shields.io/github/v/release/SilyNoMeta/rtx-encore?include_prereleases&style=for-the-badge&label=Latest&color=76b900)](https://github.com/SilyNoMeta/rtx-encore/releases)
[![Downloads](https://img.shields.io/github/downloads/SilyNoMeta/rtx-encore/total?style=for-the-badge&color=2f81f7)](https://github.com/SilyNoMeta/rtx-encore/releases)
[![Sponsor](https://img.shields.io/badge/Sponsor-%E2%9D%A4-db61a2?style=for-the-badge&logo=githubsponsors&logoColor=white)](https://github.com/sponsors/SilyNoMeta)
[![Ko-fi](https://img.shields.io/badge/Ko--fi-Support-ff5e5b?style=for-the-badge&logo=kofi&logoColor=white)](https://ko-fi.com/silynometa)
[![Join the Discord](https://img.shields.io/badge/Join_Discord-454FC4?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/Zwmq4Xc2Br)

[![GPUs](https://img.shields.io/badge/GPUs-RTX%2020%20%7C%2030%20%7C%2040-76b900?style=flat-square&logo=nvidia&logoColor=white)](docs/COMPATIBILITY.md)
[![Graphics APIs](https://img.shields.io/badge/APIs-DirectX%2011%20%7C%2012%20%7C%20Vulkan-0078d4?style=flat-square)](docs/COMPATIBILITY.md)

**[Download](https://github.com/SilyNoMeta/rtx-encore/releases)** ·
**[Nexus Mods](https://www.nexusmods.com/site/mods/2547)** ·
**[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)** ·
**[Installation](docs/INSTALLATION.md)** ·
**[Français](README.fr.md)** ·
**[中文](README.zh-CN.md)**

<sub>Free, made on personal time. If it is useful to you, you can support the project on <a href="https://github.com/sponsors/SilyNoMeta">GitHub Sponsors</a> or <a href="https://ko-fi.com/silynometa">Ko-fi</a>, or endorse it on <a href="https://www.nexusmods.com/site/mods/2547">Nexus Mods</a> ❤</sub>

<p>❤️ Thank you to <a href="https://github.com/mennogreg">@mennogreg</a>, <a href="https://github.com/Scarhead666">@Scarhead666</a> and <a href="https://github.com/MrMicko">@MrMicko</a> for supporting the development of RTX Encore.</p>

</div>

## RTX Encore 2.4.0

RTX Encore 2.4.0 adds NR placement per pass, spatial anti-aliasing across DirectX and Vulkan with HDR, and DLSS custom scales down to 33%. Hub 2.4.0 lets you choose a mod version and roll back.

[2.4.0](CHANGELOG.md#240)


**RTX Encore now has a Discord.** Share your 2.0.0 results, compare settings, or get help with the mod and Hub. [Join the Discord](https://discord.gg/Zwmq4Xc2Br).

> [!IMPORTANT]
> RTX Encore is an independent, experimental mod. It is not affiliated with or endorsed by NVIDIA.
> It loads inside the game: do not use it in online or anti-cheat protected games, where it can be blocked or get an
> account flagged. Back up every file you replace.

**[Latest: 2.2.0](https://github.com/SilyNoMeta/rtx-encore/releases/tag/v2.2.0)** · [Release notes](CHANGELOG.md) · [Hub 2.2.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.2.0)

**New in 2.2.0:** NVIDIA Image Scaling (NIS) for games without DLSS, optional FXAA/SMAA before or after NIS, customizable overlay styles and metrics, experimental XeFG Tensor Cores on RTX 20, and updated XeSS Tensor Cores paths for RTX 30/40. [All changes and limits](CHANGELOG.md#220).

**Also new in 2.1.0:** steady game pacing makes XeFG X3–X6 smoother; added frame generation can use a DLSS game's own depth and motion in DirectX 11 and Vulkan; XeFG Tensor Cores reach those APIs too. The release also brought fixes for Baldur's Gate 3, Final Fantasy VII Rebirth, Forza Horizon 6, Kingdom Come: Deliverance, Islands of Insight and Black Flag Resynced. [2.1.0 details](CHANGELOG.md#210).


## 2.0.0: DLSS, XeSS, Neural Rendering and frame generation for games without DLSS

Many games ship no DLSS at all. RTX Encore 2.0.0 brings them **NVIDIA DLSS Super Resolution or DLAA, Intel XeSS, Neural Rendering and frame generation**, from the image they display, in **DirectX 11, DirectX 12 and Vulkan**, without ReShade or any other tool. The game needs nothing: no DLSS, no motion vectors, no frame generation of its own.

| | |
| :--- | :--- |
| 🔍 **Upscaling** | DLAA at the game's resolution, or a lower game resolution upscaled to the screen's, with **DLSS** or **XeSS** (XeSS on NVIDIA Tensor Cores in DirectX 12). |
| ✨ **Neural Rendering** | NVIDIA's NR on the displayed image, with every RTX Encore NR control. |
| 🎞️ **Frame generation** | **DLSS Frame Generation** X2 to X6 in DirectX 11, DirectX 12 and Vulkan; **Intel XeFG** up to X6 in DirectX 12 (Tensor Cores), Vulkan (HDR10 included) and DirectX 11. Change the multiplier while playing. |
| 🧭 **Motion estimation** | RTX Encore's own estimation, on the graphics card, or vort_Motion's shader method, or NVIDIA Optical Flow. |

Press **Insert**, then choose the **Engine** in Upscaling and Frame generation: **None**, **DLSS / XeSS**, or **DLSS-FG / XeFG** as appropriate. Native integrations keep **Game**. [Setup, requirements and limits](docs/GAMES-WITHOUT-DLSS.md).

| 🎞️ **[Frame generation](docs/FRAME-GENERATION.md)** | 🌀 **[Smooth Motion](docs/SMOOTH-MOTION.md)** | ✨ **[Neural Rendering](docs/NEURAL-RENDERING.md)** | 🎮 **[Games without DLSS](docs/GAMES-WITHOUT-DLSS.md)** |
| :--- | :--- | :--- | :--- |
| Choose the multiplier in compatible games. | Interpolation for games without native FG. | Tune the image with independent NR controls. | DLSS, XeSS, NR and frame generation where the game ships none. |

## Start playing

Install with **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)**, choose your game, then press **Insert** in game. Prefer a manual install? [Follow the four steps below](#quick-start).

<details>
<summary>Experimental engines and support</summary>

XeSS, XeFG and the native XeFG → DLSS FG adapter are experimental and require compatible inputs and runtimes. In Mirage, select DLSS-FG in Encore, restart, then enable Intel FG in the game. FSR stays native. [Engine guide](docs/INTEL-ENGINES.md). For a problem, use the Hub’s Health page to prepare a diagnostic, review it, then attach it to an [issue](https://github.com/SilyNoMeta/rtx-encore/issues/new/choose).

</details>

## What it does

| | Feature | In short |
| :---: | :--- | :--- |
| 🧪 | **[Intel engines (experimental)](docs/INTEL-ENGINES.md)** | XeSS reconstruction or XeFG frame generation with XeLL, where compatible game inputs and runtimes allow them. Off by default. |
| 🔆 | **[Live sharpening](docs/SUPER-RESOLUTION.md)** | Off / Game / Custom, after reconstruction and NR. Custom 0–100%, default 30%; appearance and Vulkan operation remain unvalidated. |
| 🎞️ | **[Frame generation X2 to X6](docs/FRAME-GENERATION.md)** | DLSS Multi Frame Generation on RTX 20, 30 and 40, in games with compatible DLSS or native Intel frame generation. Fixed multiplier, Dynamic mode with a target frame rate, or the game's own choice. |
| 🛡️ | **Cleaner generated frames** | Protection against tearing on thin geometry (fences, wires, foliage) and against artifacts in moving shadows; sharp HUD and text. |
| ⚡ | **Lighter frame generation** | A performance option that lowers the GPU cost of each generated frame without changing the image. Pushed even further in 1.6.0: on an RTX 4090, a generated frame costs 34 % less GPU time than with the option off at 1080p X2, 16 % at 1080p X6. |
| 🌀 | **[Smooth Motion on RTX 20 / 30](docs/SMOOTH-MOTION.md)** | NVIDIA's driver frame interpolation, including games without DLSS: DirectX 11, DirectX 12 and Vulkan on RTX 30; **experimental RTX 20 support** in DirectX 12 with driver 621.01 and V1 / V2 variants. Off by default. |
| ✨ | **[Neural Rendering](docs/NEURAL-RENDERING.md)** | NR on RTX 20, 30 and 40, with RTX Encore's own performance options for Ampere: optimized processing, Fast precision, independent NR resolution. One to four passes, a style for each, HDR support. Experimental, off by default. |
| 🎮 | **[Games without DLSS](docs/GAMES-WITHOUT-DLSS.md)** | DLSS or XeSS upscaling (DLAA or to the screen's resolution), Neural Rendering and frame generation (DLSS Frame Generation or Intel XeFG, X2 to X6) in DirectX 11, DirectX 12 and Vulkan games that ship no DLSS, from the image they display, with RTX Encore's own motion estimation. New in 2.0.0. Experimental, off by default. |
| ⚡ | **[XeSS and XeFG on NVIDIA Tensor Cores](docs/INTEL-ENGINES.md)** | Independent experimental XeSS reconstruction and XeFG frame generation options. DirectX 12, with additional DirectX 11/Vulkan routes since 2.1.0; XeFG also admits RTX 20 from 2.2.0. Off by default; an active route shows a **Tensor Cores** badge. Availability and game validation vary by route and GPU. |
| 🎛️ | **[Built-in menu and overlay](docs/MENU-AND-OVERLAY.md)** | Press **Insert** in the game: every setting, the state of each feature, and an overlay with frame rate, multiplier, frame pacing, GPU and VRAM. DirectX 11, DirectX 12 and Vulkan. In eleven languages since 2.0.0. |
| 🚀 | **[Fast super resolution on RTX 30](docs/SUPER-RESOLUTION.md)** | A faster DLSS Super Resolution for RTX 30 in DirectX 12 games with preset M or L: about 16 to 20 % less GPU time per upscaled frame on the system measured, with an image very close to the original. Off by default. |
| 🔍 | **DLSS render resolution** | Choose DLAA, Ultra (75%), Quality, Balanced, Performance, Ultra Performance or a custom scale in games that already use DLSS. |
| 🧩 | **One universal file** | `rtx-encore.dll` takes the name the game loads (`version.dll`, `dinput8.dll`, `winmm.dll`, `dxgi.dll` and fifteen others) or runs as an `.asi` plugin. |

## Neural Rendering built for Ampere

RTX Encore offers optimized NVIDIA NR processing, optional Fast precision and an NR resolution independent of DLSS. One to four passes and per-pass appearance controls remain available. [See the controls and measurements](docs/NEURAL-RENDERING.md).

## Supported hardware

| GPU | Frame generation | Smooth Motion | Neural Rendering |
| :--- | :---: | :---: | :---: |
| RTX 40 | ✅ | provided by NVIDIA | ✅ |
| RTX 30 | ✅ | ✅ | ✅ |
| RTX 20 | 🧪 experimental | 🧪 experimental (DirectX 12, driver 621.01) | 🧪 experimental |

RTX Encore is made for RTX 20, 30 and 40; RTX 50 cards get frame generation and Smooth Motion from NVIDIA.
Details, graphics APIs, driver and game notes: **[Compatibility](docs/COMPATIBILITY.md)**.

## Install and update with RTX Encore Hub

**[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)** is the Windows companion for RTX Encore: the simplest way to install the mod, and to
keep it and the NVIDIA files it works with up to date.

| | With the Hub |
| :---: | :--- |
| 🎮 | **Your games in one place.** The games of Steam, Epic and GOG are found by themselves, and each game's page says what RTX Encore can do there on your card and driver. |
| 📦 | **The right NVIDIA files, taken for you.** The latest DLSS and Streamline versions this RTX Encore supports, with the Streamline plugins kept as a matched set, and NVIDIA's Neural Rendering file downloaded and checked. Many reported problems come from outdated DLSS or Streamline files. |
| 🔄 | **Updates in one click.** A new RTX Encore is announced with its notes, **Update all** updates every game, and the Hub updates itself. |
| ↩️ | **A way back.** Every replaced file is backed up first, and **Restore game** puts the game back as it was. |
| 🎚️ | **Settings explained.** A form for the settings file, where what has no effect on your card is greyed out. |
| 🩺 | **A first check.** **Launch and verify** watches the game's first minute and says whether the mod loaded. |
| 👥 | **Community profiles.** Settings per game shared by other players, ranked for your machine, to import with a way back, or to share your own. New since Hub 1.0.0-rc.2, and experimental. |

Download the stable Hub from **[its releases](https://github.com/SilyNoMeta/rtx-encore-hub/releases)**, as an installer or portable ZIP. Manual installation, below, remains available.

## Quick start

The simplest way is **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)**: install it, open the game's page and press **Install**. By hand:

1. Download the **[latest release](https://github.com/SilyNoMeta/rtx-encore/releases)** and extract it.
2. Copy `rtx-encore.dll` next to the game's executable and rename it `version.dll`.
3. Start the game. The menu opens once on first launch; press **Insert** to bring it back.
4. Turn DLSS Frame Generation on in the game's settings. If it was already on, turn it off and on again. In a game
   that ships no DLSS, turn on what you want in the menu instead: see **[Games without DLSS](docs/GAMES-WITHOUT-DLSS.md)**.

Other file names, the `.asi` plugin, updating and removing: **[Installation](docs/INSTALLATION.md)**.

> [!TIP]
> Start with the defaults: the game, or NVIDIA Profile Inspector, chooses the multiplier. On an 8 GB card, X2 or X3
> (or X4 with High or Medium textures) is the safer starting point.

### Runtimes for a manual install

RTX Encore is made and tested with these exact runtimes, in every game; the Hub puts them in place. By hand, take these files, then restart the game:

| For | Files | Download | Put them |
|---|---|---|---|
| DLSS Frame Generation 310.9.16 and Streamline 2.14.16 | `nvngx_dlssg.dll` and the `sl.*.dll` files, from `bin/x64` (not `development`) | [NVIDIA Streamline SDK 2.14.16](https://github.com/NVIDIA-RTX/Streamline/releases/download/v2.14.16/streamline-sdk-v2.14.16.zip) (the whole `bin/x64` folder works) | next to the game's executable, or in place of the game's own copies where it keeps them |
| DLSS / DLAA and Ray Reconstruction 310.9.16 | `nvngx_dlss.dll`, `nvngx_dlssd.dll` | [NVIDIA DLSS SDK 310.9.16](https://github.com/NVIDIA/DLSS/tree/v310.9.16/lib/Windows_x86_64/rel) | next to the game's executable, or in place of the game's own copies where it keeps them |
| Neural Rendering 310.9.16 | `nvngx_dlssnr.dll` | [nvngx_dlssnr-310.9.16.zip](https://encore-dl.nometa.fr/nvngx_dlssnr-310.9.16.zip) | next to RTX Encore's DLL |
| XeSS-SR 2.0.2.68, XeFG 1.3.1.78, XeLL 1.3.2.10 | `libxess.dll`, `libxess_fg.dll`, `libxell.dll`, from `bin` | [Intel XeSS SDK 3.0.2](https://github.com/intel/xess/releases/download/v3.0.2/XeSS_SDK_3.0.2.zip) | next to RTX Encore's DLL, or in `rtx-encore-runtimes\intel` beside it |

In games without DLSS or without frame generation, only these exact files are used, and the menu's **Engine** setting lists what it found. In an Unreal Engine game, the game's own copies are often in a `Plugins\...\ThirdParty\Win64` folder rather than next to the executable.

## Controls

| Shortcut | Action |
| :--- | :--- |
| `Insert` | Open or close the menu |
| `Ctrl + Alt + 2…6` (also numpad) | Fixed multiplier X2 to X6 |
| `Ctrl + Alt + PageUp / PageDown` | Raise or lower the multiplier |
| `Ctrl + Alt + D` | Switch between Fixed and Dynamic mode |
| `Ctrl + Alt + Up / Down` or `Ctrl + Alt + Plus / Minus` (also numpad `+` / `−`) | Raise or lower the Dynamic target by 5 FPS (hold `Shift` as well: 1 FPS) |
| `Ctrl + Alt + G` | Give the choice back to the game |
| `Ctrl + Alt + O` | Show or hide the overlay |
| `Ctrl + Alt + P` | Move the overlay to the next corner |
| `Ctrl + Alt + N` | Turn Neural Rendering on or off |
| `Ctrl + Alt + F9` | Record an SR capture burst (DirectX 12; enable SR capture in the menu first) |

These are the default shortcuts; each can be changed in the menu. They only act while the game window has focus.

## Documentation

| Guide | English | Français | 中文 |
| :--- | :---: | :---: | :---: |
| Installation, updating, removing | [Open](docs/INSTALLATION.md) | [Ouvrir](docs/INSTALLATION.fr.md) | [打开](docs/INSTALLATION.zh-CN.md) |
| Frame generation | [Open](docs/FRAME-GENERATION.md) | [Ouvrir](docs/FRAME-GENERATION.fr.md) | [打开](docs/FRAME-GENERATION.zh-CN.md) |
| Super Resolution | [Open](docs/SUPER-RESOLUTION.md) | [Ouvrir](docs/SUPER-RESOLUTION.fr.md) | [打开](docs/SUPER-RESOLUTION.zh-CN.md) |
| Smooth Motion on RTX 20 / 30 | [Open](docs/SMOOTH-MOTION.md) | [Ouvrir](docs/SMOOTH-MOTION.fr.md) | [打开](docs/SMOOTH-MOTION.zh-CN.md) |
| Neural Rendering | [Open](docs/NEURAL-RENDERING.md) | [Ouvrir](docs/NEURAL-RENDERING.fr.md) | [打开](docs/NEURAL-RENDERING.zh-CN.md) |
| Games without DLSS | [Open](docs/GAMES-WITHOUT-DLSS.md) | [Ouvrir](docs/GAMES-WITHOUT-DLSS.fr.md) | [打开](docs/GAMES-WITHOUT-DLSS.zh-CN.md) |
| Intel XeSS and XeFG | [Open](docs/INTEL-ENGINES.md) | [Ouvrir](docs/INTEL-ENGINES.fr.md) | [打开](docs/INTEL-ENGINES.zh-CN.md) |
| Menu and overlay | [Open](docs/MENU-AND-OVERLAY.md) | [Ouvrir](docs/MENU-AND-OVERLAY.fr.md) | [打开](docs/MENU-AND-OVERLAY.zh-CN.md) |
| Settings file | [Open](docs/SETTINGS.md) | [Ouvrir](docs/SETTINGS.fr.md) | [打开](docs/SETTINGS.zh-CN.md) |
| Compatibility | [Open](docs/COMPATIBILITY.md) | [Ouvrir](docs/COMPATIBILITY.fr.md) | [打开](docs/COMPATIBILITY.zh-CN.md) |
| Troubleshooting | [Open](docs/TROUBLESHOOTING.md) | [Ouvrir](docs/TROUBLESHOOTING.fr.md) | [打开](docs/TROUBLESHOOTING.zh-CN.md) |
| Changelog | [Open](CHANGELOG.md) | [Ouvrir](CHANGELOG.fr.md) | [打开](CHANGELOG.zh-CN.md) |

## Releases

RTX Encore is distributed as ready-to-use releases on this page; its source code is not published. GitHub shows the
SHA-256 of each downloadable archive: compare it with the one in the release notes before installing.

Version numbers start at 1.0.0. The releases published earlier under the name DLSSG-Transfusion, up to
`v1.4.5.3-rtx20-30-40`, are no longer distributed;
[upgrading from them](docs/INSTALLATION.md#upgrading-from-an-earlier-name) keeps your settings.

## Reporting a problem

Open an [issue](https://github.com/SilyNoMeta/rtx-encore/issues) with the game and its graphics API, your GPU and
driver version, and the report the menu copies for you (**System** tab, **Copy**).
[Troubleshooting](docs/TROUBLESHOOTING.md) lists the first things to check.

## Credits

RTX Encore began as a continuation of [DLSSG-Transfusion](https://github.com/TonyJoaca/DLSSG-Transfusion) by
TonyJoaca and has since grown into a project of its own. It also owes much to the work of:

- [dashdogy/RTX40MFG-Unlock](https://github.com/dashdogy/RTX40MFG-Unlock): frame-generation research, menu rendering and input handling;
- [mavismmg/MFGAdaUnlock-RenoDx](https://github.com/mavismmg/MFGAdaUnlock-RenoDx): frame-generation quality research;
- [sdli1995/dlssg_for_sm86](https://github.com/sdli1995/dlssg_for_sm86): frame-generation research on RTX 20 and RTX 30;
- [Intel XeSS](https://github.com/intel/xess): XeSS, XeFG and XeLL SDK interfaces;
- [maximedgoulet/FoveatedDLSS5](https://github.com/maximedgoulet/FoveatedDLSS5): the foveated Neural Rendering idea;
- [janblade/OptiScaler F5](https://github.com/janblade/OptiScaler-F5-DLSSNR-Multipass) inspired NR looks and automatic exposure;
- [Skynizz/optiscaler-dlss5](https://github.com/Skynizz/optiscaler-dlss5) inspired multipass NR profiles;
- [jlrouzies-fr/DLSS5-Feeder](https://github.com/jlrouzies-fr/DLSS5-Feeder): Neural Rendering fed from the displayed image, and its motion validation;
- [vortigern11/vort_Shaders](https://github.com/vortigern11/vort_Shaders) (vort_Motion): the shader motion estimation for games without DLSS;
- [crosire/reshade](https://github.com/crosire/reshade) (Generic Depth add-on): how to find the depth buffer a game draws with;
- [Dear ImGui](https://github.com/ocornut/imgui) by Omar Cornut: the toolkit the in-game menu and overlay are built with;
- [SMAA](https://github.com/iryoku/smaa): Jorge Jimenez, Jose I. Echevarria, Belen Masia, Fernando Navarro and Diego Gutierrez; [FXAA 3.11](https://github.com/GameTechDev/CMAA2): NVIDIA, Timothy Lottes;
- [Noto Sans SC](https://github.com/google/fonts/tree/main/ofl/notosanssc): menu typeface;
- everyone who tests a build and reports a bug: several fixes exist only because of those reports.

The components included in the releases and their licenses are listed in
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).

## Disclaimer

RTX Encore is provided as is, without warranty: use it at your own risk. Image quality, stability and performance
vary with the game, its version, the driver and the hardware. High multipliers raise latency and VRAM use.

The components included in the releases keep their own licenses: see
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md), which must stay with any copy of a release. NVIDIA, GeForce, RTX
and DLSS are trademarks of NVIDIA Corporation. NVIDIA software and games remain subject to their own terms; none is
included in the releases.
