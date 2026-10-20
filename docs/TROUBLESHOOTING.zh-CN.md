# 故障排除

*[English](TROUBLESHOOTING.md) · [Français](TROUBLESHOOTING.fr.md)*

## Intel XeSS 与 XeFG（实验性）

RTX Encore 1.2.0-beta.1 可选择重建和帧生成引擎。保留 **Game** 可继续使用游戏原有引擎。Intel 模式仍属实验性，默认关闭，需要兼容的游戏输入；选择某个引擎不能证明它已激活。

[Intel XeSS 与 XeFG（实验性）](INTEL-ENGINES.zh-CN.md)

## 首先检查

1. **模组已加载吗？** 加载后，模组文件旁边会出现 `rtx-encore-logs` 文件夹，**Insert** 会打开菜单。
   两者都没有出现时，游戏没有加载文件。
2. **是否只有一份？** 在同一游戏中以两个名称安装两份模组，会造成崩溃、异常行为，以及开启 Neural Rendering 时画面劣化。
3. **菜单显示什么？** 顶部四个标签显示各功能状态；持续存在的问题会显示在卡片中，并附处理方法。

## 模组没有加载

| 检查项 | 处理方法 |
| :--- | :--- |
| 文件夹不正确 | 文件必须放在实际运行游戏的可执行文件旁边。Unreal Engine 游戏通常是 `<Game>\Binaries\Win64\`。 |
| 游戏不加载此名称 | 依次尝试 `version.dll`、`dinput8.dll`、`winmm.dll`、`dxgi.dll`。 |
| 名称被其他模组占用 | 保留该模组的文件，为 RTX Encore 选择其他名称。 |
| 改名后的文件被拒绝 | 使用 `alternative-proxies` 中已命名的副本。 |
| 游戏有反作弊 | 它可能阻止加载。请勿在这些游戏中使用 RTX Encore。 |

## 帧生成

| 现象 | 首先检查 |
| :--- | :--- |
| 游戏没有 Frame Generation 选项 | RTX Encore 扩展游戏已有的 DLSS 帧生成，不能凭空添加。其他游戏可参阅 [Smooth Motion](SMOOTH-MOTION.zh-CN.md)。 |
| 有选项，但呈灰色 | 检查 DLSS 是否开启、游戏是否使用 DirectX 12 或 Vulkan，以及模组是否加载。 |
| 倍率停留在 X2 | 在游戏中关闭再开启帧生成，或重启。检查菜单模式：Game decides 模式由游戏决定。 |
| Dynamic 模式达不到 X5 或 X6 | 开启 **Allow 5x and 6x**。 |
| 此游戏版本没有任何变化 | 游戏的 `nvngx_dlssg.dll` 可能是不支持的版本：见[兼容性](COMPATIBILITY.zh-CN.md#dlss-帧生成版本)。 |
| 运动时有双重轮廓或扭曲帧 | 关闭 **Automatic UI recomposition**。 |
| 栅栏、植被或阴影出现瑕疵 | 保持 **Anti-tearing / anti-ghosting** 开启，并尝试另一种 **Protection tuning**。 |
| 高倍率下卡顿或崩溃 | 显存可能已满：降低倍率或纹理质量。 |
| 在菜单或加载画面中崩溃 | 保持 **Disable menu detection** 关闭。 |

## Smooth Motion 和 Neural Rendering

参阅各自页面末尾：[Smooth Motion](SMOOTH-MOTION.zh-CN.md#如果不起作用)、
[Neural Rendering](NEURAL-RENDERING.zh-CN.md#如果不起作用)。

## 菜单和设置

| 现象 | 首先检查 |
| :--- | :--- |
| **Insert** 无反应 | 模组未加载（见上文），或快捷键已修改：检查 `rtx-encore.jsonc` 的 `menuState`。 |
| 菜单在独立窗口中打开 | 少数游戏中这是预期行为：见[菜单和叠加层](MENU-AND-OVERLAY.zh-CN.md#菜单在独立窗口中打开时)。 |
| 点击无效 | 松开所有鼠标按钮后再点击。无边框窗口模式可能有帮助。 |
| 设置立即恢复 | 设置文件未能保存：确认文件不是只读，也未被编辑器锁定。 |
| 设置没有效果 | 标有 `*` 的设置在游戏启动时读取：请重启游戏。 |
| 快捷键无反应 | 仅在游戏窗口获得焦点时生效，并且 `disableKeybinds` 必须为 `false`。 |

## 游玩一段时间后性能下降

开启 **System → Diagnostics → Log performance**，从机器冷却后的状态开始游玩，直到出现性能下降。
采集期间保持游戏设置不变，并记下下降的大致时间。请提供 `rtx-encore-logs` 中游戏进程的会话日志；
如果生成了帧时间 CSV，也请一并提供。之后关闭此选项。

`[GPU-PERF]` 行记录温度、功耗、频率、报告的限制、CPU 使用率及外接电源/电池状态，并同时记录游戏帧间隔。
日志会标明所监测的 NVIDIA 显卡；如有多块 NVIDIA 显卡，请核对名称。无法获取的数据标为 `NA`，不是零。
不包括 CPU 温度、CPU 功耗和笔记本风扇转速。这些读数无法直接判断导热材料的状况。

报告的温度限制有助于确认与发热有关的降频。仅有功耗限制不能区分笔记本电源配置、CPU/GPU 功率分配或其他供电约束。
两次采样之间的短暂事件可能被遗漏。比较性能下降前后同一场景的数据尤其有用。

另做一组对比时，可尝试 **Frames → Frame pacing → Suggested Reflex cap**（如可用），
或手动请求 Reflex 上限。建议值低于显示器刷新率（240 Hz 时为 224 FPS），不是温度目标。FG 的计入方式取决于游戏与驱动。
若应用后实际渲染帧率没有下降，可尝试更低的请求值。
请对比实际源帧率和 GPU 读数；仅显示 `accepted` 不能证明有效。保存的上限不会自行变化。
参见[帧率上限控制](MENU-AND-OVERLAY.zh-CN.md#建议的-reflex-帧率上限)。

## 报告问题

即使关闭诊断选项，也会自动记录 Streamline/NGX 的常规消息，包括它们报告的功能拒绝原因。
消息数量有限，防止重复消息填满日志。如果支持人员需要更多详情，请在 `rtx-encore.jsonc` 中将
`diagnostics.logStreamline` 设为 `true`，然后重启游戏。收集日志后请关闭该选项并重启；详细日志可能降低性能。

即使关闭诊断选项，会话日志也会自动记录基本信息：Windows 版本、CPU、内存、显示适配器和桌面显示模式，
以及已加载 DLL 的版本和路径（包括游戏、模组、Streamline、DLSS 和驱动组件）。无法读取的版本会明确标注。
加载了多个副本并不能证明实际使用的是哪一个。极早发生的崩溃或在被观察前已卸载的 DLL 可能无法记录。

观察到的 DLSS 创建还会记录图形 API、SR/RR 模式、质量、分辨率和请求的预设。
**请求的预设不等于已确认的实际预设**：驱动或覆盖设置可能选择其他模型。默认、缺失和未知值会分别标明。
报告有数量上限，并在适当位置去重；逐帧性能及详细图形/NR 跟踪仍需启用相应选项。
请压缩发生问题的那次启动所生成的整个 `rtx-encore-logs` 文件夹，以包含游戏及加载器/错误报告程序的日志。
菜单的 **Copy** 报告只是简要信息，不能代替完整日志文件夹。

请提交 [issue](https://github.com/SilyNoMeta/rtx-encore/issues)，并附上：

- 游戏、版本或商店，以及图形 API；
- 显卡和 NVIDIA 驱动版本；
- RTX Encore 版本和你使用的文件名；
- 菜单中复制的报告（**System** 选项卡，**Copy**）；
- `rtx-encore-logs` 中出现问题的会话日志。日志可能包含电脑上的文件夹名称：发布前请先检查。

每个 issue 报告一个问题，说明操作步骤和观察结果。
