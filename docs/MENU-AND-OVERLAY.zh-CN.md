# 菜单和叠加层

*[English](MENU-AND-OVERLAY.md) · [Français](MENU-AND-OVERLAY.fr.md)*

RTX Encore 在 DirectX 11、DirectX 12 和 Vulkan 游戏中绘制自己的菜单和叠加层，无需安装其他软件。

## 菜单

在游戏窗口中按 **Insert** 打开或关闭菜单。首次启动时菜单会自动打开一次。快捷键可以在菜单中修改。

每个页面顶部都有四个状态标签：**Upscaling**、**Frame Generation**、**Smooth Motion** 和 **Neural Rendering**。
每个标签通过彩色圆点和一个数值显示相应功能的状态；悬停查看详情，点击进入设置。持续存在的问题会显示在标签下方的
卡片中，并给出处理方法。

| 选项卡 | 内容 |
| :--- | :--- |
| **Frames** | 帧生成及其模型预设（A/B Normal、C Lite）、独立的 Fast 选项、画质和界面选项、Smooth Motion、渲染帧率限制、V-Sync |
| **Image** | DLSS 渲染分辨率、SR/RR 模型预设、Neural Rendering |
| **Overlay** | 叠加层预览、位置、文字大小和显示项目 |
| **System** | 快捷键、兼容性、诊断和会话详情，以及用于报告问题的 **Copy** 按钮 |

使用提示：

- 每次修改都会立即保存到 `rtx-encore.jsonc`，保留文件中的注释。
- 大部分设置实时生效。名称后的 `*` 表示该设置在游戏启动时读取；修改后变为橙色，菜单底部会列出等待重启的修改。
- 已修改的设置名称更亮，并带有重置按钮。
- **FG model preset** 需要重启游戏：**A/B 使用 Normal；C (Lite) 自动启用 Lite**。**Fast frame generation** 保持独立。Lite 覆盖仅供 JSON 诊断使用，菜单不再提供单独的设置。见[设置](SETTINGS.zh-CN.md#dlsspresets)。
- 每部分显示主要设置，其余设置位于可展开的分组中，例如 **Fine tuning**、**Performance** 和 **Diagnostics**。
- 拖动标题栏移动菜单，拖动右下角调整大小。位置和尺寸会被记住。
- 打开菜单时如果仍按着鼠标按钮，请先松开再点击。

### 菜单在独立窗口中打开时

少数游戏无法安全地在画面内绘制菜单。此时设置会在独立窗口中打开，叠加层则变为游戏上方的一小块可穿透点击的文字。
建议使用无边框窗口模式。设置及其效果相同。

### 功能可用性

**System → Component compatibility** 显示已加载的 Streamline、DLSS-G、SR、RR 版本，以及功能可用性与限制原因。不支持或未经资格检查的选项会变灰。尚未加载的组件显示为等待中；在游戏内启用 DLSS/FG 后可能更新。仍可关闭已保存的覆盖设置或恢复游戏设置。可提交预设请求并不代表已确认驱动实际使用的模型。

## 叠加层

在 **Overlay** 选项卡中开启，或按 `Ctrl + Alt + O`。`Ctrl + Alt + P` 将其移至下一个角落。

| 示例 | 含义 |
| :--- | :--- |
| `74 fps` | 游戏帧率；未观察到帧生成 |
| `74/37 fps 2x` | 显示帧率 / 渲染帧率，以及观察到的倍率 |
| `148/74 fps sm` | 使用 Smooth Motion 的估算输出 / 游戏帧率 |
| `296/74 fps 2x+sm` | 帧生成 X2，再加 Smooth Motion |

可选项目均默认关闭：

| 项目 | 显示内容 |
| :--- | :--- |
| Neural Rendering | 状态和工作尺寸 |
| Frame pacing | 帧时间：平均值、第 99 百分位和抖动 |
| GPU | 负载、温度、功耗和频率 |
| VRAM | 显卡已用 / 总显存，包含所有程序 |
| Versions | 游戏加载的 DLSS、DLSS 帧生成和 Streamline 版本。`SL` 是游戏自带的 Streamline 核心，更新插件时它保持不变；插件版本不同时，第二行同时显示两者（`SL 2.7.11.0 PLUGINS 2.14.16.0`） |
| Interface | 是否启用界面单独处理，以及无界面场景和界面层的来源 |

同一选项卡还可以设置位置（四个角落）和文字大小（10 至 32 px）。

## 其他控制

| 设置 | 选项卡 | 作用 |
| :--- | :--- | :--- |
| **Render resolution** | Image | 指定游戏在 DLSS 处理前的渲染分辨率：DLAA、Quality、Balanced、Performance、Ultra Performance，或 50 至 100 % 的自定义比例。游戏必须已使用 DLSS。菜单显示实际观察到的分辨率，以及游戏是否遵从请求；出现问题时改回 Game。Unreal Engine 4 和 5 游戏会在可行时实时调整分辨率比例。 |
| **Frame limit (FPS)** | Frames | 通过受支持的 NVIDIA Reflex 请求帧率上限。0 恢复游戏设置。生成帧是否计入上限取决于游戏与驱动。在 Dynamic 模式下暂停。 |
| **Request V-Sync off** | Frames | 请求 DirectX 游戏关闭 V-Sync。NVIDIA 控制面板强制设置的优先级更高。对 Vulkan 游戏无效。 |
| **Compatible ray-traced hair** | System | The Witcher 3（DirectX 12，游戏构建 25575366 和 25646871）：使游戏的光线追踪毛发可用于 RTX 20、30 和 40。游戏自身的毛发设置仍决定是否渲染毛发。尚未在 RTX 20 和 RTX 30 上进行游戏内验证。需重启游戏。 |

### 建议的 Reflex 帧率上限

Reflex 限制器接受 DLL 版本 2.1.1–2.14.1，以及 2.14.16；其他版本保留游戏设置。
已对该范围内公开发布的 SDK 进行离线检查，但并未逐个版本进行游戏内验证。
没有公开标签的中间版本（包括 2.13 和 2.14.0）尚无公开 SDK 验证。
实际限帧仍取决于游戏的 Reflex 集成和驱动支持。
不会添加基于 CPU 等待的备用限制器。

在 **Frames → Frame pacing** 中，**For this display** 建议 `刷新率 - 刷新率² / 3600`（向下取整），
例如 120 Hz 时请求 **116 FPS**，144 Hz 时为 138，240 Hz 时为 224，不再除以 FG 倍率。生成帧是否计入上限取决于游戏和驱动。
Starfield 搭配 Streamline 2.12 时，请求 90 曾得到约 22.5 FPS 渲染帧率和 ×4 FG。
请比较实测源帧率与包含 FG 的帧率。点击 **Use … FPS** 保存建议值；更换显示器或倍率不会自动调整上限。
也可直接在 **Frame limit (FPS)** 中输入 1 至 1000，按 **Enter** 或点击 **Apply** 保存。
**Use game settings**（或 0）恢复游戏自身的上限，不会强制关闭所有限帧器。
点击其他位置不会保存尚未提交的输入。

Dynamic 模式、Smooth Motion，或显示器刷新率及当前帧生成状态未知时，不提供建议。
仍可手动设置上限，但需要受支持的 Reflex。菜单分别显示 Reflex 请求及其状态和实测源帧率：
**Reflex accepted … FPS** 仅表示请求已被接受，不证明实测帧率或显示输出符合该值。

余量随刷新率增大，只是初始建议，不会自动检测 VRR，也不保证始终处于其范围内。
较低的上限只有在实际降低渲染帧率时，才可能降低负载、功耗和温度。
上限高于游戏当前可达到的帧率时可能没有收益；请在机器热起来后，对比同一场景再调整。

## 快捷键

默认快捷键见[帧生成](FRAME-GENERATION.zh-CN.md#快捷键)。在 **System** 选项卡中，每个操作可以绑定多个组合键，
也可以不绑定。快捷键仅在游戏窗口获得焦点时生效。

## 日志

会话日志写入模组文件旁边的 `rtx-encore-logs` 文件夹；默认保留最近三份（**Session logs kept**，1 至 100）。
**Log performance** 将本次会话的帧率和帧时间写入同一文件夹中的 CSV 文件。

异常日志单独保留，数量与常规日志相同。每个进程使用独立文件；仍在使用的日志不会删除。

开启 **Log performance** 后，会话日志还会约每秒记录一次 GPU 温度、功耗、频率及报告的性能限制。隐藏叠加层或关闭帧生成时仍可记录。无法获取的读数标为 `NA`。参阅[长时间游戏后的性能下降](TROUBLESHOOTING.zh-CN.md#游玩一段时间后性能下降)。
