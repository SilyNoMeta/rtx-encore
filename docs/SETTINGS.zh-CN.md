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
| `frameGenerationEngine` | `game` | `game` 或 `xefg`：在受支持的 DirectX 12 或 Vulkan 路径上使用 XeFG。DirectX 11 仍属实验性，此处未完成游戏认证。 |
| `xefgMfg4VariablePacing` | `true` | XeFG 高于 X2 的实验性帧间隔控制；关闭后，较高倍率可能不可用。 |
| `xefgUiHandling` | `interpolate` | `interpolate` 或 `game`；请求单独处理游戏界面需要兼容输入。 |
| `xefgUiAlpha` | `unknown` | `unknown`、`straight` 或 `premultiplied`；不清楚界面格式时请保留 unknown。 |

修改引擎、XeFG 帧间隔或界面设置后请重启游戏。使用 XeFG 时，修改 FPS 上限也需要重启。FPS 上限不能证明源帧节奏。

[Intel XeSS 与 XeFG（实验性）](INTEL-ENGINES.zh-CN.md)

### `frameGeneration`

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `mode` | `"game"` | `"game"`（由游戏或 NVIDIA Profile Inspector 决定）、`"fixed"`、`"dynamic"` |
| `multiplier` | `4` | `2` 至 `6`，用于固定模式。`5` 和 `6` 是实验性的。 |
| `dynamicTargetFrameRate` | `0` | 动态模式的目标 FPS。`0` 跟随显示游戏的显示器的刷新率。 |
| `dynamicExperimental56` | `false` | 允许动态模式达到 X5 和 X6。 |
| `flipMeteringBypass` | `false` | 平滑 X3 / X4：由 Streamline 自行安排生成帧的间隔，而不是交给 NVIDIA 驱动。需重启游戏生效。 |

本地策略 `providerPatchPolicy` 默认为 `heuristic`。`exact` 仅接受已审查的 NVIDIA 提供程序文件；`heuristic` 还允许与其他符合条件的签名 NVIDIA 文件进行受限兼容。该准入仍未经认证，并会明确报告。修改后需重启。Hub 不会在社区配置中分享此设置；它不是游戏兼容性承诺。

### `dlssSuperResolution`

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `dlssRenderScale` | `"game"` | `"game"`、`"dlaa"`、`"quality"`、`"balanced"`、`"performance"`、`"ultra-performance"`、`"custom"` |
| `dlssCustomScale` | `67` | `"custom"` 时的百分比，`50` 至 `100` |
| `fastSuperResolution` | `false` | RTX 30、DirectX 12：在预设为 M 或 L 时加速 DLSS 超分辨率，在测量的系统上每个放大帧约减少 16% 至 20% 的 GPU 时间，画面与原版非常接近。在其他显卡上不起作用。需重启游戏。见[超分辨率](SUPER-RESOLUTION.zh-CN.md#fast-super-resolution-rtx-30)。 |

### `superResolutionInputs`

| 键 | 默认值 | 值 |
| :--- | :---: | :--- |
| `srHdr` | `"game"` | `"game"`, `"on"`, `"off"` |
| `srAutoExposure` | `"game"` | `"game"`, `"on"`, `"off"` |

这两个通用 SR/DLAA 设置适用于所选重建引擎，需要重启。game 保留游戏输入标志。HDR 的 on 使用 HDR 线性输入，off 使用 LDR 输入，不会转换颜色或启用显示器 HDR。错误的强制设置可能降低画质。不改变 RR 或帧生成。

自动曝光可能改善与曝光有关的拖影，但不是通用修复。DLSS 可能强制启用（预设 L）。XeSS 自动曝光增加 GPU 开销，不推荐用于 LDR。XeSS Vulkan LDR 手动曝光仍不受支持。显式模式初始化被拒绝时，保留原提供程序且不对其应用覆盖。强制模式的画质改善尚未验证。

旧 DLSS/XeSS 输入键根据所选重建引擎迁移；存在通用键时优先使用通用键。

### `dlssPresets`

三个模型选项相互独立，均需**重启游戏**。SR/RR 位于 **Image → DLSS Super Resolution**，FG 位于
**Frames → Frame generation**。这些选项不改变渲染分辨率、插帧倍率或 NR 预设。

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `dlssSrPreset` | `"game"` | `"game"`、`"default"`、`"a"`–`"f"`（旧预设）、`"j"`、`"k"`、`"l"`、`"m"` |
| `dlssRrPreset` | `"game"` | `"game"`、`"default"`、`"a"`–`"c"`（旧预设）、`"d"`、`"e"` |
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
| `nrEngine` | `"nvidia"` | `"nvidia"` 或 `"opendlss"`（高度实验性）。需重启。 |
| `nrPasses` | `1` | `1` 至 `4` |
| `nrLaterPassLocalTone` | `0` | 第一遍之后应用的色调，`0` 至 `2` |
| `nrResolution` | `"render"` | `"render"`、`"output"`、`"quality"`、`"balanced"`、`"performance"`、`"ultra-performance"`、`"custom"` |
| `nrResolutionScale` | `67` | `"custom"` 时相对于输出尺寸的百分比，`33` 至 `100` |
| `nrIntensity` | `1` | 强度，`0` 至 `2` |
| `nrStyle` | `0` | `0` 默认、`1` 自然、`2` 电影风格 |
| `nrPass1Style` … `nrPass4Style` | `"inherit"` | `"inherit"`（跟随 `nrStyle`）、`"default"`、`"natural"`、`"cinematic"` |
| `nrLocalTone`、`nrLocalStructure` | `1` | `0` 至 `2` |
| `nrAutoMask` | `false` | 人物遮罩；`nrSkinStructure` 需要它 |
| `nrSkinStructure` | `1` | `0` 至 `2` |
| `nrHdrExposure` | `1` | HDR 游戏：提供给 NR 的亮度 |
| `nrPrecision` | `"exact"` | `"exact"` 或 `"fast"`（RTX 20 和 RTX 30）。需重启。 |

#### NR 性能选项

这些设置也在 **Image → Neural Rendering → Performance** 分组中。
测量结果和画面取舍见 [Neural Rendering](NEURAL-RENDERING.zh-CN.md#rtx-30-性能设置)。
引擎专用选项仅作用于下表所列引擎；切换引擎或修改启动时读取的选项，需要重启游戏。

| 键 | 默认 | 收益与取舍 |
| :--- | :---: | :--- |
| `nrPaddingAware` | `false` | 允许 NR 使用略小的相邻尺寸，每个方向最多减少 2 %，不改变 DLSS 渲染尺寸。可能改变画面。实时生效。 |
| `nrMaxInFlight` | `0` | 支持 Reflex 的 DirectX 12 游戏中的 NVIDIA 引擎：`0` 不限制，`1` 或 `2` 个待完成的 NR 帧；帧生成启用时至少使用 `2`。实时生效，游戏内性能影响仍待验证。 |

### `openExperimental`

Open 的性能键位于此独立分组。请先在菜单中选择引擎和适合显卡的后端；这些键本身不会开启 Open。

| 键 | 默认 | 收益与取舍 |
| :--- | :---: | :--- |
| `nrOpenFast` | `false` | RTX 30 上的 Open：Fast 精度，NR 时间更短，画面有轻微差异，不增加显存。需重启。 |
| `nrOpenVramForSpeed` | `false` | RTX 30 上的 Open：NR 时间更短，画面一致；在 1440p、DLSS Balanced 下约多占 160 MB 显存。需重启。 |
| `nrOpenFastProjection` | `true` | RTX 30 上的 Open：处理更快，画面有轻微差异。需重启。 |
| `nrOpenUltraFast` | `false` | Open，一遍 NR：**Ultra-fast mode**，在文档中的对比里 NR 时间约减半。NR 层更新较晚，运动时可能出现瑕疵，多占几 MB 显存。需重启。 |
| `nrOpenUltraFastGhostTolerance` | `0.03` | `0` 至 `0.3`；越低，重影越少，但更多表面暂时等待 NR。实时生效。 |
| `nrOpenUltraFastFillTolerance` | `0.03` | `0` 至 `0.3`；控制刚出现的表面用相似表面的 NR 效果填充的程度。`0` 关闭填充。实时生效。 |

`nrResolution: "ultra-performance"` 将 NR 设置为输出宽度和高度的 33 %。它与 `nrOpenUltraFast` 和
`dlssRenderScale` 相互独立，这些设置不会自动开启彼此。

### `overlay`

| 键 | 默认 | 值 |
| :--- | :---: | :--- |
| `showOverlay` | `false` | 显示叠加层。 |
| `overlayPosition` | `"top-left"` | `"top-left"`、`"top-right"`、`"bottom-left"`、`"bottom-right"` |
| `overlayFontSize` | `14` | 文字大小，`10` 至 `32` 像素 |
| `overlayShowNr`、`overlayShowFramePacing`、`overlayShowGpu`、`overlayShowVram`、`overlayShowVersions` | `false` | 可选显示项目 |

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
| `vsyncOff` | `false` | 请求 DirectX 游戏关闭 V-Sync |
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

文件中的键比本页列出的更多，包括 Smooth Motion、画质和兼容性开关，以及 Open 引擎的实验性选项。
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
