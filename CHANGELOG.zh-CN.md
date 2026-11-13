# 更新日志

*[English](CHANGELOG.md) · [Français](CHANGELOG.fr.md)*

## 2.4.1

**RTX Encore 2.4.1 包含逐遍 NR 位置、支持 HDR 的 DirectX/Vulkan 空间抗锯齿、最低 33% 的自定义 DLSS 比例，以及游戏 FG 菜单停留在 X2 的修复。配套 Hub 2.4.0 提供模组版本选择，无需更新 Hub。**

### 游戏 FG 菜单：启动时的 X3/X4

当游戏安装的 DLSS-G DLL 与 NVIDIA 缓存模型同时加载时，游戏首次查询能力时可能尚未识别实际使用的提供方。Encore 现在在该阶段保留原生返回的最大值，不再将其改为 X2。在确认提供方支持之前，FG 请求仍限制为 X2；已识别旧版提供方继续遵守其实际限制。

回归测试编译实际用于提供方选择与能力公告的函数，覆盖启动时尚未识别提供方的查询、随后对更高倍率的支持、仅支持 X2 或较低倍率的提供方、过期模块身份及各游戏菜单上限。测试在 2.4.0 行为下失败，在修复后通过。

感谢 S.T.A.L.K.E.R. 2 报告者提供详细启动日志并检查 X3/X4。日志确认了原有回归；修复版本仍待该游戏内确认。Hub 2.4.0 继续兼容，无需更新 Hub。

### 跨阶段 Neural Rendering

将一至四遍 NR 分别分配至 Pre-SR、Post-SR 或最终画面，也可继承全局位置。执行顺序遵循画面管线，保留每遍的外观设置。已有配置行为不变。兼容的 DX11、DX12 和 Vulkan 原生游戏 DLSS 钩子支持此功能；各阶段仍有输入、HUD、HDR 和 FG 条件。前一阶段成功并不保证后一阶段可用。

### FXAA 和 SMAA，包含 HDR

NIS/FSR 1 缩放前后的可选空间抗锯齿现支持兼容的 DirectX 11、DirectX 12 和 Vulkan 路径，覆盖 SDR、HDR10 和 scRGB，扩展了 2.3.0 的 DX11 SDR 支持。实体 HDR 显示与各游戏的效果仍取决于配置。

### 自定义 DLSS 比例：33–100%

DLSS Super Resolution 自定义输入比例现最低为每轴 33%，Hub 同步调整范围。更低输入分辨率以源细节换取较低渲染开销，结果取决于游戏和 GPU。

### 帧生成与显存

物理显存余量检查避免重复计算分配。显存预算警告改为提示信息，不再仅凭警告关闭帧生成。这不保证所有配置都有足够显存。

### 兼容性修复

整合 FC27 原生 SR、游戏运动输入与 Intel 路由修复。KCD2 在 NGX 客户端导出存在时保留菜单引擎控制。修复旧版 DLSS-G 运行库限制与 X2 活动状态显示。新增 NVIDIA 621.24 离线驱动配置，仍要求兼容的硬件和运行库。

### Horizon 与最终画面

修复 D3D12 提交钩子输入、原生 FG 遥测、RGB10 Pre-SR 输入和最终画面场景/HUD 合成。较早候选版本的 Pre-SR/Post-SR/最终画面与遥测已获游戏内确认。最新 HUD 闪烁修复已有自动化测试与回放覆盖，但仍待游戏内确认；过场动画中间歇 Waiting 仍在调查。


D3D12 抖动运动输入 NR 仍为需明确启用的诊断试验，默认关闭；本版本不代表这些输入已获得普遍支持。感谢 [@OliverBokan](https://github.com/OliverBokan) 的 Horizon 报告（[#126](https://github.com/SilyNoMeta/rtx-encore/issues/126)），以及 [@deusimperatork](https://github.com/deusimperatork) 提供 KCD2 报告与日志（[#125](https://github.com/SilyNoMeta/rtx-encore/issues/125)）。

### Hub 版本选择与组件修复

在 Hub 设置中选择已发布模组版本，可固定版本或回退。配套 Hub 提供 2.4.0 逐遍 NR 位置与最低 33% 比例，识别 NVIDIA 621.24，并安装/修复所需 Streamline SR 依赖。较旧模组继续使用适用的控制选项。

Neural Rendering 仍为实验功能，默认关闭。兼容性取决于游戏、API、GPU、驱动和运行库，不保证普遍的 FPS 或画质提升。

## 2.4.0

**RTX Encore 2.4.0 新增逐遍 NR 处理位置、支持 HDR 的 DirectX/Vulkan 空间抗锯齿，以及最低 33% 的自定义 DLSS 比例。Hub 2.4.0 可选择模组版本并回退。**

### 跨阶段 Neural Rendering

将一至四遍 NR 分别分配至 Pre-SR、Post-SR 或最终画面，也可继承全局位置。执行顺序遵循画面管线，保留每遍的外观设置。已有配置行为不变。兼容的 DX11、DX12 和 Vulkan 原生游戏 DLSS 钩子支持此功能；各阶段仍有输入、HUD、HDR 和 FG 条件。前一阶段成功并不保证后一阶段可用。

### FXAA 和 SMAA，包含 HDR

NIS/FSR 1 缩放前后的可选空间抗锯齿现支持兼容的 DirectX 11、DirectX 12 和 Vulkan 路径，覆盖 SDR、HDR10 和 scRGB，扩展了 2.3.0 的 DX11 SDR 支持。实体 HDR 显示与各游戏的效果仍取决于配置。

### 自定义 DLSS 比例：33–100%

DLSS Super Resolution 自定义输入比例现最低为每轴 33%，Hub 同步调整范围。更低输入分辨率以源细节换取较低渲染开销，结果取决于游戏和 GPU。

### 帧生成与显存

物理显存余量检查避免重复计算分配。显存预算警告改为提示信息，不再仅凭警告关闭帧生成。这不保证所有配置都有足够显存。

### 兼容性修复

整合 FC27 原生 SR、游戏运动输入与 Intel 路由修复。KCD2 在 NGX 客户端导出存在时保留菜单引擎控制。修复旧版 DLSS-G 运行库限制与 X2 活动状态显示。新增 NVIDIA 621.24 离线驱动配置，仍要求兼容的硬件和运行库。

### Horizon 与最终画面

修复 D3D12 提交钩子输入、原生 FG 遥测、RGB10 Pre-SR 输入和最终画面场景/HUD 合成。较早候选版本的 Pre-SR/Post-SR/最终画面与遥测已获游戏内确认。最新 HUD 闪烁修复已有自动化测试与回放覆盖，但仍待游戏内确认；过场动画中间歇 Waiting 仍在调查。


D3D12 抖动运动输入 NR 仍为需明确启用的诊断试验，默认关闭；本版本不代表这些输入已获得普遍支持。感谢 [@OliverBokan](https://github.com/OliverBokan) 的 Horizon 报告（[#126](https://github.com/SilyNoMeta/rtx-encore/issues/126)），以及 [@deusimperatork](https://github.com/deusimperatork) 提供 KCD2 报告与日志（[#125](https://github.com/SilyNoMeta/rtx-encore/issues/125)）。

### Hub 版本选择与组件修复

在 Hub 设置中选择已发布模组版本，可固定版本或回退。配套 Hub 提供 2.4.0 逐遍 NR 位置与最低 33% 比例，识别 NVIDIA 621.24，并安装/修复所需 Streamline SR 依赖。较旧模组继续使用适用的控制选项。

Neural Rendering 仍为实验功能，默认关闭。兼容性取决于游戏、API、GPU、驱动和运行库，不保证普遍的 FPS 或画质提升。

## 2.3.0

**RTX Encore 2.3.0 带来最终画面 Neural Rendering、默认使用游戏运动矢量、全新游戏内控制台，以及支持 HDR 的 FSR 1。**

### 最终画面 Neural Rendering

在兼容的 DirectX 12、DirectX 11 和 Vulkan 路径上，于游戏后期处理之后、帧生成和显示之前处理画面。兼容的独立场景/HUD 输入可保留 HUD；已合成进场景的 HUD 会随画面一起处理。兼容路径支持一至四遍处理、注视点 NR 和 HDR。Vulkan 请在启动游戏前选择最终画面，或在切换后重启游戏。具体组合请参阅 NR 指南。

### 默认使用游戏运动矢量

为有 DLSS Super Resolution、但没有 DLSS Frame Generation 的游戏添加帧生成时，Game motion vectors 在 DirectX 12、DirectX 11 和 Vulkan 上优先使用游戏提供的运动矢量，不可用时自动回退至 RTX Encore 估算。兼容的最终画面 NR 也可使用这些矢量。保留已有的明确选择。此前的 DX11/Vulkan 游戏输入路径继续保留；2.3.0 提供统一选项并将其设为默认。模组与 Hub 现在一致显示全部五个选项。

### 全新游戏内控制台

重新组织导航，使用更清晰的引擎卡片和可见的处理流程，方便调整超分辨率、帧生成、NR 与性能。NR 包含全局外观、逐遍设置和完整的多遍配置。

### FSR 1 与 NIS，支持 HDR

没有 DLSS 的游戏现在也可选择 AMD FidelityFX Super Resolution 1。NIS 和 FSR 1 支持兼容的 DirectX 11、DirectX 12 和 Vulkan 呈现路径，包含 SDR、HDR10 和 scRGB。锐度可实时调整。缩放前后的 FXAA/SMAA 仍仅适用于 DirectX 11 SDR。需要较低的窗口分辨率、相同宽高比，且每轴严格放大、最高 2 倍。

### NR 外观与曝光

模组和 Hub 提供标准、柔和、自然、电影、人脸、柔肤及 NVIDIA 参考七种外观。应用外观会修改全局外观并将逐遍外观覆盖恢复为继承，不改变遍数、模型、分辨率或曝光。NR 曝光提供游戏、自动、测量和手动模式，适用于兼容的场景线性 DLSS 输入。新配置使用自动；已有文件缺少该键时保留游戏模式。

### 比较画面与开销

图像与性能实验室支持比较 Post-SR、Pre-SR 和最终画面，包含截图、测量和导出。Hub 在比较卡片中标明选择的处理位置。请保持比较条件一致；NR GPU 时间只覆盖 NR 处理，不包含全部传输或完整图形管线。

### 更有针对性的 XeFG 帧节奏

从 X3 起，自动模式先检测持续不均匀的游戏帧间隔，再尝试修正；只有在源帧率损失预算内改善节奏时才保留修正。逐步放宽限制，必要时恢复有效修正。手动模式可实时调整 0–30% 损失预算、检测和自动调整开关。自动模式采用 4% 预算。

### 兼容性与恢复

修复 The Sims 4 DX11 DLSS-G 启动、BG3 Vulkan Script Extender/MCM 与最终画面、Cyberpunk 2077 旧版 Streamline 最终画面输入、Forza Horizon 6 最终画面/HUD、GTA V Enhanced 输入跟踪，以及 RDR2 Vulkan 队列/引导数据和探测 Vulkan 后的 DX12 启动。Encore 先初始化 FG 时仍保留原生 SR。另有 ReShade 与空间缩放/FG、缓冲格式、窗口和光标修复；Encore 管理的兼容 DXGI FG 可在 VRAM 预算压力后恢复。

### RTX Encore Hub 2.3.0

配套 Hub 识别稳定版 FSR 1 压缩包与已安装的代理/ASI，提供游戏运动矢量、NR 曝光及手动 XeFG 节奏设置，并限制旧模组不支持的选项。引导安装支持 NIS/FSR 1 和适用的 AA。社区配置架构接受最终画面与新增的共享 XeFG 控制。此前的 NIS、Overlay Studio、Tensor Core 选项和逐文件组件选择均继续保留。

兼容性取决于游戏、图形 API、GPU 和受支持的运行库。Neural Rendering 及现有实验性路径仍有各自指南所述的限制。本次不宣称普遍的 FPS 提升。

## 2.2.0

**RTX Encore 2.2.0 新增空间缩放和可自定义的性能叠加层。**没有 DLSS 的游戏现在可以选择 NVIDIA Image Scaling（NIS），并可在缩放前后使用 FXAA 或 SMAA。叠加层新增样式和独立指标选项。XeFG Tensor Cores 扩展到 RTX 20，XeSS Tensor Cores 则获得 RTX 30/40 优化。

### 缩放与抗锯齿

- **NVIDIA Image Scaling（NIS）**成为无 DLSS 游戏的新选项，包括 DirectX 11。可选择在 NIS 前或后运行 **FXAA** 或 **SMAA**，也可以关闭。请按游戏画面选择；空间滤镜无法还原源图像中不存在的细节。Hub 2.2.0 提供相应设置。
- 空间缩放现在能更好地与所选重建引擎组合，并更准确地处理游戏源画面的尺寸。模拟人生 4 的修复已在游戏中确认。动态加载引擎时，NIS 窗口尺寸也会考虑 Windows 显示缩放。
- **XeSS Super Resolution Tensor Cores** 新增 RTX 30 和 RTX 40 优化路径。RTX 3070 Ti 与 RTX 4090 已检查输出一致性；RTX 3070 Ti 的测量结果随分辨率变化，因此不保证所有场景都提速。

### 帧生成与叠加层

- **DLSS-G 与游戏 Auto 模式：**新设置 `overrideAutoFrameGeneration` 默认开启。当游戏请求 Auto 且 NVIDIA 可能在高负载场景关闭帧生成时，它会请求保持 On。关闭后会原样传递游戏的 Auto 请求。设置即时生效，Hub 2.2.0 也提供该选项。感谢 [@avikpal693](https://github.com/avikpal693) 的报告（[#116](https://github.com/SilyNoMeta/rtx-encore/issues/116)）；报告者尚未确认最终版本。
- **XeFG NVIDIA Tensor Cores** 现在实验性支持 RTX 20（Turing）。兼容性仍取决于驱动、运行时、游戏和图形 API；此版本不宣称已在 RTX 20 的所有游戏中验证。感谢 [@MxHassan195](https://github.com/MxHassan195) 提出请求（[#117](https://github.com/SilyNoMeta/rtx-encore/issues/117)）。
- **Overlay Studio：**可选择性能指标和显示样式。新增经典样式，恢复白色文字条；可选图表和已选指标继续可用。Hub 2.2.0 可预览和设置样式及各项指标。
- **博德之门 3：**在 DirectX 11 中添加独立 DLSS 帧生成时，Script Extender 的 ImGui/MCM 菜单现在可以正常打开。维护者已用测试版本在游戏中确认。
- **天国：拯救：**独立 DLSS 帧生成不再绑定到游戏的小型测试窗口，游戏窗口可正常运行 X2。NVIDIA 驱动的 Streamline override 也可用于原生 DLSS-G。维护者已用测试版本在游戏中确认两项修复。

最终发布包的构建和检查与上述游戏测试分别进行。实验性功能仍取决于各游戏的集成；如无法启用，请提供游戏、图形 API、GPU 和 Hub 诊断。


## 2.1.0

**RTX Encore 2.1.0 让 Intel XeFG 在 X3 及以上倍率下保持流畅。**新设置 **XeFG 稳定游戏帧节奏**默认开启，消除了 XeFG 高倍率下可能出现的镜头抖动。自带 DLSS 但没有帧生成的游戏，现在会把自己的深度和运动数据交给 RTX Encore 添加的帧生成；XeFG 在 DirectX 11 和 Vulkan 中也能使用 Tensor Core；此外还有一系列修复：博德之门 3、最终幻想 VII 重生、极限竞速：地平线 6、天国：拯救、Islands of Insight 以及刺客信条：黑旗 Resynced。

### XeFG 稳定游戏帧节奏（新增，默认开启）

在 X3 及以上倍率时，即使帧数计数看起来正常，XeFG 也可能让镜头运动出现抖动：游戏自身的帧时长忽短忽长，导致每组生成帧覆盖的时间各不相同。**XeFG 稳定游戏帧节奏**会以略低于游戏可维持帧率的固定节奏开始游戏的每一帧，并持续随游戏自动调整，使每一张生成帧之间的运动都保持均匀。

- 默认开启，位于菜单 **帧率** 选项卡中，与其他 XeFG 设置在一起；[RTX Encore Hub 2.1.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.1.0) 中也可设置。即时生效：关闭再打开即可对比效果。
- 代价是游戏自身帧率降低几 FPS。它不会催促迟到的帧，也不会一次性集中追赶。
- 需配合 XeFG 可变帧节奏（默认开启），从 X3 起生效；X2 不受影响。
- XeFG 自身的输出也更稳定：卡顿之后生成帧仍保持均匀间隔，且不再随时间漂移。

维护者已在刺客信条：黑旗 Resynced 中以 X4、不限帧验证：帧时长交替消失，运动非常流畅。感谢 [Discord](https://discord.gg/Zwmq4Xc2Br) 上的 **Konan** 提供报告和日志（黑旗 Resynced、极限竞速：地平线 6、阿凡达：潘多拉边境），并发现抖动与 XeFG 可变帧节奏有关。设置项为 `xefgSourcePacing`，参见[设置](docs/SETTINGS.zh-CN.md)和 [Intel 引擎](docs/INTEL-ENGINES.zh-CN.md)。

### 使用游戏自身的深度和运动数据进行帧生成

- **有 DLSS 但没有帧生成的游戏**（例如博德之门 3）：RTX Encore 添加的 DLSS 帧生成或 XeFG 现在会使用游戏已经提供给其 DLSS 的深度和运动矢量，而不再从画面中估计，适用于 DirectX 11 和 Vulkan。若游戏的数据无法使用，帧生成会回退到 RTX Encore 的运动估计，此时菜单会显示 **运动估计**。维护者在 RTX 3070 Ti Laptop 上于博德之门 3 的四种组合（DirectX 11 与 Vulkan、DLSS-FG 与 XeFG）中均看到了 X4 生成。
- **博德之门 3（DirectX 11）：即使 Streamline 文件已就位，DLSS 帧生成仍停留在“等待中”。**已修复；同时支持 10 位（RGB10）显示输出。感谢 [@deusimperatork](https://github.com/deusimperatork) 的报告（[#109](https://github.com/SilyNoMeta/rtx-encore/issues/109)）。报告者尚未重新测试。
- **博德之门 3（Vulkan）：Neural Rendering 与 DLSS 帧生成可同时使用**，无需任何第三方工具。维护者已验证：超分辨率之后的 NR，以及 1440p 下 DLSS-FG 先 X2 后 X4。

### DirectX 11 和 Vulkan 中的 Tensor Core（实验性）

- **在 NVIDIA Tensor Core 上运行的 XeFG** 现在也适用于没有帧生成的 DirectX 11 和 Vulkan 游戏。在博德之门 3 的 DirectX 11 和 Vulkan 中，维护者看到绿色 **Tensor Cores** 标记处于激活状态，XeFG 为 X4。画面一致性和性能提升尚未测量。
- **在 Tensor Core 上运行的 XeSS 超分辨率**现在可用于没有 DLSS 的 DirectX 11 和 Vulkan 游戏，以及 XeSS 替换 DirectX 11 游戏的 DLSS 时。尚未在游戏中验证。在 Vulkan 游戏中替换 DLSS 时仍使用标准 XeSS。

### 修复

- **最终幻想 VII 重生：帧生成一直不可用**，原因是开启了 NVIDIA App（或 NVIDIA Profile Inspector）的 DLSS 覆盖：此时游戏使用驱动下载的 Streamline 文件，而 RTX Encore 无法识别它们，于是 RTX 30 显卡被当作无法运行帧生成。现在已能识别。感谢 [@yannduwooz83](https://github.com/yannduwooz83) 的报告并找到了相关设置（[#106](https://github.com/SilyNoMeta/rtx-encore/issues/106)）。
- **XeFG 与 RTSS（RivaTuner Statistics Server、MSI Afterburner）：**RTSS 会在 RTX Encore 之后接管游戏交换链的创建，从第二次创建起就绕过了 XeFG，导致游戏重新创建窗口时 XeFG 可能不生效或失败。RTX Encore 现在会重新排到 RTSS 之前，并把每次创建交给它，两者都能继续工作。已在 RTSS 运行时通过 RTX Encore 的 XeFG 自动化测试验证；尚未在游戏中验证。感谢 Discord 上的 **Konan**，其 RTSS 报告指明了方向。
- **天国：拯救（DirectX 11）：**通过 Ultimate ASI Loader 启动游戏时菜单无法打开；使用 DLSS 帧生成时游戏失去焦点（Alt+Tab、最小化）会崩溃。两者均已修复，并由维护者在游戏中确认。感谢 Discord 上的 **SMN888** 的报告和诊断。
- **刺客信条：黑旗 Resynced（XeFG）：**修复了约一秒的卡顿和黑屏，菜单在 XeFG 下可正常使用，叠加层会将游戏自身帧率与生成后的输出分开显示。维护者已确认游戏运行正常；菜单和计数器的改动尚未在游戏中确认。感谢 Discord 上的 **Konan** 的报告。
- **极限竞速：地平线 6：**DirectX 12 中的 XeSS 超分辨率现在会正常启用而不再回退；游戏重置图形设备后，Tensor Core 上的 XeSS 仍能继续运行；并避免了与 Steam 叠加层相关的崩溃。维护者已在游戏中确认。感谢 Discord 上的 **Konan** 的报告。
- **Islands of Insight 及其他放大到屏幕分辨率的无 DLSS 游戏：**在调整窗口大小和无边框窗口中，画面、尺寸与鼠标指针保持对齐；DirectX 11 中帧生成能够可靠启动；选择 **无** 时会真正关闭添加的超分辨率。维护者已在游戏中确认。感谢 Discord 上的 **SMN888**，其在无 DLSS 游戏中（配合 Windows 显示缩放）的测试促成了这些修复。新增指南说明 Windows 显示缩放如何限制虚幻引擎游戏的分辨率并导致文字模糊：[分辨率与 Windows 显示缩放](docs/GAMES-WITHOUT-DLSS.zh-CN.md#分辨率与-windows-显示缩放)。
- **菜单：**游戏自带的 DLSS 在引擎选项中显示为 **游戏（DLSS）**；DLSS-FG 无法启动时会提示需通过 Hub 安装哪些 Streamline 组件；超分辨率引擎选择 **无** 时，XeSS 的 Tensor Cores 开关会隐藏，且仅帧生成使用 **运动估计** 时，该设置会移到帧生成部分。
- **手动安装：**[运行库列表](README.zh-CN.md#手动安装所需的运行库)现在适用于所有游戏，并新增 NVIDIA DLSS SDK 310.9.16 中的光线重建（`nvngx_dlssd.dll`）。

### 已验证的内容

上述游戏由维护者在 RTX 4090 或 RTX 3070 Ti Laptop 上使用这些改动的测试版本运行过，尚未使用 2.1.0 最终二进制文件重新测试。发布检查包括编译、这些功能的回归测试套件以及压缩包完整性。

**已知限制：**RTSS（RivaTuner Statistics Server）现在会被保持在 XeFG 之后（见“修复”），但其叠加层尚未在游戏中与 XeFG 一起实测。如果某个游戏仍然崩溃或 XeFG 未生效，请关闭 RTSS 叠加层并反馈。

## 2.0.1

- **使用 NVIDIA 官方文件手动安装：**为没有帧生成的游戏提供的帧生成现在也接受 NVIDIA 官方的 DLSS 帧生成 310.9.16（Streamline SDK 2.14.16 和 DLSS SDK 中的 `nvngx_dlssg.dll`），所有文件都可以取自 NVIDIA 的 SDK。此前由于检测错误被拒绝。感谢 [@Kbx-7](https://github.com/Kbx-7) 的报告（[#108](https://github.com/SilyNoMeta/rtx-encore/issues/108)）。
- **Intel XeSS 文件可放在 RTX Encore 旁边：**XeSS-SR、XeFG 和 XeLL 也会在 RTX Encore 的 DLL 旁边被找到，不再只限于 `rtx-encore-runtimes\intel`。
- 手动安装所需的文件和下载链接：[README](README.zh-CN.md#手动安装所需的运行库)。[RTX Encore Hub 2.0.1](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.0.1) 会安装 NVIDIA 官方的 310.9.16 文件。

## 2.0.0

添加的重建与帧生成引擎提供“无”选项；缺失运行库的引擎会隐藏，旧版兼容运行库会显示警告。DLSS SR 和 XeSS 新增 Ultra（75%）渲染比例。添加的 DLSS-FG 支持 DirectX 11、DirectX 12 和 Vulkan。

**RTX Encore 2.0.0 是一个大版本。**完全没有 DLSS 的游戏现在也能获得 DLSS、帧生成和 Neural Rendering，由 RTX Encore 专为它们编写的运动估计引擎驱动。RTX Encore 还换上了全新的视觉形象和标志，并开设了自己的 Discord。

**RTX Encore 的 Discord 服务器现已开放。** 欢迎分享 2.0.0 的测试结果、交流设置，或就模组和 Hub 寻求帮助。[加入 Discord](https://discord.gg/Zwmq4Xc2Br)。

RTX Encore 2.0.0 为**没有 DLSS 的游戏带来 NVIDIA DLSS Super Resolution 和 DLAA、Intel XeSS、Neural Rendering 和帧生成**，支持 DirectX 11、DirectX 12 和 Vulkan，基于游戏显示的画面，无需 ReShade 或任何其他工具。游戏本身不需要任何东西：不需要 DLSS，不需要运动向量，也不需要帧生成。全部为实验性功能，默认关闭；[没有 DLSS 的游戏](docs/GAMES-WITHOUT-DLSS.zh-CN.md)是完整指南。

- **为没有 DLSS 的游戏启用超分辨率：**游戏分辨率下的 DLAA，或把较低的游戏分辨率放大到屏幕分辨率（游戏使用窗口化模式，RTX Encore 以无边框窗口覆盖屏幕），可用 **DLSS** 或 **XeSS**；引擎可即时切换。在 DirectX 12 游戏中 XeSS 运行于 NVIDIA Tensor Cores 上，验证范围与其他地方相同，运行时**超分辨率**卡片显示 **Tensor Cores** 标记。
- **对显示的画面运行 Neural Rendering：**在没有 DLSS 的游戏中，**Neural Rendering** 开关直接在显示的画面上启动 NR，在超分辨率之后或单独运行，所有 NR 控件均可使用。
- **为没有帧生成的游戏启用帧生成：****DLSS 帧生成** X2 至 X6（DirectX 11、DirectX 12 和 Vulkan）；**Intel XeFG** X2 至 X6（DirectX 12 可用 NVIDIA Tensor Cores；Vulkan 支持 SDR 和 HDR10；以及 DirectX 11）。游戏决定为 X2，固定为你选择的倍率，可即时更改。在 DirectX 11 中，游戏的下一帧会等待 XeFG 的工作完成，X3 的节奏均匀；在屏幕允许画面撕裂时，输出不再受刷新率限制。HUD 与画面一起插帧。
- **运动估计，四种选择：****RTX Encore**，模组自己在显卡上的估计，为默认值（每 2×2 像素一个向量，跟随对象边缘并利用深度缓冲；RTX 4090 上 4K 每帧约 0.8 毫秒）；**着色器 (vort)**，vortigern11 的 vort_Motion 方法（约 0.6 毫秒）；**Optical Flow (质量)** 和 **Optical Flow (快速)**，NVIDIA Optical Flow。可即时切换并对比。
- **自动找到深度缓冲**：当游戏绘制时使用深度缓冲，三种 API 均可；没有时一切仍会运行，运动稳定性略差。
- **Hub 安装全部文件：**[RTX Encore Hub 2.0.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.0.0) 在没有 DLSS 的游戏中安装或更新模组时，把该游戏可用的 DLSS、Neural Rendering、帧生成和 Intel 文件放到模组旁边，并通过**还原游戏**移除。这不会开启任何功能。
- **手动安装：**为没有 DLSS 或没有帧生成的游戏添加的功能使用经过验证的精确运行库（DLSS 帧生成 310.9.16 与 Streamline 2.14.16、DLSS 310.9.16、Neural Rendering 310.9.16、XeSS-SR 2.0.2.68、XeFG 1.3.1.78 + XeLL 1.3.2.10）。Hub 会安装它们；手动安装时，文件及下载链接见 [README](README.zh-CN.md#手动安装所需的运行库)。

维护者已在 RTX 4090（NVIDIA 621.01）上验证：PEAK 的 DirectX 12（DLAA、1440p 放大到 4K、NR、DLSS 帧生成 X2 和 X4、带 Tensor Cores 的 XeFG X4）、DirectX 11（XeFG X2）和 Vulkan（DLAA、放大、NR、DLSS 帧生成 X4、XeFG X4）；The Sims 4 的 DirectX 11（XeFG X2 和 X3，在 144 Hz 屏幕上超过 144 FPS）；Skyrim Special Edition 的 DirectX 11（XeFG X3 至 X6）；Football Life 2027 的 DirectX 11 和 HDR（DLAA 和 NR，1440p 放大到 4K 和 NR）。需要了解的限制：HUD 与画面一起处理和插帧；放大后的画面比游戏自带的 DLSS 稍软，后者拥有游戏的镜头抖动（DLAA 没有这一限制）；Unity 游戏低于屏幕分辨率时请用窗口化而非全屏窗口；以 scRGB（16 位浮点）显示的游戏保留自己的帧，没有帧生成；Tensor Cores 仅适用于 DirectX 12；使用帧生成时请关闭 Smooth Motion。尚未在任何 RTX 20、RTX 30 或 RTX 50 显卡以及其他驱动上测试。

### 2.0.0 的其他更改

- **菜单支持十一种语言：**德语、西班牙语、荷兰语、波兰语、巴西葡萄牙语、越南语、俄语和日语加入英语、法语和简体中文。阿拉伯语是 Hub 的语言之一，在菜单中显示为英语。菜单现基于 Dear ImGui 1.93。感谢 [@Matarto](https://github.com/Matarto) 提出波兰语的需求（[Hub issue 12](https://github.com/SilyNoMeta/rtx-encore-hub/issues/12)）。
- **使用 NVIDIA App 最新的 DLSS 模型：**原“Force NVIDIA OTA models”选项（系统选项卡）现在名副其实，适用于帧生成、Super Resolution、Ray Reconstruction 和 Neural Rendering：当 NVIDIA App 已下载比游戏更新且经 RTX Encore 认可的模型时，就使用该模型，不向游戏或 NVIDIA App 的文件写入任何内容。这让自带较旧 DLSS-G 的游戏也能使用帧生成预设 C，FG 预设下方会有提示。帧生成已在 RTX 4090 和 RTX 3070 Ti Laptop 上的赛博朋克 2077 中验证；Super Resolution、Ray Reconstruction 和 Neural Rendering 尚未在游戏中检查。感谢 [@Kbx-7](https://github.com/Kbx-7) 的建议（[#101](https://github.com/SilyNoMeta/rtx-encore/issues/101)）。
- **Ghost of Tsushima 中的 XeFG：**XeFG 以 X2 被接受却没有生成任何帧：游戏传递深度和运动的方式 RTX Encore 以前不接受。现已支持；维护者在游戏中看到 X2 和 X4 生成。感谢 [@MoDymm-debug](https://github.com/MoDymm-debug) 的报告和日志（[#100](https://github.com/SilyNoMeta/rtx-encore/issues/100)）。
- **使用 XeFG 时开关帧生成导致崩溃（Crimson Desert）：**以 XeFG 为引擎时切换游戏的帧生成可能导致游戏关闭。已在 RTX 3070 Ti Laptop 上复现并修复；随后连续多次开关均未崩溃。感谢 [@pradeepkumar35](https://github.com/pradeepkumar35) 的报告（[#103](https://github.com/SilyNoMeta/rtx-encore/issues/103)）。报告者尚未复测。
- **自带 DirectX 12 Agility SDK 的游戏中的 Tensor Cores（Crimson Desert 等）：**自 1.5.3 起，XeFG 和 XeSS Tensor Cores 在所有此类游戏中被拒绝，改走标准 Intel 路径。两者现已恢复，已在 RTX 4090 上的 Crimson Desert 中检查（复现 #103 时发现）。XeSS Tensor Cores 路径还接受 NVIDIA 621.12 热修复驱动，依据驱动包识别，尚未在游戏中验证。
- **No Man's Sky 无法启动（Vulkan）：**自 1.2.2 起，游戏旁边找到的 Intel XeSS 运行库即使选择了 DLSS 也会在游戏启动时准备，报告者的配置在此处崩溃。现在只有在游戏启动时 XeSS 是所选引擎才会触及它；Vulkan 中不再提供即时切换到 XeSS。维护者的机器上未能复现；感谢 [@sweddd](https://github.com/sweddd) 的报告和日志（[#104](https://github.com/SilyNoMeta/rtx-encore/issues/104)）。报告者尚未确认。
- **No Man's Sky（Vulkan）中用 XeFG 替换游戏的 DLSS 帧生成：**接受 HDR10 输出（不接受 scRGB：保留游戏的 DLSS-G）；修复了安装 ReShade 6.8 时第一帧崩溃的问题；生成帧不再等待屏幕刷新即显示，与游戏自己的 DLSS-G 相同，游戏帧率因此不再被限制在刷新率除以倍率；倍率可即时更改。维护者已在 HDR 下以 X4 验证。**已知限制：**游戏使用 DLAA 时 XeFG 不生成任何帧（游戏的全分辨率运动不符合 XeFG 的约定）：请选择 DLSS 质量模式。
- **No Man's Sky（Vulkan）中的 XeSS Super Resolution：**游戏的深度缓冲被拒绝，DLSS 继续运行。XeSS 现已接受它；当选择了 XeSS 却运行了 DLSS 时，菜单会在**引擎**下方说明原因。
- **平滑 X3 / X4 改为自动：**该设置现在为**自动**（默认）、**开**或**关**。自动模式下，RTX Encore 观察 X3 及以上时生成帧到达屏幕的方式，并为该游戏学习是否应由 Streamline 自行安排帧的节奏；通知会说明它看到了什么、改变了什么，从下次启动生效。Hub 显示学习到的选择并可重置。已在 PEAK 的 DirectX 12 中检查。
- **Football Life 2027（及其他受保护的可执行文件）安装 RTX Encore 后启动约 1.5 秒即关闭。**已修复；菜单照常附加。
- **没有 DLSS 的游戏中的菜单文字（Skyrim Special Edition）：**使用 RTX Encore 运动估计时，静止的文字会变形；现在它的运动严格为零。菜单中滚动、淡入淡出或放大的文字在 DLSS 或 XeSS 之后留下的拖影更少。尚未在游戏中确认。当 DirectX 11 游戏把显示移到另一个线程时（如 Skyrim SE），XeFG 也会继续工作。
- **菜单：**前两个选项卡现在名为**帧率**和**画质**，在每种语言中名称都不同；添加的引擎直接在**引擎**中选择，选择**无**即可关闭；XeSS 或 XeFG 运行于 Tensor Cores 上时，超分辨率和帧生成卡片显示绿色的 **Tensor Cores** 标记；Tensor Cores 开关在 DirectX 11 和 Vulkan 游戏中隐藏，因为那里它们无法起作用；**Fast super resolution** 仅在适用的 RTX 30 显卡上显示。

### 已检查的内容

上述游戏由维护者使用这些更改的测试版本在 RTX 4090 上游玩（#101 和 #103 还使用了 RTX 3070 Ti Laptop）。游戏未使用最终的 2.0.0 二进制文件复测。发布检查涵盖编译、这些功能的回归测试套件和压缩包完整性；完整测试套件未重新运行。

## 1.6.0

1.6.0 将帧生成加速推向更进一步：在 RTX 40 和 RTX 30 上，每一帧生成帧的开销再次降低，且与 NVIDIA 的输出逐像素一致。

RTX 4090，DLSS-G 310.9.16，每一帧生成帧的 GPU 时间：

| 场景 | 关闭优化 | 1.5.3 | 1.6.0 | 相比 1.5.3 | 相比关闭 |
| :--- | ---: | ---: | ---: | ---: | ---: |
| 1080p X2 | 0.781 ms | 0.538 ms | 0.512 ms | −4.8 % | −34.4 % |
| 1080p X6 | 0.330 ms | 0.292 ms | 0.276 ms | −5.5 % | −16.4 % |
| 4K X2 | 1.406 ms | 1.120 ms | 1.100 ms | −1.8 % | −21.8 % |
| 4K X6 | 0.766 ms | 0.687 ms | 0.675 ms | −1.7 % | −11.9 % |

RTX 3070 Ti：重写后的各阶段比 1.5.3 快 4% 到 5%（第一网络）和 21% 到 24%（第二网络）；整帧收益尚未在游戏中实测，RTX 20 也尚未测量。

- **ReShade 与 Neural Rendering 同时使用时崩溃（星空）：** 加载 Luma 等 ReShade 6.8 插件时，Neural Rendering 启动或重建几分钟后，游戏可能因堆损坏而关闭。Neural Rendering 现在复用其描述符堆，而不再每帧创建并销毁一个，后者会不断触发 ReShade 代理中一条脆弱的路径。已在维护者的星空安装上用报告者的模组组合复现并修复；维护者随后使用该修复和同一模组组合（ReShade 与 Luma）游玩了 35 分钟，未出现崩溃。感谢 [issue 98](https://github.com/SilyNoMeta/rtx-encore/issues/98) 的报告者提供详细的崩溃日志。
- **星空及其他 Raw Input 游戏中的菜单光标：** 在不移动 Windows 光标就读取鼠标的游戏中，菜单光标会停在屏幕中央，直到游戏自己的暂停菜单释放它。菜单现在直接根据鼠标数据包移动光标。已在星空中验证；ReShade 叠加层打开时，ReShade 仍会像对待其他叠加层一样独占鼠标。

### 已知问题

- **星空搭配 Luma 2.0（原生 HDR）：** 镜头移动时，生成的帧呈现浮雕般的效果，并带有红色和青色边缘。这是 Luma 的 HDR 输出与 DLSS 帧生成共同造成的，并非本版本引入：旧版本同样出现，关闭 Luma 后即消失。请关闭 Luma 或其 HDR 输出。

## 1.5.3

- **Microsoft Flight Simulator 2024 菜单：**修复兼容 Streamline 配置中菜单无法打开的问题。感谢 [@jtsorlinis](https://github.com/jtsorlinis) 提供报告与详细日志（[#94](https://github.com/SilyNoMeta/rtx-encore/issues/94)）。仍待报告者确认修复。

- **XeFG 与 ReShade 插件及手动安装：**当游戏的设备经由 ReShade 代理（RenoDX 等插件）到达 Streamline，或在未使用 Hub 手动安装 Mod 时，即使游戏自带合格的 Intel 运行库，XeFG 引擎也会拒绝启动。两种情况现在均可正常进行；存在 Hub 安装的运行库时仍优先使用。于 Crimson Desert 报告（[issue 93](https://github.com/SilyNoMeta/rtx-encore/issues/93)）；报告者尚未在游戏内确认。
- **NVIDIA 621.12 热修复驱动：**XeFG 与 XeSS Tensor Core 选项此前在 621.01 之外的任何驱动上都会被拒绝，现在也接受 10 月 27 日的 621.12 热修复驱动。其驱动文件从安装包本身识别，尚未经游戏内验证；若驱动行为与预期不完全一致，选项将回退到标准 Intel 路径。RTX 20/30 的驱动 Smooth Motion 仍需其列出的驱动之一。
- **Ray Reconstruction 预设 F：**RR 模型预设现在提供 F，即 Ray Reconstruction DLL 310.9.1 及更新版本附带的第二代 transformer（RR2）。旧版 DLL 会忽略它并保留默认值（[issue 95](https://github.com/SilyNoMeta/rtx-encore/issues/95)）。
- **XeFG 与 XeSS Tensor Core 支持 RTX 30、全部 RTX 40 与 RTX 50：**两个选项此前仅在 RTX 4090 上准入。现在在 NVIDIA 621.01 下接受所有 NVIDIA RTX 30、RTX 40 和 RTX 50 显卡，并使用为各架构编译的同一加速路径。RTX 20 仍被拒绝并保留标准 Intel 路径。目前仅 RTX 4090 完成了游戏内验证；试用时请报告你的 GPU、驱动和游戏（[issue 99](https://github.com/SilyNoMeta/rtx-encore/issues/99)）。

也感谢 [@pradeepkumar35](https://github.com/pradeepkumar35)（#93）、[@dm1z0r](https://github.com/dm1z0r)（#95）及 [@Edward24RD](https://github.com/Edward24RD)（#99）的报告与建议。新增 Tensor 准入和 621.12 仍属实验性；游戏内验证仍限于较早的 RTX 4090 / 621.01 配置。不宣称新的性能提升。最终 1.5.3 文件未单独进行游戏内复测。发布检查涵盖编译、针对性回归与压缩包完整性；未重新运行完整测试套件。

## 1.5.2

- **开启 V-Sync：**Frames → Frame pacing 中的 V-Sync 设置现在提供 游戏、关闭 和 开启。开启 会请求 DirectX 游戏开启 V-Sync，适用于启用帧生成后禁用自身 V-Sync 选项的游戏，以及 NVIDIA 控制面板 V-Sync 无效的混合显卡笔记本。NVIDIA 控制面板强制设置的优先级更高；对 Vulkan 游戏和 XeSS-FG 无效；在 60 Hz 屏幕上使用高倍数时，开启 可能增加延迟。已保存的“关闭 V-Sync”选择会被保留。感谢 [@pradeepkumar35](https://github.com/pradeepkumar35) 的建议（[#93](https://github.com/SilyNoMeta/rtx-encore/issues/93)）。

维护者在混合显卡 RTX 3070 Ti Laptop、NVIDIA 621.01、D3D12、DLSS-G 310.9.16 上使用 Crimson Desert 检查了即时切换。观察到的会话中，游戏接受了请求的 V-Sync 呈现更改，没有 Present 失败。尚未测量可见撕裂的消除、显示帧节奏或额外延迟；报告者尚未测试此修复。这些观察来自测试版本，最终 1.5.2 文件未单独进行游戏内复测。

请在游戏内菜单使用新的三选项控制：Hub 1.5.2 尚未提供该选项。未重新运行完整自动化测试套件；已验证编译、针对性检查和发布文件完整性。

## 1.5.1

- **菜单和启动兼容性：**修复通过 Streamline 创建显示的兼容游戏中菜单不出现的问题，以及 MechWarrior 5 使用 `dxgi.dll` 代理时的启动失败。感谢 [@kisoo8](https://github.com/kisoo8) 的报告和日志（[#92](https://github.com/SilyNoMeta/rtx-encore/issues/92)）。维护者使用测试版本确认了 A Plague Tale: Requiem 的菜单和帧生成控制，以及 MechWarrior 5 的 D3D11/D3D12 运行。
- **RTX 20 Smooth Motion：**现在始终使用 V2。移除 V1/V2 选择器及不再使用的 V1 文件，忽略旧配置中的选择。功能仍为实验性，图形 API 和驱动要求不变。Hub 1.5.2 对 mod 1.5.1 及更高版本隐藏已移除的选择器。
- **项目名称：**统一菜单和诊断标识为 RTX Encore。

最终发布文件未单独进行游戏内复测。发布验证仅包括编译、针对性检查和文件完整性；未重新运行完整自动化测试套件。

- **赞助支持：**感谢 [@Scarhead666](https://github.com/Scarhead666) 赞助 RTX Encore 的开发。公开赞助者致谢显示在“关于”选项卡和项目 README 中。

## 1.5.0

- **XeSS Tensor Core：**新增独立、默认关闭的 NVIDIA Tensor XeSS Super Resolution 选项，支持即时切换及最终重建优化，并修复自动曝光和非整数渲染尺寸。
- **XeFG Tensor Core：**新增独立、默认关闭的 NVIDIA Tensor XeFG 选项，在支持容量内即时切换固定 X2–X6 及开关状态。改善兼容配置在切换 Ray Reconstruction 后的恢复。
- **Hub 1.5.0 与配置：**提供两个开关，说明初始硬件范围，并在社区配置中保留其值。安装新模组前请关闭游戏；切换 FG 引擎仍需重启。

初始验证范围为 RTX 4090、NVIDIA 621.01、DirectX 12 和 Hub 提供的兼容 Intel 运行库。准备不受支持时保留标准 Intel 路径；Tensor 执行故障后须重启。两个选项均属实验性质。

较早巫师 3 测量在同一场景中显示 XeSS GPU 时间约减少 30.7%，渲染源帧 FPS 约提高 2.8–3.2%。后续最终重建更改通过确定性重放验证，没有新的游戏内对比。尚未证明 XeFG 的普遍 FPS 或延迟收益。维护者在测试版本中确认了 XeFG 即时控制与 RR 恢复；最终 1.5.0 二进制文件未单独在游戏内复测。[条件与控制](docs/INTEL-ENGINES.zh-CN.md)。

## 1.4.7

- **巫师 3：**修复 1.4.6 中仍然存在的 NR 快速旅行后阴影闪烁。维护者测试修复版本时未见闪烁，随后重新安装原版 1.4.6 再次复现问题。感谢 [@Scarhead666](https://github.com/Scarhead666) 提供报告和版本对比（[#88](https://github.com/SilyNoMeta/rtx-encore/issues/88)）。报告者尚未测试此次新修复。

- **文档：**补全默认快捷键表，并标明 RTX 20 Smooth Motion 的实验性质以及 DirectX 12 和驱动要求。

本地自动化测试覆盖修复路径。维护者的游戏内对比使用测试版本；最终 1.4.7 发布二进制文件未经过单独的游戏内复测。

## 1.4.6

- **更简洁的菜单：**高级控件仅在其主功能启用时显示，包括注视点 NR、叠加层详情和捕获设置。已保存的值保持不变。
- **Mirage 与 Shadows：**修复 Mirage 菜单缺失及 Shadows 倍率报告。在 Mirage 等兼容的 DirectX 12 游戏中，在 Encore 选择 **DLSS-FG**，并启用游戏的 **Intel 帧生成**，即可使用 NVIDIA 帧生成及经过资格检查的固定倍率。原生 FSR 和已有原生 DLSS FG 保持原引擎。感谢 [@kisoo8](https://github.com/kisoo8) 提供报告和日志（[#89](https://github.com/SilyNoMeta/rtx-encore/issues/89)）。维护者已在 Mirage 确认最终测试版本及运动伪影修复；发布文件尚未单独在游戏中复测。
- **Reflex 与延迟：**菜单和叠加层增加游戏 / On / On+Boost 控件及延迟报告。不会降低游戏的选择，在动态模式或 XeLL 管理延迟时暂停。不保证降低延迟。感谢 [@kisoo8](https://github.com/kisoo8) 提供日志（[#87](https://github.com/SilyNoMeta/rtx-encore/issues/87)）；维护者已在游戏中测试控件。
- **REFramework：**改进通过重定向路径加载相同兼容 NR 运行库时的识别。感谢 [@aziznasrimay45-sys](https://github.com/aziznasrimay45-sys) 提供报告（[#90](https://github.com/SilyNoMeta/rtx-encore/issues/90)）。已通过自动化检查，无单独游戏验证。

- **巫师 3：**尝试修复 NR 快速旅行后的阴影闪烁；[@Scarhead666](https://github.com/Scarhead666) 报告此问题始于 1.4.3（[#88](https://github.com/SilyNoMeta/rtx-encore/issues/88)）。此次尝试未完全解决问题：报告者和维护者随后均确认 1.4.6 仍会闪烁。

## 1.4.5

- **RTX 40 / RTX 50 原生 Smooth Motion：**现有开关可在受支持的驱动上启用 NVIDIA 原生路径。请选择游戏实际使用的图形 API，并重启。RTX 20/30 的行为及精确驱动要求保持不变。[要求](docs/SMOOTH-MOTION.zh-CN.md)。
- **计数与状态：**使用所要求的简单 2x FPS 估算，包括原生支持的显卡。这不是对实际显示或插值帧的测量。状态会区分驱动已准备与图形 API 不匹配。
- **DirectX 11 菜单：**修复部分 DirectX 11 游戏中菜单不显示的问题，包括战争之人 II。
- **实时设置与稳定性：**改进 DLSS 预设切换、延迟图形工作、资源处理及缺少完成跟踪时的处理。部分 DirectX 12 配置最多支持四次实时预设替换；菜单提示时，请重启后再继续切换。保留资源的内存成本尚未在游戏中测量。
- **HUD 与控制：**在有限时间内重试暂时失败的 HUD 纹理分配，并修复待处理帧生成设置未重新应用的问题。

维护者已使用测试版本在战争之人 II 中确认菜单可见，并在 RTX 4090、621.01 驱动上确认原生 Smooth Motion 的准备及呈现路径。这不证明插值帧数量、画质、延迟或长时间稳定性。RTX 50 与原生 Vulkan 尚无相应硬件验证。本地自动化测试覆盖修正路径；最终 1.4.5 文件尚未单独在游戏中复测。Intel XeSS / XeFG、Pre-SR 和 RTX 20 实验性 Smooth Motion 仍保留既有限制。[Hub 1.4.4](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.4) 提供对应设置。

## 1.4.4

- **阿凡达：潘多拉边境：**修复 Neural Rendering 在极暗 HDR 场景中产生的彩色噪点及绿色伪影。为保留原始图像，最暗阴影中的 NR 效果会适当减弱。感谢 [@DangerousBerries](https://github.com/DangerousBerries) 的报告（[#80](https://github.com/SilyNoMeta/rtx-encore/issues/80)）。
- **燕云十六声及 Direct3D NR 兼容性：**修复游戏自带旧版着色器编译器时 Neural Rendering 无法启动的问题。请保留游戏原有文件，无需重命名或替换其编译器。感谢 [@mikotorz](https://github.com/mikotorz) 的报告（[#85](https://github.com/SilyNoMeta/rtx-encore/issues/85)）。
- [Hub 1.4.2](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.2) 可识别燕云十六声的游戏程序，并要求确认不确定的安装目标。

维护者已使用测试版本确认所测阿凡达场景的改善，以及燕云十六声中的 NR 可用。这不覆盖所有场景或配置，最终 1.4.4 文件尚未单独在游戏中复测。本地自动化测试覆盖 HDR 阴影重建及 Direct3D 着色器兼容性；不代表普遍验证了画质、性能或时间稳定性。Intel XeSS / XeFG 和 Pre-SR 仍属实验性。

## 1.4.3

- **绝区零：**修复 XeFG 与游戏原有 Streamline 文件的兼容性，并改进内置菜单及 Neural Rendering 的启动。请保留游戏原有文件。感谢 [@WBW333](https://github.com/WBW333) 的报告（[#83](https://github.com/SilyNoMeta/rtx-encore/issues/83)）。
- **Intel 运行库：**识别游戏原生目录中的兼容组件。[Hub 1.4.1](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.1) 提供对应的安装和修复改进。
- **Neural Rendering：**支持 R16 浮点深度，包括心灵杀手 2 报告的格式，并根据图形 API 支持更多归一化运动及浮点输入格式。感谢 [@MrMicko](https://github.com/MrMicko) 提供报告和日志（[#76](https://github.com/SilyNoMeta/rtx-encore/issues/76)）。

绝区零修复已在本地测试版本中通过 XeFG、内置菜单和 NR 的游戏内检查。最终启动清理及最终 1.4.3 文件尚无单独的游戏内验证。NR 输入转换已有本地自动化检查，包括 WARP 和 RTX 4090 上的 Vulkan；这些测试不验证 NVIDIA NR 的画质或性能。尚未获得报告者对心灵杀手 2 修复的确认。Intel XeSS / XeFG 和 Pre-SR 仍属实验性。

## 1.4.2

- **Pre-SR / Post-SR 比较：**修复游戏内截图预览严重像素化的问题。可将完整图像适配面板，或取消 **Fit to panel**，以 **1x** 检查，每个截图像素对应一个屏幕像素，并通过缩放和平移查看细节。
- **截图检查：**重新截图后更新预览；尺寸不同的截图改为并排显示，避免叠加不匹配的图像。

像素映射、显示缩放及截图更新已有本地自动化 CPU 测试和合成面板预览覆盖。最终 1.4.2 二进制文件尚未在游戏中验证；画质、性能、内存开销和延迟仍待测量。截图来自连续的不同帧，HDR 显示为 SDR 预览。现有 Hub 设置和比较报告保持兼容。

## 1.4.1

- **稳定性：**改进图形操作失败时的分辨率切换、重建设置、HUD 捕获和帧生成处理。
- **错误处理：**修复资源生命周期、加载和内存分配失败时可能留下过期状态或导致游戏中断的问题。
- **设置与诊断：**改进配置验证、菜单状态和可选诊断的可靠性。
- **安装包：**加强安装归档准备过程中的检查。

修正内容已有本地编译和自动化 CPU 测试覆盖。最终 1.4.1 二进制文件尚未在游戏中验证；画质、性能、延迟及内存开销仍待测量。部分同步和运行库生命周期风险仍未解决。Intel XeSS / XeFG、Pre-SR 以及 RTX 20 上的 Smooth Motion 仍属实验性。

## 1.4.0

- **实验性重建前 NR（Pre-SR）：** 在普通 DLSS Super Resolution 之前应用 Neural Rendering，支持一至四轮，每轮可独立选择模型配置和外观。Post-SR 仍为默认模式。Pre-SR 需要渲染尺寸的均匀输入；不支持 RR、DLAA、中央优先处理、带填充的尺寸和带抖动的运动输入。不支持的输入保留原始 SR 图像。
- **图像与性能实验室：** 分别采集 Post-SR 和 Pre-SR 数据，使用缩放或 A/B 滑动查看截图，并导出 JSON、CSV 和本地图像查看器。缺失的测量保持不可用。内存计数保留各自范围，不能证明 Pre-SR 带来的节省。只有观测到的 SR/FG 条件及其视口关联均已确认，才允许比较两组数据。当前缺少该关联的路径仅显示各组测量和截图，不报告相对提升。
- **NR 逐轮编辑器：** 自适应轮次卡片、处理顺序显示，以及各轮独立的模型和外观控制。[要求与比较限制](docs/NEURAL-RENDERING.zh-CN.md)。
- **DLSS-G 控制：** 修复 Encore 对固定、动态和游戏模式的控制，并应用 Encore 请求的动态目标，避免继续使用游戏目标。目标并非实际显示 FPS 的严格上限。 感谢 [@wer2357666](https://github.com/wer2357666) 提供报告与日志（[Hub #4](https://github.com/SilyNoMeta/rtx-encore-hub/issues/4)）。
- **游戏内菜单语言：** 英语、法语和简体中文，可按游戏保存选择，未翻译消息回退到英语。感谢 [@pandaligx](https://github.com/pandaligx) 提出请求（[#78](https://github.com/SilyNoMeta/rtx-encore/issues/78)）。
- **关于选项卡：** 版本、项目与 Hub 链接、GitHub Sponsors、Ko-fi，以及对公开赞助者的致谢，包括 [@mennogreg](https://github.com/mennogreg)。

新的 Pre-SR 路径、多轮处理变更、比较工具和 DLSS-G 控制经过本地编译及 CPU 测试；本版本的 GPU、游戏内画质、性能及物理延迟仍未验证。连续截图来自不同帧，不能证明时间稳定性。Intel XeSS / XeFG 和 RTX 20 的 Smooth Motion 仍为实验性。[Hub 1.4.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.0) 提供相应设置和报告读取功能。

## 1.3.1

- **Alan Wake 2：**修复游戏曝光输入导致 Neural Rendering 无法启动的问题，并改善菜单打开时的鼠标处理。感谢 [@SymbioticTortoise](https://github.com/SymbioticTortoise) 提供报告和日志（[#76](https://github.com/SilyNoMeta/rtx-encore/issues/76)）。
- **Neural Rendering：**在 DirectX 11、DirectX 12 和 Vulkan 中支持更多浮点曝光格式（RG16、RG32 和 RGB32），具体取决于各 API 的格式支持。
- **Vulkan Neural Rendering：**在 RTX 30 之外启用 RTX 20、RTX 40 和 RTX 50 路径。感谢 [@Yukk1No](https://github.com/Yukk1No) 提供 RTX 4070 SUPER 报告（[#74](https://github.com/SilyNoMeta/rtx-encore/issues/74)）。新增显卡世代仍属实验性，本版本尚未在这些实体显卡上验证。
- **Vulkan 兼容性：**改善与其他图形模组的兼容性，跟进 [@deusimperatork](https://github.com/deusimperatork) 的 Baldur's Gate 3 / bg3fgvk 报告（[#72](https://github.com/SilyNoMeta/rtx-encore/issues/72)）。仍不支持与 OptiScaler 组合使用。
- **WRC Generations：**支持游戏原版 Streamline 1.3.3 集成，并修复游戏启用帧生成后菜单仍显示 Waiting 的问题。请保留整套原版 Streamline 文件；不支持混用新旧组件。感谢 [@dm1z0r](https://github.com/dm1z0r) 的报告（[#75](https://github.com/SilyNoMeta/rtx-encore/issues/75)）。

维护者已在测试版本中确认 Alan Wake 2 正常运行及 WRC 的 FG 菜单状态。WRC 菜单只表示已启用的模式，不测量实际显示倍率。最终 1.3.1 二进制文件尚未在游戏中复测。新增曝光格式已有本地自动化测试，但未在每种 API 上验证画质与性能。Intel XeSS / XeFG 和 RTX 20 上的 Smooth Motion 仍属实验性。

## 1.3.0

- 中央优先 Neural Rendering（实验性，默认关闭）：NR 在图像中央以全强度运行，越靠近边缘越轻，
  以更柔和的边缘 NR 效果换取更高 FPS。在菜单中开启 **Foveated NR** 并选择预设（Light、Balanced、Strong）；
  预设背后的数值位于 **Foveated NR: custom values**。在 RTX 3070 Ti Laptop 上，Strong 在 Cyberpunk 2077 中
  带来平均 FPS 提升 27%。感谢 [maximedgoulet](https://github.com/maximedgoulet) 提供这一构想
  （[FoveatedDLSS5](https://github.com/maximedgoulet/FoveatedDLSS5)）。

- 在运动矢量为输出分辨率的游戏中（Unreal Engine 5、Black Myth: Wukong），Neural Rendering 现在能正确跟随运动：
  此前它只能看到部分运动。

- **《博德之门 3》（Vulkan）搭配 bg3fgvk：** 不再在启动时崩溃，Neural Rendering、DLSS 与 DLSS 帧生成现在可以同时运行。感谢 [@deusimperatork](https://github.com/deusimperatork) 提供详细的报告和日志（[#72](https://github.com/SilyNoMeta/rtx-encore/issues/72)）。不支持在该游戏中同时使用 OptiScaler。

- 移除可选的 Cyber Engine Tweaks 面板。请使用内置菜单（Insert）或 Hub。首次启动时，若模组旁没有现有配置，将复制旧面板的配置并保留原文件。

- 移除实验性的 OpenNR 引擎及其设置。NVIDIA Neural Rendering 仍可使用。升级旧 OpenNR 配置时会关闭 NR；重新开启即可使用 NVIDIA NR。

- RTX 20 可在 NVIDIA 621.01 驱动和 DirectX 12 下使用 Smooth Motion，启用后自动选择显卡。
  在 Hub 或 RTX 20 的游戏内菜单中选择 **V1（测试版 1，默认）** 或 **V2（测试版 2）**，然后重启游戏。
  感谢 [@aziznasrimay45-sys](https://github.com/aziznasrimay45-sys) 在 RTX 2070 上测试五款游戏，
  并在 Resident Evil Requiem 中偏好 V1；感谢 [@MxHassan195](https://github.com/MxHassan195)
  在 RTX 2060 的 The Witcher 3 中比较两版，并认为 V2 更流畅
  （[#53](https://github.com/SilyNoMeta/rtx-encore/issues/53)）。性能取决于显卡和游戏；
  这些反馈针对实验测试版，并非最终 DLL 的验证。

## 1.2.2

- **重建后的锐化。** DLSS、DLAA、Ray Reconstruction 和替代 XeSS 可选择 Off、Game 或 Custom。默认关闭；Custom 默认强度为 30%，修改即时生效。强度过高可能产生光晕并放大噪点。原始 DLSS 版本低于 2.5.1 时保留其自身锐化。感谢 [@aziznasrimay45-sys](https://github.com/aziznasrimay45-sys) 提出需求（[#70](https://github.com/SilyNoMeta/rtx-encore/issues/70)）。
- **Smooth Motion 可在 SurrounDead（DirectX 12）中正常启动。** 维护者已在 RTX 3070 Ti Laptop、NVIDIA 621.01 驱动上确认游戏内正常工作。
- **Baldur's Gate 3（Vulkan）：** 修复使用 bg3fgvk 和 OptiScaler F5 时，内置菜单在特定情况下退回独立窗口的问题。感谢 [@deusimperatork](https://github.com/deusimperatork) 提供报告（[#72](https://github.com/SilyNoMeta/rtx-encore/issues/72)）。
- **DLSS Frame Generation 与 OptiScaler F5 的兼容性：** 修复 RTX 20/30 上帧生成在特定情况下不可用的问题，包括 Final Fantasy VII Rebirth。感谢 [@lovebabyx](https://github.com/lovebabyx) 提供报告（[#73](https://github.com/SilyNoMeta/rtx-encore/issues/73)）。

锐化已有本地自动化检查；Vulkan 运行、实际游戏画质和性能仍待验证。#72 和 #73 的报告者尚未确认此版本中的修复。Intel XeSS 和 XeFG 仍为实验性功能。[锐化设置](docs/SUPER-RESOLUTION.zh-CN.md)。

## 1.2.1

- 在启动前安装兼容的 Intel 运行库后，No Man’s Sky 可在游戏中直接切换 DLSS 与 XeSS，无需重启。

- **Elden Ring 与 ERSS-FG：** 改善启动与菜单兼容性。感谢
  [@mennogreg](https://github.com/mennogreg) 提供[问题报告](https://github.com/SilyNoMeta/rtx-encore/issues/64)。
- **菜单兼容性：** 改善 Quarantine Zone 与 OptiScaler、HumanitZ 与 ReShade 的兼容性。感谢
  [@PhantomGamers](https://github.com/PhantomGamers)（[#54](https://github.com/SilyNoMeta/rtx-encore/issues/54)）和
  [@sweddd](https://github.com/sweddd)（[#68](https://github.com/SilyNoMeta/rtx-encore/issues/68)）提供报告。
- **Neural Rendering** 在渲染分辨率变化时保留配置的处理轮次。感谢
  [@amernet91](https://github.com/amernet91) 提供[报告](https://github.com/SilyNoMeta/rtx-encore/issues/65)。
- 更新 Intel 运行库版本兼容性。
- **NVIDIA 运行库兼容性**不再要求文件位于预设列表中。旧的 `providerPatchPolicy` 设置将被忽略。Intel 运行库检查保持不变。

## 1.2.0

**Intel XeSS 和 XeFG 是新增的实验性功能。** 请预期可能出现错误、崩溃、画面伪影，以及不同游戏和图形 API 之间的行为差异。
欢迎报告问题：在 RTX Encore Hub 中打开游戏的 **健康** 页面，选择 **生成诊断包**，检查并移除个人信息后，将诊断压缩包
附加到 [RTX Encore issue](https://github.com/SilyNoMeta/rtx-encore/issues/new/choose)。请注明选择的引擎、图形 API
和请求的倍率。Hub 不会自动上传该压缩包。

### 新增

- **Intel XeSS 与 XeFG（实验性）。** 可选择重建引擎（DLSS 或 XeSS Super Resolution）和帧生成引擎（DLSS-G 或搭配 XeLL
  的 XeFG）。保留 **Game** 可继续使用游戏原有引擎。Intel 模式默认关闭，需要兼容的游戏输入；选择某个引擎不能证明它已
  激活。请通过 RTX Encore Hub 安装 Intel 运行库。见 [Intel XeSS 与 XeFG（实验性）](docs/INTEL-ENGINES.zh-CN.md)。
- **Ray Reconstruction + Neural Rendering（Direct3D 12）。** 使用兼容线性深度输入的游戏现在可在 RR 之后运行 NR。
  Direct3D 11 和 Vulkan 仍不支持此线性深度情况。
- **按轮设置 Neural Rendering。** 四轮 NR 处理中的每一轮都可替代强度、局部色调、局部结构、皮肤结构和角色遮罩，也可
  继承全局值。数值范围为 0 至 2；遮罩可选 `inherit`、`true` 或 `false`。首轮之后继承的色调来自 `nrLaterPassLocalTone`。
  强度为零仍会运行该轮处理，不会消除 GPU/VRAM 开销。
- **HDR 输入和自动曝光** 使用所选重建引擎共用的 `srHdr` 与 `srAutoExposure`。默认 `game` 跟随游戏，修改后需重启。
  它们不会启用显示器 HDR，也不会将 SDR 转换为 HDR；错误的输入格式会损害画质。某些 DLSS 模型强制使用自动曝光；XeSS
  自动曝光有 GPU 开销，不推荐用于 LDR。XeSS Vulkan 的 LDR 与手动曝光组合仍不受支持。
- 现有 JSONC 设置文件按功能重新排序，Intel 选项位于引擎选择附近。保留已选值、注释和自定义设置。
- **游戏运行中即可切换 DLSS 预设和重建引擎。** Super Resolution 与 Ray Reconstruction 预设，以及 DLSS 与 XeSS Super
  Resolution 之间的选择，现在在 Direct3D 12、Direct3D 11 和 Vulkan 游戏中无需重启即可生效。帧生成预设和帧生成引擎仍需
  重启。在 Vulkan 游戏中，必须在游戏启动时就已选择 XeSS。
- **NVIDIA 621.01 驱动上的 Smooth Motion**（RTX 30）。已在维护者电脑上的 Enshrouded（Vulkan）中验证。
- **Vulkan 游戏中的 Smooth Motion** 现在在游戏的 NVIDIA 配置文件排除它时也始终运行。**Force Smooth Motion** 设置已移除；
  `rtx-encore.jsonc` 中残留的 `smoothMotionForce` 会被忽略。NVIDIA 排除某个游戏可能是有原因的：在这类游戏中仍可能出现
  意外结果。见 [Smooth Motion](docs/SMOOTH-MOTION.zh-CN.md)。
- **菜单会显示引擎和预设。** Upscaling 标签显示 DLSS SR、DLSS RR 或 XeSS-SR，以及 DLSS 功能创建时请求的预设；Frame
  Generation 标签显示 DLSS-G 和 RTX Encore 提供的 FG 预设。驱动不会报告它实际运行的模型。
- 菜单现在可以识别 NVIDIA 驱动提供的 DLSS 模型（例如通过 NVIDIA 应用的 DLSS 覆盖）。

### 修复

- **Vulkan 游戏中的 Smooth Motion 可能保持关闭**：现在即使驱动在游戏启动期间被重新加载，它也能可靠启动。
- **XeFG**：Cyberpunk 2077 窗口大小调整、无边框窗口尺寸、由游戏控制的多帧生成（已在 GTA V Enhanced 中验证 X4）、在
  XeFG 呈现画面上绘制菜单，以及在游戏初始化 Streamline 之前创建 XeFG。
- **XeSS Super Resolution**：在自身命令列表上录制的 Direct3D 12 游戏、XeSS 之后恢复游戏的计算状态、将旧式 Vulkan 集成
  导向 XeSS，以及实时更新 DLSS 回退预设。
- 只有在游戏实际运行 Ray Reconstruction 时，菜单才会提示 XeSS 与 Ray Reconstruction 的组合问题。

维护者已验证：Enshrouded 在 621.01 驱动上的 Smooth Motion、Cyberpunk 2077 中的 XeFG X2 和 X4、GTA V Enhanced 中的
XeFG X4、007 First Light 中的 XeSS SR、No Man's Sky 和 DOOM: The Dark Ages 中的 XeFG X2（Vulkan、无边框、关闭 NR）、
007 First Light 路径追踪下 RR 之后的 NR（尚未测量画质和开销）。运行中切换预设、HDR/曝光覆盖设置和提供程序准入只有
自动化测试；其他游戏、GPU 和驱动未经测试。在 Intel 路径上菜单可能不可用：请使用 Hub 调整设置。

### 已知问题

- **007 First Light** 仍可能在启动时间歇性崩溃；再次启动可能成功。
- **SCSKiller 录制器与 RTX Encore 不兼容。** SCSKiller 录制某个游戏时，其录制器会先于 RTX Encore 接管游戏的交换链创建，
  因此 XeFG 被拒绝，游戏继续使用自己的 DLSS-G（Frame Generation 标签显示 DLSS-G）。在 The Witcher 3 中，游戏还可能在
  加载时卡住，并在帧生成启动时出现 GPU 重置。仍可继续使用 SCSKiller：在其
  **Settings** 中关闭 **Record in all compatible games**（如果曾在该游戏页面单独开启录制，也一并关闭），然后重启游戏。
  其他拦截 DXGI 交换链创建的工具也可能造成同样的结果。

感谢 [@SanyaGames227](https://github.com/SanyaGames227) 提出 HDR 输入与自动曝光设置请求
（[#63](https://github.com/SilyNoMeta/rtx-encore/issues/63)），以及 [@Theoryedz](https://github.com/Theoryedz) 在
[#58](https://github.com/SilyNoMeta/rtx-encore/issues/58) 中提供问题报告。报告者尚未测试此版本。

感谢 [@dashdogy](https://github.com/dashdogy)（MFG Unlocker）提出在游戏运行时切换 DLSS 预设和重建引擎的想法。

## 1.2.0-beta.1

**XeSS / XeFG：高度实验性。** 这部分集成的开发过程非常艰难，也相当折腾 :) 请预期可能出现错误、崩溃、画面伪影，以及不同游戏和图形 API 之间的行为差异。欢迎报告问题：在 RTX Encore Hub 中打开游戏的 **健康** 页面，选择 **生成诊断包**，检查并移除个人信息后，将诊断压缩包附加到 [RTX Encore issue](https://github.com/SilyNoMeta/rtx-encore/issues/new/choose)。请注明选择的引擎、图形 API 和请求的倍率。Hub 不会自动上传该压缩包。

- 现有 JSONC 设置文件按功能重新排序，Intel 选项位于引擎选择附近。保留已选值、注释和自定义设置。
- **Ray Reconstruction + Neural Rendering（Direct3D 12）** — 使用兼容线性深度输入的游戏现在可在 RR 之后运行 NR。已在 RTX 3070 Ti Laptop 上用 007 First Light 路径追踪进行本地检查：NR 可以运行，但尚未测量画质和开销。Direct3D 11 和 Vulkan 仍不支持此线性深度情况。

- **Intel XeSS 与 XeFG（实验性）** — RTX Encore 1.2.0-beta.1 可选择重建和帧生成引擎。保留 **Game** 可继续使用游戏原有引擎。Intel 模式仍属实验性，默认关闭，需要兼容的游戏输入；选择某个引擎不能证明它已激活。
- 四轮 NR 处理中的每一轮都可替代强度、局部色调、局部结构、皮肤结构和角色遮罩，也可继承全局值。数值范围为 0 至 2；遮罩可选 `inherit`、`true` 或 `false`。首轮之后继承的色调来自 `nrLaterPassLocalTone`。强度为零仍会运行该轮处理，不会消除 GPU/VRAM 开销。
- HDR 输入和自动曝光使用所选重建引擎共用的 `srHdr` 与 `srAutoExposure`。默认 `game` 跟随游戏，修改后需重启。它们不会启用显示器 HDR，也不会将 SDR 转换为 HDR。错误的输入格式会损害画质。某些 DLSS 模型强制使用自动曝光；XeSS 自动曝光有 GPU 开销，不推荐用于 LDR。XeSS Vulkan 的 LDR 与手动曝光组合仍不受支持。RR 和帧生成保持不变。
- 本地策略 `providerPatchPolicy` 默认为 `heuristic`。`exact` 仅接受已审查的 NVIDIA 提供程序文件；`heuristic` 还允许与其他符合条件的签名 NVIDIA 文件进行受限兼容。该准入仍未经认证，并会明确报告。修改后需重启。Hub 不会在社区配置中分享此设置；它不是游戏兼容性承诺。

No Man’s Sky 和 DOOM: The Dark Ages 的有限本地 Vulkan 测试支持 XeFG X2 路径；维护者已接受 DOOM 在无边框模式、关闭 NR 时的测试。这不认证独占全屏、DirectX 11、XeFG 与 NR 组合、所有更高倍率或其他游戏。某些 Intel 路径上菜单或叠加层可能不可用；可使用 Hub 调整设置并查看会话报告。

007 First Light 偶发启动崩溃仍未解决。新的 HDR/曝光覆盖设置和提供程序准入有静态及本地自动化检查，但不代表广泛的视觉或游戏验证。

感谢 [@SanyaGames227](https://github.com/SanyaGames227) 提出 HDR 输入与自动曝光设置请求（[#63](https://github.com/SilyNoMeta/rtx-encore/issues/63)）。这是功能请求，不代表报告者验证了最终测试版。

[Intel XeSS 与 XeFG（实验性）](docs/INTEL-ENGINES.zh-CN.md)

感谢 [@Theoryedz](https://github.com/Theoryedz) 在 [#58](https://github.com/SilyNoMeta/rtx-encore/issues/58) 中提供问题报告。报告者尚未测试更新后的版本。

## 1.1.4

包含 1.1.3 的全部内容。

### 修复

- **Neural Rendering 在两遍或更多遍时闪烁**（[#62](https://github.com/SilyNoMeta/rtx-encore/issues/62)）。在 Aion 2 中，画面会闪烁，在角色创建等繁忙菜单中尤其明显，同时 Neural Rendering 指示灯先变黄再变绿。使用两遍或更多遍时，如果游戏恰好在同一时刻提交自己的工作，某一帧可能不会应用 Neural Rendering。现在已不会再发生。
- **Aion 2：菜单光标卡在屏幕中央**，导致无法修改设置。菜单打开时，游戏不再把光标拉回中央。

已在维护者的电脑上用 Aion 2 和两遍进行检查：角色创建界面不再闪烁。报告者尚未测试此版本。维护者此后已在 Aion 2 中确认光标修复。其他游戏、其他显卡以及三遍或四遍均未测试。

### 已知问题

- **007 First Light** 仍可能在启动时偶发崩溃。本版未修复此问题；再次启动可能成功。

### 致谢

- [@unsungloser](https://github.com/unsungloser) — 报告了 Aion 2 中多遍 Neural Rendering 的闪烁问题，并提供了日志和视频（[#62](https://github.com/SilyNoMeta/rtx-encore/issues/62)）。

## 1.1.3

### 修复

- **The Witcher 3: Wild Hunt（DirectX 12）在使用 1.1.2 时启动崩溃**（[#60](https://github.com/SilyNoMeta/rtx-encore/issues/60)）。本版修复了这一回归问题，并保留 1.1.2 的菜单改进。

已在维护者的 GOG 版本上进行本地检查：以 `version.dll` 或 `dxgi.dll` 安装时，均能成功启动并启用菜单。这些是自动化启动检查，并非长时间游戏测试。Steam 报告者尚未测试此修复。

### 已知问题

- **007 First Light** 仍可能在启动时偶发崩溃。本版未修复此问题；再次启动可能成功。

### 致谢

- [@Herror7](https://github.com/Herror7) — 报告了 The Witcher 3 的启动回归问题，并提供了帮助定位原因的日志（[#60](https://github.com/SilyNoMeta/rtx-encore/issues/60)）。

## 1.1.2

包含 1.1.2 各预发布版的全部内容，以及 1.1.2-beta.3 之后的修复。

### 新功能

- **Smooth out X3 / X4**（[#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)）。在部分游戏中，X3 和 X4 会成批显示生成帧，画面看起来比 X2 更卡顿：例如使用 bg3fgvk 的 Baldur's Gate 3。开启此设置后，由 Streamline 自行安排生成帧的间隔，而不交给 NVIDIA 驱动，现在 Vulkan 游戏和 DirectX 12 游戏均适用。默认关闭：可在菜单的 Frame generation 部分（倍数设置下方）、RTX Encore Hub 中，或在 `frameGeneration` 部分设置 `flipMeteringBypass` 来开启，然后重启游戏。已有的设置会被保留。参见[帧生成](docs/FRAME-GENERATION.zh-CN.md#平滑-x3--x4)。
- **覆盖层的版本行**：现在会列出游戏加载的所有 NVIDIA DLL，包括 Ray Reconstruction 和 Neural Rendering。相同版本的 DLL 共用一行（`SR-RR-FG-NR 310.9.16`），未加载的 DLL 不显示。

### 修复

- **Neural Rendering 导致部分 DirectX 12 游戏崩溃**（[#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)）。在 RTX 20、30 和 40 显卡上启用 Neural Rendering 后，无论使用哪个驱动版本，游戏都可能随即崩溃：例如 Resident Evil Requiem、Pragmata 和 007 First Light。现在 Neural Rendering 会将游戏的渲染状态恢复原样。NVIDIA 屏幕 DLSS 指示器也可能导致其失败；现在 Neural Rendering 已具备指示器所需的全部内容。如果 Neural Rendering 启动时仍有步骤失败，日志会指出是哪一步。
- **RTX Encore 菜单不显示**（按 Insert 没有反应）的问题在更多游戏中得到修复：
  - **Pillars of Eternity II: Deadfire** 以及其他在单线程上进行渲染的 DirectX 11 游戏，无论 RTX Encore 以何种文件名安装（[#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)）。
  - **RTX Encore 安装为 `dxgi.dll` 时**，在从它获取画面的游戏中（[#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)）。
  - **使用旧版 NVIDIA Streamline 的游戏**，例如 The Last of Us Part I（[#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)）和 Dying Light 2。在 Dying Light 2 中，请将 RTX Encore 安装为 `winmm.dll`。
  - **DirectX 11 游戏**：以菜单此前无法跟踪的方式创建画面的游戏，包括在启动后才加载其图形部分的游戏。
- **覆盖层**（RivaTuner Statistics Server、Steam、Epic、Rockstar Social Club、Overwolf、GOG Galaxy、Discord、Ubisoft Connect）：RTX Encore 菜单不再附着到它们自己的窗口或微小的辅助画面上，只附着到游戏画面。

已在维护者的电脑（RTX 3070 Ti Laptop）上进行游戏内验证：Resident Evil Requiem 以前在 Neural Rendering 启动时立即崩溃，现在可以正常运行；Black Myth: Wukong 在开启和关闭 DLSS 指示器时仍然正常；Dying Light 2、Rise of the Tomb Raider 和 The Blood of Dawnwalker 中可以打开菜单；在 No Man's Sky 中开启 Smooth out X3 / X4 后，X4 的成批现象消失。尚未在游戏内确认：Pragmata、007 First Light、Pillars of Eternity II、The Last of Us Part I 和 Baldur's Gate 3。

Windows Release 构建，全部六个发行变体；自动回归测试在游戏外通过。

### 致谢

- [@Theoryedz](https://github.com/Theoryedz) — 报告 Resident Evil Requiem 的问题，随后在 Pragmata 和 007 First Light 中开启和关闭 DLSS 指示器进行测试，提供的日志和崩溃记录显示了 Neural Rendering 启动时游戏停止的位置（[#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)）。
- [@sewer86](https://github.com/sewer86) — 报告 Pillars of Eternity II 的问题，提供 1.1.2-beta.1 和 1.1.2-beta.3 的日志，显示了菜单丢失的位置（[#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)）。
- [@deusimperatork](https://github.com/deusimperatork) — 报告 Baldur's Gate 3 的问题，提供的日志和 PresentMon 捕获显示 X4 时生成帧成批到达（[#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)）。
- [@SanyaGames227](https://github.com/SanyaGames227) — 提供 The Last of Us Part I 在 591.86 驱动下的日志，显示菜单从未附着（[#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)）。

## 1.1.2-beta.3 — 预发布版

### 修复

- **开启 NVIDIA DLSS 指示器时的 Neural Rendering**（[#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)）。在开启 NVIDIA 屏幕 DLSS 指示器的情况下，于 RTX 20、30 和 40 显卡上启用 Neural Rendering 后，游戏可能在约两秒后崩溃（在 Resident Evil Requiem、RTX 3090 Ti 上报告）。现在 Neural Rendering 已具备指示器所需的全部内容。如果 Neural Rendering 启动时仍有步骤失败，日志会指出是哪一步，以及如何关闭指示器。

  已在维护者的电脑（RTX 3070 Ti Laptop，Black Myth: Wukong，DirectX 12）上开启指示器进行游戏内验证：Neural Rendering 可以启动并运行。1.1.1 版本在该电脑上同样没有崩溃，因此该修复尚未在 Resident Evil Requiem 中得到确认。

- **以 `dxgi.dll` 安装 RTX Encore 时菜单不显示**：在从它获取画面的游戏中，例如 Pillars of Eternity II: Deadfire（DirectX 11，[#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)）。尚未在游戏中确认：维护者的电脑上无法测试 Pillars of Eternity II。

Windows Release 构建，全部六个发行变体；自动回归测试在游戏外通过。

### 致谢

- [@Theoryedz](https://github.com/Theoryedz) — Resident Evil Requiem 的报告，附带的日志和崩溃记录显示了启动 Neural Rendering 时游戏停在何处（[#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)）。
- [@sewer86](https://github.com/sewer86) — 提供 Pillars of Eternity II 的报告，以及 1.1.2-beta.1 的日志，显示菜单在何处丢失（[#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)）。

## 1.1.2-beta.2 — 预发布版

### 新增

- **Flip metering 绕过现在也适用于 Vulkan 游戏**（[#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)）。在部分 Vulkan 游戏中，X3 和 X4 会成批显示生成帧，画面运动看起来比 X2 更卡顿，例如搭配 bg3fgvk 的 Baldur's Gate 3。开启该设置后，由 Streamline 自行安排生成帧的间隔，而不是交给 NVIDIA 驱动。默认关闭：可在菜单中开启（**Compatibility** > **Flip metering bypass**），或使用 `flipMeteringBypass`，然后重启游戏。参见[帧生成](docs/FRAME-GENERATION.zh-CN.md#兼容性选项)。

  已在维护者的电脑（RTX 3070 Ti Laptop，X4）上进行游戏内验证：在 No Man's Sky 中，开启该设置后成批显示的问题消失；在 DOOM: The Dark Ages 中，驱动原本就能正确安排帧间隔，该设置没有可见差异。Baldur's Gate 3 尚未确认。

### 变更

- 该设置现在名为 **Flip metering bypass**，其说明会告诉你何时值得尝试。
- **覆盖层的版本行**：现在列出游戏加载的所有 NVIDIA DLL，包括 Ray Reconstruction 和 Neural Rendering。版本相同的 DLL 共用一行（`SR-RR-FG-NR 310.9.16`），未加载的 DLL 不显示。

Windows Release 构建，全部六个发行变体；自动回归测试在游戏外通过。

### 致谢

- [@deusimperatork](https://github.com/deusimperatork) — 提供 Baldur's Gate 3 报告，附带日志和 PresentMon 捕获，显示 X4 下生成帧成批到达（[#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)）。

## 1.1.2-beta.1 — 预发布版

### 修复

- **RTX Encore 菜单不显示**（按 Insert 没有反应）的问题在更多游戏中得到修复：
  - **使用旧版 NVIDIA Streamline 的游戏**，例如 The Last of Us Part I（[#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)）和 Dying Light 2。这些游戏中 Smooth Motion 和 DLSS 原本就能工作，只是缺少菜单。在 Dying Light 2 中，请将 RTX Encore 安装为 `winmm.dll`。
  - **DirectX 11 游戏**：以菜单此前无法跟踪的方式创建画面的游戏，包括在启动后才加载其图形部分的游戏。
- **覆盖层**（RivaTuner Statistics Server、Steam、Epic、Rockstar Social Club、Overwolf、GOG Galaxy、Discord、Ubisoft Connect）：RTX Encore 菜单不再附着到它们自己的窗口或微小的辅助画面上，只附着到游戏画面。

已在维护者的电脑（RTX 30）上进行游戏内验证：Dying Light 2（DirectX 11）现在可以打开菜单；Rise of the Tomb Raider（Epic Games Store）和 The Blood of Dawnwalker 仍可正常打开菜单。The Last of Us Part I 无法在此测试，尚未确认。Windows Release 构建，全部六个发行变体；自动回归测试在游戏外通过。

### 致谢

- [@SanyaGames227](https://github.com/SanyaGames227) — 提供 The Last of Us Part I 在 591.86 驱动下的日志，显示菜单从未附着（[#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)）。

## 1.1.1

### 修复

- **带有 Epic Online Services 覆盖层的游戏**：使用 Epic 在线服务的游戏都会附带这个覆盖层，无论你在哪个商店购买。与它相关的两个问题已修复：
  - **RTX 30 在 DirectX 12 下的 Smooth Motion**：游戏启动后不久、覆盖层一出现，就可能以“Device removed”（`0x887A002B`）退出。Rise of the Tomb Raider（Epic Games Store）在使用条款之后退出；现在开启 Smooth Motion 也能进入主菜单。
  - **RTX Encore 菜单不显示**（按 Insert 没有反应，首次启动提示也从不出现）：出现在以某种方式初始化图形的游戏中，例如 Rise of the Tomb Raider，无论是否开启 Smooth Motion（[#50](https://github.com/SilyNoMeta/rtx-encore/issues/50)，感谢 @kakaroto-cmd 指出问题并提供日志）。

  两项修复都已在 Rise of the Tomb Raider（Epic Games Store、DirectX 12、RTX 3070 Ti Laptop）中验证。菜单是在关闭 Smooth Motion 的情况下验证的；其他带有该覆盖层的游戏尚未确认。

## 1.1.0

这个版本迎来了 RTX Encore 的**第一份赞助**。衷心感谢：对一个利用业余时间制作的项目来说，这意义重大。

### 新增

- **Fast super resolution (RTX 30)**，默认关闭：在 RTX 30 显卡上的 DirectX 12 游戏中，DLSS Super Resolution 的预设 **M** 和 **L** 会使用更快版本的 NVIDIA 模型。在 RTX 3070 Ti Laptop 上，我们的测试台测得放大所用的 GPU 时间 **M 减少约 18%，L 减少约 15%**；在游戏中，**The Witcher 3 减少 22%，Black Myth: Wukong 减少 25%**（均为 M）。画面与 NVIDIA 的非常接近，在我们的测试中看不出差别。这些是初步数据，来自一台笔记本和两款游戏：实际收益取决于你的显卡和游戏。见[超分辨率](docs/SUPER-RESOLUTION.zh-CN.md#fast-super-resolution-rtx-30)。
- **即使游戏的 NVIDIA 驱动配置文件强制使用其他预设（NVIDIA 应用的 DLSS 覆盖），在 RTX Encore 中选择的 SR 模型预设现在也会生效**，适用于 DLSS Super Resolution 310.9.1 和 310.9.16。其他版本下仍由驱动优先。
- **Neural Rendering 现在除 310.8.0 外也接受 NVIDIA 的 `nvngx_dlssnr.dll` 310.9.16**。该文件包含两个网络，可在新的 **NR model preset**（位于 SR 和 RR 模型预设旁边）中选择：**B**（默认），更小且快得多；**A**，即 310.8.0 所运行的网络。在 RTX 3070 Ti Laptop 上的 1080p 本地测试中，B 的 NR 时间约 15 ms，A 约 27 ms。RTX Encore 针对 RTX 20 和 30 的优化现在同样适用于该文件的两个网络，画面相同。使用 310.8.0 时没有任何变化。对 310.9.16 的支持是实验性的，测试有限。见 [Neural Rendering](docs/NEURAL-RENDERING.zh-CN.md)。
- 同样接受 Streamline SDK 2.14.16 附带的 DLSS-G 310.9.16，功能与 NVIDIA 应用下载的版本相同：预设 A、B 和 C、Lite、Fast frame generation 以及 UI recomposition。
- 同样识别 Streamline SDK 2.14.16 附带的 Super Resolution 310.9.16（与 NVIDIA DLSS GitHub 上的文件相同）：即使驱动配置文件强制使用其他预设，在 RTX Encore 中选择的 SR 模型预设仍然生效。该文件运行的是 310.9.1 的代码，因此不包含预设 A 至 D；Fast super resolution 可以与它配合使用。
- **开关 Neural Rendering 的快捷键**：默认 **Ctrl+Alt+N**，可在菜单的快捷键编辑器中修改（[#47](https://github.com/SilyNoMeta/rtx-encore/issues/47)，由 @SiTWulf 提出）。
- **RTX 30 上的 Smooth Motion 也接受 NVIDIA 591.86 驱动**（[#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)，由 @SanyaGames227 提出）。仅在游戏外检查过：尚未在游戏中确认。

- `rtx-encore-logs` 文件夹中还有 `rtx-encore-status.json`，即当前会话的状态，供 RTX Encore Hub 读取。

### 变更

- `nrPreset` 更名为 `dlssNrPreset`，位于 `dlssPresets` 部分，与其他模型预设并列，取值为 `"auto"`、`"a"` 或 `"b"`（默认 `"b"`）。旧版本的设置文件仍然有效：其中的 `nrPreset` 会连同取值一起改名，`0` 读作 auto，`1` 读作 a，`2` 读作 b，`3` 读作 auto。
- 菜单中的 **Component compatibility** 区块并入 **Session details**，后者现在还列出 Streamline 帧生成、SR 和 RR 的版本，以及已加载文件不支持的每项功能及其原因。

### 修复

- **同一游戏中的两份 RTX Encore**（以两个文件名存在，例如 `version.dll` 和 `dxgi.dll`）都会修改游戏；同时开启 Ray Reconstruction 和 Neural Rendering 时，画面严重劣化。现在只有第一份运行，日志会指出另一份，请将其删除。
- **DLSS Super Resolution 和 Ray Reconstruction 的尺寸**：现在在 DLSS 创建功能之前，按游戏提交的原样读取输入和输出尺寸；在 Starfield 中，日志可能显示为互换（[#51](https://github.com/SilyNoMeta/rtx-encore/issues/51)，日志由 @Avictuu 提供）。该问题中报告的功耗升高不在此修改的范围内。
- **RTX 30 上使用 616.64 和 620.12 驱动的 Smooth Motion**：在这些驱动下，Smooth Motion 所需的部分工作被拒绝。这可能是 [#35](https://github.com/SilyNoMeta/rtx-encore/issues/35)（由 @CorwinMaximus 报告）的原因；尚未在游戏中确认。
- 文档现在说明：使用 Ray Reconstruction 时，Neural Rendering 处理的是 RR 降噪后的图像，而不是光线追踪的原始图像（[#48](https://github.com/SilyNoMeta/rtx-encore/issues/48)，由 @wisecase2 提出）。

## 1.0.0

**RTX Encore** 的首个稳定版本。它接替以 DLSSG-Transfusion 名义发布的 `v1.4.5.3-rtx20-30-40` 并取而代之；版本号从 1.0.0 重新开始。本条目汇总了自那以后的全部变化，包括各测试版和候选版带来的内容。

### 新增

- **内置菜单和叠加层。** 在游戏中按 **Insert**：所有设置、每项功能的状态，以及显示帧率、倍数、帧节奏、GPU、显存和已加载的 DLSS、DLSS-G、Streamline 版本的叠加层。适用于 DirectX 11、DirectX 12 和 Vulkan 游戏；不再需要 ReShade。**System → Component compatibility** 列出已加载的组件、它们支持的功能，以及某个选项不可用的原因。见[菜单与叠加层](docs/MENU-AND-OVERLAY.zh-CN.md)。
- **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)**，Windows 配套应用：在你的游戏中安装 RTX Encore，同时带来最新的已验证 DLSS 和 Streamline 文件以及 NVIDIA 的 Neural Rendering 文件，一键更新所有游戏，并且随时可以把游戏恢复原状。手动安装方式与以前一样；见[安装](docs/INSTALLATION.zh-CN.md#最简单的方式rtx-encore-hub)。
- **Neural Rendering**，适用于 RTX 20、RTX 30 和 RTX 40，作用于 DLSS 输出的图像：一到四个处理阶段，每个阶段可选风格，支持 HDR，可选择 NVIDIA 引擎或实验性的 Open 引擎。默认关闭；需要 NVIDIA 的 `nvngx_dlssnr.dll` 310.8.0。Open 不支持 RTX 20。
- **我们针对 Ampere（RTX 30）的 NR 性能优化：** 在本地 NVIDIA 引擎参考测试中处理更快且输出一致；NVIDIA 快速精度在 RTX 30 上测得 NR 时间减少约 17%；独立的 NR 分辨率，包括 Ultra Performance；Open 的快速精度以及可选的速度/显存控制。我们的 **超快模式** 在 RTX 3070 Ti Laptop、FG X3 的 Wukong 基准测试中，将 NR 时间从 **18.7 ms 降至 9.8 ms（−48%）**，显示帧率从 **73.8 提高到 94.9（+29%）**，每种配置各测一次；单个 NR 阶段，可能出现运动伪影，结果不作保证。见 [Neural Rendering](docs/NEURAL-RENDERING.zh-CN.md)。
- **支持更多 Streamline 版本。** 已验证的正式版 Streamline 2.11.1、2.12.0、2.14.1 和 2.14.16 支持原生动态 MFG 和界面重组；已验证的较旧 DirectX 12 Streamline 2.7.2–2.10.3 支持 Encore 自适应 X2–X4；支持 2026 年 10 月 13 日 NVIDIA 应用测试版下载的文件（Streamline 2.14.16、DLSS-G 310.9.16）。Vulkan 保留其自适应方式；DirectX 11 保留 Smooth Motion X2。见[兼容性](docs/COMPATIBILITY.zh-CN.md#streamline-版本)。
- **DLSS 预设：** 可分别选择超分辨率、光线重构和帧生成的预设，重启后生效，包括帧生成预设 **C (Lite)**，即 NVIDIA 更快的性能模式，需配合已验证的 DLSS-G 310.9.16.0。A/B 保持 Normal，**Fast frame generation** 与预设无关。默认保留游戏自己的选择。
- **帧率控制：** 通过 NVIDIA Reflex 的帧率限制，可直接输入 FPS，提供 **Use game settings** 和 **For this display**（余量随刷新率增大：120 Hz 为 116 FPS，144 Hz 为 138，240 Hz 为 224）；请求关闭 V-Sync 呈现；以及 The Witcher 3 中兼容的光线追踪毛发。
- **RTX 30 上的 Smooth Motion**，可在菜单中设置，接受 NVIDIA 驱动 620.12、617.42、617.14、616.92 和 616.64（617.14 已在游戏中验证）。**Force Smooth Motion**（默认关闭）适用于 NVIDIA 配置文件中关闭了它的 Vulkan 游戏；当 Smooth Motion 实际未运行时，叠加层和菜单会提示。见 [Smooth Motion](docs/SMOOTH-MOTION.zh-CN.md)。
- **一个通用文件：** `rtx-encore.dll` 接受十九种文件名，可以与已经使用常见名称的模组并存。
- **Cyber Engine Tweaks 可选面板**，用于 Cyberpunk 2077（实验性）。
- **`disableInGameMenu`** 不附加菜单，用于拒绝该菜单的游戏（Star Wars Outlaws）；帧生成仍可正常工作。
- **诊断：** 会话日志保存在 `rtx-encore-logs` 文件夹中；自动记录系统、已加载组件的版本、DLSS 质量和分辨率，以及 Streamline/NGX 的常规消息和功能拒绝原因；每个进程有单独的异常日志，并记录结束进程的线程。可选：`logPerformance`（帧时间、GPU 温度、功耗、频率和限制）、`logGraphics`、`logStreamline`、`logNr`、`profileNrGpu`、`profileFrameGpu`（DirectX 12 上每次 DLSS 处理的 GPU 时间），以及用于崩溃报告的 `logDxgiWrappers` 和 `crashDetails`（内存细节和 `.dmp` 快照）。见[设置](docs/SETTINGS.zh-CN.md)。
- 当旧的 dlssg_for_sm86 附加组件仍与 RTX Encore 一起安装时给出警告：两者同时启用时，帧生成可能不显示任何生成帧。

### 变更

- 文件名为 `rtx-encore.*`。设置、快捷键和旧插件会自动沿用：见[从旧名称升级](docs/INSTALLATION.zh-CN.md#从旧名称升级)。
- 设置文件为 `rtx-encore.jsonc`，按部分组织，每个键都有注释；多项设置和菜单项名称更清晰，旧设置文件会连同其值一起沿用。
- 在 RTX 40 显卡上，帧生成的 GPU 时间减少 2% 至 3%。
- 叠加层文字更紧凑，并可调整大小；当游戏的 Streamline 插件比其 Streamline 核心更新时，叠加层会显示两个版本。
- 不再提供旧的 ReShade 附加组件。

### 修正

- **Star Wars Outlaws**（[#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)）：完整版游戏（Ubisoft Connect）在附加菜单时会在启动时关闭；设置 `"disableInGameMenu": true`，或在 Hub 中应用该游戏的社区配置，游戏即可启动并使用最高 X4 的帧生成，两位用户已用测试构建确认。另外修正：视频设置显示帧生成选项时的崩溃，以及在主菜单 alt-tab 后的崩溃。
- **Starfield：** 菜单存在时开启帧生成的崩溃（[#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)）、Streamline 2.12 下的退出问题（[#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)），以及帧生成运行时界面重组的更改被忽略（[#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)）。
- **Forza Horizon 6** 在附加菜单时于启动标志画面关闭（[#11](https://github.com/SilyNoMeta/rtx-encore/issues/11)、[#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)）。
- **Final Fantasy VII Rebirth** 在 RTX 20 和 30 显卡上隐藏帧生成（[#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)）。
- **Where Winds Meet** 使用 `version.dll` 或 `winmm.dll` 时无法启动（0xc0000142）（[#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)）；在该游戏中请使用 `version.dll`。另外修正了 `dinput8.dll` 名称与 Epic 叠加层一起使用时的崩溃。
- **Euro Truck Simulator 2：** 菜单不显示（[#22](https://github.com/SilyNoMeta/rtx-encore/issues/22)）；在该游戏中请以 `winmm.dll` 的名称安装 RTX Encore。
- **Cyberpunk 2077** 与 Streamline 重启时 Reflex 插件相关的启动问题（[#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)、[#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)），以及 **The Witcher 3** 使用动态帧生成时的问题（[#4](https://github.com/SilyNoMeta/rtx-encore/issues/4)）：仍待游戏内确认。
- **The Last of Us** 搭配 OptiScaler 和游戏较旧 Streamline 时的启动崩溃（[#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)）：仍待游戏内确认。
- 在 DirectX 12 中与 **Special K** 一起使用时菜单无法附加（[#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)）。
- 未检测到 DLSS **光线重构** 的活动（[#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)）；Neural Rendering 现在会提示其深度输入不可用。
- Reflex 帧率限制的更改生效延迟，或在不同游戏线程间未恢复。
- 从旧设置文件更新后的首次启动中，部分启动设置要到第二次启动才生效；以及游戏的崩溃报告程序用自己的会话日志覆盖真实日志。

### 已知限制

- RTX 20 支持仍为实验性，尚未在实体 RTX 20 显卡上验证；RTX 20 上的帧生成故障（[#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)）仍在调查中。
- 在 Star Wars Outlaws（完整版）中无法使用游戏内菜单。
- 多阶段 Neural Rendering、Open 引擎、Cyber Engine Tweaks 面板以及 RTX 20 和 RTX 30 上的光线追踪毛发在游戏中测试很少或尚未测试。X5 和 X6 为实验性，需要大量显存。
- 上述多项修正是用测试构建或本地运行确认的，而非用此最终构建；报告者尚未重新测试的情况已在上文注明。自动化检查不能替代对每种游戏、显卡和驱动组合的测试。

Windows Release 构建，包含 Open NR、默认诊断和全部六个分发变体，在维护者的电脑上构建并测试：自动化回归测试在游戏外通过。最终 1.0.0 构建在发布前未在游戏内运行，除上文所列测量外，不宣称性能提升。

### 致谢

- [@ivanmestre](https://github.com/ivanmestre)：Star Wars Outlaws 报告、日志、隔离测试和跟踪测试以及崩溃快照（[#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)）。
- [@yannduwooz83](https://github.com/yannduwooz83)：Star Wars Outlaws 测试以及与之前项目的对比（[#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)），Final Fantasy VII Rebirth 报告和日志（[#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)）。
- [@Avictuu](https://github.com/Avictuu)：多次 Starfield 测试、组件对比、Reflex 和退出日志（[#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)、[#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)）。
- [@mennogreg](https://github.com/mennogreg)：Starfield 报告、日志和多次测试（[#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)），OptiScaler 报告（[#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)），光线重构问题的确认（[#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)）。
- [@hunan-NRT](https://github.com/hunan-NRT)：光线重构报告和 Cyberpunk 测试（[#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)）。
- [@naruto490610-alt](https://github.com/naruto490610-alt)：Cyberpunk 报告和日志（[#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)），Euro Truck Simulator 2 报告（[#22](https://github.com/SilyNoMeta/rtx-encore/issues/22)）。
- [@wisecase2](https://github.com/wisecase2)：The Witcher 3 会话日志和 Cyberpunk 相关证据（[#4](https://github.com/SilyNoMeta/rtx-encore/issues/4)、[#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)）。
- [@avikpal693](https://github.com/avikpal693) 和 [@wer2357666](https://github.com/wer2357666)：Forza Horizon 6 报告和日志（[#11](https://github.com/SilyNoMeta/rtx-encore/issues/11)、[#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)）。
- [@lovebabyx](https://github.com/lovebabyx)：提供 RTX 2070 Super 日志，显示了会话日志缺少的信息（[#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)）。
- [@wd3dmlys](https://github.com/wd3dmlys)：关于 GPU 架构的提示（[#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)）。
- [@ranmo-1](https://github.com/ranmo-1)：Where Winds Meet 报告（[#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)）。
- [@PhantomGamers](https://github.com/PhantomGamers)：随刷新率变化的余量公式（[#23](https://github.com/SilyNoMeta/rtx-encore/issues/23)）和 Special K 报告（[#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)）。
- [@PrincessofCelery](https://github.com/PrincessofCelery)：帮助改进卡顿诊断的报告（[#19](https://github.com/SilyNoMeta/rtx-encore/issues/19)）。
- [@CorwinMaximus](https://github.com/CorwinMaximus)：提出支持 620.12 驱动的请求（[#35](https://github.com/SilyNoMeta/rtx-encore/issues/35)）。
- [@hasnainb237](https://github.com/hasnainb237)：提供 Star Wars Zero Company 日志，显示了残留的旧附加组件（[#1](https://github.com/SilyNoMeta/rtx-encore/issues/1)）。

感谢每一位分享日志并测试各测试版和候选版的朋友。

## 以前的版本

截至 `v1.4.5.3-rtx20-30-40` 的版本以 DLSSG-Transfusion 名称发布，现已不再提供。
