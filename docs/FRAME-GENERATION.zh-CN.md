# 帧生成

*[English](FRAME-GENERATION.md) · [Français](FRAME-GENERATION.fr.md)*

RTX Encore 为 RTX 20、RTX 30 和 RTX 40 显卡带来 X2 至 X6 的 DLSS 多帧生成。

## Intel XeSS 与 XeFG（实验性）

RTX Encore 1.2.0 可选择重建和帧生成引擎。保留 **Game** 可继续使用游戏原有引擎。Intel 模式仍属实验性，默认关闭，需要兼容的游戏输入；选择某个引擎不能证明它已激活。

[Intel XeSS 与 XeFG（实验性）](INTEL-ENGINES.zh-CN.md)

## 要求

- 游戏提供 DirectX 12 或 Vulkan 原生 **DLSS 帧生成**，或兼容的 DirectX 12 原生 **Intel FG** 输入。后者请使用实验性 [DLSS-FG 引擎](INTEL-ENGINES.zh-CN.md)，通过 Hub 安装符合要求的运行库，重启后在游戏中启用 Intel FG。原生 FSR 保持不变。
- 受支持的 `nvngx_dlssg.dll` 和兼容运行库：[兼容性](COMPATIBILITY.zh-CN.md#dlss-帧生成版本)。可用功能取决于精确版本。
- 在游戏中启用帧生成。游戏没有兼容的原生 FG 输入时，请参阅 [Smooth Motion](SMOOTH-MOTION.zh-CN.md)。

## 模式

在菜单（**Frames** 选项卡）中或用快捷键选择模式。

| 模式 | 作用 |
| :--- | :--- |
| **Game decides**（默认） | 由游戏或 NVIDIA Profile Inspector 决定倍率。 |
| **Fixed** | 始终使用你选择的倍率：X2、X3、X4、X5 或 X6。 |
| **Dynamic** | 倍率随帧率变化以达到目标。目标是显示游戏的显示器的刷新率，或你设定的数值。 |

倍率是指游戏每渲染一帧所显示的帧数：X3 表示每个渲染帧之后显示两个生成帧。

- **X5 和 X6 是实验性的。** 它们需要大量显存，而且倍率越高延迟越大。
- 除非开启 **Allow 5x and 6x**，否则动态模式最高为 X4。
- 在 Vulkan 游戏中，动态模式根据实测帧率和目标在 X2 到 X6 之间选择。
- 8 GB 显存的显卡请从 X2 或 X3 开始，或在高/中纹理质量下使用 X4。

### 平滑 X3 / X4

在部分游戏中，X3 和 X4 看起来比 X2 更卡顿：生成帧成批到达屏幕，而不是依次显示。**Smooth out X3 / X4**（`flipMeteringBypass`，默认关闭）让 Streamline 自行安排生成帧的间隔，而不是交给 NVIDIA 驱动，适用于 DirectX 12 和 Vulkan。可在菜单中倍率下方或在 RTX Encore Hub 中开启，然后重启游戏。在驱动已能正确安排帧间隔的游戏中，它没有可见效果。

在符合条件的旧 Streamline 中，DX12 动态模式使用 Encore 自适应 X2–X4。从 X2 开始，依据游戏源帧率调整，在切换之间等待，并在游戏拒绝请求时暂停。符合条件的新版保留原生动态 MFG。见[版本矩阵](COMPATIBILITY.zh-CN.md#streamline-版本)。D3D11 仅提供独立的 Smooth Motion X2。

## 快捷键

| 快捷键 | 作用 |
| :--- | :--- |
| `Ctrl + Alt + 2…6` | 固定倍率 X2 至 X6 |
| `Ctrl + Alt + PageUp / PageDown` | 提高或降低倍率（切换到固定模式） |
| `Ctrl + Alt + D` | 在固定模式和动态模式之间切换 |
| `Ctrl + Alt + ↑` 或 `+` | 将动态模式的目标提高 5 FPS（按住 `Shift`：1 FPS） |
| `Ctrl + Alt + ↓` 或 `-` | 将动态模式的目标降低 5 FPS（按住 `Shift`：1 FPS） |
| `Ctrl + Alt + G` | 回到 **Game decides** |

快捷键会保存新的选择并立即应用。可以在菜单（**System** 选项卡）中修改快捷键，或逐个停用。

## 画质

**Frames** 选项卡的 **Quality** 分组包含作用于生成帧的选项。它们默认开启，并在游戏启动时读取。

- **Anti-tearing / anti-ghosting**：保护生成帧中的细小几何体（栅栏、电线、植被）和移动阴影。
- **Protection tuning**：*Refined*（默认）或 *Classic*。如果某个游戏出现 Refined 无法消除的瑕疵，再尝试 Classic。
- **High-multiplier quality**：让 X3 到 X6 的生成帧保持干净。请保持开启。
- **Fast frame generation**：下文所述的性能选项。请保持开启。

选择 **FG model preset** 后重启游戏：**A/B 使用 Normal；C (Lite) 自动启用 Lite**，需要通过资格检查的
DLSS-G 310.9.16.0。无需再选择第二个模式。**Fast frame generation 保持独立**，可与每个预设一起开启。
Lite 保留 NVIDIA 的原生图像策略，因此 Anti-tearing 和 Protection tuning 在此模式下无效，界面重组也不可用。
仅供诊断的覆盖选项保留在 JSON 中，请保持默认值。见[设置](SETTINGS.zh-CN.md#dlsspresets)。

### Fast frame generation

**Fast frame generation** 在保持预期画面的同时减少 GPU 工作量。以下为此前使用 DLSS-G 310.9.1，
在 RTX 3070 Ti Laptop、1080p 下的测量结果：

| 倍率 | 选项关闭 | 选项开启 | 差异 |
| :---: | ---: | ---: | ---: |
| X2 | 2.33 ms | 1.57 ms | −33 % |
| X6 | 6.38 ms | 5.23 ms | −18 % |

这些数据来自单一系统，并不代表所有显卡或游戏都能达到，也不适用于 Lite。
该选项支持经过逐一资格检查的 310.9.0、310.9.1 和 310.9.16 文件。使用 310.9.16 时，它会匹配当前的 Normal 或 Lite
合成方式，菜单会显示可用状态。310.9.16 新增画质和性能支持已通过本地检查，尚待游戏内验证；尚未测得 Lite 的性能增益。
神经网络优化需要 DirectX 12；Vulkan 仅受益于图像处理优化。

## 游戏界面

当游戏界面与 3D 场景分开处理时，生成帧最为干净。**Frames** 选项卡中有三个开关：

| 设置 | 默认 | 作用 |
| :--- | :---: | :--- |
| **Automatic UI recomposition** | 开启 | 只要游戏提供了所需内容，就使用界面单独处理。如果生成帧在运动中出现扭曲或重影，请关闭它。 |
| **UI assist (D3D12)** | 开启 | 在没有提供无界面场景和界面层的 DirectX 12 游戏中，由 RTX Encore 构建它们。游戏自身提供的内容始终优先。 |
| **Force UI recomposition** | 关闭 | 即使游戏没有要求，也请求单独处理。用于测试。 |

针对特定游戏的建议见[兼容性](COMPATIBILITY.zh-CN.md#游戏说明)。

## 兼容性选项

位于 **System** 选项卡：

| 设置 | 默认 | 作用 |
| :--- | :---: | :--- |
| **Disable menu detection** | 关闭 | 关闭时：帧生成会在菜单和加载画面中暂停，可避免在这些地方崩溃。请保持关闭。 |
| **Force NVIDIA OTA models** | 关闭 | 使用 NVIDIA 应用下载的帧生成模型。需重启游戏生效。 |
| **Graphics card series** | auto | 除非显卡未被识别，否则保持 auto。 |

## 读懂叠加层

| 示例 | 含义 |
| :--- | :--- |
| `74 fps` | 游戏的帧率；未观察到帧生成 |
| `74/37 fps 2x` | 显示帧率 / 渲染帧率，以及观察到的倍率 |
| `296/74 fps 2x+sm` | 帧生成与 Smooth Motion 同时工作 |

如果本次会话中没有检测到 DLSS 帧生成，选择倍率并不能凭空产生帧生成：菜单会给出提示。

## 限制

- 对 RTX 20 的支持是实验性的：尚未在真实的 RTX 20 显卡上验证。
- RTX 20 和 RTX 30 上的 Vulkan 支持是实验性的。
- 帧生成提高的是显示帧率，而不是游戏的响应速度：倍率越高延迟越大，而且渲染帧率本身足够高时效果更好。
