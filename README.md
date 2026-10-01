# COD

**A procedural, high-fidelity 3D tactical first-person shooter built entirely in the browser with Three.js r180 and WebGL2.**

Authored and maintained by [Delight Chukwu](https://github.com/delightc123/COD). Roughly 55,000 lines across 11 core subsystems.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Three.js](https://img.shields.io/badge/Three.js-r180-black.svg)](https://threejs.org)
[![Vite](https://img.shields.io/badge/Vite-7.x-646CFF.svg)](https://vitejs.dev)

---

## Overview

**Zero external art assets.** Every texture, procedural normal map, 3D mesh, animation rig, weapon model, and sound effect is generated in real-time from code at load time. There are no imported 3D models, no static HDRIs, no image files, and no audio samples. The runtime relies solely on `three`.

---

## Quick Start

```bash
# Clone the repository
git clone https://github.com/delightc123/COD.git
cd COD

# Install dependencies
npm install

# Run the development server
npm run dev
```

Open `http://localhost:5173` in any WebGL2-compatible browser (Chrome, Edge, Brave, Firefox, Safari).

### Controls

| Action | Key / Input |
| :--- | :--- |
| **Move** | `W` `A` `S` `D` |
| **Aim / Look** | Mouse |
| **Fire** | Left Mouse Button (LMB) |
| **Aim Down Sights (ADS)** | Right Mouse Button (RMB) |
| **Sprint** | `Shift` |
| **Crouch** | `Ctrl` |
| **Jump** | `Space` |
| **Lean Left / Right** | `Q` / `E` |
| **Reload** | `R` |
| **Lock / Unlock Cursor** | Click canvas to lock / `Esc` to release |

---

## Architecture & Subsystems

| Subsystem | Location | Description |
| :--- | :--- | :--- |
| **Rendering** | `src/render/` | HDR rendering pipeline, cascaded shadow maps (CSM) in `sampler2DArray` with texel snapping, PCSS contact hardening, MRT depth/normal/velocity prepass, GTAO, TAA with YCoCg variance clipping, tile-dilated motion blur, Karis bloom pyramid, GPU EV100 metering, and AgX composite. |
| **Materials** | `src/materials/` | GPU procedural texture engine: 19 procedural surface shaders (concrete, brick, plaster, asphalt, rusted/brushed metals, fabrics), periodic noise tiling, Sobel height-to-normal, parallax occlusion mapping, and curvature edge wear. |
| **Sky & Atmosphere** | `src/sky/` | Real-time atmospheric Rayleigh/Mie scattering, dynamic time of day, PMREM environment maps, and volumetric fog with light shafts. |
| **World & Environment** | `src/world/` | 120×120m market street with modular buildings, real wall thickness, fully enterable interiors, and hundreds of instanced props. |
| **Physics** | `src/physics/` | Custom physics engine written from scratch. Binned-SAH BVH (29k triangles into 14k nodes in 22ms), swept-capsule character controller with 5-plane crease solver, rigid bodies with CCD, PBD ragdolls, and multi-layer bullet penetration. |
| **Player Controller** | `src/player/` | Movement state machine, slide, mantle, lean, weapon inertia, and camera springs. |
| **Weapons & Ballistics** | `src/weapons/` | Procedural weapon geometry (pistol, SMG, rifle), viewmodel animation rig, ADS alignment, spring recoil, procedural reloads, and ballistics simulation with travel time and bullet drop. |
| **FX** | `src/fx/` | GPU particles, bullet decals, impact effects, muzzle flash, and explosions. |
| **AI Combatants** | `src/ai/` | Skinned soldiers, navmesh pathfinding, sensory perception, cover behavior, and dynamic ragdoll transitions. |
| **UI HUD** | `src/ui/` | Dynamic DOM/CSS HUD: tactical crosshair, hitmarkers, minimap, compass, and killfeed. |
| **Audio** | `src/audio/` | Web Audio synthesis engine — layered gunfire synthesis, convolution reverb, HRTF spatialization, and distance occlusion. |

---

## Developer Tooling & Harness

The repository includes a suite of headless verification and performance profiling tools in `tools/`:

- `tools/capture.mjs`: Captures individual high-precision camera shots via headless GPU-backed Chromium.
- `tools/shotset.mjs`: Runs and reviews all 11 evaluation shots in a single pass.
- `tools/baseline.mjs`: Isolates each shot in a fresh browser context for bit-identical deterministic regression testing.
- `tools/imagediff.mjs`: Per-pixel regression gate for visual fidelity.
- `tools/profile.mjs`: Frame-time distribution profiling at native device pixel ratio, reporting p50, p95, and p99 metrics.

---

## Contributing

Contributions and pull requests are welcome! Please open an issue first to discuss substantial architectural changes.

---

## License

[MIT](LICENSE) © 2026 Delight Chukwu
