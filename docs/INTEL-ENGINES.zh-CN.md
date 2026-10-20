# Intel XeSS 与 XeFG（实验性）

[English](INTEL-ENGINES.md) · [Français](INTEL-ENGINES.fr.md) · [简体中文](INTEL-ENGINES.zh-CN.md)

本文档为即将推出的测试版做准备，不代表安装包已发布。可用版本请查看[发布页面](https://github.com/SilyNoMeta/rtx-encore/releases)。

RTX Encore 1.2.0-beta.1 可选择重建和帧生成引擎。保留 **Game** 可继续使用游戏原有引擎。Intel 模式仍属实验性，默认关闭，需要兼容的游戏输入；选择某个引擎不能证明它已激活。

使用 **RTX Encore Hub 1.2.0-beta.1 或更高版本**，一起安装或更新模组和 Intel 依赖。Hub 会验证文件，保留随附的 Intel 许可证，并支持撤销操作。关闭 NVIDIA 下载不会省略此模组必需的 Intel 依赖。游戏自身提供的 Intel 文件保持独立。

| 设置 | 默认 | 作用 |
| :--- | :---: | :--- |
| `reconstructionEngine` | `game` | `game` 或 `xess`：以 XeSS 替代兼容的 Super Resolution/DLAA 处理；Ray Reconstruction 保留原引擎。 |
| `frameGenerationEngine` | `game` | `game` 或 `xefg`：在受支持的 DirectX 12 或 Vulkan 路径上使用 XeFG。DirectX 11 仍属实验性，此处未完成游戏认证。 |
| `xefgMfg4VariablePacing` | `true` | XeFG 高于 X2 的实验性帧间隔控制；关闭后，较高倍率可能不可用。 |
| `xefgUiHandling` | `interpolate` | `interpolate` 或 `game`；请求单独处理游戏界面需要兼容输入。 |
| `xefgUiAlpha` | `unknown` | `unknown`、`straight` 或 `premultiplied`；不清楚界面格式时请保留 unknown。 |

修改引擎、XeFG 帧间隔或界面设置后请重启游戏。使用 XeFG 时，修改 FPS 上限也需要重启。FPS 上限不能证明源帧节奏。

HDR 输入和自动曝光使用所选重建引擎共用的 `srHdr` 与 `srAutoExposure`。默认 `game` 跟随游戏，修改后需重启。它们不会启用显示器 HDR，也不会将 SDR 转换为 HDR。错误的输入格式会损害画质。某些 DLSS 模型强制使用自动曝光；XeSS 自动曝光有 GPU 开销，不推荐用于 LDR。XeSS Vulkan 的 LDR 与手动曝光组合仍不受支持。RR 和帧生成保持不变。

XeFG 不能为所有游戏添加帧生成。更高倍率取决于获准的 Intel 运行库和完整游戏配置；X5/X6 仍属实验性，本测试版没有建立 X6 游戏支持证明。请求倍率、依赖已安装或生成帧计数，都不能测量实际显示内容或物理延迟。在 XeFG 测试中启用 NR 或 Smooth Motion，并不能证明这些组合已获认证。

No Man’s Sky 和 DOOM: The Dark Ages 的有限本地 Vulkan 测试支持 XeFG X2 路径；维护者已接受 DOOM 在无边框模式、关闭 NR 时的测试。这不认证独占全屏、DirectX 11、XeFG 与 NR 组合、所有更高倍率或其他游戏。某些 Intel 路径上菜单或叠加层可能不可用；可使用 Hub 调整设置并查看会话报告。

[设置参考](SETTINGS.zh-CN.md) · [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)
