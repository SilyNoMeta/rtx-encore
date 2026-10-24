# Neural Rendering

*[Français](NEURAL-RENDERING.fr.md) · [中文](NEURAL-RENDERING.zh-CN.md)*

DLSS Neural Rendering (NR) enhances the image made by DLSS Super Resolution or DLAA. RTX Encore runs it on RTX 20,
RTX 30 and RTX 40 cards, in DirectX 11, DirectX 12 and Vulkan games.
Vulkan also admits RTX 50 cards experimentally. Vulkan validation on physical RTX 20, RTX 40 and RTX 50 hardware
with an official build is still pending; accepting the card does not establish image quality or performance.

> [!NOTE]
> Neural Rendering is experimental and off by default. It costs a lot of GPU time: see [Cost](#cost).

Each feature has its own job: **DLSS Super Resolution** reconstructs the game's image, **Neural Rendering** changes
how it looks, **frame generation** adds frames in between. Turning NR on does not turn frame generation on.

## What RTX Encore brings to it

| | |
| :--- | :--- |
| **Three generations of cards** | RTX 40, RTX 30 and, experimentally, RTX 20, in DirectX 11, DirectX 12 and Vulkan games. |
| **Faster processing, same image** | Optimizations enabled by default for the NVIDIA engine on RTX 20 and RTX 30, with identical output in the local reference tests. Performance measured on RTX 30; physical RTX 20 validation is still pending. Part of it is specific to 310.8.0: with 310.9.16, network A takes slightly longer than with 310.8.0. |
| **Fast precision** | NVIDIA engine: about 17 % less NR processing time measured on RTX 30, with small image differences. |
| **Independent NR resolution** | NR can work below the screen resolution while DLSS keeps its own render resolution and reconstructed detail. Performance and Ultra Performance reduce the amount of NR work without requiring a lower DLSS setting. |
| **Built to live with frame generation** | NR runs once per rendered frame, never on generated frames. |
| **Several passes, a style for each** | One to four passes, each with its own look. |
| **HDR games** | Handled, with a brightness control. |
| **Background preparation** | When NR is preparing or required game inputs are unavailable, the image stays as DLSS made it. |

Options that use more VRAM are off by default.

## Requirements

- NVIDIA's NR file, **`nvngx_dlssnr.dll` version 310.8.0 or 310.9.16**. It is not included in RTX Encore: supply it
  yourself and put it beside the mod's file, normally in the folder of the game's executable. Any other version is
  refused. [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub) downloads this file, checks it and puts it there for you.
- A game with **DLSS Super Resolution or DLAA turned on**. NR is applied to its result.
  With **Ray Reconstruction**, NR is applied to RR's result, after its denoising: never to the raw ray-traced image.
  The game must provide the depth input NR needs; otherwise the image stays as DLSS made it and the menu says so.
- With the 1.2 beta, compatible linear-depth inputs are also supported after RR on Direct3D 12. Invalid or unsupported depth inputs leave NR inactive; Direct3D 11 and Vulkan still do not support this linear-depth case. Local testing in 007 First Light with path tracing confirmed that NR runs, without measuring image quality or cost.
- An RTX 40 or RTX 30 card. RTX 20 is experimental; RTX 50 is also admitted experimentally under Vulkan.

## Turn it on

**Ctrl + Alt + N** turns NR on or off during the game, to compare the image and the frame rate with and without it.
The choice is saved like the menu's switch. The shortcut can be changed or turned off in the menu (**System** tab).

NR gets ready in the background: the first frames can stay as DLSS made them. The **Neural Rendering** chip at the
top of the menu shows whether NR is really running, and why not when it is not.

## Main settings

| Setting | Default | What it does |
| :--- | :---: | :--- |
| **Resolution** | Render | The size NR works at: the DLSS render size, the full output size, or Quality (67 %), Balanced (58 %), Performance (50 %), Ultra Performance (33 %) of the output, or a custom scale. **This is the main performance lever**: smaller is faster. |
| **Strength** | 100 % | How strong the effect is, from 0 to 200 %. |
| **Style** | Default | The look of the image: Default, Natural or Cinematic. |
| **Passes** | 1 | NR can be applied again to its own result, up to four times. Each pass costs GPU time and VRAM, and more passes do not guarantee a better image. |
| **Precision** | Exact | NVIDIA engine, RTX 20 and RTX 30: *Fast* reduces NR time, with about 17 % measured on RTX 30 and small image differences. |
| **NR model preset** | B | With `nvngx_dlssnr.dll` 310.9.16: **B** is a smaller, much faster network, **A** the larger one that 310.8.0 runs, **DLL default** lets the file choose (B). With 310.8.0, every choice runs its single network. It sits beside the SR and RR model presets (**Image** tab, Upscaling) and applies live. |

In a local test at 1080p on an RTX 3070 Ti Laptop, network B took about 15 ms of NR time and network A about
27 ms. B gives a different image from A. Support for 310.9.16 is experimental and lightly tested.

### Performance controls on RTX 30

The **Image → Neural Rendering → Performance** group exposes the work RTX Encore has done to reduce NR cost.
These options have different tradeoffs; their gains are not percentages you can add together.

| Control | Engine | Default | Benefit and tradeoff |
| :--- | :--- | :---: | :--- |
| **Precision: Fast** | NVIDIA on RTX 30 | Exact | Faster processing with a slightly different image. The 17 % measurement above applies to the NVIDIA engine. Restart to apply. |
| **Avoid NR padding** | NVIDIA | Off | Allows a nearby smaller NR size, at most 2 % less per axis. Can change the NR image; does not change the DLSS render size. Applies live. |
| **NR frames in flight** | NVIDIA, DirectX 12 with Reflex | 0 | Limits outstanding NR frames to 1 or 2; active frame generation uses at least 2. Applies live; its in-game performance effect remains unverified. |

### Passes and styles

With more than one pass, each pass can have its own style: **Global** (follows the **Style** setting), **Default**,
**Natural** or **Cinematic**. For example, Style Cinematic with pass one set to Natural and pass two to Global gives
Natural, then Cinematic. **Tone after pass one** (default 0) avoids applying the tone correction again at each pass.

Start with one pass and keep it as your reference when comparing. Several passes have not been validated in games
yet.

### Fine tuning

Local tone, local structure, a character mask with its own skin structure setting, and the brightness given to NR
in HDR games are in the groups that open under the main settings. Each has a description in the menu.

## Engines

OpenNR was removed in 1.3.0. Neural Rendering uses NVIDIA's runtime. Upgrading an OpenNR configuration turns NR off; turn it on again to use NVIDIA NR.

## Cost

NR adds GPU work to every rendered frame. Measured in the built-in benchmark of Black Myth: Wukong on an RTX 3070 Ti
Laptop (2560×1440, DLSS at 58 %, frame generation X3):

| Configuration | Average FPS | 5 % low |
| :--- | ---: | ---: |
| NR off | 146 | 127 |
| NR on, Precision Exact | 76 | 71 |
| NR on, Precision Fast | 82 | 76 |
| NR on, Precision Fast, Resolution Performance | 90 | 83 |

These are **displayed FPS with frame generation X3**, using the NVIDIA NR engine. In this test, going from the
default to Precision Fast and Resolution Performance raises the displayed frame rate by 18 % (76 to 90 FPS).
This does not measure a 17 % increase in game FPS: that figure concerns NR processing time. With frame generation,
NR runs on the frames the game renders, never on the generated ones.

## When something does not work

| Symptom | First check |
| :--- | :--- |
| The switch cannot be turned on | `nvngx_dlssnr.dll` is missing beside the mod's file, or is neither version 310.8.0 nor 310.9.16. |
| The chip says it is waiting for DLSS | Turn DLSS or DLAA on in the game and load a 3D scene. |
| The first frames have no NR | NR is getting ready in the background; this is expected. |
| The image is degraded, blurry or washed out | Check that there is only one copy of RTX Encore in the game: two copies under two names spoil the image. |
| The image stays as DLSS made it | Read the reason shown in the menu: when the game does not give NR what it needs, the image is left untouched. |
| Settings revert at once | The settings file could not be saved: check that it is not read-only or locked by an editor. |
| The frame rate drops | NR is costly. Lower **Resolution**, and compare in the same scene. |

When you report a problem, include the game and its graphics API, the engine chosen, and the menu's report
(**System** tab, **Copy**).

## Per-pass controls (since 1.2.0)

Each of up to four NR passes can override intensity, local tone, local structure, skin structure and the character mask, or inherit the global value. Numeric overrides are 0 to 2; the mask is `inherit`, `true` or `false`. Passes after the first inherit their tone from `nrLaterPassLocalTone`. An intensity of zero still evaluates the pass and does not remove its GPU/VRAM cost.
