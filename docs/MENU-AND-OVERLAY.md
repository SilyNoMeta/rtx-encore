# Menu and overlay

*[Français](MENU-AND-OVERLAY.fr.md) · [中文](MENU-AND-OVERLAY.zh-CN.md)*

RTX Encore draws its own menu and overlay in DirectX 11, DirectX 12 and Vulkan games. Nothing else has to be
installed.

## Intel XeSS and XeFG (experimental)

Restart the game after changing the engine, XeFG pacing or UI settings. With XeFG, changing the FPS limit also requires a restart. Do not infer source-frame cadence from an FPS cap.

[Intel XeSS and XeFG (experimental)](INTEL-ENGINES.md)

## The menu

Press **Insert** in the game window to open or close it. It opens once by itself on first launch. The shortcut can
be changed in the menu.

Choose **English**, **Français** or **简体中文** in the language selector. The choice applies immediately and is saved per game; untranslated messages stay in English. The session report copied for a bug report stays in English. The **About** tab shows the build version, project and Hub links, website, issue reporting, GitHub Sponsors, Ko-fi, third-party notices and public sponsor thanks. Links open in your default browser when selected.

A strip of four chips heads every page: **Upscaling**, **Frame Generation**, **Smooth Motion** and **Neural
Rendering**. Each shows the state of its feature with a colored dot and one figure; hover it for the detail, click it
to go to its settings. When a problem lasts, a card under the strip names it and says how to fix it.
**Upscaling** also names the engine at work (DLSS SR, DLSS RR or XeSS-SR) and, for DLSS, the preset the game or RTX
Encore asked for when the feature was created; **Frame Generation** names DLSS-G or XeSS-FG and the FG preset RTX
Encore supplies. The driver does not report the model it actually runs, so a driver override can still replace it.

| Tab | What it holds |
| :--- | :--- |
| **Frames** | Frame generation with its model preset (A/B Normal, C Lite), independent Fast option, quality and interface options, Smooth Motion, the Reflex frame limit, V-Sync |
| **Image** | DLSS render resolution, SR/RR/NR model presets, Fast super resolution (RTX 30), Neural Rendering |
| **Overlay** | A preview of the overlay, its position, its text size and the lines it shows |
| **System** | Shortcuts, compatibility, diagnostics and the session details, with a **Copy** button for bug reports |
| **About** | Build version, project links, support links, credits and public sponsor thanks |

Good to know:

- Each change is saved at once in `rtx-encore.jsonc`, with its comments kept.
- Most settings apply live. A `*` after a name marks a setting read when the game starts; it turns orange once
  changed, and a line at the bottom of the menu lists the changes waiting for a restart.
- A modified setting has a brighter name and a reset button.
- **FG model preset** requires a restart: **A/B use Normal; C (Lite) automatically enables Lite**. **Fast frame generation** stays independent. The Lite override is diagnostic-only in JSON, with no separate menu control. See [settings](SETTINGS.md#dlsspresets).
- Each section shows its main settings; the others are in groups that open in place (**Fine tuning**,
  **Performance**, **Diagnostics**, ...).
- Drag the title bar to move the menu and its bottom-right corner to resize it. Position and size are remembered.
- If a button was held while the menu opened, release it before clicking.

### When the menu opens in a separate window

In a few games the menu cannot be drawn safely inside the image. The settings then open in a separate window and
the overlay becomes a small click-through text over the game. Borderless window mode is recommended in that case.
The settings and their effect are the same.

### Component availability

**System → Session details** shows the loaded Streamline, DLSS-G, SR and RR versions, and lists each feature the loaded components do not allow, with the reason. Unavailable or unqualified choices are greyed out. A component that has not loaded yet is shown as not loaded, and the features that depend on it as waiting; this can change after enabling DLSS/FG in the game. You can still disable a saved override or return to the game’s settings. An available preset request does not prove which model the driver actually selected.

## The overlay

Turn it on in the **Overlay** tab or with `Ctrl + Alt + O`; `Ctrl + Alt + P` moves it to the next corner.

| Example | Meaning |
| :--- | :--- |
| `74 fps` | The game's frame rate; no frame generation observed |
| `74/37 fps 2x` | Displayed / rendered frame rate, with the observed multiplier |
| `148/74 fps sm` | Estimated output with Smooth Motion / the game's frame rate |
| `296/74 fps 2x+sm` | Frame generation X2, then Smooth Motion |

Optional lines, all off by default:

| Line | Shows |
| :--- | :--- |
| Neural Rendering | Its state and the size it works at; with 310.9.16, the NVIDIA network (preset A, B or Auto) |
| Frame pacing | Frame time: average, 99th percentile and jitter |
| Latency report | `PCL avg/max RQ GPU`: NVIDIA Reflex's own report for the game's rendered frames, as the driver fills it: simulation start to GPU render end, render queue wait and GPU time. With XeSS-FG, `XELL avg/max SIM>PRESENT`: XeLL's report, simulation start to the end of the Present call (XeLL records no GPU or queue time). Not display latency; generated frames are not timed; a game without latency markers shows nothing |
| GPU | Load, temperature, power and clocks |
| VRAM | Used / total on the card, all programs included |
| Versions | Versions of the NVIDIA DLLs the game loaded: Super Resolution (`SR`), Ray Reconstruction (`RR`), Frame Generation (`FG`), Neural Rendering (`NR`) and Streamline (`SL`). DLLs of the same version share a line (`SR-RR-FG-NR 310.9.16`); a DLL that is not loaded is not shown. `SL` is the game's own Streamline core, which plugin updates leave in place; when the plugins are of another version, its line shows both (`SL 2.7.11 PLUGINS 2.14.16`) |
| Interface | Whether the separate interface handling is on, and where the clean scene and interface layer come from |

Position (four corners) and text size (10 to 32 px) are in the same tab.

## Other controls

| Setting | Tab | What it does |
| :--- | :--- | :--- |
| **Render resolution** | Image | Forces the resolution the game renders at before DLSS: DLAA, Quality, Balanced, Performance, Ultra Performance, or a custom scale from 50 to 100 %. The game must already use DLSS. The menu shows the resolution observed and whether the game follows the request; set it back to *Game* if a game misbehaves. In Unreal Engine 4 and 5 games the screen percentage is adjusted live when possible. |
| **Fast super resolution (RTX 30)** | Image | Off by default. On RTX 30 cards, in DirectX 12 games using DLSS Super Resolution with preset M or L, runs a faster version of the upscaling: about 16 to 20 % less GPU time per upscaled frame on the system measured, with an image very close to the original. No effect on other cards. See [Super Resolution](SUPER-RESOLUTION.md#fast-super-resolution-rtx-30). |
| **Frame limit (FPS)** | Frames | Requests a frame limit through NVIDIA Reflex when supported. 0 restores the game's settings. Generated frames may count toward the limit depending on the game/driver. Suspended in Dynamic mode. |
| **V-Sync** | Frames | Game: the game's own V-Sync. Off: asks DirectX games to present without V-Sync. On: asks them to present with V-Sync, for games that gray out V-Sync with frame generation and laptops where the NVIDIA control panel V-Sync has no effect. A setting forced in the NVIDIA control panel keeps priority. No effect in Vulkan games or with XeSS-FG (the choice is grayed out while XeSS-FG presents the image). On can add latency with high multipliers on a 60 Hz screen: lower the multiplier or use the frame limit. |
| **Compatible ray-traced hair** | System | The Witcher 3 (DirectX 12, game builds 25575366 and 25646871): makes the game's ray-traced hair available on RTX 20, 30 and 40 cards. The game's own hair settings still decide whether hair is rendered. Not validated in game on RTX 20 and RTX 30 yet. Restart to apply. |

### Suggested Reflex cap

The Reflex limiter accepts DLL versions 2.1.1–2.14.1, and 2.14.16. Other versions retain the game's settings.
The published SDKs in this range have been examined offline; this is not in-game validation of every version.
Untagged intermediate versions, including 2.13 and 2.14.0, have no public SDK validation.
Actual limiting still depends on the game's Reflex integration and driver support. This does not add a CPU-wait fallback.

**Reflex mode** — `game` follows the game's mode (default); `on` / `on-boost` request On / On+Boost, never lower than the game. Applied live independently of the cap, suspended in Dynamic and while XeLL owns latency. Returning to `game` restores the game's mode at the next safe boundary. Boost keeps GPU clocks high when CPU limited; compare the latency report, since no gain is guaranteed. The menu shows the game and accepted modes: an accepted options call does not prove the driver applies it. XeLL always uses Low Latency and its runtime does not support Boost.

In **Frames → Frame pacing**, **For this display** suggests `refresh rate - refresh rate² / 3600`, rounded down:
for example, **116 FPS at 120 Hz**, 138 at 144 Hz and 224 at 240 Hz. It does not divide the request by the FG multiplier. Depending on the
game and driver, generated frames may count toward the cap. In Starfield with Streamline 2.12, a request
of 90 was observed at about 22.5 rendered FPS with FG ×4. Compare the measured source and FG-inclusive rates.
Click **Use … FPS** to save the suggestion. Display or multiplier changes never adjust your saved limit automatically.
Or type a value directly into **Frame limit (FPS)** and press **Enter** or **Apply**. Values from 1 to 1000 FPS
are accepted. **Use game settings** (or 0) restores the game’s own limit; it does not force all limiters off.
An unfinished edit is not saved when you click elsewhere.

The suggestion is unavailable in Dynamic mode, with Smooth Motion, or while the display rate or current
Frame Generation state is unknown. The manual limit remains available and requires supported Reflex.
The menu shows the Reflex request/status separately from measured source FPS: **Reflex accepted … FPS** confirms the
request, not that the measured rate or display output matches it.

The margin grows with the refresh rate. It is a starting point, not automatic VRR detection or a guarantee of staying within its range.
A lower limit can reduce load, power and heat if it reduces the actual rendering rate. A limit above the
rate the game already achieves may have no benefit; adjust it after comparing the same scene at operating temperature.

## Shortcuts

Defaults are listed in [Frame generation](FRAME-GENERATION.md#shortcuts). In the **System** tab, each action can be
given several key combinations or none. Shortcuts only act while the game window has focus.

## Logs

Session logs are written in the `rtx-encore-logs` folder beside the mod's file; the three most recent are kept
(**Session logs kept**, 1 to 100). **Log performance** writes the frame rate and frame times of the session to a CSV
file in the same folder.

Exception logs are kept separately, with the same count. Each process has its own file; logs still in use are preserved.

With **Log performance** enabled, the session log also records GPU temperature, power, clocks and reported performance limits about once per second. This works with the overlay hidden and Frame Generation off. Unavailable readings are marked `NA`. See [performance drops](TROUBLESHOOTING.md#performance-drops-after-playing-for-a-while).
