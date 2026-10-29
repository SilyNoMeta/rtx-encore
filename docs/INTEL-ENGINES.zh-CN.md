# Intel XeSS 与 XeFG（实验性）

[English](INTEL-ENGINES.md) · [Français](INTEL-ENGINES.fr.md) · [简体中文](INTEL-ENGINES.zh-CN.md)

适用于 RTX Encore 1.5.0。可用版本请查看[发布页面](https://github.com/SilyNoMeta/rtx-encore/releases)。

RTX Encore 1.2.0 可选择重建和帧生成引擎。保留 **Game** 可继续使用游戏原有引擎。Intel 模式仍属实验性，默认关闭，需要兼容的游戏输入；选择某个引擎不能证明它已激活。

使用 **RTX Encore Hub 1.2.0 或更高版本**，一起安装或更新模组和 Intel 依赖。Hub 会验证文件，保留随附的 Intel 许可证，并支持撤销操作。关闭 NVIDIA 下载不会省略此模组必需的 Intel 依赖。游戏自身提供的 Intel 文件保持独立。

## Tensor Core：两个独立控制项

为 **XeSS Tensor Core** 选择 XeSS 重建，为 **XeFG Tensor Core** 选择 XeFG 帧生成。两开关相互独立，不会自动切换另一个开关或引擎。请用 Hub 1.5.0 安装兼容运行库并使用新控制项。安装新 DLL 前须关闭游戏；此后启动的会话可即时切换两个开关及受支持的固定 XeFG 倍率。切换 FG 引擎仍需重启游戏。

在较早测试版本的巫师 3 场景中，RTX 4090 / 621.01、4K 输出的 XeSS GPU 时间从 1.243 ms 降至 0.861 ms（约 30.7%），渲染源帧 FPS 约提高 2.8–3.2%。这**不是整体 FPS 提升 31%**。后续最终重建优化在确定性重放中验证，包括自动曝光与非整数输入尺寸；该后续版本未单独在游戏中复测。尚未证明 XeFG 的普遍 FPS 或延迟收益。

| 设置 | 默认 | 作用 |
| :--- | :---: | :--- |
| `reconstructionEngine` | `game` | `game` 或 `xess`：以 XeSS 替代兼容的 Super Resolution/DLAA 处理；Ray Reconstruction 保留原引擎。 |
| `frameGenerationEngine` | `game` | `game`、`dlss-fg` 或 `xefg`。游戏保留原生 FG；DLSS-FG 将经过资格检查的原生 Intel FG 替换为 NVIDIA DLSS FG；XeFG 将兼容 DLSS FG 替换为 Intel XeFG。需要重启。 |
| `xessTensorCores` | `false` | 实验性 NVIDIA Tensor Core XeSS Super Resolution，在下次 XeSS 执行时即时切换。初始验证：RTX 4090、NVIDIA 621.01、DirectX 12 和 Hub 提供的 Intel 运行库。准备未通过时保留标准 XeSS。 |
| `xefgTensorCores` | `false` | 实验性 NVIDIA Tensor Core XeFG；在初始化容量内支持固定 X2–X6。切换在命令重新记录时生效。初始验证：RTX 4090、NVIDIA 621.01、DirectX 12 和 Hub 提供的 Intel 运行库。准备未通过时保留标准 XeFG；执行故障后须重启。 |
| `xefgMfg4VariablePacing` | `true` | XeFG 高于 X2 的实验性帧间隔控制；关闭后，较高倍率可能不可用。 |
| `xefgUiHandling` | `interpolate` | `interpolate` 或 `game`；请求单独处理游戏界面需要兼容输入。 |
| `xefgUiAlpha` | `unknown` | `unknown`、`straight` 或 `premultiplied`；不清楚界面格式时请保留 unknown。 |

兼容游戏可在运行中切换 DLSS 与 XeSS 重建。Vulkan 游戏需要在启动前安装兼容的 Intel 运行库；添加或替换运行库后必须重启游戏。修改帧生成引擎、XeFG 帧间隔或界面设置后仍需重启。使用 XeFG 时，修改 FPS 上限也需要重启。FPS 上限不能证明源帧节奏。

HDR 输入和自动曝光使用所选重建引擎共用的 `srHdr` 与 `srAutoExposure`。默认 `game` 跟随游戏，修改后需重启。它们不会启用显示器 HDR，也不会将 SDR 转换为 HDR。错误的输入格式会损害画质。某些 DLSS 模型强制使用自动曝光；XeSS 自动曝光有 GPU 开销，不推荐用于 LDR。XeSS Vulkan 的 LDR 与手动曝光组合仍不受支持。RR 和帧生成保持不变。

XeFG 不能为所有游戏添加帧生成。更高倍率取决于获准的 Intel 运行库和完整游戏配置；X5/X6 仍属实验性，本测试版没有建立 X6 游戏支持证明。请求倍率、依赖已安装或生成帧计数，都不能测量实际显示内容或物理延迟。在 XeFG 测试中启用 NR 或 Smooth Motion，并不能证明这些组合已获认证。

No Man’s Sky 和 DOOM: The Dark Ages 的有限本地 Vulkan 测试支持 XeFG X2 路径；维护者已接受 DOOM 在无边框模式、关闭 NR 时的测试。这不认证独占全屏、DirectX 11、XeFG 与 NR 组合、所有更高倍率或其他游戏。某些 Intel 路径上菜单或叠加层可能不可用；可使用 Hub 调整设置并查看会话报告。

[设置参考](SETTINGS.zh-CN.md) · [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)

在 Mirage 等使用原生 Intel FG 的游戏中，通过 Hub 安装兼容运行库，在 Encore 选择 **DLSS-FG** 并重启，再在游戏中启用 **Intel FG**。使用 **Fixed** 选择允许的倍率；此适配器的 **Game** 使用 X2，动态模式不可用。原生 FSR 和已有原生 DLSS FG 保持不变。维护者已确认 Mirage 的修复测试版本；最终发布文件尚未在游戏中单独复测。
