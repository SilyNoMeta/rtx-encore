# Intel XeSS and XeFG (experimental)

[English](INTEL-ENGINES.md) · [Français](INTEL-ENGINES.fr.md) · [简体中文](INTEL-ENGINES.zh-CN.md)

For RTX Encore 1.5.0. Available versions are listed on the [releases page](https://github.com/SilyNoMeta/rtx-encore/releases).

RTX Encore 1.2.0 adds a choice of reconstruction and frame-generation engines. Leave **Game** selected to keep the game’s normal engine. Intel modes are experimental, off by default and require compatible game inputs; choosing an engine does not prove it became active.

Use **RTX Encore Hub 1.2.0 or later** to install or update the mod and its Intel dependencies together. The Hub verifies the files, includes the accompanying Intel license, and keeps the operation reversible. Disabling NVIDIA downloads does not remove the Intel dependencies required by this mod. Intel files already supplied by the game are kept separate.

## Tensor Cores: two independent controls

Select XeSS reconstruction for **XeSS Tensor Cores**, and XeFG generation for **XeFG Tensor Cores**. The switches are independent: enabling one does not enable the other or change the engine. Use Hub 1.5.0 to install the compatible runtimes and expose the new controls. Close the game when installing the new DLL; after that startup, the two switches and qualified fixed XeFG multipliers can change live. Changing the FG engine still requires restarting the game.

On an earlier test build in The Witcher 3, RTX 4090 / 621.01 at 4K, XeSS GPU time fell from 1.243 ms to 0.861 ms (about 30.7%), while rendered-source FPS rose by about 2.8–3.2%. This is **not 31% more overall FPS**. The subsequent final-pass optimization was validated in deterministic replays, including automatic exposure and fractional input sizes; that later build has no separate game retest. No general XeFG FPS or latency gain is established.

| Setting | Default | Effect |
| :--- | :---: | :--- |
| `reconstructionEngine` | `game` | `game` or `xess`; replaces compatible Super Resolution/DLAA processing with XeSS; Ray Reconstruction stays with its original engine. |
| `frameGenerationEngine` | `game` | `game`, `dlss-fg` or `xefg`. Game preserves native FG; DLSS-FG replaces qualified native Intel FG with NVIDIA DLSS FG; XeFG replaces compatible DLSS FG with Intel XeFG. Restart required. |
| `xessTensorCores` | `false` | Experimental NVIDIA Tensor Cores for XeSS Super Resolution; toggle live at the next XeSS execution. Initial qualification: RTX 4090, NVIDIA 621.01, DirectX 12 and the Intel runtime supplied by Hub. Refused preparation retains standard XeSS. |
| `xefgTensorCores` | `false` | Experimental NVIDIA Tensor Cores for XeFG; fixed X2–X6 within initialized capacity. Toggle live as command recordings renew. Initial qualification: RTX 4090, NVIDIA 621.01, DirectX 12 and the Intel runtime supplied by Hub. Refused preparation retains standard XeFG; an execution fault requires a restart. |
| `xefgMfg4VariablePacing` | `true` | Experimental pacing for XeFG above X2; disabling it can make higher factors unavailable. |
| `xefgUiHandling` | `interpolate` | `interpolate` or `game`; requesting separate game UI handling only works with supported inputs. |
| `xefgUiAlpha` | `unknown` | `unknown`, `straight` or `premultiplied`; leave unknown unless the game’s UI format is known. |

Reconstruction can switch between DLSS and XeSS while playing in compatible games. For Vulkan, install compatible Intel runtimes before starting the game; adding or replacing them requires a restart. Restart after changing the frame-generation engine, XeFG pacing or UI settings. With XeFG, changing the FPS limit also requires a restart. A requested FPS cap does not prove source-frame cadence.

HDR input and auto exposure use the common `srHdr` and `srAutoExposure` controls for the selected reconstruction engine. Both default to `game`; restart after changing them. They do not turn on display HDR or convert SDR to HDR. Incorrect input assumptions can damage image quality. DLSS models may impose auto exposure; XeSS auto exposure has a GPU cost and is not recommended for LDR input. XeSS Vulkan LDR with manual exposure remains unsupported. RR and frame generation are unchanged.

XeFG is not a way to add frame generation to every game. Higher factors depend on the accepted Intel runtime and the complete game configuration; X5/X6 remain experimental, and this beta does not establish X6 game support. A requested multiplier, an installed dependency or a generated-frame counter is not a measurement of displayed content or physical latency. Do not combine an experimental XeFG test with NR or Smooth Motion and assume that combination is covered.

Limited local Vulkan trials in No Man’s Sky and DOOM: The Dark Ages support the XeFG X2 route; the DOOM test was accepted by the maintainer in borderless mode with NR off. This does not qualify exclusive fullscreen, DirectX 11, XeFG with NR, all higher multipliers or other games. The menu/overlay can be unavailable on an Intel route; use the Hub to change settings and review its session findings.

[Settings reference](SETTINGS.md) · [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)

For native Intel FG games such as Mirage, install compatible runtimes through the Hub, select **DLSS-FG** in Encore, restart, then enable **Intel FG** in the game. Use **Fixed** for the qualified multiplier range; **Game** uses X2 on this adapter and Dynamic is unavailable. Native FSR and games already using native DLSS FG keep their engine. The maintainer validated the corrected test build in Mirage; final release binaries have no separate in-game retest.
