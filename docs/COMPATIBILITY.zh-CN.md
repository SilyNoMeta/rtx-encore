# 兼容性

*[English](COMPATIBILITY.md) · [Français](COMPATIBILITY.fr.md)*

这些说明记录测试和观察结果，并非兼容性认证。游戏更新、其他驱动或其他模组都可能改变结果。

## 显卡

| 显卡 | 帧生成 | Smooth Motion | Neural Rendering |
| :--- | :--- | :--- | :--- |
| RTX 40 | 支持。已在 RTX 4090 上测试 DirectX 12 和 Vulkan。 | 由 NVIDIA 提供 | 支持 |
| RTX 30 | 支持。已在 RTX 3070 Ti Laptop 上测试 DirectX 12。 | 支持（需使用下文列出的驱动） | 支持 |
| RTX 20 | 实验性：尚未在真实 RTX 20 显卡上验证。 | 不支持 | 实验性 |

RTX Encore 面向 RTX 20、30 和 40。不支持 GTX 显卡及其他厂商的显卡。

## 图形 API

| 功能 | DirectX 11 | DirectX 12 | Vulkan |
| :--- | :---: | :---: | :---: |
| X2 至 X6 帧生成 | — | ✅ | ✅（RTX 20 和 RTX 30 上为实验性） |
| UI assist | — | ✅ | — |
| Smooth Motion（RTX 30） | ✅ | ✅ | ✅ |
| Neural Rendering | 🧪 | 🧪 | 🧪 |
| 菜单和叠加层 | ✅ | ✅ | ✅ |

不支持 DirectX 9 和 OpenGL 游戏。

## DLSS 帧生成版本

帧生成支持游戏的以下 `nvngx_dlssg.dll` 版本：

`310.1.0` · `310.2.0` · `310.2.1` · `310.3.0` · `310.4.0` · `310.5.0` · `310.5.2` · `310.5.3` · `310.6.0` ·
`310.7.0` · `310.7.128` · `310.7.129` · `310.8.0` · `310.9.0` · `310.9.1` · `310.9.16`

- 其他版本保持原样，游戏继续使用自己的帧生成。
- **310.9.0 和 310.9.1** 保留现有画质支持和 Fast frame generation。RTX 20 用户应使用通过资格检查的
  310.9.0、310.9.1 或 310.9.16 文件。
- **310.9.16** 是 NVIDIA 应用测试版下载的文件，也是唯一提供帧生成预设 C 的版本。**A/B 使用 Normal；选择 C 后重启游戏即可自动启用 Lite。**
  Fast frame generation 对每个预设都保持独立。Normal 支持防撕裂策略。Lite 保留 NVIDIA
  原生图像策略：Anti-tearing 和 Protection tuning 无效，界面重组不可用，Fast frame generation 仅使用适合 Lite 的优化。
  这些新增功能已通过本地检查，尚待游戏内验证；尚未测得 Lite 的性能增益。
- 游戏的其他 DLSS 文件（超分辨率、光线重建等）可以使用任意版本。

## Streamline 版本

Streamline 的支持范围因功能而异。已离线检查从 1.0.0 到 2.14.1 的可用 Windows x64 SDK，
包括已发布的生产版和开发版文件。这不等于对每一种 SDK、游戏和驱动组合进行了游戏内验证。

| 功能 | 已检查的范围与限制 |
| :--- | :--- |
| Streamline 控制 | 需要 Streamline 2.x；不支持 Streamline 1.x 接口。 |
| X2 以上的固定 MFG | 已在公开 SDK 2.7.2、2.7.30、2.7.32、2.8.0、2.9.0、2.10.0、2.10.3、2.11.1、2.12.0 和 2.14.1 的文件，以及 NVIDIA 应用测试版下载的 2.14.16 文件中识别。更早的 SDK 未获这些倍率的资格确认。还需要受支持的 DLSS 帧生成文件。 |
| 原生动态 MFG 扩展 | 官方 Streamline 2.11.1、2.12.0、2.14.1、2.14.16 生产版文件通过离线资格检查。帧节奏因版本而异，仍需各游戏验证。其他构建保留自身原生能力。 |
| Encore 自适应 MFG（DX12） | 官方生产版与开发版：2.7.2, 2.7.30, 2.7.32, 2.8.0, 2.9.0, 2.10.0, 2.10.3。仅 X2–X4，即使开启实验性 X5/X6 也不例外。需要兼容的游戏集成，部分旧集成仍不支持。已离线检查，尚待游戏内确认。 |
| 界面重组覆盖 | 2.11.1、2.12.0、2.14.1、2.14.16 生产版文件通过本地资格检查。明确的开/关请求保持优先；未指定时保留现有选择。仍需游戏提供可用的界面数据，2.14.16 尚待游戏内验证。其他构建保留自身处理方式。此功能与 UI assist 独立。 |
| Reflex 帧率上限 | DLL 版本 2.1.1–2.14.1，以及 2.14.16；见[菜单和叠加层](MENU-AND-OVERLAY.zh-CN.md#建议的-reflex-帧率上限)。 |

本次检查没有找到 2.13 或 2.14.0 的公开 SDK 标签。仅凭中间版本号不能确认所有功能均受支持。
请保留游戏原有的整套 Streamline 文件；这些说明不建议单独替换其中的文件。

**请在菜单中逐项查看功能可用状态。** Streamline 2.14.16 和 DLSS-G 310.9.16 新增支持尚待游戏内验证。旧版可用功能较少；未经检查的新版本不会自动受支持。请保持 Streamline 文件版本匹配，并确认与游戏兼容。SR/RR 预设仍取决于各自 DLL 和驱动；Neural Rendering 仍明确要求 310.8.0。

**DirectX 11：** 不提供 DLSS MFG 或动态模式。独立的 Smooth Motion 路径仅支持 X2，无法切换 X3/X4，仍需满足显卡与驱动要求。

## Smooth Motion 驱动

RTX 30 上的 Smooth Motion 仅接受 NVIDIA 驱动 **617.42**、**617.14**、**616.92** 和 **616.64**。
只有 **617.14** 已在游戏中验证。见 [Smooth Motion](SMOOTH-MOTION.zh-CN.md)。

## Neural Rendering 文件

Neural Rendering 需要 NVIDIA 的 `nvngx_dlssnr.dll` **310.8.0**，由你自行提供，或由 [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub) 获取。
见 [Neural Rendering](NEURAL-RENDERING.zh-CN.md)。

## 游戏说明

| 游戏 | API | 说明 |
| :--- | :---: | :--- |
| Cyberpunk 2077 | DX12 | 在 RTX 3070 Ti Laptop 上测试了帧生成。可作为 `version.dll` 或 `.asi` 插件运行；见[安装](INSTALLATION.zh-CN.md#cyberpunk-2077)。 |
| Black Myth: Wukong（黑神话：悟空） | DX12 | 在 RTX 4090 上测试了帧生成。用户确认 NR 运行。 |
| No Man's Sky | Vulkan | 在 RTX 4090 上测试了帧生成。**请关闭 Automatic UI recomposition**：它在此游戏的镜头移动中造成双重轮廓。用户确认了 1080p 和 1440p 下 NR 与帧生成 X2 一起运行。 |
| DOOM: The Dark Ages | Vulkan | 在 RTX 4090 上测试了帧生成。无论模组设置如何，游戏都保留自己的界面处理选择。 |
| Starfield | DX12 | 开启 UI assist 时可运行；根据用户报告修复了读取存档时的崩溃。 |
| The Witcher 3 | DX12 | 游戏构建 25575366 和 25646871 支持兼容的光线追踪毛发；见[菜单和叠加层](MENU-AND-OVERLAY.zh-CN.md#其他控制)。 |
| Manor Lords | DX12 | 在 RTX 30、驱动 617.14 上观察到 Smooth Motion 运行。 |
| Shadows of Doubt | DX11 | 在 RTX 30、驱动 617.14 上观察到 Smooth Motion 运行。用户确认 NR 运行。 |
| Enshrouded | Vulkan | 在 RTX 30、驱动 617.14 上观察到 Smooth Motion 运行。用户确认 NR 运行。 |
| Bodycam、Palworld、Portal with RTX、Star Wars Zero Company | — | 用户确认 NR 运行。 |

欢迎通过 [issue](https://github.com/SilyNoMeta/rtx-encore/issues) 报告其他游戏中的观察结果：
游戏、图形 API、显卡、驱动，以及哪些功能可用或不可用。

## 其他工具

| 工具 | 说明 |
| :--- | :--- |
| ReShade、Special K | 保留它们自己的 `dxgi.dll`，给 RTX Encore 选择其他名称。菜单不需要 ReShade。 |
| DXVK | 不要把 RTX Encore 命名为 `dxgi.dll`：DXVK 提供该文件。菜单会绘制在它的 Vulkan 输出中。 |
| OptiScaler | 有用于特定配置的专用选项：见[帧生成](FRAME-GENERATION.zh-CN.md#兼容性选项)。 |
| Cyber Engine Tweaks、RED4ext | 保留它们自己的文件名。Cyber Engine Tweaks 有可选的实验性面板。 |
| NVIDIA Profile Inspector | 在 **Game decides** 模式中遵循它设置的倍率。 |

## 反作弊和在线游戏

RTX Encore 在游戏进程内加载。带有反作弊保护的游戏可能拒绝启动或标记账号。请勿在这些游戏中使用。
