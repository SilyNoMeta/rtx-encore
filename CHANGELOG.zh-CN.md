# 更新日志

*[English](CHANGELOG.md) · [Français](CHANGELOG.fr.md)*

## 1.0.0-rc.2 — 预发布版本

- 修正 Star Wars Outlaws 在主菜单中 alt-tab 后关闭的问题：当时自动界面重组正在捕获场景。已在 RTX 3070 Ti Laptop 上于游戏内确认。
- 当 Streamline 插件版本与游戏自带的 Streamline 核心不同时，叠加层会显示插件版本：更新插件时核心保持不变，因此叠加层以前只显示游戏的原始版本。见[菜单与叠加层](docs/MENU-AND-OVERLAY.zh-CN.md)。
- 会话日志现在自动包含 Streamline/NGX 的常规消息及其报告的功能拒绝原因，无需开启诊断选项。消息数量有限；额外的详细日志仍为可选项。感谢 [@lovebabyx](https://github.com/lovebabyx) 在 [#30](https://github.com/SilyNoMeta/rtx-encore/issues/30) 中提供日志，帮助发现这一信息缺失。这项改动改善诊断；报告的 RTX 20 帧生成故障仍在调查中。

Windows Release 构建，包含 Open NR、默认诊断和全部六个分发变体，在维护者的电脑上构建并测试：自动化回归测试在游戏外通过。Star Wars Outlaws 的修正已用包含同一改动的测试构建在游戏内验证；最终 rc.2 构建在发布前未在游戏内运行，本次不宣称性能提升。

### 致谢

- [@lovebabyx](https://github.com/lovebabyx)：提供 RTX 2070 Super 日志，显示了会话日志缺少的信息（[#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)）。

感谢每一位分享日志并花时间测试构建的朋友。

## 1.0.0-rc.1 — 预发布版本

- **支持最新的 NVIDIA 文件。** 完成对 2026 年 10 月 13 日 NVIDIA 应用测试版所下载文件的资格检查：Streamline 2.14.16（动态 MFG、X2 以上固定 MFG、Reflex 帧率限制、界面重组）和 DLSS-G 310.9.16（预设、画质选项和 Fast frame generation）。新增帧生成预设 **C (Lite)**，即 NVIDIA 更快的性能模式，仅适用于已验证的 DLSS-G 310.9.16.0；A/B 仍使用 Normal，**Fast frame generation** 保持独立。这些新增内容属于实验性功能，测试尚少；见[兼容性](docs/COMPATIBILITY.zh-CN.md#streamline-版本)。
- **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)**，全新的 Windows 配套应用，现以候选发布版提供。它会在你的游戏中安装 RTX Encore，同时带来最新的已验证 DLSS 和 Streamline 文件以及 NVIDIA 的 Neural Rendering 文件，一键更新所有游戏，并且随时可以把游戏恢复原状。手动安装方式与以前完全一样；见[安装](docs/INSTALLATION.zh-CN.md#最简单的方式rtx-encore-hub)。
- 新增 **Force Smooth Motion**（`"smoothMotionForce"`，默认关闭），用于 NVIDIA 配置文件未启用 Smooth Motion 的 Vulkan 游戏。强制启用可能带来意外结果（光标重影、画面伪影、崩溃）。覆盖层和菜单现在会提示 Smooth Motion 实际未运行，或所选图形 API 与游戏不符，而不再显示翻倍的帧率（见于 RPCS3）。
- 修正 Forza Horizon 6 在菜单附加时于启动 logo 阶段退出的问题（[#11](https://github.com/SilyNoMeta/rtx-encore/issues/11)、[#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)）。本地多次带菜单启动均保持运行；报告者的安装环境未复测。
- 修正 Final Fantasy VII Rebirth 在 RTX 20/30 显卡上隐藏帧生成的问题（[#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)、[#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)）。已在 RTX 3070 Ti 上使用游戏自带的 Streamline 2.8.0 于游戏内确认；未在该游戏中测试 Streamline 2.14.1 更新。
- 修正 Where Winds Meet 使用 `version.dll` 或 `winmm.dll` 时无法启动（0xc0000142）的问题（[#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)），以及 `dinput8.dll` 名称搭配 Epic 覆盖层时的崩溃。该游戏请使用 `version.dll`：以 `dinput8.dll` 加载时，RTX Encore 到达过晚，游戏已放弃 DLSS 帧生成。报告者的安装环境未复测。
- 修正搭配 Special K 时菜单在 Direct3D 12 下拒绝附加的问题（[#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)）。游戏内确认待定。
- 修正 **For this display**：建议的上限现在保留随刷新率增大的余量（120 Hz 为 116 FPS，144 Hz 为 138，240 Hz 为 224）（[#23](https://github.com/SilyNoMeta/rtx-encore/issues/23)）。
- 修正从旧版设置文件更新后的首次启动：Smooth Motion 及部分其他启动设置要到第二次启动才生效（见于 Shadows of Doubt）。
- RTX Encore 在游戏的崩溃报告程序中保持不活动；此前它会写入自己的会话日志，把真正的游戏日志挤出保留范围（见于 Shadows of Doubt）。
- 异常日志现在会记录结束进程的线程，以区分游戏自身退出与外部终止。

Windows Release 构建，包含 Open NR、默认诊断和全部六个分发变体，在维护者的电脑上构建并测试：自动化回归测试在游戏外通过。Forza Horizon 6、Final Fantasy VII Rebirth 和 Where Winds Meet 的修正已用此前的测试构建验证；最终 rc.1 构建在发布前未在游戏内运行，本次不宣称性能提升。

### 致谢

- [@avikpal693](https://github.com/avikpal693) 和 [@wer2357666](https://github.com/wer2357666)：提供 Forza Horizon 6 报告和日志（[#11](https://github.com/SilyNoMeta/rtx-encore/issues/11)、[#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)）。
- [@yannduwooz83](https://github.com/yannduwooz83)：提供 Final Fantasy VII Rebirth 报告和日志（[#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)）；[@wd3dmlys](https://github.com/wd3dmlys)：提供 GPU 架构线索（[#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)）；[@lovebabyx](https://github.com/lovebabyx)：提供 beta.5 报告（[#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)）。
- [@ranmo-1](https://github.com/ranmo-1)：提供 Where Winds Meet 报告（[#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)）。
- [@PhantomGamers](https://github.com/PhantomGamers)：提供刷新率余量公式（[#23](https://github.com/SilyNoMeta/rtx-encore/issues/23)）以及 Special K 报告和日志（[#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)）。

感谢每一位分享日志并花时间测试构建的朋友。

## 1.0.0-beta.5 — 预发布版本

- 修正帧生成运行期间界面重组（UIR）设置变更被忽略的问题（[#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)）。这包括启用和关闭 UIR、**Force UIR** 以及游戏或自动模式发出的请求，适用于已验证的 Streamline 2.11.1、2.12.0、2.14.1 生产版文件。关闭 Force UIR 后，游戏或自动模式仍可请求启用 UIR。
- 兼容范围与 beta.4 相同。建议使用最新已验证的 Streamline 2.14.1 和 DLSS-G 310.9.1.0，以获得最完整的功能；详见[兼容性](docs/COMPATIBILITY.zh-CN.md#streamline-版本)。

UIR 修正已通过游戏外自动化检查。最终 beta.5 构建仍待在 Starfield 及其他游戏中确认；本次不宣称性能提升。

### 致谢

- [@Avictuu](https://github.com/Avictuu)：提供 Starfield 报告、组件对比和日志，帮助定位游戏运行期间 UIR 设置不生效的问题（[#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)）。

## 1.0.0-beta.4 — 预发布版本

- 在已验证的 Streamline 2.14.1 生产版之外，为 2.11.1、2.12.0 增加原生动态 MFG 和界面重组支持。为符合条件的旧 DX12 Streamline 2.7.2–2.10.3 增加 Encore 自适应 X2–X4。Vulkan 保留自适应路径；D3D11 仅限 Smooth Motion X2。具体版本与限制见[兼容性](docs/COMPATIBILITY.zh-CN.md#streamline-版本)。
- 新增 **System → Component compatibility**，显示已加载版本、功能可用性及原因。不兼容选项变灰，仍可关闭旧覆盖设置。建议使用最新已验证的 Streamline 2.14.1 和 DLSS-G 310.9.1.0，以获得最完整的功能。
- 修正 The Witcher 3 动态 FG 中发现的无效内存访问（[#4](https://github.com/SilyNoMeta/rtx-encore/issues/4)）以及 Starfield 搭配 Streamline 2.12 的退出问题（[#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)）。此前测试构建的正常退出已获确认；最终 beta.4 的复测及 Witcher 崩溃是否解决仍待确认。
- 修正 Streamline 重启时 Reflex 插件的生命周期，以处理调查中的 Cyberpunk 启动问题（[#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)、[#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)）。尚待游戏内确认。
- 修正跨线程 Reflex 上限更新及恢复延迟。新增直接 FPS 输入、**Use game settings** 与 **For this display**（刷新率减 3 FPS）。FG 是否计入取决于游戏/驱动；动态 MFG 暂停此覆盖设置。Reflex 兼容范围扩展至已验证的 DLL 版本 2.1.1–2.14.1；没有官方 2.13 SDK 可供核验。
- 新增独立 SR、RR、FG 预设选择，重启游戏生效。FG A/B 支持逐一验证的 DLSS-G 310.6.0.0、310.7.0.0、310.7.128.0、310.7.129.0、310.8.0.0、310.9.0.0、310.9.1.0 生产版文件。SR/RR 取决于 DLL 与驱动覆盖设置，不能保证实际模型。默认保留游戏选择。
- 自动记录系统与已加载组件版本、DLSS 质量/分辨率和请求的 SR/RR 预设。可选性能日志新增 GPU 温度、功耗、频率、报告的性能限制及超过一秒的间隔。这只改善诊断，不代表 [#12](https://github.com/SilyNoMeta/rtx-encore/issues/12) 或 [#19](https://github.com/SilyNoMeta/rtx-encore/issues/19) 已修复。

自动化检查不能代替各游戏、显卡、驱动组合的实际测试。新增自适应路径、兼容性显示和预设功能仍需要游戏内反馈。

### 致谢

- [@Avictuu](https://github.com/Avictuu)：反复测试 Starfield、核对倍率、提供详细 Reflex/退出日志，并确认测试构建正常退出（[#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)）。
- [@wisecase2](https://github.com/wisecase2)：提供同一会话的 Witcher 3 日志及 Cyberpunk 后续证据（[#4](https://github.com/SilyNoMeta/rtx-encore/issues/4)、[#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)）。
- [@hunan-NRT](https://github.com/hunan-NRT)、[@naruto490610-alt](https://github.com/naruto490610-alt)：Cyberpunk 报告、日志与后续测试（[#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)、[#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)）。
- [@yannduwooz83](https://github.com/yannduwooz83)、[@PrincessofCelery](https://github.com/PrincessofCelery)：帮助完善支持信息与卡顿诊断的报告（[#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)、[#19](https://github.com/SilyNoMeta/rtx-encore/issues/19)）。

感谢所有分享日志并花时间测试构建的用户。


## 1.0.0-beta.3 — 预发布版本

- 修正 The Last of Us 中 RTX Encore、OptiScaler 与游戏旧版 Streamline 同时使用时发现的启动崩溃原因。尚待游戏内确认；Dead Space 的情况仍未确认。
- 正确检测 DLSS Ray Reconstruction 活动。NR 缺少兼容深度输入时会说明原因，不再掩盖 DLSS 活动；兼容性仍取决于游戏提供的数据。
- 为每个进程和启动分别保存异常日志，避免错误报告程序覆盖游戏的诊断数据。这有助于调查 Cyberpunk 的启动崩溃，但不代表该崩溃已修复。

- 修复启用内置菜单时，在 Starfield 中开启帧生成导致的崩溃。
  初始测试版本已在游戏中确认正常，包括关闭/重新开启帧生成以及打开菜单。
  最终变体还修正了对象生命周期和诊断功能，并通过了 Windows 自动化测试；尚未对该变体单独进行游戏内复测。
- 限制可选图形跟踪的日志数量，并避免记录已加载模块时产生诊断异常。

### 致谢

- 感谢 [@mennogreg](https://github.com/mennogreg) 提供 Starfield 的问题报告、日志和多次游戏内测试（[#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)），以及 OptiScaler 兼容性问题的报告和日志（[#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)）。
- 感谢 [@hunan-NRT](https://github.com/hunan-NRT) 报告 Ray Reconstruction 检测问题，并感谢 @mennogreg 确认同类现象（[#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)）。
- 感谢 [@naruto490610-alt](https://github.com/naruto490610-alt) 提供 Cyberpunk 报告和日志，帮助发现异常日志缺少进程隔离的问题（[#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)）。

也感谢所有分享问题报告和日志、帮助调查其余问题的用户。

## 1.0.0-beta.2 — 预发布版本

以 **RTX Encore** 名称发布的首个版本。版本号重新从 1.0.0 开始：此版本接替并取代以 DLSSG-Transfusion
名称发布的 `v1.4.5.3-rtx20-30-40`。

### 新增

- **内置菜单和叠加层。** 游戏中按 **Insert** 即可访问全部设置、各功能状态，以及显示帧率、倍率、帧时间稳定性、
  GPU 和显存的叠加层。支持 DirectX 11、DirectX 12 和 Vulkan 游戏。不再需要 ReShade，也不再提供以前的
  ReShade 附加组件。
- **Neural Rendering** 在 RTX 20、RTX 30 和 RTX 40 上作用于 DLSS 输出：一至四遍处理，每遍可选风格，支持
  HDR，可选择 NVIDIA 引擎或实验性的 Open 引擎。实验性功能，默认关闭；需要 NVIDIA 的 `nvngx_dlssnr.dll`
  310.8.0。Open 不支持 RTX 20。
- **我们针对 Ampere（RTX 30）的 NR 性能工作。** NVIDIA 引擎在本地参考测试中保持输出一致的加速处理；NVIDIA
  Fast 精度在 RTX 30 上实测 NR 时间约减少 17 %；独立的 NR 分辨率，包括 Ultra Performance；以及 Open 的
  Fast 精度和可选的速度/显存设置。我们的 **Ultra-fast mode** 超快模式，在 RTX 3070 Ti Laptop、FG X3
  的悟空基准测试中，将 NR 时间从 **18.7 降至 9.8 ms（−48 %）**，显示帧率从 **73.8 提升至 94.9 FPS（+29 %）**，
  每种配置各运行一次。只支持一遍 NR，运动时可能出现瑕疵，收益不保证。[测量结果与取舍](docs/NEURAL-RENDERING.zh-CN.md)。
- **RTX 30 上的 Smooth Motion** 在原有 617.14 之外，新增接受 NVIDIA 驱动 617.42、616.92 和 616.64，
  并可从菜单设置。只有 617.14 已在游戏中验证。
- **一个通用文件。** `rtx-encore.dll` 现在接受十九个文件名，便于与占用常见名称的其他模组共存。
- **新控制项**：通过 NVIDIA Reflex 限制渲染帧率、请求关闭 V-Sync，以及 The Witcher 3 中兼容的光线追踪毛发。
- Cyberpunk 2077 中 **Cyber Engine Tweaks 的可选面板**（实验性）。
- **会话日志**保存在 `rtx-encore-logs` 文件夹中，默认保留最近三份。

### 变更

- 文件改名为 `rtx-encore.*`。设置、快捷键和以前的插件会自动迁移：见[从旧名称升级](docs/INSTALLATION.zh-CN.md#从旧名称升级)。
- 设置文件为 `rtx-encore.jsonc`，按分组组织，每个键附有注释。
- 若干设置和菜单项改用了更清晰的名称。旧版本的设置文件会连同其中的值一并沿用。
- RTX 40 显卡上的帧生成 GPU 时间减少 2 至 3 %。
- 叠加层文字更紧凑，并可设置大小。

### 已知限制

- RTX 20 支持是实验性的，尚未在真实 RTX 20 显卡上验证。
- 多遍 Neural Rendering、Open 引擎、Cyber Engine Tweaks 面板，以及 RTX 20 和 RTX 30 上的光线追踪毛发，
  游戏内测试仍然有限或尚未进行。
- X5 和 X6 是实验性的，需要充足显存。

## 以前的版本

截至 `v1.4.5.3-rtx20-30-40` 的版本以 DLSSG-Transfusion 名称发布，现已不再提供。
