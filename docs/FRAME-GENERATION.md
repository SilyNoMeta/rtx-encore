# Frame generation

*[Français](FRAME-GENERATION.fr.md) · [中文](FRAME-GENERATION.zh-CN.md)*

RTX Encore brings DLSS Multi Frame Generation, from X2 to X6, to RTX 20, RTX 30 and RTX 40 cards.

## Intel XeSS and XeFG (experimental)

RTX Encore 1.2.0 adds a choice of reconstruction and frame-generation engines. Leave **Game** selected to keep the game’s normal engine. Intel modes are experimental, off by default and require compatible game inputs; choosing an engine does not prove it became active.

[Intel XeSS and XeFG (experimental)](INTEL-ENGINES.md)

## Requirements

- A game with native **DLSS Frame Generation** in DirectX 12 or Vulkan, or a compatible native **Intel FG** route in DirectX 12. For the latter, use the experimental [DLSS-FG engine](INTEL-ENGINES.md), install the qualified runtimes through the Hub, restart, and enable Intel FG in the game. Native FSR is preserved.
- A supported `nvngx_dlssg.dll` and compatible runtimes: see [Compatibility](COMPATIBILITY.md#dlss-frame-generation-versions). Exact versions determine available features.
- Frame generation enabled in the game's settings. For games with no compatible native FG inputs, see [Smooth Motion](SMOOTH-MOTION.md).

## Modes

Choose the mode in the menu (**Frames** tab) or with the shortcuts.

| Mode | What it does |
| :--- | :--- |
| **Game decides** (default) | The game, or NVIDIA Profile Inspector, chooses the multiplier. |
| **Fixed** | Always the multiplier you choose: X2, X3, X4, X5 or X6. |
| **Dynamic** | The multiplier follows the frame rate to reach a target. The target is the refresh rate of the monitor showing the game, or a value you set. |

The multiplier is the number of frames shown for each frame the game renders: X3 shows two generated frames after
each rendered one.

- **X5 and X6 are experimental.** They need plenty of VRAM, and latency grows with the multiplier.
- Dynamic mode stays at X4 or below unless **Allow 5x and 6x** is on.
- In Vulkan games, Dynamic mode chooses between X2 and X6 from the measured frame rate and the target.
- On an 8 GB card, start with X2 or X3, or X4 with High or Medium textures.

### Smooth out X3 / X4

In some games, X3 and X4 look choppier than X2: the generated frames reach the screen in bursts instead of one after the other. **Smooth out X3 / X4** (`flipMeteringBypass`, off by default) lets Streamline space the generated frames itself instead of leaving it to the NVIDIA driver, in DirectX 12 and Vulkan. Turn it on in the menu, under the multiplier, or in RTX Encore Hub, then restart the game. Where the driver already spaces the frames well, it brings nothing visible.

On qualified older Streamline versions, DX12 Dynamic uses Encore’s adaptive X2–X4 mode. It starts at X2, reacts to the measured source-frame cadence, waits between changes and suspends adaptation if the game refuses a request. Modern qualified providers retain native Dynamic MFG. See [the version matrix](COMPATIBILITY.md#streamline-versions). D3D11 uses the separate Smooth Motion X2 path only.

## Shortcuts

| Shortcut | Action |
| :--- | :--- |
| `Ctrl + Alt + 2…6` | Fixed multiplier X2 to X6 |
| `Ctrl + Alt + PageUp / PageDown` | Raise or lower the multiplier (switches to Fixed mode) |
| `Ctrl + Alt + D` | Switch between Fixed and Dynamic mode |
| `Ctrl + Alt + Up` or `+` | Raise the Dynamic target by 5 FPS (`Shift`: 1 FPS) |
| `Ctrl + Alt + Down` or `-` | Lower the Dynamic target by 5 FPS (`Shift`: 1 FPS) |
| `Ctrl + Alt + G` | Back to **Game decides** |

A shortcut saves the new choice and applies it at once. Shortcuts can be changed, or each one turned off, in the
menu (**System** tab).

## Image quality

The **Quality** group of the **Frames** tab holds the options that act on the generated frames. They are on by
default and are read when the game starts.

- **Anti-tearing / anti-ghosting**: protects thin geometry (fences, wires, foliage) and moving shadows in generated
  frames.
- **Protection tuning**: *Refined* (default) or *Classic*. Try Classic if a game shows artifacts that Refined does
  not remove.
- **High-multiplier quality**: keeps generated frames clean from X3 to X6. Leave it on.
- **Fast frame generation**: the performance option described below. Leave it on.

Choose **FG model preset** and restart: **A/B use Normal; C (Lite) automatically enables Lite** with qualified
DLSS-G 310.9.16.0. There is no second mode setting to select. **Fast frame generation stays independent** and can
remain enabled with every preset. Lite keeps NVIDIA's native image policy, so Anti-tearing and Protection tuning
have no effect there, and UI recomposition is unavailable. A JSON-only override is reserved for diagnostics;
leave it at its default. See [settings](SETTINGS.md#dlsspresets).

### Fast frame generation

**Fast frame generation** reduces GPU work while preserving the intended image. Earlier measurements with
DLSS-G 310.9.1 on an RTX 3070 Ti Laptop at 1080p:

| Multiplier | Option off | Option on | Difference |
| :---: | ---: | ---: | ---: |
| X2 | 2.33 ms | 1.57 ms | −33 % |
| X6 | 6.38 ms | 5.23 ms | −18 % |

These figures come from one system; they are not a promise for every card or game and do not apply to Lite.
The option is available for individually qualified 310.9.0, 310.9.1 and 310.9.16 files. With 310.9.16 it follows
the active Normal or Lite composition path; the menu reports its availability. The new 310.9.16 quality and
performance support is locally checked but awaits in-game validation. No Lite performance gain has been measured.
The neural-network optimizations require DirectX 12; Vulkan benefits only from the image-processing optimizations.

## HUD and interface

Generated frames are cleanest when the game's interface is handled apart from the 3D scene. Three switches, in the
**Frames** tab:

| Setting | Default | What it does |
| :--- | :---: | :--- |
| **Automatic UI recomposition** | on | Uses the separate interface handling whenever the game supplies what it needs. Turn it off if generated frames are distorted or doubled in motion. |
| **UI assist (D3D12)** | on | In DirectX 12 games that do not supply a clean scene and an interface layer, RTX Encore builds them. What the game supplies itself always has priority. |
| **Force UI recomposition** | off | Asks for the separate handling even when the game does not. For testing. |

Game-specific recommendations are in [Compatibility](COMPATIBILITY.md#game-notes).

## Compatibility options

In the **System** tab:

| Setting | Default | What it does |
| :--- | :---: | :--- |
| **Disable menu detection** | off | Off: frame generation idles in menus and loading screens, which avoids crashes there. Leave it off. |
| **Force NVIDIA OTA models** | off | Uses the frame generation models downloaded by the NVIDIA app. Restart the game to apply. |
| **Graphics card series** | auto | Leave on auto unless the card is not recognized. |

## Reading the overlay

| Example | Meaning |
| :--- | :--- |
| `74 fps` | The game's frame rate; no frame generation observed |
| `74/37 fps 2x` | Displayed frame rate / rendered frame rate, with the observed multiplier |
| `296/74 fps 2x+sm` | Frame generation and Smooth Motion together |

If no DLSS Frame Generation is detected in the session, choosing a multiplier cannot create one: the menu says so.

## Limits

- RTX 20 support is experimental: it has not been validated on a physical RTX 20 card yet.
- Vulkan on RTX 20 and RTX 30 is experimental.
- Frame generation raises the displayed frame rate, not the game's responsiveness: latency grows with the
  multiplier, and the result is better when the rendered frame rate is already comfortable.
