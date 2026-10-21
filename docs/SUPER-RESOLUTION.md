# Super Resolution

*[Français](SUPER-RESOLUTION.fr.md) · [中文](SUPER-RESOLUTION.zh-CN.md)*

DLSS Super Resolution is the game's own upscaler: the game renders at a lower resolution and DLSS rebuilds the
full image. With the Game engine selected, RTX Encore keeps it. It lets you choose the render resolution and the model preset, and, on
RTX 30 cards, offers an option that makes the upscaling cheaper for the GPU.

For the render resolution, see [Menu and overlay](MENU-AND-OVERLAY.md#other-controls). For the preset choice, see
[Settings](SETTINGS.md#dlsspresets).

## Intel XeSS and XeFG (experimental)

RTX Encore 1.2.0 adds a choice of reconstruction and frame-generation engines. Leave **Game** selected to keep the game’s normal engine. Intel modes are experimental, off by default and require compatible game inputs; choosing an engine does not prove it became active. HDR input and auto exposure use the common `srHdr` and `srAutoExposure` controls for the selected reconstruction engine. Both default to `game`; restart after changing them. They do not turn on display HDR or convert SDR to HDR. Incorrect input assumptions can damage image quality. DLSS models may impose auto exposure; XeSS auto exposure has a GPU cost and is not recommended for LDR input. XeSS Vulkan LDR with manual exposure remains unsupported. RR and frame generation are unchanged.

[Intel XeSS and XeFG (experimental)](INTEL-ENGINES.md)

## Fast super resolution (RTX 30)

**Fast super resolution (RTX 30)** lowers the GPU time spent on DLSS Super Resolution. On RTX 30 cards (Ampere), in
DirectX 12, when the game uses DLSS Super Resolution with preset **M** or **L**, RTX Encore runs the upscaling with
a faster version of the model, calibrated on captures from real games. The render resolution, the quality mode and
the preset do not change.

It is **off by default**. Turn it on in the menu: **Image → DLSS Super Resolution → Fast super resolution (RTX 30)**.
In the settings file, the key is [`fastSuperResolution`](SETTINGS.md#dlsssuperresolution).
**Restart the game** for the change to apply, as for the model preset.

### Requirements

- An **RTX 30** card. On RTX 20, RTX 40 and RTX 50 cards the option stays inactive and changes nothing.
- A **DirectX 12** game.
- DLSS Super Resolution file `nvngx_dlss.dll` in version **310.9.x**; **310.9.16** is recommended. See
  [Compatibility](COMPATIBILITY.md#dlss-super-resolution-versions).
- **Preset M or L** in use. If the game asks for another preset, set **SR model preset** to **M** or **L** in the same
  section of the menu (restart the game after changing a preset, as for every preset).

### What to expect

Measured on an RTX 3070 Ti Laptop, the option removes about **16 to 20 %** of the GPU time spent on each upscaled
frame, at 1080p and at 1440p. In these measurements the image stays very close to the original, and the
differences are not visible to the eye.

These figures come from one system. They are not a promise for every card, game or scene.

### Limits

- Off by default.
- No effect with Ray Reconstruction.
- DLAA (render resolution equal to the output resolution) is not covered: it keeps NVIDIA's model.
- If the NVIDIA driver profile of the game forces another preset (the DLSS override of the NVIDIA App), the preset
  chosen in RTX Encore still applies with DLSS 310.9.1 and 310.9.16. With other versions the driver wins, the game
  runs a transformer model (presets J and K) and the option changes nothing; the log says so.
- If the game or the version of the DLSS file is not recognized, RTX Encore leaves the original Super Resolution
  in place, without any error message. The menu and the game behave as if the option were off.
- The image is not identical, bit for bit, to NVIDIA's: it is very close, not the same.
- Measured on a single system: results can differ on other cards, games and drivers.
