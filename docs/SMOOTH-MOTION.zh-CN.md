# RTX 30 上的 Smooth Motion

*[English](SMOOTH-MOTION.md) · [Français](SMOOTH-MOTION.fr.md)*

Smooth Motion 是 NVIDIA 驱动的插帧功能：在 DirectX 11、DirectX 12 和 Vulkan 游戏中，它会在两个渲染帧之间
插入一帧，**包括没有 DLSS 的游戏**。RTX Encore 让 RTX 30 显卡也能使用它。

> [!NOTE]
> 该功能默认关闭。它与 DLSS 帧生成相互独立：两者可以分别用于不同的游戏，也可以在同时具备两者的
> 游戏中一起使用。

## 要求

- 一块 **RTX 30** 显卡。RTX 40 显卡的 Smooth Motion 由 NVIDIA 提供；不支持 RTX 20 显卡。
- NVIDIA 驱动必须恰好是以下版本之一：**620.12**、**617.42**、**617.14**、**616.92** 或 **616.64**。
  目前只有 617.14 在游戏中得到过验证。使用其他任何驱动时，该功能保持关闭并说明原因；RTX Encore 的其余功能
  照常工作。

## 开启方法

1. 打开菜单（**Insert**），进入 **Frames** 选项卡的 **Smooth Motion** 部分。
2. 开启 **Smooth Motion**。
3. 把 **Graphics API** 设为游戏实际使用的 API：Direct3D 12、Direct3D 11 或 Vulkan。它不一定是引擎给人的
   印象中的那个；可以在 [PCGamingWiki](https://www.pcgamingwiki.com/) 的游戏页面上查到。
4. 重启游戏。

菜单顶部的 **Smooth Motion** 标签会显示状态。“Driver prepared”表示驱动已就绪；仅凭这一点并不能确认插值帧
已经显示出来。请用能测量显示帧数的工具（例如 NVIDIA FrameView）检查结果。

## 已观察到的结果

| API | 游戏 | 报告的结果 |
| :--- | :--- | :--- |
| DirectX 12 | Manor Lords | 游戏限制在 60 FPS 时显示 120 FPS |
| DirectX 11 | Shadows of Doubt | 游戏限制在 60 FPS 时显示 120 FPS |
| Vulkan | Enshrouded | 插值帧正常显示，游戏的 NVIDIA 配置文件设为 Off |

这些是少数用户在 617.14 驱动下的观察结果，并不保证在其他游戏中的兼容性、延迟或画质。

## 在叠加层中

| 示例 | 含义 |
| :--- | :--- |
| `148/74 fps sm` | 使用 Smooth Motion 的估算输出 / 游戏的帧率 |
| `296/74 fps 2x+sm` | DLSS 帧生成 X2，再加 Smooth Motion |

Smooth Motion 的数值是估算值：驱动可能暂停插帧，而且叠加层并不测量屏幕实际接收到的内容。只有在驱动开始
插帧之后才会显示该数值；在此之前，叠加层只显示游戏的帧率，并附带下列提示之一，菜单也会显示相同的状态和
处理方法：

| 提示行 | 含义 |
| :--- | :--- |
| `SM OFF: SET GRAPHICS API TO VULKAN` | 游戏使用 Vulkan 呈现，而 **Graphics API** 设为 Direct3D |
| `SM NOT RUNNING: TRY FORCE SMOOTH MOTION` | Vulkan 游戏：驱动没有为它启动 Smooth Motion；见下文 |
| `SM NOT RUNNING: NO DRIVER MODULE LOADED` | 驱动已就绪，但尚未开始插帧 |

## 在 Vulkan 游戏中强制启用 Smooth Motion

在 Vulkan 游戏中，NVIDIA 驱动会按游戏决定是否允许 Smooth Motion 运行。当驱动拒绝时，菜单会显示
**Not running**，并建议启用 **Force Smooth Motion**（**Frames** 选项卡，**Smooth Motion** 部分），即
`rtx-encore.jsonc` 中的 `"smoothMotionForce": true`。该设置默认关闭；修改后请重启游戏。

> [!WARNING]
> NVIDIA 排除某个游戏可能是有原因的。在这类游戏中强制启用 Smooth Motion 可能产生意外结果：鼠标指针重影、
> 画面瑕疵或崩溃。如果游戏表现异常，请将其关闭。

它对 DirectX 游戏没有作用。如果启用后 Smooth Motion 仍未启动，说明它无法在该游戏中运行。

## 如果不起作用

- **菜单提示驱动不受支持**：请安装上述四个版本之一。
- **游戏中没有任何变化**，或菜单显示 **Wrong API** 或 **Not running**：检查 **Graphics API** 的选择，并在
  每次修改后重启游戏。模拟器通常使用 Vulkan。
- **Vulkan**：尝试在 NVIDIA 应用的游戏配置文件中把 Smooth Motion 设为 Off。如果菜单提示驱动没有为该游戏
  启动它，请参阅[强制启用 Smooth Motion](#在-vulkan-游戏中强制启用-smooth-motion)。
- 提交 issue 时请附上菜单的报告（**System** 选项卡，**Copy**）。
