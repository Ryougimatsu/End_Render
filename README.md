# EndField_Render - UE5 Advanced Cel-Shading Character Rendering

![截图1](./Docs/Chen_01.png)
![截图2](./Docs/Chen_02.png)

这是一个基于 **Unreal Engine 5** 开发的二次元卡通渲染（Cel-shading / Anime Style）技术展示项目。

本项目致力于探索如何在实时渲染环境下，突破传统 PBR 的限制，构建一套高度模块化、可精细控制的角色材质管线。通过定制化的着色器逻辑，项目在保持极高视觉表现力的同时，兼顾了 ARPG 等高频动作游戏的性能需求与管线热重载效率。

---

## 🚀 核心渲染技术 (Rendering Features)

### 1. 模块化角色材质系统 (Modular Character Shading Pipeline)
项目摒弃了臃肿的单一下集材质，采用高度解耦的父子材质架构（Master Material & Instances）。针对角色的不同物理部位，分别开发了专属的着色逻辑：
* **`M_Common_Face` (面部精细着色)**：为了解决二次元面部阴影的“死亡角度”问题，引入了定制的面部阴影贴图与法线权重控制。内置了独立的 `Nose_HighLight`（鼻尖高光）与 `Nose_Matcap`（唇部/局部材质捕捉）偏移参数，确保面部特征在任何光照下都立体且干净。
* **`M_Common_Hair` (各向异性头发高光)**：实现了一套基于 Kajiya-Kay 模型变种的卡通头发渲染方案（`KK_Spe`）。支持自定义 Ramp 贴图（色阶映射）与各向异性偏移（Shift UV），完美还原了动漫中标志性的“天使环”高光动态效果。
* **`M_Common_Cloth` (PBR与NPR的混合织物)**：在衣物与硬表面材质上，创新性地混合了 GGX 物理高光与 Matcap（材质捕捉）。通过 `Cloth_Metallic` 与 `Cloth_Rim` 的双向控制，既保留了金属/皮革的物理质感，又维持了整体赛璐璐风格的一致性。
* **`M_Common_Eye` & `EyeShadow` (眼部特写优化)**：采用多层遮罩（EyeMask）独立控制瞳孔高光与底层自发光强度，并配备了独立的眼影/头发阴影（HairShadow）材质，避免阴影穿模脏乱。

### 2. 动态边缘与高光控制 (Advanced Edge & Rim Light)
* **独立法线混合引擎**：在各个部位的材质中，开放了 `CameraNormal_Strength`（摄像机空间法线强度）与 `LightNormal_Strength`（光照空间法线强度）的独立滑块，赋予美术极大的光影微调自由度。
* **自适应边缘光 (Rim Light)**：通过 `Rim_Offset` 与 `Rim_Smooth` 参数，边缘光可根据角色与光源的夹角实现平滑过渡，并在衣物材质中支持自定义的高光法线模式（`Rim_HighLight_Normal_Mode`），增强受光面的体积感。

### 3. 高效的工业级工作流 (Production-Ready Workflow)
* **参数化全局控制**：所有核心视觉元素（如 AO 颜色、基础色偏、高光强度）均已暴露为材质实例（MI）参数。
* **性能友好**：材质层级清晰，舍弃了昂贵的实时全局光追阴影，转而使用优化的阈值步进（Step）和 Ramp 映射来计算明暗交界线（Terminator），非常适合高帧率游戏场景。

---

## 📸 材质管线展示 (Material Architecture)
| 头发高光控制 (Hair Shading / KK_Spe) | 面部细节调控 (Face & Nose Specular) |
| :---: | :---: |
| ![Hair_KK](./Docs/Hair_KK.png) | ![Face](./Docs/Face.png) |
| ![Hair_Matcap](./Docs/Hair_Matcap.png)|
| 服装质感混合 (Cloth NPR + PBR) | 皮肤与环境光遮蔽 (Skin & AO) |
| ![Cloth_01](./Docs/Cloth_01.png) | ![Skin](./Docs/Skin.png) |
| ![Cloth_02](./Docs/Cloth_01.png) |


---

---

##材质实例展示
| 头发 | 面部 |
| :---: | :---: |
| ![Hair](./Docs/MI_Chen_Hair.png) | ![Face](./Docs/MI_Chen_Face.png) |
| 服装质感 | 皮肤 |
| ![Cloth](./Docs/MI_Chen_Cloth.png) | ![Skin](./Docs/MI_Body_Skin.png) |
---

## 🛠️ 开发环境 (Environment)

* **引擎版本**：Unreal Engine 5.7.4
* **渲染管线**：Deferred Rendering (延期渲染) + Custom Post-Process
* **核心架构**：基于 Material Instance 的纯蓝图/资产驱动流

---

## 📦 本地部署指南 (Setup Instructions)

本项目包含高精度的模型与贴图资产，已启用 **Git LFS (Large File Storage)**。

1. 确保本地已正确安装并初始化 Git LFS：
   ```bash
   git lfs install
# EndField_Render - UE5 Advanced Cel-Shading Character Rendering

![Screenshot 1](./Docs/Chen_01.png)
![Screenshot 2](./Docs/Chen_02.png)

This is a technical showcase project featuring anime-style cel-shading developed in **Unreal Engine 5**. 

This project explores how to break the limitations of traditional PBR in a real-time rendering environment to build a highly modular and precisely controllable character material pipeline. Through customized shader logic, the project maintains extremely high visual fidelity while accommodating the performance demands of high-action games like ARPGs and ensuring efficient pipeline hot-reloading.

---

## 🚀 Rendering Features

### 1. Modular Character Shading Pipeline
The project discards bloated all-in-one master materials in favor of a highly decoupled Master Material & Instances architecture. Custom shading logic was developed for different physical parts of the character:
* **`M_Common_Face` (Advanced Face Shading)**: To solve the "dead angle" issue of anime-style facial shadows, customized face shadow maps and normal weight controls were introduced. Built-in `Nose_HighLight` and `Nose_Matcap` (lip/local matcap) offset parameters ensure facial features remain three-dimensional and clean under any lighting condition.
* **`M_Common_Hair` (Anisotropic Hair Highlight)**: Implemented a cartoon hair rendering solution (`KK_Spe`) based on a variation of the Kajiya-Kay model. It supports custom Ramp maps (color step mapping) and anisotropic offsets (Shift UV), perfectly recreating the iconic "angel ring" dynamic highlight effect seen in anime.
* **`M_Common_Cloth` (NPR & PBR Hybrid Fabric)**: For clothing and hard-surface materials, it innovatively blends GGX physical specular with Matcap. Through the dual control of `Cloth_Metallic` and `Cloth_Rim`, it retains the physical texture of metal/leather while maintaining the consistency of the overall cel-shaded style.
* **`M_Common_Eye` & `EyeShadow` (Eye Close-up Optimization)**: Utilizes a multi-layer mask (EyeMask) to independently control pupil highlights and base emission intensity, equipped with a separate EyeShadow/HairShadow material to prevent messy shadow clipping.

### 2. Advanced Edge & Rim Light Control
* **Independent Normal Blending Engine**: Independent sliders for `CameraNormal_Strength` and `LightNormal_Strength` are exposed in each part's material, granting artists maximum freedom to fine-tune lighting and shadows.
* **Adaptive Rim Light**: Through `Rim_Offset` and `Rim_Smooth` parameters, the rim light smoothly transitions based on the angle between the character and the light source. The clothing material also supports a custom highlight normal mode (`Rim_HighLight_Normal_Mode`) to enhance the volumetric feel of lit surfaces.

### 3. Production-Ready Workflow
* **Parametrized Global Control**: All core visual elements (e.g., AO color, base color tint, specular intensity) are exposed as Material Instance (MI) parameters.
* **Performance Friendly**: The material hierarchy is clean, discarding expensive real-time ray-traced shadows in favor of optimized Step functions and Ramp mapping to calculate terminators, making it highly optimized for high-framerate game scenes.

---

## 📸 Material Architecture

| Hair Shading / KK_Spe | Face & Nose Specular |
| :---: | :---: |
| ![Hair_KK](./Docs/Hair_KK.png) | ![Face](./Docs/Face.png) |
| ![Hair_Matcap](./Docs/Hair_Matcap.png)| |

| Cloth NPR + PBR | Skin & AO |
| :---: | :---: |
| ![Cloth_01](./Docs/Cloth_01.png) | ![Skin](./Docs/Skin.png) |
| ![Cloth_02](./Docs/Cloth_01.png) | |

---

## 🎨 Material Instances Showcase

| Hair | Face |
| :---: | :---: |
| ![Hair](./Docs/MI_Chen_Hair.png) | ![Face](./Docs/MI_Chen_Face.png) |
| **Cloth** | **Skin** |
| ![Cloth](./Docs/MI_Chen_Cloth.png) | ![Skin](./Docs/MI_Body_Skin.png) |

---

## 🛠️ Environment

* **Engine Version**: Unreal Engine 5.7.4
* **Rendering Pipeline**: Deferred Rendering + Custom Post-Process
* **Core Architecture**: Material Instance-based pure Blueprint/Asset-driven workflow

---

## 📦 Setup Instructions

This project contains high-fidelity models and texture assets and has **Git LFS (Large File Storage)** enabled.

1. Ensure Git LFS is properly installed and initialized locally:
   ```bash
   git lfs install
