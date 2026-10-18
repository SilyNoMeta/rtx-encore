# 更新日志

*[English](CHANGELOG.md) · [Français](CHANGELOG.fr.md)*

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
