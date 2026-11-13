# 设置文件

*[English](SETTINGS.md) · [Français](SETTINGS.fr.md)*

所有设置都可以在[菜单](MENU-AND-OVERLAY.zh-CN.md)（**Insert**）中修改，这是最方便的方法。
本页供偏好直接编辑文件的用户参考。

## 文件

- 名称：`rtx-encore.jsonc`，位于模组文件旁边。首次启动时会创建，使用默认值。
- 格式：带 `//` 注释的 JSON。每个键都有说明其作用的注释。有些编辑器会提示注释错误，但模组能正常读取。
- 设置按分组组织。大部分实时生效；需要重启游戏时，注释会说明。
- 菜单自身的快捷键和布局保存在同一文件的 `menuState` 分组中。
- 旧版本留下的设置文件会保留原有设置并迁移：见[安装](INSTALLATION.zh-CN.md#从旧名称升级)。

设置键不包含路径或命令，因此可以直接将设置文件分享给其他人。

## 主要键

### `renderingEngines` / `xefg`

| 设置 | 默认 | 作用 |
| :--- | :---: | :--- |
| `reconstructionEngine` | `game` | `game` 或 `xess`：以 XeSS 替代兼容的 Super Resolution/DLAA 处理；Ray Reconstruction 保留原引擎。 |
| `frameGenerationEngine` | `game` | `game`、`dlss-fg` 或 `xefg`。游戏保留原生 FG；DLSS-FG 将经过资格检查的原生 Intel FG 替换为 NVIDIA DLSS FG；XeFG 将兼容 DLSS FG 替换为 Intel XeFG。需要重启。 |
| `xessTensorCores` | `false` | 实验性 NVIDIA Tensor Core XeSS Super Resolution，在下次 XeSS 执行时即时切换。初始验证：RTX 4090、NVIDIA 621.01、DirectX 12 和 Hub 提供的 Intel 运行库。其他 RTX 40、RTX 30 与 RTX 50 显卡在 NVIDIA 621.01 或 621.12 热修复驱动下以为各架构编译的同一加速路径准入，尚未在该硬件上完成游戏内验证。准备未通过时保留标准 XeSS。 |
| `xefgTensorCores` | `false` | 实验性 NVIDIA Tensor Core XeFG；在初始化容量内支持固定 X2–X6。切换在命令重新记录时生效。初始验证：RTX 4090、NVIDIA 621.01、DirectX 12 和 Hub 提供的 Intel 运行库。其他 RTX 40、RTX 30 与 RTX 50 显卡在 NVIDIA 621.01 或 621.12 热修复驱动下以为各架构编译的同一加速路径准入，尚未在该硬件上完成游戏内验证。准备未通过时保留标准 XeFG；执行故障后须重启。 |
| `xefgMfg4VariablePacing` | `true` | XeFG 高于 X2 的实验性帧间隔控制；关闭后，较高倍率可能不可用。 |
| `xefgSourcePacing` | `true` | XeFG 在 X3 及以上时：观察源帧节奏，不限制正常帧。仅在帧时长持续交替时尝试修正，并仅在节奏改善且帧率损失较小时保留。逐步放宽节奏限制，若帧时长再次交替则立即恢复有效节奏；即时生效。 |
| `xefgSourcePacingMode` | `"auto"` | `auto` 保持现有的条件式调整和 4% 的实测源帧率损失上限。`manual` 显示下方的损失上限和检查选项。更改即时生效并重新校准。 |
| `xefgSourcePacingLossPercent` | `4` | 手动源帧率损失上限，0–30%。开启自动调整时是上限而非强制损失；关闭后，完整设定值根据未限速的实测帧节奏确定并保持固定间隔。 |
| `xefgSourcePacingManualDetection` | `true` | 仅手动模式：先等待持续不均匀的帧节奏。关闭后在校准完成时应用，即使原有节奏正常。 |
| `xefgSourcePacingManualAdjustments` | `true` | 仅手动模式：保留效果检查和逐步放宽限制。关闭后保持完整设定值，直到设置或上下文变化。呈现错误仍会停止调整。 |
| `xefgUiHandling` | `interpolate` | `interpolate` 或 `game`；请求单独处理游戏界面需要兼容输入。 |
| `xefgUiAlpha` | `unknown` | `unknown`、`straight` 或 `premultiplied`；不清楚界面格式时请保留 unknown。 |

兼容游戏可在运行中切换 DLSS 与 XeSS 重建。Vulkan 游戏需要在启动前安装兼容的 Intel 运行库；添加或替换运行库后必须重启游戏。修改帧生成引擎、XeFG 帧间隔或界面设置后仍需重启。使用 XeFG 时，修改 FPS 上限也需要重启。FPS 上限不能证明源帧节奏。

[Intel XeSS 与 XeFG（实验性）](INTEL-ENGINES.zh-CN.md)

### `frameGeneration`

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `mode` | `"game"` | `"game"`（由游戏或 NVIDIA Profile Inspector 决定）、`"fixed"`、`"dynamic"` |
| `multiplier` | `4` | `2` 至 `6`，用于固定模式。`5` 和 `6` 是实验性的。 |
| `overrideAutoFrameGeneration` | `true` | 仅在 `mode` 为 `"game"` 时有效。当游戏以 Auto 模式请求帧生成时，NVIDIA 会持续判断是否值得开启，并可能自行将其关闭（例如在高负载场景中），而菜单仍显示为开启。`true`：改为以 On 模式请求，只要游戏需要就保持开启；当游戏不再提供帧生成所需内容（菜单、过场动画）时仍会短暂暂停。`false`：由 NVIDIA 决定。固定和动态模式本来就会保持开启。实时生效。 |
| `dynamicTargetFrameRate` | `0` | 动态模式的目标 FPS。`0` 跟随显示游戏的显示器的刷新率。 |
| `dynamicExperimental56` | `false` | 允许动态模式达到 X5 和 X6。 |
| `flipMeteringBypass` | `"auto"` | 平滑 X3 / X4：`"auto"`、`true` 或 `false`。`true`：由 Streamline 自行安排生成帧的间隔，而不是交给 NVIDIA 驱动（DirectX 12 和 Vulkan）；`false`：由驱动安排。`"auto"`：RTX Encore 测量 X3 及以上时生成帧到达屏幕的方式，并学习该游戏需要哪一种，写入 `flipMeteringLearned`。需重启游戏生效。参见[帧生成](FRAME-GENERATION.zh-CN.md#平滑-x3--x4)。 |
| `flipMeteringLearned` | `""` | `flipMeteringBypass` 为 `"auto"` 时由 RTX Encore 写入：`"bypass"`（Streamline 安排帧的节奏）或 `"metering"`（驱动安排）。下次启动生效；清空以重新测量。 |
| `frameGenerationWithoutGameFg` | `false` | 为没有帧生成的游戏基于显示的画面生成帧：`frameGenerationEngine` 为 `"game"` 或 `"dlss-fg"` 时使用 DLSS 帧生成（DirectX 12 和 Vulkan），为 `"xefg"` 时使用 Intel XeFG（DirectX 12、DirectX 11 和 Vulkan）。`mode` 为 `"game"` 时 X2，为 `"fixed"` 时使用所选倍率。需要游戏可执行文件旁边的帧生成文件（由 Hub 安装）；请关闭 Smooth Motion。实验性功能。需重启生效。参见[没有 DLSS 的游戏](GAMES-WITHOUT-DLSS.zh-CN.md#帧生成)。 |

NVIDIA 运行库的兼容性不再要求文件位于预设列表中。旧的 `providerPatchPolicy` 设置将被忽略。Intel 运行库检查保持不变。兼容性仍取决于运行库和游戏提供的功能。

### `dlssSuperResolution`

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `dlssRenderScale` | `"game"` | `"game"`、`"dlaa"`、`"ultra"`、`"quality"`、`"balanced"`、`"performance"`、`"ultra-performance"`、`"custom"` |
| `dlssCustomScale` | `67` | `"custom"` 时的百分比，`33` 至 `100` |
| `fastSuperResolution` | `false` | RTX 30、DirectX 12：在预设为 M 或 L 时加速 DLSS 超分辨率，在测量的系统上每个放大帧约减少 16% 至 20% 的 GPU 时间，画面与原版非常接近。在其他显卡上不起作用。需重启游戏。见[超分辨率](SUPER-RESOLUTION.zh-CN.md#fast-super-resolution-rtx-30)。 |
| `dlssWithoutGameDlss` | `false` | 没有 DLSS 的游戏：使用 DLSS 或 XeSS 对显示的画面运行 DLAA 或放大。实验性功能。参见[没有 DLSS 的游戏](GAMES-WITHOUT-DLSS.zh-CN.md)。 |
| `dlssWithoutGameDlssEngine` | `"dlss"` | 可选 `"dlss"`、`"xess"`、`"nis"` 或 `"fsr1"`。XeSS 支持 1x 到 3x，需要 Intel 运行库。NIS 和 FSR 1 是空间缩放引擎，无需 DLSS 或 Intel 运行库。即时生效。 |
| `dlssWithoutGameDlssOutput` | `"game"` | `"game"`（游戏分辨率下的 DLAA）或 `"screen"`（放大到屏幕分辨率；游戏使用较低分辨率的窗口化模式）。 |
| `presentOpticalFlow` | `"game"` | 用于额外帧生成、NR 最终画面模式和没有 DLSS 的游戏的运动来源。`"game"` 优先使用捕获到的游戏运动矢量；不可用时回退到 `"encore"`。`"encore"` 使用 RTX Encore 的着色器，`"shader"` 使用 vort_Motion 的方法，`"quality"` 或 `"fast"` 使用 NVIDIA Optical Flow。 |

### `superResolutionInputs`

| 键 | 默认值 | 值 |
| :--- | :---: | :--- |
| `srHdr` | `"game"` | `"game"`, `"on"`, `"off"` |
| `srAutoExposure` | `"game"` | `"game"`, `"on"`, `"off"` |

这两个通用 SR/DLAA 设置适用于所选重建引擎，需要重启。game 保留游戏输入标志。HDR 的 on 使用 HDR 线性输入，off 使用 LDR 输入，不会转换颜色或启用显示器 HDR。错误的强制设置可能降低画质。不改变 RR 或帧生成。

自动曝光可能改善与曝光有关的拖影，但不是通用修复。DLSS 可能强制启用（预设 L）。XeSS 自动曝光增加 GPU 开销，不推荐用于 LDR。XeSS Vulkan LDR 手动曝光仍不受支持。显式模式初始化被拒绝时，保留原提供程序且不对其应用覆盖。强制模式的画质改善尚未验证。

旧 DLSS/XeSS 输入键根据所选重建引擎迁移；存在通用键时优先使用通用键。

### `superResolutionSharpening`

| Key | Default | Effect |
| :--- | :---: | :--- |
| `srSharpening` | `"off"` | 模式：`off`（默认）、`game` 或 `custom`，即时生效。 |
| `srSharpness` | `30` | Custom 强度：0–100%，默认 30%。0 禁用效果。 |
| `spatialSharpness` | `30` | NIS 和 FSR 1 的锐化强度，0–100%。即时生效。 |
| `spatialAa` | `"off"` | DirectX 11 下可在 NIS 或 FSR 1 缩放前后使用抗锯齿。可选 `"off"`、`"smaa"` 或 `"fxaa"`。需要 SDR，也会处理 HUD。 |
| `spatialAaPlacement` | `"before"` | `"before"` 在空间缩放前按源分辨率处理，`"after"` 在缩放后按输出分辨率处理。Post-SR NR、最终画面 NR 和兼容的帧生成都在空间缩放和已配置的抗锯齿之后运行。 |
| `spatialAaQuality` | `"high"` | SMAA 1x 质量：`"low"`、`"medium"`、`"high"` 或 `"ultra"`。FXAA 使用固定预设。 |

会与游戏后续锐化叠加，高强度可能产生光晕并放大噪点。实际效果取决于游戏后续图像处理，包括 HDR。不支持的输出格式或图形路径会保持图像不变。2.5.1 之前的原版 DLSS 保留其自身锐化；无法识别版本时，额外锐化也保持关闭。Vulkan 与实际游戏画质尚未经验证。

### `dlssPresets`

三个模型选项相互独立，均需**重启游戏**。SR/RR 位于 **Image quality → DLSS Super Resolution**，FG 位于
**Frame rate → Frame generation**。这些选项不改变渲染分辨率、插帧倍率或 NR 预设。

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `dlssSrPreset` | `"game"` | `"game"`、`"default"`、`"a"`–`"f"`（旧预设）、`"j"`、`"k"`、`"l"`、`"m"` |
| `dlssRrPreset` | `"game"` | `"game"`、`"default"`、`"a"`–`"c"`（旧预设）、`"d"`、`"e"`、`"f"`（RR2，需 Ray Reconstruction DLL 310.9.1 或更新） |
| `dlssFgPreset` | `"game"` | `"game"`、`"a"`、`"b"`、`"c"` |
| `dlssFgLite` | `"game"` | 仅供诊断，JSON：`"game"`、`"off"`、`"on"` |
| `dlssNrPreset` | `"b"` | 使用 `nvngx_dlssnr.dll` 310.9.16 时的 Neural Rendering 网络：`"b"`（更小、快得多）、`"a"`（310.8.0 的网络）或 `"auto"`（由文件选择，即 B）。310.8.0 只有一个网络。实时生效。旧文件中的 `nrPreset` 会以此名称连同取值读取；其 `0` 至 `3` 仍可读取。 |

对于预设键，`"game"` 保留游戏的选择。SR/RR 选择适用于所有质量模式，包括 DLAA；`"default"` 明确请求 DLSS DLL 的默认值。
可用模型取决于安装的 DLL，NVIDIA 驱动覆盖设置或模型回退可能优先生效。选择 RR 预设不会启用 RR。
使用 DLSS Super Resolution **310.9.1.0** 和 **310.9.16.0** 时，即使 NVIDIA 驱动配置文件强制使用其他预设，在 Encore 中选择的 SR 预设仍然生效（310.9.1.0 以及 Streamline SDK 和 NVIDIA DLSS GitHub 上的 310.9.16.0 不包含预设 A 至 D）。

FG 预设选择支持以下经过逐一验证的 NVIDIA **DLSS-G 正式版二进制文件**：

- **310.6.0.0、310.7.0.0、310.7.128.0、310.7.129.0**
- **310.8.0.0、310.9.0.0、310.9.1.0**
- **310.9.16.0**，唯一接受预设 C 的版本

预设 C 是 NVIDIA 更快的帧生成性能模式，画质略有取舍。使用其他二进制文件时，已保存的 `"c"` 会被拒绝，并保留游戏的选择。

**选择 FG 预设即可：A/B 使用 Normal；C 自动启用 Lite。** Fast frame generation 保持独立，可与每个预设一起开启。
更改预设后需重启游戏。

`dlssFgLite` 是**仅供诊断的 JSON 覆盖选项**，不在菜单设置中显示。保持 `"game"` 可跟随所选预设或游戏/驱动。
仅在使用经过单独资格检查的 310.9.16.0 文件进行诊断时，`"off"` 强制 Normal，`"on"` 强制 Lite，并优先于预设。
修改后需要重启游戏；其他二进制文件会拒绝此覆盖。Lite 保留 NVIDIA 的原生图像策略，
因此 **Anti-tearing / anti-ghosting** 和 **Protection tuning** 在此模式下不生效，界面重组也不可用。Fast frame generation 仅使用适合当前合成模式的优化。
310.9.16 新增功能尚待游戏内验证；尚无 Lite 性能增益的实测结论。

此列表仅适用于预设覆盖；帧生成本身的兼容范围更广。
版本号相同并不保证接受，开发版本不在支持范围内。
其他二进制文件保留原有行为，日志会记录拒绝原因。A 使用经典插帧；B 的 UI 重组需要兼容的无 HUD/UI 输入。
模组不写入 NVIDIA 驱动配置文件。会话详情显示保存的选项、启动时的选项以及 FG 请求是否被接受，
但不能证明实际执行了哪个模型。保持 `"game"` 可保留现有行为。

### `neuralRendering`

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `nrEnabled` | `false` | 开启 Neural Rendering。 |
| `nrPlacement` | `"after-sr"` | `"after-sr"` 对应 Post-SR，`"before-sr"` 对应实验性 Pre-SR，`"present"` 对应最终画面模式，处理呈现前的画面，兼容配置可单独处理 HUD。`"present"` 独立于 `nrWithoutGameDlss`，可在兼容的 DirectX 11 配置中接在 NIS 或 FSR 1 之后，有单独的[使用条件](NEURAL-RENDERING.zh-CN.md#present-模式)。若要回退到没有最终画面模式的版本，请先手动改回 `"after-sr"`。 |
| `nrPasses` | `1` | `1` 至 `4` |
| `nrLaterPassLocalTone` | `0` | 第一遍之后应用的色调，`0` 至 `2` |
| `nrResolution` | `"render"` | `"render"`、`"output"`、`"quality"`、`"balanced"`、`"performance"`、`"ultra-performance"`、`"custom"`。Pre-SR 始终使用渲染尺寸。在最终画面模式下，`"render"` 以游戏的渲染尺寸为基准，无法获取时改用输出尺寸。 |
| `nrResolutionScale` | `67` | `"custom"` 时相对于输出尺寸的百分比，`33` 至 `100` |
| `nrIntensity` | `1` | 强度，`0` 至 `2` |
| `nrStyle` | `0` | `0` 默认、`1` 自然、`2` 电影风格 |
| `nrPass1Style` … `nrPass4Style` | `"inherit"` | `"inherit"`（跟随 `nrStyle`）、`"default"`、`"natural"`、`"cinematic"` |
| `nrLocalTone`、`nrLocalStructure` | `1` | `0` 至 `2` |
| `nrAutoMask` | `false` | 人物遮罩；`nrSkinStructure` 需要它 |
| `nrSkinStructure` | `1` | `0` 至 `2` |
| `nrHdrExposure` | `1` | HDR 游戏：提供给 NR 的亮度 |
| `nrPrecision` | `"exact"` | `"exact"` 或 `"fast"`（RTX 20 和 RTX 30）。需重启。 |
| `nrWithoutGameDlss` | `false` | 没有 DLSS 的游戏：对显示的画面运行 NR。实验性功能。参见[没有 DLSS 的游戏](GAMES-WITHOUT-DLSS.zh-CN.md)。 |

#### 中央优先 NR（实验性）

需要启用 `nrEnabled`。NR 在屏幕中央以全强度运行，越靠近边缘越轻，因为视线较少停留在边缘：速度更快，图像边缘附近的 NR 效果更柔和。菜单（**Image quality → Neural Rendering**）中只显示开关和预设；预设背后的四个数值位于 **Foveated NR: custom values** 中。实时生效。无法使用中央优先处理时，NR 照常运行。

| 预设 | 效果 |
| :--- | :--- |
| `"light"` | 大部分图像保持全强度；收益最小。 |
| `"balanced"` | 默认的折中方案。 |
| `"strong"` | 收益最大：在一款游戏中于 RTX 3070 Ti Laptop 上测得平均 FPS 提升 27%，仔细观察时边缘明显更柔和。 |
| `"custom"` | 使用下面的四个数值。 |

| 键 | 默认 | 效果 |
| :--- | :---: | :--- |
| `nrFoveation` | `false` | 在兼容的 Post-SR 和最终画面路径上启用中央优先 NR。 |
| `nrFoveaPreset` | `"balanced"` | `"light"`、`"balanced"`、`"strong"` 或 `"custom"`。 |
| `nrFoveaWidth` | `55` | 使用 `"custom"` 时：全强度中央区域的宽度，占屏幕的 1–100%。 |
| `nrFoveaHeight` | `55` | 使用 `"custom"` 时：全强度中央区域的高度，占屏幕的 1–100%。 |
| `nrFoveaFalloff` | `8` | 使用 `"custom"` 时：每侧过渡宽度，占屏幕的 0–100%。 |
| `nrFoveaEdge` | `50` | 使用 `"custom"` 时：边缘保留的 NR 细节，25–100%；越低越快、越柔和。 |

#### NR 性能选项

这些设置也在 **Image quality → Neural Rendering → Performance** 分组中。
测量结果和画面取舍见 [Neural Rendering](NEURAL-RENDERING.zh-CN.md#rtx-30-性能设置)。
引擎专用选项仅作用于下表所列引擎；切换引擎或修改启动时读取的选项，需要重启游戏。

| 键 | 默认 | 收益与取舍 |
| :--- | :---: | :--- |
| `nrPaddingAware` | `false` | 允许 NR 使用略小的相邻尺寸，每个方向最多减少 2 %，不改变 DLSS 渲染尺寸。支持兼容的 Post-SR 和最终画面路径，可与中央优先 NR 同时使用。可能改变画面。实时生效。 |
| `nrMaxInFlight` | `0` | 支持 Reflex 的 DirectX 12 游戏中的 NVIDIA 引擎：`0` 不限制，`1` 或 `2` 个待完成的 NR 帧；帧生成启用时至少使用 `2`。实时生效，游戏内性能影响仍待验证。 |

### `overlay`

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `showOverlay` | `false` | 显示叠加层。 |
| `overlayPosition` | `"top-left"` | `"top-left"`、`"top-right"`、`"bottom-left"`、`"bottom-right"` |
| `overlayFontSize` | `14` | 文字大小，`10` 至 `32` 像素 |
| `overlayStyle` | `"card"` | `"minimal"` 紧凑文字、`"card"` 单面板、`"dashboard"` 分组面板及使用率条、`"legacy"` 经典白色文字条 |
| `overlayLabels`、`overlayUnits` | `true` | 分别显示指标名称和单位；关闭两者即可只显示数值 |
| `overlayOpacity` | `75` | 背景不透明度，`0` 至 `100`；文字保持清晰 |
| `overlayGraph` | `false` | 最近源帧的呈现间隔，单位为毫秒 |
| `overlayShowFps`、`overlayShowSourceFps` | `true` | 分别选择输出 FPS 和渲染／源 FPS |
| `overlayShowGpuLoad`、`overlayShowGpuTemperature`、`overlayShowGpuPower`、`overlayShowGpuClock`、`overlayShowGpuMemoryClock` | `false` | 独立选择 GPU 负载、温度、功耗、核心频率和显存频率；缺少的键继承旧设置 `overlayShowGpu` |
| `overlayShowCpu`、`overlayShowRam` | `false` | 整个系统的 CPU 负载和物理内存使用情况 |
| `overlayShowNr`、`overlayShowFramePacing`、`overlayShowLatency`、`overlayShowVram`、`overlayShowVersions` | `false` | 可选指标及引擎信息 |

在叠加层菜单中选择样式和内容预设，然后单独调整各项指标。
这些新样式和设置目前适用于 `+overlay-studio` 开发版；配套开发版 Hub
仅在安装了该版本时提供这些选项。

不可用的传感器显示 `--`，不会显示为错误的零值。GPU 和显存数据覆盖整张
NVIDIA 显卡；安装多张 NVIDIA 显卡时，第一张显卡可能不是游戏的渲染显卡。
CPU 和内存数据覆盖整个系统。图表测量源帧呈现间隔，不代表生成帧或 GPU
渲染时间。输出 FPS 使用观测到的 FG 倍率；Smooth Motion 包含标称 2× 估算。
Reflex 和 XeLL 报告不测量完整的显示延迟。

### `hudUi`

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `autoUiRecomposition` | `true` | 游戏提供所需内容时，使用界面单独处理 |
| `uiAssist` | `true` | DirectX 12：游戏未提供无界面场景和界面层时，由模组构建 |
| `forceUiRecomposition` | `false` | 即使游戏没有要求，也请求单独处理 |

### `nativeMenuFeatures`

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `reflexFrameLimit` | `0` | `0` 恢复游戏的 Reflex 设置；`1` 至 `1000` 请求帧率上限。FG 的计入方式取决于游戏与驱动 |
| `reflexMode` | `game` | `game` 跟随游戏（默认），`on` / `on-boost` 请求 Reflex On / On+Boost，绝不低于游戏；实时生效，与上限独立，Dynamic 及 XeLL 下暂停覆盖。不保证降低延迟 |
| `vsync` | `game` | `game`：使用游戏自身的 V-Sync；`off`：请求 DirectX 游戏关闭 V-Sync；`on`：请求其开启 V-Sync，包括游戏在启用帧生成后禁用自身 V-Sync 选项，或 NVIDIA 控制面板 V-Sync 在笔记本上无效的情况。NVIDIA 控制面板强制设置的优先级更高。对 Vulkan 游戏和 XeSS-FG 无效。在 60 Hz 屏幕上使用高倍数时，`on` 可能增加延迟。即时生效。取代 `vsyncOff`，其值会被保留 |
| `hairEnabled` | `true` | 在具备该功能的游戏中启用兼容的光线追踪毛发。需重启。 |

### `compatibility`

| 键 | 默认值 | 取值 |
| :--- | :---: | :--- |
| `disableInGameMenu` | `false` | `true` 时不附加 RTX Encore 菜单，用于拒绝该菜单的游戏（Star Wars Outlaws）。帧生成仍可正常工作；请在此文件中或通过 RTX Encore Hub 更改设置。重启游戏后生效。 |

### `diagnostics`

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `logPerformance` | `false` | 将帧时间写入 CSV，并将 GPU 温度、功耗和性能限制原因写入 `rtx-encore-logs` 中的会话日志 |
| `logFilesKept` | `3` | 保留的常规日志数及同等数量的异常日志，`1` 至 `100` |
| `logDxgiWrappers` | `false` | 在会话日志和异常日志中记录游戏对菜单交换链的每一次调用及调用模块。用于菜单附加后发生的崩溃。重启后生效 |
| `crashDetails` | `false` | 异常日志会额外记录寄存器指向的内存和堆栈；游戏或 RTX Encore 崩溃时会在 `rtx-encore-logs` 中留下 `.dmp` 内存快照（通常有数十 MB）。用于崩溃报告，之后请关闭。重启后生效 |

### `keyboardShortcuts`

每个 `hotkey…` 键可以接受一个或多个用逗号分隔的组合键，例如
`"hotkeyFixed4": "Ctrl+Alt+4, Ctrl+Alt+Num4"`。空字符串 `""` 停用该操作。
`"disableKeybinds": true` 停用全部快捷键。

## 其他键

文件中的键比本页列出的更多，包括 Smooth Motion、画质和兼容性开关，以及其他性能选项。
每个键在文件中有注释，在菜单中有对应的设置；推荐从菜单修改。

### `neuralRendering.passTuning`

四轮 NR 处理中的每一轮都可替代强度、局部色调、局部结构、皮肤结构和角色遮罩，也可继承全局值。数值范围为 0 至 2；遮罩可选 `inherit`、`true` 或 `false`。首轮之后继承的色调来自 `nrLaterPassLocalTone`。强度为零仍会运行该轮处理，不会消除 GPU/VRAM 开销。

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `nrPass1Intensity` … `nrPass4Intensity` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1LocalStructure` … `nrPass4LocalStructure` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1LocalTone` … `nrPass4LocalTone` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1SkinStructure` … `nrPass4SkinStructure` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1AutoMask` … `nrPass4AutoMask` | `"inherit"` | `"inherit"`, `true`, `false` |


## 2.3.0

### 默认使用游戏运动矢量

为有 DLSS Super Resolution、但没有 DLSS Frame Generation 的游戏添加帧生成时，Game motion vectors 在 DirectX 12、DirectX 11 和 Vulkan 上优先使用游戏提供的运动矢量，不可用时自动回退至 RTX Encore 估算。兼容的最终画面 NR 也可使用这些矢量。保留已有的明确选择。此前的 DX11/Vulkan 游戏输入路径继续保留；2.3.0 提供统一选项并将其设为默认。模组与 Hub 现在一致显示全部五个选项。

### NR 外观与曝光

模组和 Hub 提供标准、柔和、自然、电影、人脸、柔肤及 NVIDIA 参考七种外观。应用外观会修改全局外观并将逐遍外观覆盖恢复为继承，不改变遍数、模型、分辨率或曝光。NR 曝光提供游戏、自动、测量和手动模式，适用于兼容的场景线性 DLSS 输入。新配置使用自动；已有文件缺少该键时保留游戏模式。

### 更有针对性的 XeFG 帧节奏

从 X3 起，自动模式先检测持续不均匀的游戏帧间隔，再尝试修正；只有在源帧率损失预算内改善节奏时才保留修正。逐步放宽限制，必要时恢复有效修正。手动模式可实时调整 0–30% 损失预算、检测和自动调整开关。自动模式采用 4% 预算。
