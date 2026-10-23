# Settings file

*[Français](SETTINGS.fr.md) · [中文](SETTINGS.zh-CN.md)*

Every setting can be changed in the [menu](MENU-AND-OVERLAY.md) (**Insert**), which is the easiest way. This page is
for those who prefer to edit the file.

## The file

- Name: `rtx-encore.jsonc`, beside the mod's file. It is created on first launch with the default values.
- Format: JSON with `//` comments. Each key carries a comment that says what it does; some editors flag the
  comments, the mod reads them fine.
- Settings are grouped in sections. Most apply live; the comment says when a restart of the game is needed.
- The menu's own shortcut and layout are stored in the same file, in its `menuState` section.
- A settings file left by an earlier version is taken over with its values: see
  [Installation](INSTALLATION.md#upgrading-from-an-earlier-name).

No key holds a path or a command: a settings file can be shared with someone else as it is.

## Main keys

### `renderingEngines` / `xefg`

| Setting | Default | Effect |
| :--- | :---: | :--- |
| `reconstructionEngine` | `game` | `game` or `xess`; replaces compatible Super Resolution/DLAA processing with XeSS; Ray Reconstruction stays with its original engine. |
| `frameGenerationEngine` | `game` | `game` or `xefg`; uses XeFG on supported DirectX 12 or Vulkan routes. DirectX 11 remains experimental and is not game-qualified here. |
| `xefgMfg4VariablePacing` | `true` | Experimental pacing for XeFG above X2; disabling it can make higher factors unavailable. |
| `xefgUiHandling` | `interpolate` | `interpolate` or `game`; requesting separate game UI handling only works with supported inputs. |
| `xefgUiAlpha` | `unknown` | `unknown`, `straight` or `premultiplied`; leave unknown unless the game’s UI format is known. |

Reconstruction can switch between DLSS and XeSS while playing in compatible games. For Vulkan, install compatible Intel runtimes before starting the game; adding or replacing them requires a restart. Restart after changing the frame-generation engine, XeFG pacing or UI settings. With XeFG, changing the FPS limit also requires a restart. A requested FPS cap does not prove source-frame cadence.

[Intel XeSS and XeFG (experimental)](INTEL-ENGINES.md)

### `frameGeneration`

| Key | Default | Values |
| :--- | :---: | :--- |
| `mode` | `"game"` | `"game"` (the game or NVIDIA Profile Inspector decides), `"fixed"`, `"dynamic"` |
| `multiplier` | `4` | `2` to `6`, used in fixed mode. `5` and `6` are experimental. |
| `dynamicTargetFrameRate` | `0` | Target FPS of dynamic mode. `0` follows the refresh rate of the monitor showing the game. |
| `dynamicExperimental56` | `false` | Lets dynamic mode go up to X5 and X6. |
| `flipMeteringBypass` | `false` | Smooth out X3 / X4: Streamline spaces the generated frames itself instead of the NVIDIA driver. Restart the game to apply. |

NVIDIA runtime compatibility no longer requires a file to appear in a fixed list. An old `providerPatchPolicy` entry is ignored. Intel runtime checks remain unchanged. Compatibility still depends on the features available in the runtime and game.

### `dlssSuperResolution`

| Key | Default | Values |
| :--- | :---: | :--- |
| `dlssRenderScale` | `"game"` | `"game"`, `"dlaa"`, `"quality"`, `"balanced"`, `"performance"`, `"ultra-performance"`, `"custom"` |
| `dlssCustomScale` | `67` | Percent, `50` to `100`, with `"custom"` |
| `fastSuperResolution` | `false` | RTX 30, DirectX 12: faster DLSS Super Resolution with preset M or L, about 16 to 20 % less GPU time per upscaled frame on the system measured, image very close to the original. Inactive on other cards. Restart to apply. See [Super Resolution](SUPER-RESOLUTION.md#fast-super-resolution-rtx-30). |

### `superResolutionInputs`

| Key | Default | Values |
| :--- | :---: | :--- |
| `srHdr` | `"game"` | `"game"`, `"on"`, `"off"` |
| `srAutoExposure` | `"game"` | `"game"`, `"on"`, `"off"` |

These common SR/DLAA controls apply to the selected reconstruction engine. Restart to apply. Game preserves the game's input flags. HDR on expects HDR linear input; off expects LDR input. This does not convert colors or enable HDR display output. An incorrect override can degrade the image. RR and frame generation are unchanged.

Auto exposure may help exposure-related ghosting; it is not a general ghosting fix. DLSS models may impose it (preset L). XeSS auto exposure has an extra GPU cost and is not recommended for LDR input. XeSS Vulkan LDR manual exposure remains unsupported. An explicit XeSS mode rejected at initialization retains the original provider without applying that override to the fallback. Visual improvement from forced modes has not been validated.

Earlier DLSS/XeSS input keys are migrated using the selected reconstruction engine; the common keys take precedence when present.

### `superResolutionSharpening`

| Key | Default | Effect |
| :--- | :---: | :--- |
| `srSharpening` | `"off"` | Mode: `off` (default), `game` or `custom`. Applied live. |
| `srSharpness` | `30` | Custom strength: 0 to 100%, default 30%. 0 disables the effect. |

It adds to any sharpening the game performs afterwards. High strengths can create halos and amplify noise. Appearance varies with each game's later image processing, including HDR. Unsupported output formats and graphics paths leave the image unchanged. Original DLSS versions older than 2.5.1 keep their own sharpening; the extra pass also stays off when that version cannot be identified. Vulkan and real-game image quality have not yet been validated.

### `dlssPresets`

These independent model choices require a **game restart**. SR/RR are in **Image → DLSS Super Resolution**;
FG is in **Frames → Frame generation**. They do not change render resolution, the frame multiplier or the NR preset.

| Key | Default | Values |
| :--- | :---: | :--- |
| `dlssSrPreset` | `"game"` | `"game"`, `"default"`, `"a"`–`"f"` (legacy), `"j"`, `"k"`, `"l"`, `"m"` |
| `dlssRrPreset` | `"game"` | `"game"`, `"default"`, `"a"`–`"c"` (legacy), `"d"`, `"e"` |
| `dlssFgPreset` | `"game"` | `"game"`, `"a"`, `"b"`, `"c"` |
| `dlssFgLite` | `"game"` | Diagnostic only, JSON: `"game"`, `"off"`, `"on"` |
| `dlssNrPreset` | `"b"` | Neural Rendering network with `nvngx_dlssnr.dll` 310.9.16: `"b"` (smaller, much faster), `"a"` (the network of 310.8.0) or `"auto"` (the file's choice, B). One network with 310.8.0. Applies live. The `nrPreset` of earlier files is read under this name, with its value; its `0` to `3` still read. |

For the preset keys, `"game"` leaves the game's choice intact. SR/RR choices apply to every quality mode, including DLAA;
`"default"` explicitly requests the DLSS DLL's default. Available models depend on the installed DLL,
and NVIDIA driver overrides or model fallback can take priority. Choosing an RR preset does not enable RR.
With DLSS Super Resolution **310.9.1.0** and **310.9.16.0**, the SR preset chosen in Encore applies even when
the NVIDIA driver profile forces another one (310.9.1.0, and the 310.9.16.0 of the Streamline SDK and of NVIDIA's
DLSS GitHub, have no presets A to D).

FG preset selection supports the individually verified NVIDIA **DLSS-G release binaries** listed below:

- **310.6.0.0, 310.7.0.0, 310.7.128.0, 310.7.129.0**
- **310.8.0.0, 310.9.0.0, 310.9.1.0**
- **310.9.16.0**, the only one that accepts preset C

Preset C is NVIDIA's faster performance mode for frame generation, with minor trade-offs in image quality.
With any other binary a saved `"c"` is refused and the game's choice is kept.

**The FG preset is sufficient: A/B use Normal; C automatically enables Lite.** Fast frame generation stays
independent and can remain enabled with every preset. Restart after changing the preset.

`dlssFgLite` is a **JSON-only diagnostic override**, absent from the menu controls. Leave it at `"game"` to follow
the selected preset or the game/driver. Only for diagnostics with the individually qualified 310.9.16.0 file,
`"off"` forces Normal and `"on"` forces Lite, taking priority over the preset. Changing it requires a restart;
other binaries refuse this override. Lite keeps NVIDIA's native image
policy: **Anti-tearing / anti-ghosting** and **Protection tuning** have no effect in that mode, and UI recomposition is unavailable. Fast frame generation
uses the optimizations applicable to the active composition mode. The 310.9.16 additions await in-game validation;
no performance gain is established for Lite.

This list applies to preset overrides; it is narrower than overall frame-generation compatibility.
A matching version number alone does not guarantee acceptance, and development variants are excluded.
Other binaries keep their existing behavior and the log records the refusal. A uses classic FG;
B needs compatible HUD-less/UI inputs for UI recomposition. The mod does not write NVIDIA driver profiles.
Session details show saved/startup choices and whether FG accepted the request; they do not prove which
model executed. Leave `"game"` selected to preserve the existing behavior.

### `neuralRendering`

| Key | Default | Values |
| :--- | :---: | :--- |
| `nrEnabled` | `false` | Turns Neural Rendering on. |
| `nrPasses` | `1` | `1` to `4` |
| `nrLaterPassLocalTone` | `0` | Tone applied after pass one, `0` to `2` |
| `nrResolution` | `"render"` | `"render"`, `"output"`, `"quality"`, `"balanced"`, `"performance"`, `"ultra-performance"`, `"custom"` |
| `nrResolutionScale` | `67` | Percent of the output size, `33` to `100`, with `"custom"` |
| `nrIntensity` | `1` | Strength, `0` to `2` |
| `nrStyle` | `0` | `0` default, `1` natural, `2` cinematic |
| `nrPass1Style` … `nrPass4Style` | `"inherit"` | `"inherit"` (follows `nrStyle`), `"default"`, `"natural"`, `"cinematic"` |
| `nrLocalTone`, `nrLocalStructure` | `1` | `0` to `2` |
| `nrAutoMask` | `false` | Character mask; needed for `nrSkinStructure` |
| `nrSkinStructure` | `1` | `0` to `2` |
| `nrHdrExposure` | `1` | HDR games: brightness given to NR |
| `nrPrecision` | `"exact"` | `"exact"` or `"fast"` (RTX 20 and RTX 30). Restart to apply. |

#### Foveated NR (experimental)

Requires `nrEnabled`. NR runs at full strength in the middle of the screen and lighter towards the edges, where the eye rests less: faster, with a softer NR effect near the edges of the image. In the menu (**Image → Neural Rendering**), only the switch and the preset are shown; the four values behind the presets are under **Foveated NR: custom values**. Applied live. Where foveation cannot run, NR keeps running normally.

| Preset | What it does |
| :--- | :--- |
| `"light"` | Most of the image at full strength; the smallest gain. |
| `"balanced"` | The default trade-off. |
| `"strong"` | The largest gain: +27% average FPS measured in one game on an RTX 3070 Ti Laptop, edges visibly softer on close inspection. |
| `"custom"` | Uses the four values below. |

| Key | Default | Effect |
| :--- | :---: | :--- |
| `nrFoveation` | `false` | Turns foveated NR on. |
| `nrFoveaPreset` | `"balanced"` | `"light"`, `"balanced"`, `"strong"` or `"custom"`. |
| `nrFoveaWidth` | `55` | With `"custom"`: width of the full-strength centre, 1–100% of the screen. |
| `nrFoveaHeight` | `55` | With `"custom"`: height of the full-strength centre, 1–100% of the screen. |
| `nrFoveaFalloff` | `8` | With `"custom"`: transition on each side, 0–100% of the screen. |
| `nrFoveaEdge` | `50` | With `"custom"`: NR detail kept at the edges, 25–100%; lower is faster and softer. |

#### NR performance options

These controls are also in **Image → Neural Rendering → Performance**. See
[Neural Rendering](NEURAL-RENDERING.md#performance-controls-on-rtx-30) for measured results and image tradeoffs.
Engine-specific options apply only to the engine named below; changing an engine or a restart-only option needs a
game restart.

| Key | Default | Benefit and tradeoff |
| :--- | :---: | :--- |
| `nrPaddingAware` | `false` | Allows a nearby smaller NR size, at most 2 % less per axis, without changing the DLSS render size. Can change the image. Applies live. |
| `nrMaxInFlight` | `0` | NVIDIA, DirectX 12 with Reflex: `0` no limit, `1` or `2` outstanding NR frames; active frame generation uses at least `2`. Applies live; in-game performance effect unverified. |

### `overlay`

| Key | Default | Values |
| :--- | :---: | :--- |
| `showOverlay` | `false` | Shows the overlay. |
| `overlayPosition` | `"top-left"` | `"top-left"`, `"top-right"`, `"bottom-left"`, `"bottom-right"` |
| `overlayFontSize` | `14` | Text size in pixels, `10` to `32` |
| `overlayShowNr`, `overlayShowFramePacing`, `overlayShowGpu`, `overlayShowVram`, `overlayShowVersions` | `false` | Optional lines |

### `hudUi`

| Key | Default | Values |
| :--- | :---: | :--- |
| `autoUiRecomposition` | `true` | Separate interface handling whenever the game supplies what it needs |
| `uiAssist` | `true` | DirectX 12: builds the clean scene and the interface layer when the game does not supply them |
| `forceUiRecomposition` | `false` | Asks for the separate handling even when the game does not |

### `nativeMenuFeatures`

| Key | Default | Values |
| :--- | :---: | :--- |
| `reflexFrameLimit` | `0` | `0` restores the game's Reflex settings; `1` to `1000` requests an FPS cap. FG counting depends on the game/driver |
| `vsyncOff` | `false` | Asks DirectX games to present without V-Sync |
| `hairEnabled` | `true` | Compatible ray-traced hair, in the games that have it. Restart to apply. |

### `compatibility`

| Key | Default | Values |
| :--- | :---: | :--- |
| `disableInGameMenu` | `false` | `true` does not attach the RTX Encore menu, for games that reject it (Star Wars Outlaws). Frame generation keeps working; change settings in this file or with RTX Encore Hub. Restart to apply. |

### `diagnostics`

| Key | Default | Values |
| :--- | :---: | :--- |
| `logPerformance` | `false` | Writes frame times to CSV and GPU temperature, power and slowdown reasons to the session log in `rtx-encore-logs` |
| `logFilesKept` | `3` | Regular logs kept, plus the same number of exception logs, `1` to `100` |
| `logDxgiWrappers` | `false` | Records every call the game makes on the menu's swap chain, with the calling module, in the session and exception logs. For crashes after the menu attaches. Restart to apply |
| `crashDetails` | `false` | Exception logs add register memory and the stack, and a crash in the game or RTX Encore leaves a `.dmp` memory snapshot (often tens of MB) in `rtx-encore-logs`. For crash reports; turn it off afterwards. Restart to apply |

### `keyboardShortcuts`

Each `hotkey…` key takes one or more combinations separated by commas, such as
`"hotkeyFixed4": "Ctrl+Alt+4, Ctrl+Alt+Num4"`. An empty value `""` turns the action off.
`"disableKeybinds": true` turns every shortcut off.

## Other keys

The file holds more keys than this page lists: Smooth Motion, image-quality and compatibility switches, and the
other performance options. Each has its comment in the file and a labelled control in the menu, which
is the recommended place to change them.

### `neuralRendering.passTuning`

Each of up to four NR passes can override intensity, local tone, local structure, skin structure and the character mask, or inherit the global value. Numeric overrides are 0 to 2; the mask is `inherit`, `true` or `false`. Passes after the first inherit their tone from `nrLaterPassLocalTone`. An intensity of zero still evaluates the pass and does not remove its GPU/VRAM cost.

| Keys | Default | Values |
| :--- | :---: | :--- |
| `nrPass1Intensity` … `nrPass4Intensity` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1LocalStructure` … `nrPass4LocalStructure` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1LocalTone` … `nrPass4LocalTone` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1SkinStructure` … `nrPass4SkinStructure` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1AutoMask` … `nrPass4AutoMask` | `"inherit"` | `"inherit"`, `true`, `false` |
