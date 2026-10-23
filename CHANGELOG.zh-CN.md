# 更新日志

*[English](CHANGELOG.md) · [Français](CHANGELOG.fr.md)*


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
