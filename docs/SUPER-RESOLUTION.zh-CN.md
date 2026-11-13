# 超分辨率

*[English](SUPER-RESOLUTION.md) · [Français](SUPER-RESOLUTION.fr.md)*

DLSS 超分辨率是游戏自带的放大功能：游戏以较低分辨率渲染，由 DLSS 重建完整画面。选择 Game 引擎时，RTX Encore 保留它，并让你选择渲染分辨率和模型预设，并在
RTX 30 显卡上提供一个降低 GPU 开销的选项。

渲染分辨率见[菜单和叠加层](MENU-AND-OVERLAY.zh-CN.md#其他控制)，预设的选择见[设置文件](SETTINGS.zh-CN.md#dlsspresets)。

**没有 DLSS 的游戏**自 2.0.0 起可以从 RTX Encore 获得 DLSS 或 XeSS 超分辨率：对游戏显示的画面运行 DLAA，或把较低的游戏分辨率放大到
屏幕分辨率，支持 DirectX 11、DirectX 12 和 Vulkan。SR 模型预设和下文的锐化同样适用。参见[没有 DLSS 的游戏](GAMES-WITHOUT-DLSS.zh-CN.md#超分辨率)。

## 重建后锐化

默认关闭。在 **Image quality > Super Resolution** 中选择 **Off**、**Game** 或 **Custom**。Custom 的强度范围为 0–100%，默认 30%，运行中即时生效。Game 使用游戏提供的锐化强度，游戏未提供时不生效。无效设置将禁用效果。

适用于 DLSS 超分辨率、DLAA、光线重建和替换 XeSS，也适用于快速超分辨率。启用 Neural Rendering 时，锐化在其后执行。不会锐化覆盖层或之后绘制的 HUD。Off 和强度 0 不增加锐化处理。

会与游戏后续锐化叠加，高强度可能产生光晕并放大噪点。实际效果取决于游戏后续图像处理，包括 HDR。不支持的输出格式或图形路径会保持图像不变。2.5.1 之前的原版 DLSS 保留其自身锐化；无法识别版本时，额外锐化也保持关闭。Vulkan 与实际游戏画质尚未经验证。

## Intel XeSS 与 XeFG（实验性）

RTX Encore 1.2.0 可选择重建和帧生成引擎。保留 **Game** 可继续使用游戏原有引擎。Intel 模式仍属实验性，默认关闭，需要兼容的游戏输入；选择某个引擎不能证明它已激活。 HDR 输入和自动曝光使用所选重建引擎共用的 `srHdr` 与 `srAutoExposure`。默认 `game` 跟随游戏，修改后需重启。它们不会启用显示器 HDR，也不会将 SDR 转换为 HDR。错误的输入格式会损害画质。某些 DLSS 模型强制使用自动曝光；XeSS 自动曝光有 GPU 开销，不推荐用于 LDR。XeSS Vulkan 的 LDR 与手动曝光组合仍不受支持。RR 和帧生成保持不变。

[Intel XeSS 与 XeFG（实验性）](INTEL-ENGINES.zh-CN.md)

## Fast super resolution (RTX 30)

**Fast super resolution (RTX 30)** 可减少 DLSS 超分辨率占用的 GPU 时间。在 RTX 30（Ampere）显卡上，于 DirectX 12 下，当游戏使用预设为
**M** 或 **L** 的 DLSS 超分辨率时，RTX Encore 会改用一个更快的模型版本来完成放大，该版本根据真实游戏的捕获画面校准。渲染分辨率、画质模式和预设均不改变。

此选项**默认关闭**。在菜单中开启：**Image quality → DLSS Super Resolution → Fast super resolution (RTX 30)**。在设置文件中，对应的键是
[`fastSuperResolution`](SETTINGS.zh-CN.md#dlsssuperresolution)。
更改后需**重启游戏**才会生效，与模型预设相同。

### 要求

- **RTX 30** 显卡。在 RTX 20、RTX 40 和 RTX 50 显卡上，该选项保持未启用，不起任何作用。
- **DirectX 12** 游戏。
- DLSS 超分辨率文件 `nvngx_dlss.dll` 版本为 **310.9.x**，推荐 **310.9.16**。见
  [兼容性](COMPATIBILITY.zh-CN.md#dlss-超分辨率版本)。
- 正在使用**预设 M 或 L**。如果游戏要求其他预设，请在菜单的同一部分把 **SR model preset** 设为 **M** 或 **L**（与所有预设一样，更改预设后需重启游戏）。

### 预期效果

在 RTX 3070 Ti Laptop 上测得，1080p 和 1440p 下，每个放大帧的 GPU 时间约减少 **16% 至 20%**。在这些测量中，画面与原版非常接近，差异肉眼不可见。

这些数据来自单一系统，不代表每张显卡、每款游戏或每个场景都能达到。

### 限制

- 默认关闭。
- 与 Ray Reconstruction（光线重建）同时使用时无效。
- 不适用于 DLAA（渲染分辨率等于输出分辨率）：DLAA 保留 NVIDIA 的模型。
- 如果游戏的 NVIDIA 驱动配置文件强制使用其他预设（NVIDIA App 的 DLSS 覆盖），在 DLSS 310.9.1 和 310.9.16 下 RTX Encore 中选择的预设仍然生效。其他版本下驱动优先，游戏运行 Transformer 模型（预设 J 和 K），该选项不起作用；日志会说明原因。
- 如果游戏或 DLSS 文件的版本未被识别，RTX Encore 会保留原有的超分辨率，且不显示任何错误信息。菜单和游戏的表现与该选项关闭时一致。
- 画面与 NVIDIA 的结果并非逐位一致：非常接近，但不完全相同。
- 仅在单一系统上测量：其他显卡、游戏和驱动下结果可能不同。

## 渲染比例与锐化

原生 DLSS SR 和 XeSS 重建新增 **Ultra（75%）**，与 DLAA、质量、平衡、性能及超级性能并列。它请求输出宽高各 75% 的输入分辨率（4K 对应 2880 × 1620），属于分辨率设置，不是模型预设。游戏需要遵循该分辨率请求。没有原生重建的游戏请使用[没有 DLSS 的游戏](GAMES-WITHOUT-DLSS.zh-CN.md)中的窗口模式输入分辨率方法。

选择**锐化 → 自定义**后，可将强度设为 0–100%（默认 30%）。设置实时生效；锐化默认关闭，可能叠加游戏自身的锐化。

## 运行库可用性与版本

缺少运行库或版本不兼容时，菜单不提供对应引擎。旧版兼容运行库仍受支持：不会仅因存在更新版本而拒绝 NVIDIA 310.9.1。菜单中的警告会建议通过 RTX Encore Hub 更新并重启游戏。推荐版本：NVIDIA **310.9.16**、XeSS-SR **2.0.2.68**、XeFG **1.3.1.78**、XeLL **1.3.2.10**。添加的 DLSS 帧生成有更严格的要求：必须使用 **Streamline 2.14.16 + DLSS-G 310.9.16**。更新无法让不受支持的游戏或 GPU 自动变得兼容。


## 2.4.0

DLSS Super Resolution 自定义输入比例现最低为每轴 33%，Hub 同步调整范围。更低输入分辨率以源细节换取较低渲染开销，结果取决于游戏和 GPU。
