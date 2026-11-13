<div align="center">

<h1>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/wordmark-dark-theme.png">
  <img src="assets/wordmark-light-theme.png" alt="RTX Encore" width="440">
</picture>
</h1>

**为 GeForce RTX 20、RTX 30 和 RTX 40 带来最高 X6 的帧生成、\
Smooth Motion 和 Neural Rendering——现在也适用于没有 DLSS 的游戏。**

自带游戏内菜单，RTX Encore Hub 负责安装模组及所需运行库。

[![最新版本](https://img.shields.io/github/v/release/SilyNoMeta/rtx-encore?include_prereleases&style=for-the-badge&label=Latest&color=76b900)](https://github.com/SilyNoMeta/rtx-encore/releases)
[![下载量](https://img.shields.io/github/downloads/SilyNoMeta/rtx-encore/total?style=for-the-badge&color=2f81f7)](https://github.com/SilyNoMeta/rtx-encore/releases)
[![Sponsor](https://img.shields.io/badge/Sponsor-%E2%9D%A4-db61a2?style=for-the-badge&logo=githubsponsors&logoColor=white)](https://github.com/sponsors/SilyNoMeta)
[![Ko-fi](https://img.shields.io/badge/Ko--fi-Support-ff5e5b?style=for-the-badge&logo=kofi&logoColor=white)](https://ko-fi.com/silynometa)
[![加入 Discord](https://img.shields.io/badge/%E5%8A%A0%E5%85%A5_Discord-454FC4?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/Zwmq4Xc2Br)

[![显卡](https://img.shields.io/badge/GPUs-RTX%2020%20%7C%2030%20%7C%2040-76b900?style=flat-square&logo=nvidia&logoColor=white)](docs/COMPATIBILITY.zh-CN.md)
[![图形 API](https://img.shields.io/badge/APIs-DirectX%2011%20%7C%2012%20%7C%20Vulkan-0078d4?style=flat-square)](docs/COMPATIBILITY.zh-CN.md)

**[下载](https://github.com/SilyNoMeta/rtx-encore/releases)** ·
**[Nexus Mods](https://www.nexusmods.com/site/mods/2547)** ·
**[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)** ·
**[安装](docs/INSTALLATION.zh-CN.md)** ·
**[English](README.md)** ·
**[Français](README.fr.md)**

<sub>免费，利用业余时间开发。如果它对你有帮助，欢迎通过 <a href="https://github.com/sponsors/SilyNoMeta">GitHub Sponsors</a> 或 <a href="https://ko-fi.com/silynometa">Ko-fi</a> 支持本项目，或在 <a href="https://www.nexusmods.com/site/mods/2547">Nexus Mods</a> 上点赞（Endorse） ❤</sub>

<p>❤️ 感谢 <a href="https://github.com/mennogreg">@mennogreg</a>、<a href="https://github.com/Scarhead666">@Scarhead666</a> 和 <a href="https://github.com/MrMicko">@MrMicko</a> 对 RTX Encore 开发的支持。</p>

</div>

## RTX Encore 2.4.0

RTX Encore 2.4.0 新增逐遍 NR 处理位置、支持 HDR 的 DirectX/Vulkan 空间抗锯齿，以及最低 33% 的自定义 DLSS 比例。Hub 2.4.0 可选择模组版本并回退。

[2.4.0](CHANGELOG.zh-CN.md#240)


**RTX Encore 的 Discord 服务器现已开放。** 欢迎分享 2.0.0 的测试结果、交流设置，或就模组和 Hub 寻求帮助。[加入 Discord](https://discord.gg/Zwmq4Xc2Br)。

> [!IMPORTANT]
> RTX Encore 是一个独立的实验性模组，与 NVIDIA 没有关联，也未获得 NVIDIA 的认可。
> 它在游戏进程内加载：请勿在联机游戏或受反作弊保护的游戏中使用，否则可能被拦截，或导致账号被标记。
> 替换任何文件之前请先备份。

**[最新版本：2.2.0](https://github.com/SilyNoMeta/rtx-encore/releases/tag/v2.2.0)** · [版本说明](CHANGELOG.zh-CN.md) · [Hub 2.2.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.2.0)

**2.2.0 新增：**无 DLSS 游戏可选 NVIDIA Image Scaling（NIS），可在 NIS 前后使用 FXAA/SMAA，自定义叠加层样式与指标，RTX 20 实验性支持 XeFG Tensor Cores，并更新 RTX 30/40 的 XeSS Tensor Cores 路径。[查看全部更改与限制](CHANGELOG.zh-CN.md#220)。

**2.1.0 的主要新增内容仍在：**稳定游戏帧节奏让 XeFG X3–X6 更流畅；添加的帧生成可在 DirectX 11 和 Vulkan 中利用 DLSS 游戏自身的深度与运动数据；XeFG Tensor Cores 也扩展到这两种 API。该版本还修复了博德之门 3、最终幻想 VII 重生、极限竞速：地平线 6、天国：拯救、Islands of Insight 和黑旗 Resynced 的问题。[2.1.0 详情](CHANGELOG.zh-CN.md#210)。


## 2.0.0：为没有 DLSS 的游戏带来 DLSS、XeSS、Neural Rendering 和帧生成

很多游戏完全没有 DLSS。RTX Encore 2.0.0 基于游戏显示的画面，在 **DirectX 11、DirectX 12 和 Vulkan** 中为它们带来 **NVIDIA DLSS Super Resolution 或 DLAA、Intel XeSS、Neural Rendering 和帧生成**，无需 ReShade 或任何其他工具。游戏本身不需要任何东西：不需要 DLSS，不需要运动向量，也不需要自己的帧生成。

| | |
| :--- | :--- |
| 🔍 **超分辨率** | 游戏分辨率下的 DLAA，或把较低的游戏分辨率放大到屏幕分辨率，可用 **DLSS** 或 **XeSS**（DirectX 12 中 XeSS 可运行在 NVIDIA Tensor Cores 上）。 |
| ✨ **Neural Rendering** | 对显示的画面运行 NVIDIA 的 NR，RTX Encore 的所有 NR 控件均可使用。 |
| 🎞️ **帧生成** | **DLSS 帧生成** X2 至 X6（DirectX 11、DirectX 12 和 Vulkan）；**Intel XeFG** 最高 X6（DirectX 12 可用 Tensor Cores，Vulkan 支持 HDR10，以及 DirectX 11）。倍率可在游玩中更改。 |
| 🧭 **运动估计** | RTX Encore 自己的估计，在显卡上运行，或 vort_Motion 的着色器方法，或 NVIDIA Optical Flow。 |

按 **Insert**，在超分辨率和帧生成中选择**引擎**：根据功能选择**无**、**DLSS / XeSS** 或 **DLSS-FG / XeFG**。原生集成保留**游戏**选项。[设置、要求与限制](docs/GAMES-WITHOUT-DLSS.zh-CN.md)。

| 🎞️ **[帧生成](docs/FRAME-GENERATION.zh-CN.md)** | 🌀 **[Smooth Motion](docs/SMOOTH-MOTION.zh-CN.md)** | ✨ **[Neural Rendering](docs/NEURAL-RENDERING.zh-CN.md)** | 🎮 **[没有 DLSS 的游戏](docs/GAMES-WITHOUT-DLSS.zh-CN.md)** |
| :--- | :--- | :--- | :--- |
| 在兼容的游戏中选择帧倍率。 | 为没有原生帧生成的游戏提供插帧。 | 使用独立 NR 控件调整画面。 | 在游戏没有的地方提供 DLSS、XeSS、NR 和帧生成。 |

## 开始游戏

使用 **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)** 安装，选择游戏，在游戏内按 **Insert**。也可按[手动安装步骤](#快速开始)操作。

<details>
<summary>实验引擎与支持</summary>

XeSS、XeFG 和原生 XeFG → DLSS FG 适配器仍属实验功能，需要兼容的输入和运行库。在 Mirage 中，在 Encore 选择 DLSS-FG 并重启，然后在游戏中开启 Intel 帧生成。FSR 保持原生。参阅[引擎指南](docs/INTEL-ENGINES.zh-CN.md)。遇到问题时，请从 Hub 健康页面生成诊断，检查后附在 [issue](https://github.com/SilyNoMeta/rtx-encore/issues/new/choose) 中。

</details>

## 功能

| | 功能 | 简介 |
| :---: | :--- | :--- |
| 🧪 | **[Intel 引擎（实验性）](docs/INTEL-ENGINES.zh-CN.md)** | 兼容游戏输入和运行库允许时，使用 XeSS 重建或搭配 XeLL 的 XeFG 帧生成。默认关闭。 |
| 🔆 | **[实时锐化](docs/SUPER-RESOLUTION.zh-CN.md)** | Off / Game / Custom，在重建及 NR 之后执行。Custom 为 0–100%，默认 30%；画质与 Vulkan 运行仍待验证。 |
| 🎞️ | **[X2 至 X6 帧生成](docs/FRAME-GENERATION.zh-CN.md)** | 在自带 DLSS 帧生成的游戏中，为 RTX 20、30 和 40 提供 DLSS 多帧生成。可选固定倍率、带目标帧率的动态模式，或交给游戏决定。 |
| 🛡️ | **更干净的生成帧** | 防止细小几何体（栅栏、电线、植被）撕裂，减少移动阴影中的瑕疵；界面和文字保持清晰。 |
| ⚡ | **更轻量的帧生成** | 一个性能选项，在不改变画面的前提下降低每个生成帧的 GPU 开销。1.6.0 中进一步提升：在 RTX 4090 上，1080p X2 下每个生成帧的 GPU 时间比关闭该选项少 34%，1080p X6 下少 16%。 |
| 🌀 | **[RTX 20 / 30 上的 Smooth Motion](docs/SMOOTH-MOTION.zh-CN.md)** | NVIDIA 驱动的插帧功能，也适用于没有 DLSS 的游戏：RTX 30 支持 DirectX 11、DirectX 12 和 Vulkan；**RTX 20 为实验性支持**，限 DirectX 12、621.01 驱动，可选 V1 / V2。默认关闭。 |
| ✨ | **[Neural Rendering](docs/NEURAL-RENDERING.zh-CN.md)** | 在 RTX 20、30 和 40 上运行 NR，并提供 RTX Encore 自行开发的 Ampere 性能选项：优化处理、Fast 精度、独立的 NR 分辨率。支持一至四遍处理、每遍独立风格以及 HDR。实验性功能，默认关闭。 |
| 🎮 | **[没有 DLSS 的游戏](docs/GAMES-WITHOUT-DLSS.zh-CN.md)** | 在没有 DLSS 的 DirectX 11、DirectX 12 和 Vulkan 游戏中，基于游戏显示的画面运行 DLSS 或 XeSS 超分辨率（DLAA 或放大到屏幕分辨率）、Neural Rendering 和帧生成（DLSS 帧生成或 Intel XeFG，X2 至 X6），使用 RTX Encore 自己的运动估计。2.0.0 新增。实验性功能，默认关闭。 |
| ⚡ | **[使用 NVIDIA Tensor Cores 的 XeSS 与 XeFG](docs/INTEL-ENGINES.zh-CN.md)** | XeSS 重建与 XeFG 帧生成分别提供实验性选项。支持 DirectX 12；自 2.1.0 起还有额外的 DirectX 11/Vulkan 路径，2.2.0 起 XeFG 也准入 RTX 20。默认关闭；实际运行时显示 **Tensor Cores** 标记。可用性与游戏内验证范围因路径和 GPU 而异。 |
| 🎛️ | **[内置菜单和叠加层](docs/MENU-AND-OVERLAY.zh-CN.md)** | 在游戏中按 **Insert**：所有设置、每项功能的状态，以及显示帧率、倍率、帧时间稳定性、GPU 和显存的叠加层。支持 DirectX 11、DirectX 12 和 Vulkan。自 2.0.0 起支持十一种语言。 |
| 🚀 | **[RTX 30 上的 Fast super resolution](docs/SUPER-RESOLUTION.zh-CN.md)** | 为 RTX 30 在预设为 M 或 L 的 DirectX 12 游戏中提供更快的 DLSS 超分辨率：在测量的系统上每个放大帧约减少 16% 至 20% 的 GPU 时间，画面与原版非常接近。默认关闭。 |
| 🔍 | **DLSS 渲染分辨率** | 在已使用 DLSS 的游戏中选择 DLAA、Ultra（75%）、质量、平衡、性能、超级性能或自定义比例。 |
| 🧩 | **一个通用文件** | `rtx-encore.dll` 可改成游戏会加载的文件名（`version.dll`、`dinput8.dll`、`winmm.dll`、`dxgi.dll` 以及另外十五个），也可作为 `.asi` 插件运行。 |

## 面向 Ampere 优化的 Neural Rendering

RTX Encore 提供优化的 NVIDIA NR 处理、可选 Fast 精度和独立于 DLSS 的 NR 分辨率。仍支持一至四遍处理及逐遍画面设置。[查看设置和测量结果](docs/NEURAL-RENDERING.zh-CN.md)。

## 支持的硬件

| 显卡 | 帧生成 | Smooth Motion | Neural Rendering |
| :--- | :---: | :---: | :---: |
| RTX 40 | ✅ | 由 NVIDIA 提供 | ✅ |
| RTX 30 | ✅ | ✅ | ✅ |
| RTX 20 | 🧪 实验性 | 🧪 实验性（DirectX 12、621.01 驱动） | 🧪 实验性 |

RTX Encore 面向 RTX 20、30 和 40；RTX 50 显卡的帧生成和 Smooth Motion 由 NVIDIA 提供。
详细信息、图形 API、驱动和游戏说明见 **[兼容性](docs/COMPATIBILITY.zh-CN.md)**。

## 使用 RTX Encore Hub 安装和更新

**[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)** 是 RTX Encore 的 Windows 配套应用：安装模组，并让模组及其配套的 NVIDIA 文件保持最新的最简单方式。

| | 使用 Hub |
| :---: | :--- |
| 🎮 | **所有游戏集中一处。** 自动找到 Steam、Epic 和 GOG 的游戏，每个游戏的页面都会说明 RTX Encore 在你的显卡和驱动上能做什么。 |
| 📦 | **自动采用合适的 NVIDIA 文件。** 此版本 RTX Encore 支持的最新 DLSS 和 Streamline 版本（Streamline 插件始终整套更换），以及下载并校验过的 NVIDIA Neural Rendering 文件。许多被报告的问题都源于过旧的 DLSS 或 Streamline 文件。 |
| 🔄 | **一键更新。** 新版 RTX Encore 会连同其说明一起提示，**全部更新**会更新所有游戏，Hub 自身也会自动更新。 |
| ↩️ | **可以回退。** 每个被替换的文件都会先备份，**还原游戏**会把游戏恢复原状。 |
| 🎚️ | **带说明的设置。** 用表单编辑设置文件，对你的显卡无效的设置会变灰。 |
| 🩺 | **首次检查。** **启动并验证**会观察游戏的第一分钟，并说明模组是否已加载。 |
| 👥 | **社区配置。** 其他玩家分享的各游戏设置，按你的机器排序，可导入并可回退，也可以分享你自己的设置。自 Hub 1.0.0-rc.2 起提供，仍是实验性的。 |

请从**[发布页面](https://github.com/SilyNoMeta/rtx-encore-hub/releases)**下载稳定版 Hub，可选择安装程序或便携 ZIP。下面的手动安装方式仍然可用。

## 快速开始

最简单的方式是 **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)**：安装它，打开游戏页面并点击**安装**。手动安装：

1. 下载 **[最新版本](https://github.com/SilyNoMeta/rtx-encore/releases)** 并解压。
2. 把 `rtx-encore.dll` 复制到游戏可执行文件旁边，并重命名为 `version.dll`。
3. 启动游戏。首次启动时菜单会自动打开一次；按 **Insert** 可再次打开。
4. 在游戏设置中开启 DLSS 帧生成。如果之前已经开启，请先关闭再重新开启。在没有 DLSS 的游戏中，改为在菜单中开启你需要的功能：参见
   **[没有 DLSS 的游戏](docs/GAMES-WITHOUT-DLSS.zh-CN.md)**。

其他文件名、`.asi` 插件、更新和卸载见 **[安装](docs/INSTALLATION.zh-CN.md)**。

> [!TIP]
> 建议先使用默认设置：由游戏或 NVIDIA Profile Inspector 决定倍率。8 GB 显存的显卡建议从 X2 或 X3 开始
> （或在高/中纹理质量下使用 X4）。

### 手动安装所需的运行库

RTX Encore 在所有游戏中都基于以下精确的运行库开发和测试；Hub 会自动放置它们。手动安装时，请获取以下文件，然后重新启动游戏：

| 用途 | 文件 | 下载 | 放置位置 |
|---|---|---|---|
| DLSS 帧生成 310.9.16 与 Streamline 2.14.16 | `nvngx_dlssg.dll` 和 `sl.*.dll` 文件，取自 `bin/x64`（不是 `development`） | [NVIDIA Streamline SDK 2.14.16](https://github.com/NVIDIA-RTX/Streamline/releases/download/v2.14.16/streamline-sdk-v2.14.16.zip)（可直接使用整个 `bin/x64` 文件夹） | 游戏可执行文件旁边，或替换游戏自带副本所在位置的文件 |
| DLSS / DLAA 与光线重建 310.9.16 | `nvngx_dlss.dll`、`nvngx_dlssd.dll` | [NVIDIA DLSS SDK 310.9.16](https://github.com/NVIDIA/DLSS/tree/v310.9.16/lib/Windows_x86_64/rel) | 游戏可执行文件旁边，或替换游戏自带副本所在位置的文件 |
| Neural Rendering 310.9.16 | `nvngx_dlssnr.dll` | [nvngx_dlssnr-310.9.16.zip](https://encore-dl.nometa.fr/nvngx_dlssnr-310.9.16.zip) | RTX Encore 的 DLL 旁边 |
| XeSS-SR 2.0.2.68、XeFG 1.3.1.78、XeLL 1.3.2.10 | `libxess.dll`、`libxess_fg.dll`、`libxell.dll`，取自 `bin` | [Intel XeSS SDK 3.0.2](https://github.com/intel/xess/releases/download/v3.0.2/XeSS_SDK_3.0.2.zip) | RTX Encore 的 DLL 旁边，或其旁边的 `rtx-encore-runtimes\intel` 中 |

在没有 DLSS 或没有帧生成的游戏中，只使用这些精确文件，菜单中的**引擎**设置会列出已找到的内容。在 Unreal Engine 游戏中，游戏自带的副本通常位于 `Plugins\...\ThirdParty\Win64` 文件夹中，而不是可执行文件旁边。

## 操作

| 快捷键 | 作用 |
| :--- | :--- |
| `Insert` | 打开或关闭菜单 |
| `Ctrl + Alt + 2…6`（也支持数字小键盘） | 固定倍率 X2 至 X6 |
| `Ctrl + Alt + PageUp / PageDown` | 提高或降低倍率 |
| `Ctrl + Alt + D` | 在固定模式和动态模式之间切换 |
| `Ctrl + Alt + ↑ / ↓` 或 `Ctrl + Alt + Plus / Minus`（也支持数字小键盘的 `+` / `−`） | 将动态模式的目标帧率提高或降低 5 FPS（同时按住 `Shift`：1 FPS） |
| `Ctrl + Alt + G` | 把选择权交还给游戏 |
| `Ctrl + Alt + O` | 显示或隐藏叠加层 |
| `Ctrl + Alt + P` | 把叠加层移到下一个角落 |
| `Ctrl + Alt + N` | 开启或关闭 Neural Rendering |
| `Ctrl + Alt + F9` | 记录一组 SR 捕获（DirectX 12；须先在菜单中启用 SR 捕获） |

以上为默认快捷键，均可在菜单中修改。只有游戏窗口处于焦点时它们才会生效。

## 文档

| 指南 | English | Français | 中文 |
| :--- | :---: | :---: | :---: |
| 安装、更新、卸载 | [Open](docs/INSTALLATION.md) | [Ouvrir](docs/INSTALLATION.fr.md) | [打开](docs/INSTALLATION.zh-CN.md) |
| 帧生成 | [Open](docs/FRAME-GENERATION.md) | [Ouvrir](docs/FRAME-GENERATION.fr.md) | [打开](docs/FRAME-GENERATION.zh-CN.md) |
| 超分辨率 | [Open](docs/SUPER-RESOLUTION.md) | [Ouvrir](docs/SUPER-RESOLUTION.fr.md) | [打开](docs/SUPER-RESOLUTION.zh-CN.md) |
| RTX 20 / 30 上的 Smooth Motion | [Open](docs/SMOOTH-MOTION.md) | [Ouvrir](docs/SMOOTH-MOTION.fr.md) | [打开](docs/SMOOTH-MOTION.zh-CN.md) |
| Neural Rendering | [Open](docs/NEURAL-RENDERING.md) | [Ouvrir](docs/NEURAL-RENDERING.fr.md) | [打开](docs/NEURAL-RENDERING.zh-CN.md) |
| 没有 DLSS 的游戏 | [Open](docs/GAMES-WITHOUT-DLSS.md) | [Ouvrir](docs/GAMES-WITHOUT-DLSS.fr.md) | [打开](docs/GAMES-WITHOUT-DLSS.zh-CN.md) |
| Intel XeSS 与 XeFG | [Open](docs/INTEL-ENGINES.md) | [Ouvrir](docs/INTEL-ENGINES.fr.md) | [打开](docs/INTEL-ENGINES.zh-CN.md) |
| 菜单和叠加层 | [Open](docs/MENU-AND-OVERLAY.md) | [Ouvrir](docs/MENU-AND-OVERLAY.fr.md) | [打开](docs/MENU-AND-OVERLAY.zh-CN.md) |
| 设置文件 | [Open](docs/SETTINGS.md) | [Ouvrir](docs/SETTINGS.fr.md) | [打开](docs/SETTINGS.zh-CN.md) |
| 兼容性 | [Open](docs/COMPATIBILITY.md) | [Ouvrir](docs/COMPATIBILITY.fr.md) | [打开](docs/COMPATIBILITY.zh-CN.md) |
| 故障排除 | [Open](docs/TROUBLESHOOTING.md) | [Ouvrir](docs/TROUBLESHOOTING.fr.md) | [打开](docs/TROUBLESHOOTING.zh-CN.md) |
| 更新日志 | [Open](CHANGELOG.md) | [Ouvrir](CHANGELOG.fr.md) | [打开](CHANGELOG.zh-CN.md) |

## 发布版本

RTX Encore 以可直接使用的发布包形式在本页面提供；其源代码不公开。GitHub 会显示每个可下载压缩包的 SHA-256：
安装前请与发布说明中的值进行比对。

版本号从 1.0.0 开始。此前以 DLSSG-Transfusion 名称发布的版本（直到 `v1.4.5.3-rtx20-30-40`）已不再提供；
[从这些版本升级](docs/INSTALLATION.zh-CN.md#从旧名称升级)会保留你的设置。

## 报告问题

请提交 [issue](https://github.com/SilyNoMeta/rtx-encore/issues)，并注明游戏及其图形 API、显卡和驱动版本，
以及菜单为你复制的报告（**System** 选项卡，**Copy** 按钮）。
[故障排除](docs/TROUBLESHOOTING.zh-CN.md)列出了首先应检查的事项。

## 致谢

RTX Encore 最初是 TonyJoaca 的 [DLSSG-Transfusion](https://github.com/TonyJoaca/DLSSG-Transfusion) 的延续，
此后已发展为一个独立的项目。它同样受益于以下项目的工作：

- [dashdogy/RTX40MFG-Unlock](https://github.com/dashdogy/RTX40MFG-Unlock)：帧生成研究、菜单渲染和输入处理；
- [mavismmg/MFGAdaUnlock-RenoDx](https://github.com/mavismmg/MFGAdaUnlock-RenoDx)：帧生成画质研究；
- [sdli1995/dlssg_for_sm86](https://github.com/sdli1995/dlssg_for_sm86)：RTX 20 和 RTX 30 上的帧生成研究；
- [Intel XeSS](https://github.com/intel/xess)：XeSS、XeFG 和 XeLL SDK 接口；
- [maximedgoulet/FoveatedDLSS5](https://github.com/maximedgoulet/FoveatedDLSS5)：中央优先 Neural Rendering 的构想；
- [janblade/OptiScaler F5](https://github.com/janblade/OptiScaler-F5-DLSSNR-Multipass) 为 NR 外观预设和自动曝光提供了灵感；
- [Skynizz/optiscaler-dlss5](https://github.com/Skynizz/optiscaler-dlss5) 为多次处理 NR 配置方案提供了灵感；
- [jlrouzies-fr/DLSS5-Feeder](https://github.com/jlrouzies-fr/DLSS5-Feeder)：基于显示画面的 Neural Rendering 及其运动校验；
- [vortigern11/vort_Shaders](https://github.com/vortigern11/vort_Shaders)（vort_Motion）：没有 DLSS 的游戏的着色器运动估计；
- [crosire/reshade](https://github.com/crosire/reshade)（Generic Depth 插件）：找到游戏绘制所用深度缓冲的方法；
- [Dear ImGui](https://github.com/ocornut/imgui)（Omar Cornut）：游戏内菜单和叠加层所用的界面库；
- [SMAA](https://github.com/iryoku/smaa)：Jorge Jimenez、Jose I. Echevarria、Belen Masia、Fernando Navarro 和 Diego Gutierrez；[FXAA 3.11](https://github.com/GameTechDev/CMAA2)：NVIDIA、Timothy Lottes；
- [Noto Sans SC](https://github.com/google/fonts/tree/main/ofl/notosanssc)：菜单字体；
- 所有测试版本并报告问题的人：有好几项修复正是因为这些报告才得以完成。

发布包中包含的组件及其许可证列在 [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md) 中。

## 免责声明

RTX Encore 按“原样”提供，不附带任何担保：使用风险由你自行承担。画质、稳定性和性能会因游戏、游戏版本、
驱动和硬件而异。高倍率会增加延迟和显存占用。

发布包中包含的组件保留其各自的许可证：见 [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md)，分发发布包的
任何副本时都必须附带该文件。NVIDIA、GeForce、RTX 和 DLSS 是 NVIDIA Corporation 的商标。NVIDIA 软件和游戏仍受其
各自条款的约束；发布包中不包含其中任何一项。
