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
| **Frame generation** | On compatible Post-SR and Pre-SR routes, NR runs on the game's rendered frames. See the separate limits for [Final image mode](#present-mode). |
| **Several passes, a style for each** | One to four passes, each with its own look. |
| **HDR games** | Handled, with a brightness control. |
| **Background preparation** | When NR is preparing or required game inputs are unavailable, the image stays as DLSS made it. |

Options that use more VRAM are off by default.

## Experimental Pre-SR and A/B comparison

**Post-SR** remains the default. **Pre-SR** applies NR to the render input before
DLSS Super Resolution. It uses render size and supports one to four passes, with
an independent model profile and appearance for each pass. The processing ribbon
shows the requested order; check the effective mode before comparing results.
Pre-SR requires ordinary DLSS SR with uniform input. RR, DLAA, foveation,
padding-aware sizing and jittered motion inputs are unavailable in this mode.
Unsupported inputs keep the original SR image. These paths still require GPU
and in-game qualification; the new controls do not establish compatibility.

Open **Image and performance lab** in the NR menu. Keep a reproducible scene
and camera, measure Post-SR, Pre-SR or Final image separately, and choose which two
to compare. Each series excludes 60 warmup frames and collects 240 completed
source frames. Capture each mode, then enable **Inspect captures** for zoom,
side-by-side or an A/B wipe. Post-SR and Pre-SR capture after configured
sharpening at the DLSS stage, before the game adds later effects. Final image
captures the processed game image, including any preserved game HUD, before
frame generation and Encore's menu. The capture point is included in the export.

The panel reports GPU NR/SR time, available model timings, source intervals
and memory in MiB. Memory scopes are separate: tracked NR allocations,
process-local usage on node 0 of the render adapter, and global NVIDIA
framebuffer usage. Final image does not measure SR GPU time at its capture point.
NR and model GPU times are available only when the work can be timed on the
same command stream. In Final image, the NR GPU timer starts after motion and depth
preparation and excludes the Vulkan or DirectX 11 transfers used by some paths.
It measures NR processing only. Missing counters remain **N/D**. D3D11 bridge model timings
and its complete private allocation count are unavailable. Tracked allocations
exclude opaque NVIDIA allocations and are not a residency measurement. Global changes
provide context. They cannot establish the memory savings caused by a placement.

Export to a folder outside the game directory for JSON, CSV, original image
samples, SDR BMP previews and `compare.html`, a full-resolution pixel viewer.
HDR originals retain their numeric format; their preview uses tone mapping.
Routes, formats or sizes that cannot be captured report a reason. Sequential
live captures are different frames, and still images cannot establish temporal
stability. Compare camera motion, disocclusions and fine moving detail as well.
Cancel is available; restoring the starting mode preserves later manual edits.
No relative gain is presented for incompatible series. Stopping comparison
stops new instrumentation; existing GPU work retires through its completion.

<a name="present-mode"></a>

## Final image mode

In **Image quality → Neural Rendering**, turn on **Neural Rendering** and set **Processing order** to **Final image**. This experimental mode processes compatible game frames after post-processing and before frame generation, Encore’s menu and display. If a supported setup supplies a separate scene and HUD, NR enhances the scene and preserves the HUD. Fine text and moving HUD elements already drawn into an image processed as a single layer can change with the rest of the image.

Final image accepts compatible DirectX 11, DirectX 12 and Vulkan routes with game DLSS, SDR, 10-bit HDR10 (PQ) or floating-point scRGB, and one to four NR passes. **Foveated NR** and **Avoid NR padding** can be used together. With compatible **NIS** in DirectX 11, Final image instead follows NIS and its optional anti-aliasing, including in games without DLSS. This can feed compatible Encore-added frame generation. NIS retains its SDR, window mode and scaling limits. Other added upscaling engines are outside this Final image path. DLAA and Ray Reconstruction combinations await in-game validation.

- **DirectX 11.** Compatible Encore-added XeFG and DLSS Frame Generation can use the enhanced source. DirectX 11 on DirectX 12 is also supported when the game creates a compatible DirectX 11 swapchain using that configuration. A game that creates its swapchain in DirectX 12 and wraps the buffers separately for DirectX 11 is outside this path.
- **DirectX 12.** Compatible game Streamline DLSS Frame Generation can use the full image or a matching scene and premultiplied HUD in SDR or HDR10. HDR10 needs a separate high-precision UI texture. If the frame does not match the expected HUD composition, Final image preserves its original colors. Supported native FSR setups require compatible standard UI composition. Supported native XeSS Frame Generation setups can use the full image or matching full-size premultiplied layers in SDR or HDR10. The native XeSS HDR10 path requires matching RGB10A2 scene, UI and output. It preserves the originals if the stored 2-bit UI alpha does not reproduce the game image. Compatible Encore-added DLSS Frame Generation and XeFG are also supported. For Encore-added XeFG on DirectX, set **HUD/UI handling** to **Interpolate**.
- **Vulkan.** Compatible game Streamline DLSS Frame Generation and Encore-added XeFG can use the full image or supported separate scene and HUD layers. Separate layers require full-size premultiplied UI with a matching scene. Game DLSS Frame Generation supports compatible SDR, HDR10 with a high-precision UI texture, and scRGB with floating-point scene and UI. Encore XeFG separate layers require SDR and **HUD/UI handling → Game layers**. Partial layers and alpha-only UI are outside this path. Select **Final image** before starting a Vulkan game. If you switch to it in-game, including after using Pre-SR or Post-SR, restart the game to apply the change.
- **Smooth Motion.** Final image accepts specific DirectX 11 configurations and supported Encore DirectX 12 paths with the exact NVIDIA 621.14 driver. Native DirectX 12 Smooth Motion on RTX 40/50 and Vulkan Smooth Motion are outside this path.
- **Frame generation off.** Compatible game Streamline DLSS configurations can still use Final image if the game supplies separate UI inputs.

Use **Motion estimation** in the NR section to choose how motion is estimated from the submitted game image. This adds GPU work and can behave differently from the motion supplied by the game at DLSS. **Resolution → Render** uses the game's render size as its starting point when available for the current image, or output size otherwise. The output-size fallback can cost more. Percentage and custom scales are relative to output size. Check the live dimensions and **Effective mode** in the menu.

If a compatible game image is unavailable, the original image and requested mode are preserved. Use the **Image and performance lab** to capture and compare Final image with either SR placement. The supported software paths still need validation of image quality, timing and frame generation in games.

## Requirements

- NVIDIA's NR file, **`nvngx_dlssnr.dll` version 310.8.0 or 310.9.16**. It is not included in RTX Encore: supply it
  yourself and put it beside the mod's file, normally in the folder of the game's executable. Any other version is
  refused. [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub) downloads this file, checks it and puts it there for you.
- A game with **DLSS Super Resolution or DLAA turned on** for Post-SR. NR is applied to its result.
  With **Ray Reconstruction**, NR is applied to RR's result, after its denoising: never to the raw ray-traced image.
  The game must provide the depth input NR needs; otherwise the image stays as DLSS made it and the menu says so.
  **Games that ship no DLSS** are the exception since 2.0.0: there, the **Neural Rendering** switch runs NR on the
  image the game displays, with motion estimated by RTX Encore and the game's depth buffer when it has one, in
  DirectX 11, DirectX 12 and Vulkan. See [Games without DLSS](GAMES-WITHOUT-DLSS.md#neural-rendering).
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
| **NR model preset** | B | With `nvngx_dlssnr.dll` 310.9.16: **B** is a smaller, much faster network, **A** the larger one that 310.8.0 runs, **DLL default** lets the file choose (B). With 310.8.0, every choice runs its single network. It sits beside the SR and RR model presets (**Image quality** tab, Upscaling) and applies live. |

In a local test at 1080p on an RTX 3070 Ti Laptop, network B took about 15 ms of NR time and network A about
27 ms. B gives a different image from A. Support for 310.9.16 is experimental and lightly tested.

### Performance controls on RTX 30

The **Image quality → Neural Rendering → Performance** group exposes the work RTX Encore has done to reduce NR cost.
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

NR adds GPU work when it processes a frame. Measured in the built-in benchmark of Black Myth: Wukong on an RTX 3070 Ti
Laptop (2560×1440, DLSS at 58 %, frame generation X3):

| Configuration | Average FPS | 5 % low |
| :--- | ---: | ---: |
| NR off | 146 | 127 |
| NR on, Precision Exact | 76 | 71 |
| NR on, Precision Fast | 82 | 76 |
| NR on, Precision Fast, Resolution Performance | 90 | 83 |

These are **displayed FPS with frame generation X3**, using the NVIDIA NR engine. In this test, going from the
default to Precision Fast and Resolution Performance raises the displayed frame rate by 18 % (76 to 90 FPS).
The 17 % figure concerns NR processing time. These figures apply to the tested DLSS reconstruction path with X3 frame generation.

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


## 2.3.0

### NR looks and exposure

Seven appearance recipes are available in the mod and Hub: Standard, Subtle, Natural, Cinematic, Faces, Soft skin and NVIDIA reference. Applying a look changes appearance and clears per-pass appearance overrides, while preserving pass count, models, resolution and exposure. NR exposure offers Game, Automatic, Measured and Manual for compatible scene-linear DLSS inputs. New configurations use Automatic; existing files without the key keep Game behavior.
