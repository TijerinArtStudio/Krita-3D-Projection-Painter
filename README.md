# Krita-3D-Projection-Painter
# Krita 3D Mesh Painter — Official Plugin Documentation & Installation Guide

**Plugin Name:** Krita 3D Mesh Painter  
**Package ID:** `3d_projection_painter`  
**Author & Creator:** Created by **José Tijerín** (`tijegoblinight`)  
**Official Page:** [https://tijerinart.itch.io/krita-3d-projection-painter](https://tijerinart.itch.io/krita-3d-projection-painter)  
**Creation Date:** September 26, 2026  
**Compatibility:** Krita 5.x+ (Windows, Linux, macOS) — Python 3 / PyQt5 / OpenGL 3.3 Core & ES 3.0  

---

## 1. Overview

**Krita 3D Mesh Painter** is the first professional 3D projection and texture painting plugin natively integrated inside **Krita**. It embeds an always-active, hardware-accelerated 3D Unlit Orthographic Viewport directly inside Krita as a dockable panel (`DockWidget`), allowing game texture artists and digital painters to:
- View and orbit 3D models (`.obj` and `.fbx`) directly inside Krita (starting automatically with the built-in **Logo Krita 3D Painter** 3D emblem and texture).
- Use the classic **Game Artist Direct Base Texture Workflow** via two dedicated buttons with Lucide icons: **`Extract Tex` (`file-up`)** (extracts the model's base texture to Krita at its exact 1:1 native pixel resolution `W×H` with an optional `UV_Guide_<Material>` wireframe layer) and **`Import Tex` (`file-down`)** (re-imports the edited texture directly into the model's base texture, replacing the original texture without creating a projection layer).
- Paint and erase in 3D with full **digital stylus tablet pressure sensitivity (`QTabletEvent`)** controlling both radius and opacity/flow.
- Use the compact **3D Eraser (`eraser`)** right next to the **3D Brush (`paintbrush`)** at any time—even when no overlay image is active—to cleanly erase projected layers with soft/hard alpha and reveal the original base texture underneath in real time (never painting gray).
- Switch to **Object-Centered Cylindrical Projection Mapping & Cylindrical Texture Unrolling (`Cylindrical` / `cylinder`)**, where the cylinder is centered at the world origin `(0, 0, 0)` and its height and diameter are dynamically controlled by the camera's position and zoom.
- Reproject 2D Krita artwork or direct 3D brush strokes onto the model's 4K UV texture maps using GPU Framebuffer Objects (FBO) with front-face angle culling, symmetry, and UV seam bleeding (`Seams`).

---

## 2. How to Install in Krita

### Step 1: Locate Your Krita `pykrita` Directory
Depending on your operating system, open your Krita resource directory and navigate to the `pykrita` folder:
- **Windows:** `%APPDATA%\krita\pykrita`  
  *(Typically `C:\Users\<YourUsername>\AppData\Roaming\krita\pykrita`)*
- **Linux:** `~/.local/share/krita/pykrita`  
  *(Flatpak: `~/.var/app/org.kde.krita/data/krita/pykrita`)*
- **macOS:** `~/Library/Application Support/Krita/pykrita`

> **Tip:** Inside Krita, you can always find this folder by going to **Settings → Manage Resources → Open Resource Folder**, then opening the `pykrita` subfolder.

### Step 2: Copy the Plugin Files
Copy both the `3d_projection_painter` folder and the `3d_projection_painter.desktop` descriptor file into `pykrita/`:

```text
pykrita/
├── 3d_projection_painter.desktop
└── 3d_projection_painter/
    ├── __init__.py
    ├── baker.py
    ├── bridge.py
    ├── docker.py
    ├── extension.py
    ├── fbx_loader.py
    ├── gl_driver.py
    ├── glsl.py
    ├── obj_loader.py
    ├── seam_fixer.py
    ├── viewport.py
    ├── Logo Krita 3D Painter.fbx
    ├── Logo Krita 3D Painter_Texture.png
    └── README.md
```

### Step 3: Enable the Plugin in Krita
1. Launch **Krita**.
2. Open **Settings → Configure Krita...** from the top menu bar.
3. Select **Python Plugin Manager** in the left sidebar.
4. Locate **Krita 3D Mesh Painter** in the list and check the box next to it.
5. Click **OK** and **restart Krita**.

---

## 3. How to Place & Dock Inside Your Krita Workspace

1. After restarting Krita, create or open any document.
2. Go to the top menu: **Settings → Dockers → Krita 3D Mesh Painter** and enable it.
3. The **Krita 3D Mesh Painter** panel will appear immediately with the 3D viewport active and displaying the default **Logo Krita 3D Painter** 3D model and texture.

---

## 4. Tools, Lucide Icons & Features Reference

### Row 1: Model Management, Export, Unpainted, Info & Home
- **`New 3D` (`square-arrow-down`):** Loads a custom `.obj` or `.fbx` 3D mesh replacing the current model.
- **`Frame` (`scan-box`):** Resets the orthographic orbital camera to frame and center the active 3D mesh.
- **`Reset` (`refresh-cw`):** Restores the default **Logo Krita 3D Painter** 3D model and texture.
- **`Export` (`square-arrow-up`):** Exports the composited diffuse UV texture map to `.png`, `.tga`, or `.jpg`.
- **`Unpainted` (`broom-sparkles`):** Placed right next to **`Export`**. Illuminates unpainted regions on the 3D model with a soft pulsing blue wave.
- **`Info` (`info`):** Opens a comprehensive window in English detailing all keyboard/mouse/stylus shortcuts, the functionality of every button in the plugin, and creator credits (**José Tijerín**).
- **`Home` (`link`):** Placed right next to **`Info`**. Opens the official project page at `https://tijerinart.itch.io/krita-3d-projection-painter`.

### Row 2: Materials, Real-Size Base Texture Extraction/Import, Viewport Modes & History
- **Material Selector (`QComboBox`):** Switches between `usemtl` materials.
- **`Extract Tex` (`file-up`):** Extracts the active material's original base texture at its true pixel dimensions (`W×H`) directly into Krita (`Base_Texture_<Material>`) along with a non-intrusive `UV_Guide_<Material>` wireframe reference layer.
- **`Import Tex` (`file-down`):** Re-imports the edited texture from the active Krita canvas (or `Shift+Click` to load from an image file on disk) and replaces the 3D model's original base texture directly without placing it into a projection layer.
- **`3D` (`scan-box`) / `2D` (`fullscreen`):** Toggles between the 3D Viewport (`scan-box`) and the 2D UV Unwrap Inspector (`fullscreen`).
- **`Symmetry` (`rotate-ccw-clock`):** Mirrors 3D brush strokes and 2D projections across the X axis.
- **`Mesh` (`grid-3x3`):** Renders the 3D triangle wireframe over the solid unlit mesh.
- **`Compare` (`scan-box`):** Toggles between the original base texture and the composited projection layers for instant comparison.
- **Compact Undo / Redo (`undo-2` / `redo-2`):** Minimal-width buttons for multi-step history supporting brush strokes, projections, erasures, and base texture replacements.

### Row 3: Capture, Set Image, Resolution Selector, Compact Brush/Eraser & Cylindrical Projection
- **`Capture` (`camera`):** Captures either the planar orthographic view or the 360° unrolled cylindrical panorama (when **`Cylindrical`** is active) into Krita (`3D_Reference` and `3D_Paint` layers).
- **`Set Image` (`square-arrow-out-down-right`):** Placed directly next to **`Capture`**. Loads the painted Krita canvas as a stencil overlay on the 3D viewport.
- **Resolution Selector (`QComboBox` next to `Set Image`):** Sets the target resolution (`2048 × 2048 (2K Default)`, `4096 × 4096 (4K Ultra)`, `1024 × 1024 (1K Fast)`, `512 × 512 (Low)`, or `3D Viewport Size`).
- **Compact Brush (`paintbrush`) & Eraser (`eraser`):** Minimal-width buttons placed side by side. Support full digital stylus pressure sensitivity (`QTabletEvent`) for both size and opacity.
- **`Project` (`projector`):** Projects the Krita canvas directly onto the 3D model using either Planar or Object-Centered Cylindrical mapping.
- **`Cylindrical` (`cylinder`):** Enables a 3D cylinder centered at world origin `(0, 0, 0)` whose height and diameter are dynamically determined by the camera's position and zoom. Captures 360° cylindrical unrolls and reprojects them seamlessly onto the 3D mesh.
- **`Only Empty` (`circle-x`) & `Select Empty` (`circle-plus`):** Restricts projections strictly to unpainted areas or auto-selects unpainted areas in Krita upon capture.
- **`Size`, `Hardness`, `Opacity`:** Controls brush/eraser radius (`1–1024px`), edge hardness (`0–100%`), and opacity (`0–100%`).

### Row 4: Projection Layers, Seams & Previous Camera Position
- **Projection Layer Selector (`QComboBox`):** Selects the active transparent UV projection layer (`Projection 1`, `Projection 2`, etc.).
- **Compact Layer Buttons (`plus`, `minus`, `eye` / `eye-off`):** Minimal-width buttons preceding **`Seams`** to add, delete/clear, or toggle visibility of UV projection layers.
- **`Seams` (`QCheckBox`):** Extends UV island border pixels (seam bleeding) to eliminate dark seam lines.
- **`Prev Cam` (`switch-camera`):** Restores the previous camera position using an internal 3-step camera history buffer.
- **Compact Expand Button (`maximize-2` / `minimize-2`):** Opens the 3D viewport in a large standalone window.

---

## 5. Credits & Authorship

- **Plugin Name:** Krita 3D Mesh Painter (`3d_projection_painter`)
- **Conceived & Created By:** **José Tijerín** (`tijegoblinight`)
- **Official Page:** [https://tijerinart.itch.io/krita-3d-projection-painter](https://tijerinart.itch.io/krita-3d-projection-painter)
- **Date of Creation:** September 26, 2026
- **License:** Open Source — Built for Krita 3D Texture Artists & Digital Painters

