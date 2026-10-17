# 超分辨率

*[English](SUPER-RESOLUTION.md) · [Français](SUPER-RESOLUTION.fr.md)*

DLSS 超分辨率是游戏自带的放大功能：游戏以较低分辨率渲染，由 DLSS 重建完整画面。RTX Encore 并不替换它，而是让你选择渲染分辨率和模型预设，并在
RTX 30 显卡上提供一个降低 GPU 开销的选项。

渲染分辨率见[菜单和叠加层](MENU-AND-OVERLAY.zh-CN.md#其他控制)，预设的选择见[设置文件](SETTINGS.zh-CN.md#dlsspresets)。

## Fast super resolution (RTX 30)

**Fast super resolution (RTX 30)** 可减少 DLSS 超分辨率占用的 GPU 时间。在 RTX 30（Ampere）显卡上，于 DirectX 12 下，当游戏使用预设为
**M** 或 **L** 的 DLSS 超分辨率时，RTX Encore 会改用一个更快的模型版本来完成放大，该版本根据真实游戏的捕获画面校准。渲染分辨率、画质模式和预设均不改变。

此选项**默认关闭**。在菜单中开启：**Image → DLSS Super Resolution → Fast super resolution (RTX 30)**。在设置文件中，对应的键是
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
