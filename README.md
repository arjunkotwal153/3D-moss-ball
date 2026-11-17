<div align="center">

# 🌿 iMoss — Interactive 3D WebGL Moss Ball

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=22&duration=3000&pause=1000&color=7EC820&center=true&vCenter=true&width=650&lines=500%2C000+Instanced+Moss+Strands+in+Real-Time;Procedural+Flow-Field+Clumping+%26+Physics;Cinematic+GLSL+Wrap-Around+Subsurface+Shaders;Engineered+by+Kamlesh+Singh+Kotwal" alt="Typing SVG" />
</p>

[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC_BY--NC_4.0-7ec820.svg?style=for-the-badge&logo=creative-commons&logoColor=white)](LICENSE)
[![Svelte 5](https://img.shields.io/badge/Svelte_5-FF3E00?style=for-the-badge&logo=svelte&logoColor=white)](https://svelte.dev)
[![Three.js](https://img.shields.io/badge/Three.js-000000?style=for-the-badge&logo=threedotjs&logoColor=white)](https://threejs.org)
[![Vite 6](https://img.shields.io/badge/Vite_6-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev)
[![WebGL 2.0](https://img.shields.io/badge/WebGL_2.0-990000?style=for-the-badge&logo=webgl&logoColor=white)](https://www.khronos.org/webgl/)

---

### 👨‍💻 Engineered & Crafted with Passion by
## **[Kamlesh Singh Kotwal](https://github.com/)**
*Creative Technologist & Graphics Software Engineer*

<p align="center">
  <a href="https://github.com/"><img src="https://img.shields.io/badge/GitHub-Profile-181717?style=flat-square&logo=github&logoColor=white" /></a>
  <a href="https://linkedin.com/"><img src="https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin&logoColor=white" /></a>
  <a href="mailto:contact@kamleshsingh.dev"><img src="https://img.shields.io/badge/Email-Get_in_Touch-D14836?style=flat-square&logo=gmail&logoColor=white" /></a>
  <a href="https://imoss.bio"><img src="https://img.shields.io/badge/Live_Demo-Experience_Now-4CAF50?style=flat-square&logo=google-chrome&logoColor=white" /></a>
</p>

</div>

---

## 🔮 Visual Showcase

<div align="center">
  <img src="https://github.com/user-attachments/assets/42fe0a2d-f9b9-4d5a-81ad-4a632b12baf5" width="85%" style="border-radius: 12px; box-shadow: 0 10px 30px rgba(0,0,0,0.8);" alt="iMoss Live Preview" />
</div>

> **iMoss** is an ultra-realistic, interactive 3D WebGL simulation of a living Marimo moss ball. It runs at a fluid 60 FPS in real-time right in your browser, orchestrating **over half a million (500,000) dynamic instanced moss blades** with physical drag, rotational inertia, organic wind fields, and subsurface scattering light physics.

---

## ⚡ Tech Stack & 3D Ecosystem

<div align="center">

| Technology | Role | Highlights |
|:---:|:---|:---|
| <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/svelte/svelte-original.svg" width="40" height="40" /><br>**Svelte 5** | Core UI & State | Modern Runes, ultra-lightweight reactive DOM binding |
| <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/threejs/threejs-original.svg" width="40" height="40" /><br>**Three.js** | 3D Scene Engine | Scene graph, camera rigging, GPU buffer management |
| <img src="https://threlte.xyz/favicons/favicon-32x32.png" width="40" height="40" /><br>**Threlte 8** | Svelte-Three Bridge | Declarative WebGL lifecycle with zero abstraction overhead |
| <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/opengl/opengl-original.svg" width="40" height="40" /><br>**Custom GLSL** | GPU Shader Pipeline | Vertex blade bending, wrap-around lighting & subsurface glow |
| <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/vitejs/vitejs-original.svg" width="40" height="40" /><br>**Vite 6** | Build & Dev Tooling | Sub-second HMR, optimized raw shader asset bundling |

</div>

---

## 🚀 Quick Start Guide

Run the simulation on your local machine in under a minute:

```bash
# 1. Clone the repository
git clone https://github.com/your-username/imoss-mossball.git

# 2. Enter directory
cd imoss-mossball

# 3. Install dependencies
npm install

# 4. Launch the local development server
npm run dev
```

Visit [`http://localhost:5173`](http://localhost:5173) in your browser. Click, drag, and fling the moss ball to interact with the physics!

---

## 🧠 Deep Dive: How I Made It (Engineering Journey)

<details open>
<summary><h3>📖 1. The Architectural Vision</h3></summary>

The ambition was clear: **bridge the gap between offline Blender ray-traced grooming simulations and real-time 60 FPS browser WebGL.**

Most web-based hair or foliage demos render only a few thousand strands or rely on flat billboard sprites with alpha transparency maps, causing harsh sorting artifacts and flat illumination. I wanted **real 3D volumetric geometry** for each blade of moss while maintaining smooth real-time interactivity.

To accomplish this, I engineered a high-throughput pipeline combining:
1. **Procedural instanced 3D blade geometry** (6-segment tapered buffer geometry).
2. **GPU-driven vertex displacement** that enforces length preservation.
3. **Dual-tier physics:** CPU calculates macro momentum, spring recoil, and rotational torque; GPU computes per-vertex micro aerodynamic flutter, wind, and flow-field clumping.
</details>

<details>
<summary><h3>🧬 2. Procedural Sphere Distribution & Flow-Field Clumping</h3></summary>

Real Marimo moss does not grow in uniform spikes like a hedgehog; it forms sweeping, organic curved clumps ("locks") across its surface.

#### Fibonacci Sphere Placement
Blades are distributed procedurally across a sphere of radius $R = 3$ using spherical coordinates:
$$\theta = 2\pi \cdot \text{rand}(), \quad \phi = \arccos(2 \cdot \text{rand}() - 1)$$
$$x = R \sin\phi \cos\theta, \quad y = R \sin\phi \sin\theta, \quad z = R \cos\phi$$

#### 3D Simplex Flow Fields
To generate natural tufts and locks, I used multiple octaves of 3D trigonometric noise:
- **Primary Flow Direction**: Lower frequency noise evaluates wide sweeping curves across the sphere:
  $$\text{angle}_{\text{flow}} = \text{Noise3D}(0.22\mathbf{p}) \cdot 2\pi$$
- **Nested Detail Noise**: High-frequency noise creates nested micro-curves within larger tufts:
  $$\text{angle}_{\text{detail}} = \text{Noise3D}(0.9\mathbf{p}) \cdot 2\pi$$
- **Equator Bias & Outliers**: Strands at the equator are scaled taller ($H \times 1.4$) than the poles, and $0.1\%$ of strands are designated as "rogue stray strands" with $2.0 \times - 4.0 \times$ extra length and thinner girth.
</details>

<details>
<summary><h3>⚙️ 3. Dual-Tier Real-Time Physics Engine</h3></summary>

#### CPU: Macro Kinetics & Rotational Inertia
- **Spring-Damper Cursor Tracking**: Moving your cursor creates a gentle tracking force with exponential smoothing.
- **Nudge Impulse & Damped Recovery**: Clicking imparts an instantaneous directional impulse. A critically-damped spring system ($\zeta \approx 1.0$) smoothly returns the ball to the center with a realistic 3-second settle time.
- **Torque & Wheel-Rolling Simulation**: Off-center mouse gestures calculate torque around the contact normal:
  $$\vec{\tau} = \vec{r} \times \vec{F}$$
  The ball rolls like an authentic wheel on an axis dynamically updated in world space, and the rotational momentum naturally decays through simulated kinetic friction.

#### GPU: Length-Preserving Vertex Shader
When blades bend under velocity or wind, simple linear offsets would cause them to stretch or compress unnaturally. I implemented a **length-preserving trigonometric projection** in `mossball.vert`:

```glsl
// Calculate original blade segment length
float origLenSq = pos.x * pos.x + pos.y * pos.y + pos.z * pos.z;

// Apply non-linear quadratic bend factor + wind + clumping
float dx = (aBendState.x + windX + clumpX) * pow(t, 1.8);
float dz = (aBendState.y + windZ + clumpZ) * pow(t, 1.8);

float newX = pos.x + dx;
float newZ = pos.z + dz;
float newLenSq = newX * newX + pos.y * pos.y + newZ * newZ;

// Re-normalize segment to preserve physical strand length
float lenScale = sqrt(origLenSq / max(newLenSq, 0.0001));
pos.x = newX * lenScale;
pos.y = pos.y * lenScale;
pos.z = newZ * lenScale;
```
</details>

<details>
<summary><h3>🎨 4. Cinematic Lighting & Subsurface Shader Artistry</h3></summary>

Moss in nature has a distinct velvety, translucent, light-scattering appearance. Standard Blinn-Phong or basic PBR looks like plastic. I engineered a dedicated custom Fragment Shader (`mossball.frag`):

- **Wrap-Around Diffuse Lighting**: Instead of clamped Lambertian shading ($\max(0, \mathbf{N}\cdot\mathbf{L})$), wrap-around diffuse spreads light softly past the terminator:
  $$L_{\text{diff}} = \frac{\mathbf{N} \cdot \mathbf{L} + 0.5}{1.5}$$
- **Root-to-Tip Chromatic Gradient**: Dark chlorophyll-dense base (`#020a01`) smoothly transitions to sunlight-activated lime tips (`#7ec820`).
- **Subsurface Glow & Amber Rim**: Backlighting penetrates the edges with a high-exponent Fresnel rim light:
  $$\text{Rim} = (1.0 - |\mathbf{N} \cdot \mathbf{V}|)^3$$
  Bathed in golden-amber back-fill, it replicates the natural translucence of micro-foliage.
- **Tone Mapping**: Configured with Three.js **AgX Tone Mapping** for cinematic highlight roll-off without color clipping.
</details>

<details>
<summary><h3>🚀 5. Performance Optimization: 500,000 Blades at 60 FPS</h3></summary>

Rendering 500,000 distinct geometries would normally melt any browser. Here's how I achieved solid 60 FPS:
1. **Single Instanced Mesh Draw Call**: All half-million blades share a single 6-segment base geometry via `THREE.InstancedMesh`.
2. **Custom Typed Float32Array Attribute Buffers**: Tangents, bitangents, base positions, bend states, and height multipliers are tightly packed in contiguous GPU memory.
3. **Zero Per-Frame Allocations**: No garbage-collected vectors or matrices are created during the render loop. Everything uses pre-allocated static registers.
4. **Frustum & Level-of-Detail Scalability**: Integrated `lil-gui` controls allow real-time throttling from 125,000 to 500,000 instances to adapt seamlessly across mobile GPUs up to high-end desktop rigs.
</details>

---

## 🎛️ Live Interactive Controls

An intuitive floating control panel powered by `lil-gui` gives you instant control over the simulation parameters:

| Control | Description | Default | Range |
|---|---|---|---|
| **Blade Count** | Number of instanced moss strands | `125,000` | `10,000` – `500,000` |
| **Strand Length** | Global height multiplier for moss blades | `1.2×` | `0.5×` – `3.0×` |
| **Clump Strength** | Degree of curvature and lock grouping | `0.4` | `0.0` – `1.5` |
| **Wind Amplitude** | Strength of ambient atmospheric breeze | `0.25` | `0.0` – `1.0` |
| **Wind Speed** | Frequency of wind oscillation wave | `1.2` | `0.1` – `5.0` |
| **Algae Shift** | Tip hue variation from lush emerald to golden sunlight | `#7ec820` | Full Color Picker |

---

## 📂 Project Structure

```
imoss-mossball/
├── src/
│   ├── components/
│   │   ├── MossBall.svelte        # Core WebGL instancing, physics & GUI controls
│   │   └── MossBallScene.svelte   # Threlte scene container, camera, tone mapping
│   ├── shaders/
│   │   ├── mossball.vert          # GLSL: Vertex bending, wind, clumping & taper
│   │   └── mossball.frag          # GLSL: Subsurface scattering, wrap light & fog
│   ├── App.svelte                 # Root application wrapper
│   └── main.js                    # Svelte 5 application entry mount
├── index.html                     # HTML5 canvas container & GUI anchor
├── vite.config.js                 # Vite bundler configuration with Svelte plugin
├── package.json                   # Project scripts and dependency declarations
└── README.md                      # Comprehensive project documentation
```

---

## 🌟 Show Your Support

If you enjoyed interacting with this simulation or found the shader math and physics breakdown inspiring:
- Star this repository ⭐️
- Share it with fellow creative developers 🚀
- Fork it and experiment with custom shader variations 🎨

---

## 📜 Attribution & License

- **Enhanced & Engineered by**: [Kamlesh Singh Kotwal](https://github.com/)
- **Original Concept & Base by**: [Hieu Le](https://leduchieu.com)

This project is licensed under the **Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0)**.  
You are free to share and adapt the material for non-commercial purposes with appropriate attribution. See the [LICENSE](LICENSE) file for complete details.

<div align="center">
  <sub>Built with Three.js, Svelte 5, Threlte & WebGL • Made with 💚 by <strong>Kamlesh Singh Kotwal</strong></sub>
</div>
