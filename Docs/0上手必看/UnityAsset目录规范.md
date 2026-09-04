# Unity Assets 目录规范

## 1. 基本原则

项目自身资源统一放入：

```text
Assets/_Project/
```

第三方资源统一放入：

```text
Assets/ThirdParty/
```

通过 Unity Package Manager 安装的官方/第三方 Package 保持在 `Packages/` 中，不复制到 `_Project`。

### 核心原则

1. `_Project` 只存放本项目自己维护的资源。
2. `ThirdParty` 只存放外部导入的第三方资源。
3. 优先按照 **Feature / 系统归属** 组织资源，而不是把所有资源全部按照文件类型平铺。
4. 一个 Feature 自己使用的 Prefab、Script、Animation、Data 等尽量放在 Feature 内部。
5. 真正跨系统共享的内容才放入 `Core`。
6. 不提前创建大量暂时用不到的目录。
7. `Resources` 目录非必要不使用。
8. Editor 专用代码必须放入 `Editor` 目录或 Editor Assembly。
9. 测试场景、测试资源与正式游戏内容分离。
10. 文件夹和资源命名统一使用英文。

---

# 2. 推荐目录结构

```text
Assets/
│
├── _Project/
│   │
│   ├── Core/
│   │   ├── Scripts/
│   │   │   ├── Runtime/
│   │   │   └── Editor/
│   │   │
│   │   ├── Data/
│   │   └── Prefabs/
│   │
│   │
│   ├── Player/
│   │   ├── Scripts/
│   │   ├── Prefabs/
│   │   ├── Animations/
│   │   ├── Models/
│   │   ├── Materials/
│   │   ├── Textures/
│   │   └── Data/
│   │
│   │
│   ├── Camera/
│   │   ├── Scripts/
│   │   ├── Prefabs/
│   │   └── Data/
│   │
│   │
│   ├── Interaction/
│   │   ├── Scripts/
│   │   ├── Prefabs/
│   │   └── Data/
│   │
│   │
│   ├── Combat/
│   │   ├── Scripts/
│   │   ├── Prefabs/
│   │   ├── Animations/
│   │   └── Data/
│   │
│   │
│   ├── Weapons/
│   │   ├── Scripts/
│   │   ├── Prefabs/
│   │   ├── Models/
│   │   ├── Materials/
│   │   ├── Textures/
│   │   ├── Animations/
│   │   ├── Audio/
│   │   └── Data/
│   │
│   │
│   ├── AI/
│   │   ├── Scripts/
│   │   ├── Prefabs/
│   │   └── Data/
│   │
│   │
│   ├── UI/
│   │   ├── Scripts/
│   │   ├── Prefabs/
│   │   ├── Textures/
│   │   ├── Fonts/
│   │   └── Data/
│   │
│   │
│   ├── Environment/
│   │   ├── Models/
│   │   ├── Materials/
│   │   ├── Textures/
│   │   ├── Prefabs/
│   │   └── Terrain/
│   │
│   │
│   ├── VFX/
│   │   ├── Prefabs/
│   │   ├── Materials/
│   │   ├── Textures/
│   │   ├── Shaders/
│   │   └── VFXGraphs/
│   │
│   │
│   ├── Audio/
│   │   ├── BGM/
│   │   ├── SFX/
│   │   ├── Voice/
│   │   └── Mixers/
│   │
│   │
│   ├── Shaders/
│   │   ├── Common/
│   │   ├── Character/
│   │   ├── Environment/
│   │   ├── PostProcessing/
│   │   ├── Compute/
│   │   └── Includes/
│   │
│   │
│   ├── Scenes/
│   │   ├── Bootstrap/
│   │   ├── Gameplay/
│   │   ├── Levels/
│   │   └── Test/
│   │
│   │
│   ├── Settings/
│   │   ├── Rendering/
│   │   │   ├── URP/
│   │   │   └── Volume/
│   │   │
│   │   ├── Input/
│   │   ├── Physics/
│   │   └── Audio/
│   │
│   │
│   └── Editor/
│
│
└── ThirdParty/
    ├── PluginA/
    ├── PluginB/
    └── ...
```

> 注意：以上是项目发展后的完整结构示例。
>
> **不要在项目刚开始时一次性创建所有目录。**
>
> 只有当对应系统真正出现时，再创建对应 Feature。

---

# 3. Feature 目录规范

优先按照 Gameplay Feature 组织资源。

例如 Player：

```text
Player/
├── Scripts/
├── Prefabs/
├── Animations/
├── Models/
├── Materials/
├── Textures/
└── Data/
```

Player 自己使用的资源应该尽量留在 `Player` 内部，而不是拆散到项目各处。

推荐：

```text
Player/
├── Scripts/
│   ├── PlayerController.cs
│   └── PlayerLook.cs
│
├── Prefabs/
│   └── Player.prefab
│
└── Data/
    └── PlayerConfig.asset
```

不推荐：

```text
Scripts/
└── PlayerController.cs

Prefabs/
└── Player.prefab

Data/
└── PlayerConfig.asset
```

这样可以保证删除、迁移或者修改一个 Feature 时，其依赖关系更加清晰。

---

# 4. Core 目录规范

`Core` 只存放真正具有跨系统性质的基础代码和资源。

例如：

```text
Core/
├── Scripts/
│   ├── Runtime/
│   │   ├── GameManager.cs
│   │   ├── SceneManager.cs
│   │   ├── ObjectPool.cs
│   │   └── EventSystem.cs
│   │
│   └── Editor/
│
├── Data/
└── Prefabs/
```

不要因为“不知道放哪里”就放入 `Core`。

如果一个类只服务于 Player：

```text
PlayerHealth.cs
```

应该放：

```text
Player/Scripts/
```

而不是：

```text
Core/Scripts/
```

---

# 5. Prefab 规范

Prefab 优先跟随所属 Feature。

例如：

```text
Player/
└── Prefabs/
    └── Player.prefab

Weapons/
└── Prefabs/
    ├── Rifle.prefab
    └── Pistol.prefab

Camera/
└── Prefabs/
    └── CameraRig.prefab
```

跨多个系统复用的基础 Prefab 才考虑放入：

```text
Core/Prefabs/
```

---

# 6. Scene 规范

推荐：

```text
Scenes/
├── Bootstrap/
├── Gameplay/
├── Levels/
└── Test/
```

用途：

```text
Bootstrap   游戏启动、初始化场景
Gameplay    通用 Gameplay 场景
Levels      正式关卡
Test        开发/技术测试场景
```

例如：

```text
Scenes/
├── Bootstrap/
│   └── Bootstrap.unity
│
├── Levels/
│   ├── Forest/
│   │   └── Forest.unity
│   │
│   └── City/
│       └── City.unity
│
└── Test/
    ├── PlayerTest.unity
    ├── ShaderTest.unity
    └── PerformanceTest.unity
```

仅属于某个 Level 的特殊资源，可以直接跟随该 Level：

```text
Levels/
└── Forest/
    ├── Forest.unity
    │
    ├── Profiles/
    │   └── ForestVolume.asset
    │
    └── Lighting/
        └── ForestLightingSettings.asset
```

---

# 7. Rendering / URP 配置规范

URP Pipeline Asset、Renderer Data 等全局渲染配置统一放：

```text
Settings/
└── Rendering/
    └── URP/
```

例如：

```text
Settings/
└── Rendering/
    └── URP/
        ├── UniversalRenderPipelineGlobalSettings.asset
        │
        ├── Balanced/
        │   ├── URP-Balanced.asset
        │   └── URP-Balanced-Renderer.asset
        │
        ├── HighFidelity/
        │   ├── URP-HighFidelity.asset
        │   └── URP-HighFidelity-Renderer.asset
        │
        └── Performant/
            ├── URP-Performant.asset
            └── URP-Performant-Renderer.asset
```

---

# 8. Volume Profile 规范

项目通用 Volume Profile：

```text
Settings/
└── Rendering/
    └── Volume/
```

例如：

```text
Volume/
├── GlobalDefault.asset
├── Gameplay.asset
└── Cinematic.asset
```

如果 Volume Profile **只属于某一个关卡**，则跟随关卡：

```text
Scenes/
└── Levels/
    └── Forest/
        ├── Forest.unity
        └── Profiles/
            └── ForestVolume.asset
```

原则：

```text
全局共享 Volume
        ↓
Settings/Rendering/Volume/

关卡专属 Volume
        ↓
Scenes/Levels/<Level>/Profiles/
```

---

# 9. Shader 规范

跨 Feature 使用的 Shader 统一放：

```text
Shaders/
```

推荐：

```text
Shaders/
├── Common/
├── Character/
├── Environment/
├── PostProcessing/
├── Compute/
└── Includes/
```

其中：

```text
.shader        Shader
.shadergraph   Shader Graph
.compute       Compute Shader
.hlsl          公共 HLSL
```

例如：

```text
Shaders/
├── Character/
│   └── CharacterToon.shader
│
├── Environment/
│   ├── Grass.shader
│   └── Water.shader
│
├── PostProcessing/
│   └── Outline.shader
│
├── Compute/
│   ├── GPUCulling.compute
│   └── GPUAnimation.compute
│
└── Includes/
    ├── Common.hlsl
    └── Lighting.hlsl
```

如果 Shader 完全属于某个独立 Feature，也可以跟随 Feature：

```text
VFX/
└── Dissolve/
    ├── Dissolve.shader
    ├── Dissolve.mat
    └── Dissolve.prefab
```

---

# 10. ScriptableObject / Data 规范

Feature 自己的数据配置放在：

```text
<Feature>/Data/
```

例如：

```text
Weapons/
└── Data/
    ├── RifleConfig.asset
    └── PistolConfig.asset
```

对应代码：

```text
Weapons/
└── Scripts/
    └── WeaponConfig.cs
```

跨系统的全局配置才放：

```text
Core/Data/
```

---

# 11. Editor 代码规范

Editor Only 代码必须与 Runtime 代码分离。

Feature 专属 Editor：

```text
Player/
└── Scripts/
    ├── Runtime/
    └── Editor/
```

项目级 Editor Tool：

```text
_Project/
└── Editor/
```

例如：

```text
Editor/
├── Tools/
├── Inspectors/
└── Windows/
```

禁止让 Runtime Assembly 依赖 Editor Assembly。

---

# 12. 第三方资源规范

Asset Store、GitHub 或其他来源导入的第三方资源统一放：

```text
Assets/ThirdParty/
```

例如：

```text
ThirdParty/
├── DOTween/
├── Odin/
└── SomePlugin/
```

原则：

**尽量不要修改第三方插件内部文件。**

如果需要针对第三方插件编写项目自己的适配代码：

```text
_Project/
└── Integrations/
    └── SomePlugin/
```

而不是直接修改：

```text
ThirdParty/SomePlugin/
```

这样方便未来升级或删除插件。

> 如果第三方插件自身要求固定安装目录，则遵循插件官方目录要求，不强行移动。

---

# 13. Packages 规范

通过 Unity Package Manager 安装的 Package 保持在：

```text
Packages/
```

例如：

```text
Cinemachine
Input System
Burst
Collections
Mathematics
URP
Shader Graph
```

不要手动复制到：

```text
Assets/_Project/
```

项目自己的代码通过正常程序集引用使用这些 Package。

---

# 14. Resources 目录规范

非必要情况下不要使用：

```text
Resources/
```

因为 `Resources` 内资源会受到 Unity 特殊的打包和加载规则影响。

大型项目优先考虑：

```text
直接序列化引用
Addressables
AssetBundle
```

只有明确知道为什么需要 `Resources.Load()` 时才建立 `Resources`。

---

# 15. 命名规范

## 文件夹

使用 PascalCase：

```text
Player
Environment
PostProcessing
ThirdParty
```

避免：

```text
player
PLAYER
post_processing
New Folder
新建文件夹
```

---

## C# Script

```text
PlayerController.cs
PlayerMovement.cs
PlayerLook.cs
WeaponController.cs
```

文件名与主要 Class 名保持一致。

---

## Prefab

推荐使用清晰的对象名称：

```text
Player.prefab
CameraRig.prefab
Rifle.prefab
Enemy_Soldier.prefab
```

---

## Material

可以使用统一前缀：

```text
M_Player
M_Rifle
M_Grass
```

Material Instance / Variant 可根据项目约定：

```text
MI_Player_Red
MI_Player_Blue
```

---

## Texture

推荐：

```text
T_Player_BaseColor
T_Player_Normal
T_Player_Mask
T_Player_Emission
```

例如：

```text
T_Rock_BaseColor
T_Rock_Normal
T_Rock_Mask
```

---

## Mesh

推荐：

```text
SM_Rock
SM_Rifle
SM_Tree
```

角色 Skeletal Mesh / 模型资源根据 DCC 与项目约定统一命名。

---

## Animation

推荐：

```text
A_Player_Idle
A_Player_Run
A_Player_Jump
A_Player_Attack
```

---

## ScriptableObject

推荐：

```text
PlayerConfig
RifleConfig
EnemyConfig
```

如果项目资源数量较大，也可以使用：

```text
DA_PlayerConfig
DA_RifleConfig
```

---

# 16. Assembly Definition 规范

项目发展到一定规模后，使用 `.asmdef` 划分程序集。

例如：

```text
Project.Core
Project.Player
Project.Camera
Project.Combat
Project.Interaction
Project.UI
```

依赖关系尽量保持单向：

```text
              Core
               ↑
        ┌──────┼──────┐
        │      │      │
      Player Camera Combat
        │             │
        └──────┬──────┘
               ↓
               UI
```

避免：

```text
Player → Combat
Combat → Player
```

这种循环依赖。

---

# 17. Git / 版本控制规范

必须提交：

```text
Assets/
Packages/
ProjectSettings/
```

通常不提交：

```text
Library/
Temp/
Logs/
Obj/
Build/
Builds/
UserSettings/
```

Unity 的：

```text
.meta
```

文件必须进入版本控制。

不要手动删除 `.meta` 后重新生成，否则可能导致 Unity Asset GUID 改变、Prefab 引用丢失。

---

# 18. 当前项目初始结构

项目早期建议保持简单：

```text
Assets/
├── _Project/
│   │
│   ├── Core/
│   │
│   ├── Player/
│   │   ├── Scripts/
│   │   └── Prefabs/
│   │       └── Player.prefab
│   │
│   ├── Camera/
│   │   ├── Scripts/
│   │   └── Prefabs/
│   │
│   ├── Scenes/
│   │   └── Test/
│   │
│   └── Settings/
│       └── Rendering/
│           ├── URP/
│           └── Volume/
│
└── ThirdParty/
```

随着项目开发，再逐渐增加：

```text
Interaction/
Weapons/
Combat/
AI/
UI/
Environment/
Audio/
VFX/
Shaders/
```
