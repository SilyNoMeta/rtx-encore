# Games without DLSS

*[Français](GAMES-WITHOUT-DLSS.fr.md) · [中文](GAMES-WITHOUT-DLSS.zh-CN.md)*

Many games ship no DLSS at all: no upscaler, no frame generation, no Neural Rendering. Since 2.0.0, RTX Encore brings
all three to them, from the image they display, in DirectX 11, DirectX 12 and Vulkan, without ReShade or any other
tool:

- **Upscaling**: DLAA on the image the game displays, or upscaling from a lower game resolution to the screen's, with
  NVIDIA DLSS Super Resolution or Intel XeSS (XeSS on NVIDIA Tensor Cores in DirectX 12).
- **Neural Rendering**: NVIDIA's [Neural Rendering](NEURAL-RENDERING.md) on that same image, after the upscaler or
  alone.
- **Frame generation**: NVIDIA DLSS Frame Generation (DirectX 11, DirectX 12 and Vulkan, X2 to X6) or Intel XeFG (DirectX 12 up
  to X6 with Tensor Cores, Vulkan X2 to X6 with HDR10, DirectX 11 X2 to X6), with a multiplier you can change while
  playing.

Each works on its own or with the others. They are experimental and off by default. Games that have their own DLSS
keep it. For those games, see the separate experimental [NR Final image mode](NEURAL-RENDERING.md#present-mode).

**Tested by the maintainer** (RTX 4090, NVIDIA 621.01): PEAK in DirectX 12, DirectX 11 and Vulkan (upscaling, Neural
Rendering, DLSS Frame Generation X2 and X4, XeFG X2 to X4), The Sims 4 in DirectX 11 (XeFG X2), Football Life 2027 in
DirectX 11 (DLAA, upscaling to 4K and Neural Rendering, in HDR). Other games, cards and drivers have not been checked:
tell us what you see in an [issue](https://github.com/SilyNoMeta/rtx-encore/issues).

## How it works, in short

RTX Encore takes the finished image just before it is shown. The motion between two images is estimated from the
images themselves, on the graphics card (using available game motion vectors by default, with RTX Encore estimation as fallback), and the game's depth buffer is
found automatically when the game draws with one. DLSS or XeSS, Neural Rendering and frame generation then run on
that image, and the result is what you see.

Because the image is finished, **the HUD and the text are processed with it**: upscaled, enhanced and, with frame
generation, interpolated like the rest of the image. Most of the time this is invisible; small text over moving
scenes can show slight ghosting, and a HUD that moves fast can be doubled in generated frames.

## Requirements

- An NVIDIA RTX 20, 30, 40 or 50 card and a recent driver. The Optical Flow choices of motion estimation use NVIDIA
  Optical Flow, which is part of the driver.
- DirectX 11, DirectX 12 or Vulkan. In Vulkan, the game's device must accept the features RTX Encore asks for when it
  starts; if it does not, these options have no effect in that game. A Vulkan game can take up to ten seconds longer
  to start while frame generation gets ready.
- The NVIDIA and Intel files each feature uses, which **RTX Encore Hub 2.0.0 installs for you** when you install or
  update the mod in a game that ships no DLSS (see below). By hand:
  - DLSS upscaling: NVIDIA's `nvngx_dlss.dll` beside the game's executable or beside RTX Encore.
  - Neural Rendering: NVIDIA's `nvngx_dlssnr.dll`, as on the [Neural Rendering](NEURAL-RENDERING.md#requirements)
    page.
  - DLSS Frame Generation: the six NVIDIA frame generation files beside the game's executable, in the versions RTX
    Encore accepts for this use (Streamline 2.14.16 and DLSS Frame Generation 310.9.16). Other versions are refused,
    and the game then runs without frame generation.
  - XeSS and XeFG: the Intel runtimes, as on the [Intel XeSS and XeFG](INTEL-ENGINES.md) page.
- Frame generation also needs NVIDIA **Smooth Motion off** in RTX Encore for that game: the two do not stack.

## Install with RTX Encore Hub

[RTX Encore Hub 2.0.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.0.0) knows which games ship no
DLSS. For such a game, **Install**, **Set up with the mod** and the updates put beside the mod everything this page
needs: the DLSS and Neural Rendering files, the frame generation files beside the executable when the game has no
frame generation of its own, and the Intel runtimes. Nothing is turned on by that: the settings stay at the mod's
defaults, and you choose each feature in the in-game menu or in the Hub's settings form. **Restore game** removes all
of it. A game that ships its own frame generation never receives the frame generation files.

## Turn it on

Open the menu (**Insert**). In a game without DLSS, the **Image quality** and **Frame rate** tabs show these controls instead of
the ones for games with DLSS.

### Upscaling

In **Image quality → Upscaling → Engine**, choose **DLSS** or **XeSS** to enable reconstruction, or **None** to leave it off. There is no separate activation switch. **Game resolution (DLAA)** is the default output: DLSS runs at native resolution; XeSS provides native anti-aliasing. For upscaling, launch the game in **Windowed**, not borderless, at the desired input resolution, then choose **Screen resolution** in Encore. Encore enlarges the output to the screen and removes the window borders. For example, 2560 × 1440 to 3840 × 2160 corresponds to about 67% per dimension.

Choose **NIS** or **FSR 1** in the same Engine menu to upscale without a DLSS or Intel runtime. In DirectX 11 and DirectX 12, choose a lower windowed resolution with the same aspect ratio as the display. Both dimensions must increase, up to 2x per axis. The **Sharpness** control ranges from 0 to 100% and defaults to 30%. In DirectX 11, you can also choose **SMAA** or **FXAA** before scaling (source resolution) or after it (output resolution). These filters require SDR and include the HUD. Spatial anti-aliasing is unavailable in DirectX 12.

NIS and FSR 1 support SDR and HDR, including compatible Vulkan presentation paths. Vulkan availability depends on the game and its presentation format.

### Resolution and Windows display scaling

With **Screen resolution**, the game chooses the input resolution and Encore keeps the output at the display resolution. **1920×1080 → 2560×1440 is 75% per dimension**. A 1440p output label does not mean the game rendered at 1440p, and the image filling the same screen is expected. Applying 1080p again when the game already renders at 1080p produces no resolution change.

Windows display scaling is a separate setting. On a 2560×1440 display at **150%**, a game without high-DPI support can see about **1707×960** instead. This can limit the game resolution and make text blurry. Keep your preferred Windows scaling; first check the game's own high-DPI support.

For an Unreal Engine game with this symptom:

1. Close the game and back up its `Engine.ini` in its saved configuration folder, commonly `%LOCALAPPDATA%\<Game>\Saved\Config\WindowsNoEditor\` (some games use `Windows`). Use the game's file, not Encore's configuration.
2. In the existing `[/Script/Engine.UserInterfaceSettings]` section, set `bAllowHighDPIInGameMode=True`. Add the section if absent; preserve other settings:

   ```ini
   [/Script/Engine.UserInterfaceSettings]
   bAllowHighDPIInGameMode=True
   ```

3. Restart the game. Enable **DLSS** or **XeSS**, choose **Screen resolution**, and apply the desired resolution in the game's graphics settings. For a comparison on a 1440p display, apply 2560×1440 and then 1920×1080. The latter should produce 1080p input and 1440p output, about 75%.
4. Check the game buttons as well as image quality. If the cursor remains offset, return the setting to its previous value and report the game, Windows scaling, game resolution and Encore output mode. A sharper image alone does not prove correct mouse alignment.

Epic documents this [Unreal high-DPI setting](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Engine/UUserInterfaceSettings/bAllowHighDPIInGameMode?application_version=4.27). The setup was confirmed with **Islands of Insight, DirectX 11, a 1440p display and 150% Windows scaling**, using the build tested for this fix. It is not a guarantee for every Unreal game or an older Encore release: a game may ignore the setting or use another configuration location. Restore the backed-up setting if it does not help. The added upscaler still processes the HUD and text with the rest of the image.

### Neural Rendering

In the **Image quality** tab, **Neural Rendering** section, keep **Processing order** at **Post-SR** and turn on **Neural Rendering**. In a game without DLSS, this one
switch starts NR on the displayed image, after the upscaler when it is on, or alone. Every NR control of the
[Neural Rendering](NEURAL-RENDERING.md) page then applies (passes, styles, resolution, Fast precision on RTX 30, HDR
brightness). **Ctrl + Alt + N** turns it on and off while you play.

### Frame generation

In **Frame rate → Frame generation → Engine**, choose **DLSS-FG** or **XeFG**, or **None** to disable added frame generation. Both engines support DirectX 11, DirectX 12 and Vulkan on compatible configurations. Turn Smooth Motion off and restart when requested. Choose **Fixed** for X2 to X6; X5 and X6 remain experimental. **Game** is for the game’s own integration, not an activation option for a game with no frame generation.

### Motion estimation

Under the upscaling controls, **Motion estimation** chooses how RTX Encore measures the motion between two frames,
for the upscaler, Neural Rendering and frame generation alike. You can switch while you play.

| Choice | What it is | Cost at 4K on an RTX 4090 |
| :--- | :--- | :--- |
| **Game motion vectors** (default) | Uses compatible motion vectors from the game; falls back automatically to RTX Encore estimation. Available for added frame generation and compatible Final image NR in DirectX 12, DirectX 11 and Vulkan. | Depends on the available inputs |
| **RTX Encore** | RTX Encore's own estimation, in the graphics card's shaders: fine vectors, one per 2×2 pixels, that follow the edges of objects and use the depth buffer when the game has one. Made for frame generation from these images; needs no accelerator. | about 1.0 ms per frame |
| **Shader (vort)** | Estimated in shaders too, with the method of vortigern11's vort_Motion. Compare it with RTX Encore in your game. | about 0.6 ms per frame |
| **Optical Flow (Quality)** | NVIDIA Optical Flow, the accelerator of RTX cards. | — |
| **Optical Flow (Fast)** | NVIDIA Optical Flow in its faster mode: about a millisecond less per frame, but motion is slightly less precise, which can show as more ghosting on fast movement. | — |

## XeSS instead of DLSS

- XeSS goes from 1x (native anti-aliasing) to 3x: on a 4K screen, the game's resolution must be at least 1280×720.
  Below that, the game's image is shown stretched, without XeSS.
- **NVIDIA Tensor Cores (experimental)**, shown under the engine when XeSS is chosen in a DirectX 12 game, runs XeSS on
  the Tensor Cores with the same qualification as on the [Intel XeSS and XeFG](INTEL-ENGINES.md) page. DirectX 11 and
  Vulkan games run standard XeSS and do not show the switch. Turning it on or off re-creates XeSS (a short hitch). The
  line under the switch says what the last XeSS frame used: Tensor Cores active, prepared but not used, or not
  available and why; the **Upscaling** chip carries a green **Tensor Cores** badge while they run.
- The first frames after the game starts are shown without XeSS while RTX Encore checks the Intel runtime.

## Frame generation, by graphics API

| | DLSS Frame Generation | Intel XeFG |
| :--- | :--- | :--- |
| DirectX 12 | X2 to X6 | X2 to X6; **NVIDIA Tensor Cores** available, with the same qualification as on the [Intel XeSS and XeFG](INTEL-ENGINES.md) page |
| Vulkan | X2 to X6 | X2 to X6, live multiplier changes; SDR and HDR10 |
| DirectX 11 | X2–X6 | X2 to X6, live multiplier changes; SDR and HDR10 |

- **HDR.** A game that displays in HDR10 (10-bit) on a screen Windows runs in HDR mode gets frame generation in HDR.
  A game that displays in scRGB (16-bit floating point) keeps its own output: no frame generation there, the rest of
  this page still applies.
- **Exclusive fullscreen** in DirectX 11 is shown as a borderless window while XeFG runs.
- The maintainer found the pacing smooth in PEAK on DirectX 12 and Vulkan at X4. On Vulkan, when the game presents
  without V-Sync, generated frames are shown without waiting for the display's refresh, as DLSS Frame Generation
  does; the game's frame rate is then not held to the refresh rate divided by the multiplier.
- On DirectX 11, the game's next frame waits for XeFG's work on the previous one, as on DirectX 12, and X3 is paced
  evenly. Where the screen allows tearing, the output is not held to its refresh rate: the maintainer saw XeFG above
  144 FPS on a 144 Hz screen in The Sims 4, and X3 to X6 in Skyrim Special Edition.
- A game that already ships DLSS Frame Generation, Streamline, Intel XeFG or AMD frame generation keeps it: RTX
  Encore never adds a second frame generation to it. For such games, see [Frame generation](FRAME-GENERATION.md) and
  [Intel XeSS and XeFG](INTEL-ENGINES.md).

## Upscaling to the screen's resolution

1. Choose **Screen resolution** as the output.
2. In the game, choose a lower resolution (for example 2560×1440 on a 4K screen) and **windowed** mode.
3. RTX Encore makes the window borderless over the whole screen and DLSS (or XeSS) upscales the game's image to it.
   The mouse keeps working at the game's resolution. It applies from the game's next resolution change, or after a
   restart.

In **Unity** games, choose **Windowed**, not **Fullscreen Window**: below the screen's resolution, Fullscreen Window
stretches the image itself before RTX Encore sees it, and DLSS can then only run as DLAA. Exclusive fullscreen keeps
the game's resolution.

Compared with games that have DLSS, the upscaled image is a little softer: the game does not jitter its camera for
the upscaler, so fine detail cannot be rebuilt as well. DLAA at the game's resolution does not have this limit.

## Limits

- The HUD is processed with the image (see above), and interpolated with it in generated frames.
- Fast motion, disocclusions and zoomed replays can show more ghosting than in a game with DLSS, in the upscaled image
  and in generated frames.
- Without a depth buffer (or when it is not found), everything still runs, with slightly less stable motion.
- The upscaled image is a little softer than a game's own DLSS, which has the game's camera jitter; DLAA does not
  have this limit.
- XeFG Tensor Cores also work experimentally in DirectX 11 and Vulkan; XeSS Tensor Cores are offered experimentally there where the route supports them. Replacing a Vulkan game's own DLSS with XeSS keeps standard XeSS.
- scRGB (16-bit floating point) output keeps the game's own frames: no frame generation.
- Frame generation and NVIDIA Smooth Motion do not combine: turn Smooth Motion off.
- DirectX 11 games that run on top of DirectX 12 (some translation layers) are not supported.
- Vulkan games whose device does not accept the extra features keep running normally, without these options.
- Nothing here has been validated on RTX 20, RTX 30 or RTX 50 cards yet, nor on other drivers than 621.01.

## When something does not work

- Check that the options are visible: they are hidden in games where RTX Encore sees the game's own DLSS or frame
  generation.
- The **Upscaling** chip of the menu shows the engine that runs and the scale; the **Frame Generation** chip shows the
  engine and the multiplier, or why nothing is generated.
- Nothing is upscaled, enhanced or generated: check that the files are there. In the Hub, the game's page lists them
  and offers to write the missing ones; by hand, see [Requirements](#requirements). Without the right frame
  generation files the game simply runs without frame generation.
- XeSS does not run: check that the Hub installed the Intel runtimes for this game, then restart the game. Under
  **Engine**, the menu says when XeSS is selected but DLSS ran instead, and why.
- Frame generation does not start: turn **Smooth Motion** off, check that the engine was chosen before the restart,
  and that the game is not one that ships its own frame generation.
- The image is black or stretched: see [Upscaling to the screen's resolution](#upscaling-to-the-screens-resolution)
  for the window mode, and the XeSS 3x limit above.
- Report it with the menu's report (**System** tab, **Copy**): see [Troubleshooting](TROUBLESHOOTING.md).

## Compatibility and measured limits

API support does not guarantee compatibility with every game. Fixed DLSS-G can request 2x–6x on qualified routes; 5x/6x remain experimental and actual support depends on the GPU and loaded runtimes. XeFG factors depend on the admitted runtime profile.

Encore's hybrid motion estimator measured 0.985 ms GPU at 3840x2160 on the RTX 4090 test machine. This measures motion estimation, not the full reconstruction/FG cost; other hardware and scenes differ.

Reconstruction for games without DLSS also processes menus: newly appearing text can ghost while temporal history converges (observed with DLAA preset M in Elden Ring). Turning reconstruction off should leave independently enabled FG running. FG-only startup was verified in Elden Ring with reconstruction disabled.

Elden Ring's internal 60 rendered-FPS limit is separate from presentation/V-Sync limits. Encore does not remove the engine's limiter. The separately tested FPS unlocker is not bundled with Encore.

## Runtime availability and versions

Missing or incompatible runtimes make the corresponding engine unavailable in the menu. Older compatible versions remain supported: NVIDIA 310.9.1 is not rejected merely because a newer version exists. An integrated warning recommends updating with RTX Encore Hub and restarting the game. Recommended versions: NVIDIA **310.9.16**, XeSS-SR **2.0.2.68**, XeFG **1.3.1.78**, XeLL **1.3.2.10**. Added DLSS frame generation has a stricter requirement: **Streamline 2.14.16 + DLSS-G 310.9.16**. Updating does not make an unsupported game or GPU compatible.
