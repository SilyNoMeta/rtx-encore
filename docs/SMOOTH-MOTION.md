# Smooth Motion on RTX 20 / RTX 30 / RTX 40 / RTX 50

*[Français](SMOOTH-MOTION.fr.md) · [中文](SMOOTH-MOTION.zh-CN.md)*

Smooth Motion is the frame interpolation of the NVIDIA driver: it adds one frame between two rendered frames in
DirectX 11, DirectX 12 and Vulkan games, **including games without DLSS**. RTX Encore makes it available on RTX 30
cards.

> [!NOTE]
> This feature is off by default. It is separate from DLSS Frame Generation: the two can be used
> in different games, or together in a game that has both.

## Requirements

- An **RTX 30** card, or **RTX 40 / RTX 50** with RTX Encore **1.4.5 or later**. RTX 40/50 use the native driver engine and the same Smooth Motion switch in the Hub or in-game menu. RTX 20 support is experimental from RTX Encore 1.3.0; see below.
- One of these NVIDIA driver versions, exactly: **621.01**, **620.12**, **617.42**, **617.14**, **616.92**,
  **616.64** or **591.86**.
  617.14 and 621.01 have been verified in a game. With any other driver the feature stays off and says why; the
  rest of RTX Encore keeps working.

## Turn it on

1. Open the menu (**Insert**), **Frames** tab, **Smooth Motion** section.
2. Turn **Smooth Motion** on.
3. Set **Graphics API** to the API the game really uses: Direct3D 12, Direct3D 11 or Vulkan. This is not always the
   one its engine suggests; the game's page on [PCGamingWiki](https://www.pcgamingwiki.com/) tells you.
4. Restart the game.

The **Smooth Motion** chip at the top of the menu shows the state. "Driver prepared" means the driver is ready; it
does not by itself confirm that interpolated frames are displayed. Check the result with a frame rate tool that
measures displayed frames, such as NVIDIA FrameView.

The FPS counter estimates Smooth Motion output at **2x** and shows `2x sm` (for example, `148/74 fps 2x sm`). This is an estimate, not a measurement of displayed interpolated frames.

## RTX 20

RTX 20 requires **NVIDIA 621.01** and a **DirectX 12** game. From 1.5.1, RTX 20 always uses
V2, which users found the faster of the two; there is no variant to choose any
more. A `smoothMotionTuringVariant` line left in an older configuration file is ignored.
This has no effect on RTX 30, 40 or 50.

## What was observed

| API | Game | Result reported |
| :--- | :--- | :--- |
| DirectX 12 | Manor Lords | 120 FPS displayed from a game capped at 60 FPS |
| DirectX 11 | Shadows of Doubt | 120 FPS displayed from a game capped at 60 FPS |
| Vulkan | Enshrouded | Interpolated frames displayed, with the game's NVIDIA profile set to Off |

These are a few user observations on driver 617.14; Enshrouded was also confirmed on driver 621.01. They are not a
guarantee of compatibility, latency or image quality in other games.

## In the overlay

| Example | Meaning |
| :--- | :--- |
| `148/74 fps sm` | Estimated output with Smooth Motion / the game's frame rate |
| `296/74 fps 2x+sm` | DLSS Frame Generation X2, then Smooth Motion |

The Smooth Motion figure is an estimate: the driver can suspend interpolation, and the overlay does not measure
what the screen receives. It only appears once the driver has started its interpolation; until then the overlay
shows the game's frame rate alone, with one of these lines, and the menu shows the same state with what to do:

| Line | Meaning |
| :--- | :--- |
| `SM OFF: SET GRAPHICS API TO VULKAN` | The game presents with Vulkan while **Graphics API** is set to Direct3D |
| `SM NOT RUNNING: NO DRIVER MODULE LOADED` | The driver is prepared but has not started interpolating |

## Vulkan games the driver excludes

In Vulkan games, the NVIDIA driver decides per game whether Smooth Motion may run. With Smooth Motion on and
**Graphics API** set to Vulkan, RTX Encore lifts that per-game decision: Smooth Motion runs even in a game the
driver's profile excludes. There is no separate setting for it any more.

> [!WARNING]
> NVIDIA may exclude a game for a reason. There, Smooth Motion can give unexpected results: a duplicated mouse
> cursor, visual artifacts or a crash. Turn Smooth Motion off for that game if it misbehaves.

If Smooth Motion still does not start in a Vulkan game, it cannot run in that game.

## If it does not work

- **The menu reports an unsupported driver**: install one of the versions above.
- **Nothing changes in game**, or the menu says **Wrong API** or **Not running**: check the **Graphics API**
  choice, and restart the game after every change. Emulators are often Vulkan.
- **Vulkan**: try with Smooth Motion set to Off in the game's profile of the NVIDIA app. See also
  [Vulkan games the driver excludes](#vulkan-games-the-driver-excludes).
- Include the menu's report (**System** tab, **Copy**) when you open an issue.
