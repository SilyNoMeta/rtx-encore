# 没有 DLSS 的游戏

*[English](GAMES-WITHOUT-DLSS.md) · [Français](GAMES-WITHOUT-DLSS.fr.md)*

很多游戏完全没有 DLSS：没有超分辨率，没有帧生成，也没有 Neural Rendering。自 2.0.0 起，RTX Encore 基于游戏显示的画面，在
DirectX 11、DirectX 12 和 Vulkan 中为它们带来这三项功能，无需 ReShade 或任何其他工具：

- **超分辨率**：对游戏显示的画面运行 DLAA，或者把较低的游戏分辨率放大到屏幕分辨率，可使用 NVIDIA DLSS Super Resolution 或
  Intel XeSS（DirectX 12 中 XeSS 可运行在 NVIDIA Tensor Cores 上）。
- **Neural Rendering**：对同一画面运行 NVIDIA 的 [Neural Rendering](NEURAL-RENDERING.zh-CN.md)，在超分辨率之后或单独运行。
- **帧生成**：NVIDIA DLSS 帧生成（DirectX 12 和 Vulkan，X2 至 X6）或 Intel XeFG（DirectX 12 最高 X6 并可使用 Tensor
  Cores，Vulkan X2 至 X6 并支持 HDR10，DirectX 11 X2 至 X6），倍率可在游玩中更改。

每项功能可单独使用，也可组合使用。它们都是实验性功能，默认关闭。自带 DLSS 的游戏保留自己的 DLSS：这些选项在那里会隐藏，RTX Encore
像以前一样配合游戏自己的 DLSS 工作。

**维护者已测试**（RTX 4090，NVIDIA 621.01）：PEAK 的 DirectX 12、DirectX 11 和 Vulkan（超分辨率、Neural Rendering、DLSS 帧生成
X2 和 X4、XeFG X2 至 X4），The Sims 4 的 DirectX 11（XeFG X2），Football Life 2027 的 DirectX 11（DLAA、放大到 4K 和 Neural
Rendering，HDR）。其他游戏、显卡和驱动尚未验证：请在 [issue](https://github.com/SilyNoMeta/rtx-encore/issues) 中告诉我们你的结果。

## 简要原理

RTX Encore 在画面显示之前取得已完成的画面。两帧之间的运动由画面本身在显卡上估算（默认使用 RTX Encore 自己的估计）；如果游戏绘制时使用
深度缓冲，RTX Encore 会自动找到它。随后 DLSS 或 XeSS、Neural Rendering 和帧生成在这一画面上运行，显示的就是处理结果。

由于处理的是已完成的画面，**HUD 和文字会与画面一起处理**：一起放大、一起增强，开启帧生成时也一起插帧。大多数情况下看不出来；运动场景上
的小号文字可能出现轻微拖影，快速移动的 HUD 在生成帧中可能出现重影。

## 要求

- NVIDIA RTX 20、30、40 或 50 显卡和较新的驱动。运动估计的 Optical Flow 选项使用 NVIDIA Optical Flow，它包含在驱动中。
- DirectX 11、DirectX 12 或 Vulkan。在 Vulkan 中，游戏的设备必须接受 RTX Encore 在启动时请求的功能；否则这些选项在该游戏中不起作用。
  帧生成准备期间，Vulkan 游戏的启动可能多花最多十秒。
- 每项功能使用的 NVIDIA 和 Intel 文件。在没有 DLSS 的游戏中安装或更新模组时，**RTX Encore Hub 2.0.0 会为你安装它们**（见下文）。手动安装：
  - DLSS 超分辨率：游戏可执行文件或 RTX Encore 旁边的 NVIDIA `nvngx_dlss.dll`。
  - Neural Rendering：NVIDIA 的 `nvngx_dlssnr.dll`，同 [Neural Rendering](NEURAL-RENDERING.zh-CN.md#要求) 页面。
  - DLSS 帧生成：游戏可执行文件旁边的六个 NVIDIA 帧生成文件，版本须为 RTX Encore 为此用途接受的版本（Streamline 2.14.16 和 DLSS
    帧生成 310.9.16）。其他版本会被拒绝，游戏将在没有帧生成的情况下运行。
  - XeSS 和 XeFG：Intel 运行库，同 [Intel XeSS 与 XeFG](INTEL-ENGINES.zh-CN.md) 页面。
- 帧生成还要求在 RTX Encore 中为该游戏关闭 NVIDIA **Smooth Motion**：两者不能叠加。

## 使用 RTX Encore Hub 安装

[RTX Encore Hub 2.0.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.0.0) 知道哪些游戏没有 DLSS。对这类游戏，**安装**、
**与模组一起配置**以及更新会把本页需要的一切放到模组旁边：DLSS 和 Neural Rendering 文件、当游戏没有自己的帧生成时放在可执行文件旁边的帧生成
文件，以及 Intel 运行库。这不会开启任何功能：设置保持模组默认值，由你在游戏菜单或 Hub 的设置表单中选择每项功能。**还原游戏**会全部移除。
自带帧生成的游戏永远不会收到帧生成文件。

## 开启方法

打开菜单（**Insert**）。在没有 DLSS 的游戏中，**画质**和**帧率**选项卡显示的是这些控件，而不是自带 DLSS 游戏的控件。

### 超分辨率

在**画质 → 超分辨率 → 引擎**中选择 **DLSS** 或 **XeSS** 即可启用重建，选择**无**即可关闭。不再需要单独的启用开关。默认输出为**游戏分辨率（DLAA）**：DLSS 以原生分辨率运行，XeSS 使用原生抗锯齿。需要放大时，以所需的输入分辨率在**窗口模式**（不是无边框窗口模式）启动游戏，然后在 Encore 中选择**屏幕分辨率**。Encore 会放大输出并移除窗口边框。例如，从 2560 × 1440 放大到 3840 × 2160，宽高比例约为 67%。

### 分辨率与 Windows 显示缩放

选择**屏幕分辨率**时，游戏决定输入分辨率，Encore 将输出保持为显示器分辨率。**1920×1080 → 2560×1440 的单轴比例是 75%**。输出显示为 1440p，不代表游戏以 1440p 渲染；画面仍铺满同一块屏幕也属于正常现象。如果游戏已经以 1080p 渲染，再次应用 1080p 不会改变分辨率。

Windows 显示缩放是另一项设置。在 **150%** 缩放的 2560×1440 显示器上，不支持高 DPI 的游戏可能只看到约 **1707×960**。这可能限制游戏分辨率并使文字模糊。可以保留平时的 Windows 缩放比例，先检查游戏的高 DPI 支持。

对于出现此问题的 Unreal Engine 游戏：

1. 关闭游戏，备份游戏保存配置目录中的 `Engine.ini`。常见位置为 `%LOCALAPPDATA%\<游戏>\Saved\Config\WindowsNoEditor\`，部分游戏使用 `Windows` 目录。修改的是游戏配置，不是 Encore 配置。
2. 在已有的 `[/Script/Engine.UserInterfaceSettings]` 节中设置 `bAllowHighDPIInGameMode=True`。没有该节时再添加，保留其他设置：

   ```ini
   [/Script/Engine.UserInterfaceSettings]
   bAllowHighDPIInGameMode=True
   ```

3. 重新启动游戏，启用 **DLSS** 或 **XeSS**，选择**屏幕分辨率**，然后在游戏画面设置中应用所需分辨率。在 1440p 显示器上比较时，先应用 2560×1440，再应用 1920×1080。后者应为 1080p 输入、1440p 输出，比例约 75%。
4. 同时检查游戏按钮的点击位置。如果光标仍然偏移，请恢复原设置，并在反馈时说明游戏、Windows 缩放比例、游戏分辨率和 Encore 输出模式。画面变清晰并不能单独证明鼠标位置正确。

Epic 提供了此 [Unreal 高 DPI 设置的说明](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Engine/UUserInterfaceSettings/bAllowHighDPIInGameMode?application_version=4.27)。已在 **Islands of Insight、DirectX 11、1440p 显示器、Windows 150% 缩放**下使用本次修复的测试版本确认该配置。不能据此保证所有 Unreal 游戏或旧版 Encore 都正常：游戏可能忽略此设置或使用其他配置目录。无效时请恢复备份的设置。添加的超分辨率仍会与场景一起处理文字和 HUD。

### Neural Rendering

**画质**选项卡，**Neural Rendering** 部分：开启 **Neural Rendering**。在没有 DLSS 的游戏中，这一个开关就会在显示的画面上启动 NR，在超分辨率
之后（若已开启）或单独运行。随后 [Neural Rendering](NEURAL-RENDERING.zh-CN.md) 页面的所有 NR 控件都适用（遍数、风格、分辨率、RTX 30 上的
Fast 精度、HDR 亮度）。**Ctrl + Alt + N** 可在游玩时开关它。

### 帧生成

在**帧率 → 帧生成 → 引擎**中选择 **DLSS-FG** 或 **XeFG**，选择**无**可关闭添加的帧生成。两种引擎均可在兼容配置上支持 DirectX 11、DirectX 12 和 Vulkan。关闭 Smooth Motion，并按提示重启游戏。**固定**模式提供 X2 至 X6；X5 和 X6 仍为实验性功能。**游戏**代表游戏自身的集成，不是为没有帧生成的游戏启用该功能的选项。

### 运动估计

在超分辨率控件下方，**运动估计**决定 RTX Encore 如何测量两帧之间的运动，同时用于超分辨率、Neural Rendering 和帧生成。可在游玩时切换。

| 选项 | 说明 | RTX 4090 上 4K 的开销 |
| :--- | :--- | :--- |
| **RTX Encore**（默认） | RTX Encore 自己的估计，在显卡的着色器中：精细的向量（每 2×2 像素一个），能跟随对象边缘，并在游戏有深度缓冲时加以利用。为基于这些画面的帧生成而设计；不需要加速器。 | 每帧约 1.0 毫秒 |
| **着色器 (vort)** | 同样在着色器中估计，采用 vortigern11 的 vort_Motion 方法。可在你的游戏中与 RTX Encore 对比。 | 每帧约 0.6 毫秒 |
| **Optical Flow (质量)** | NVIDIA Optical Flow，RTX 显卡的加速器。 | — |
| **Optical Flow (快速)** | NVIDIA Optical Flow 的快速模式：每帧约少 1 毫秒，但运动略不精确，快速移动时拖影可能更多。 | — |

## 用 XeSS 代替 DLSS

- XeSS 支持 1x（原生抗锯齿）到 3x：在 4K 屏幕上，游戏分辨率至少需要 1280×720。低于此值时，游戏画面会直接拉伸显示，不经过 XeSS。
- 在 DirectX 12 游戏中选择 XeSS 后，引擎下方会显示 **NVIDIA Tensor Cores（实验性）**，它让 XeSS 运行在 Tensor Cores 上，验证范围与
  [Intel XeSS 与 XeFG](INTEL-ENGINES.zh-CN.md) 页面相同。DirectX 11 和 Vulkan 游戏使用标准 XeSS，不显示该开关。开启或关闭会重新创建
  XeSS（短暂卡顿）。开关下方的一行说明最近一帧 XeSS 的情况：Tensor Cores 已激活、已准备但未使用，或不可用及其原因；Tensor Cores 运行时，
  **超分辨率**卡片带有绿色的 **Tensor Cores** 标记。
- 游戏启动后的最初几帧在 RTX Encore 检查 Intel 运行库期间不经过 XeSS 显示。

## 各图形 API 的帧生成

| | DLSS 帧生成 | Intel XeFG |
| :--- | :--- | :--- |
| DirectX 12 | X2 至 X6 | X2 至 X6；可使用 **NVIDIA Tensor Cores**，验证范围与 [Intel XeSS 与 XeFG](INTEL-ENGINES.zh-CN.md) 页面相同 |
| Vulkan | X2 至 X6 | X2 至 X6，倍率可即时更改；SDR 和 HDR10 |
| DirectX 11 | X2–X6 | X2 至 X6，倍率可即时更改；SDR 和 HDR10 |

- **HDR。**在 Windows 以 HDR 模式运行的屏幕上以 HDR10（10 位）显示的游戏可获得 HDR 帧生成。以 scRGB（16 位浮点）显示的游戏保留自己的
  输出：那里没有帧生成，本页其余内容仍然适用。
- DirectX 11 的**独占全屏**在 XeFG 运行期间显示为无边框窗口。
- 维护者在 PEAK 的 DirectX 12 和 Vulkan 中以 X4 测试，帧间隔平稳。在 Vulkan 中，当游戏不使用 V-Sync 时，生成帧不等待屏幕刷新即显示，
  与 DLSS 帧生成相同；游戏帧率因此不会被限制在刷新率除以倍率。
- 在 DirectX 11 中，游戏的下一帧会等待 XeFG 处理完上一帧，与 DirectX 12 相同，X3 的节奏均匀。在屏幕允许画面撕裂时，输出不受刷新率限制：
  维护者在 The Sims 4 中看到 XeFG 在 144 Hz 屏幕上超过 144 FPS，并在 Skyrim Special Edition 中测试了 X3 至 X6。
- 已经自带 DLSS 帧生成、Streamline、Intel XeFG 或 AMD 帧生成的游戏保留原有功能：RTX Encore 绝不会为其添加第二个帧生成。此类游戏请参阅
  [帧生成](FRAME-GENERATION.zh-CN.md)和 [Intel XeSS 与 XeFG](INTEL-ENGINES.zh-CN.md)。

## 放大到屏幕分辨率

1. 选择 **屏幕分辨率** 作为输出。
2. 在游戏中选择较低的分辨率（例如 4K 屏幕上的 2560×1440）和 **窗口化** 模式。
3. RTX Encore 让窗口无边框并覆盖整个屏幕，DLSS（或 XeSS）把游戏画面放大到屏幕分辨率。鼠标仍按游戏分辨率工作。从游戏下次更改分辨率或
   重新启动后生效。

在 **Unity** 游戏中，请选择 **窗口化**，不要选择 **全屏窗口**：低于屏幕分辨率时，全屏窗口会在 RTX Encore 取得画面之前自行拉伸画面，此时
DLSS 只能以 DLAA 方式运行。独占全屏保持游戏分辨率。

与自带 DLSS 的游戏相比，放大后的画面稍软：游戏不会为超分辨率抖动镜头（jitter），因此细节重建效果较弱。在游戏分辨率下运行的 DLAA 没有
这一限制。

## 限制

- HUD 与画面一起处理（见上文），在生成帧中也一起插帧。
- 快速运动、新露出的区域和放大的回放可能比自带 DLSS 的游戏出现更多拖影，放大后的画面和生成帧都是如此。
- 没有深度缓冲（或未找到）时，一切仍会运行，运动稳定性略差。
- 放大后的画面比游戏自带的 DLSS 稍软，后者拥有游戏的镜头抖动；DLAA 没有这一限制。
- XeSS 和 XeFG 的 Tensor Cores 仅适用于 DirectX 12 游戏。
- scRGB（16 位浮点）输出保留游戏自己的帧：没有帧生成。
- 帧生成与 NVIDIA Smooth Motion 不能组合：请关闭 Smooth Motion。
- 不支持运行在 DirectX 12 之上的 DirectX 11 游戏（某些转换层）。
- 设备不接受额外功能的 Vulkan 游戏可正常运行，但没有这些选项。
- 以上内容尚未在 RTX 20、RTX 30 或 RTX 50 显卡上验证，也未在 621.01 以外的驱动上验证。

## 出现问题时

- 确认选项可见：在 RTX Encore 检测到游戏自带 DLSS 或帧生成的游戏中，这些选项会隐藏。
- 菜单的**超分辨率**卡片显示正在运行的引擎和缩放比例；**帧生成**卡片显示引擎和倍率，或说明为何没有生成。
- 没有放大、增强或生成：确认文件是否存在。在 Hub 中，游戏页面会列出它们并提供写入缺失文件的选项；手动安装请参阅[要求](#要求)。没有
  正确的帧生成文件时，游戏只是没有帧生成地运行。
- XeSS 不运行：确认 Hub 已为该游戏安装 Intel 运行库，然后重新启动游戏。在**引擎**下方，菜单会说明何时选择了 XeSS 却运行了 DLSS，及其原因。
- 帧生成没有启动：关闭 **Smooth Motion**，确认引擎在重启前已选择，并确认游戏不是自带帧生成的游戏。
- 画面黑屏或被拉伸：窗口模式见[放大到屏幕分辨率](#放大到屏幕分辨率)，以及上文 XeSS 的 3x 限制。
- 使用菜单中的报告（**System** 选项卡，**Copy**）反馈问题：参见 [故障排除](TROUBLESHOOTING.zh-CN.md)。

## 兼容性与实测限制

支持某个 API 不代表兼容所有游戏。通过验证的路径可请求固定 DLSS-G 2x–6x；5x/6x 仍属实验功能，实际支持取决于 GPU 和已加载的运行库。XeFG 倍率取决于获准的运行库配置。

在测试用 RTX 4090 上，Encore 混合运动估计器在 3840×2160 下测得 0.985 ms GPU 时间。这仅是运动估计成本，不是重建与帧生成的总成本；其他硬件和场景可能不同。

注入式重建也会处理菜单：新出现的文字可能在历史收敛期间产生残影（已在 Elden Ring 的 DLAA 预设 M 中观察到）。关闭重建应保留独立启用的帧生成。已在关闭重建的 Elden Ring 中验证了单独启动帧生成。

Elden Ring 内部的 60 渲染 FPS 上限与显示/V-Sync 限制不同。Encore 不会移除引擎限帧器；单独测试的 FPS 解锁工具不包含在 Encore 中。

## 运行库可用性与版本

缺少运行库或版本不兼容时，菜单不提供对应引擎。旧版兼容运行库仍受支持：不会仅因存在更新版本而拒绝 NVIDIA 310.9.1。菜单中的警告会建议通过 RTX Encore Hub 更新并重启游戏。推荐版本：NVIDIA **310.9.16**、XeSS-SR **2.0.2.68**、XeFG **1.3.1.78**、XeLL **1.3.2.10**。添加的 DLSS 帧生成有更严格的要求：必须使用 **Streamline 2.14.16 + DLSS-G 310.9.16**。更新无法让不受支持的游戏或 GPU 自动变得兼容。
