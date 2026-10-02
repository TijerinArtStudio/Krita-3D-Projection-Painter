# Krita 3D Mesh Painter

A 3D projection and texture painting plugin for **Krita**, designed for game artists, texture artists, and digital painters who want to work directly on 3D models without leaving Krita.

**Krita 3D Mesh Painter** adds a hardware-accelerated 3D viewport directly inside Krita, allowing you to load 3D models, paint on them with a digital tablet, project 2D artwork onto UV textures, edit base textures, and export the resulting texture maps.

[**Buy / Get Krita 3D Mesh Painter**](https://tijerinart.itch.io/krita-3d-projection-painter)

---

## Features

* **Integrated 3D Viewport:** Work with a 3D model directly inside Krita through a dockable panel.
* **OBJ & FBX Support:** Load `.obj` and `.fbx` models directly into the plugin.
* **3D Painting:** Paint directly onto your model using Krita's canvas and your digital stylus.
* **Tablet Pressure Support:** Brush and eraser size and opacity respond to digital tablet pressure.
* **Base Texture Workflow:** Extract the original texture from a model, edit it in Krita, and import it back into the model.
* **Projection Painting:** Project 2D Krita artwork directly onto the 3D model.
* **Planar Projection:** Project artwork from the current orthographic camera view.
* **Cylindrical Projection:** Capture and project 360° cylindrical panoramas around the model.
* **UV Texture Baking:** Reproject artwork onto UV texture maps using GPU framebuffer rendering.
* **UV Seam Bleeding:** Extend texture pixels across UV borders to help prevent visible seam lines.
* **2D UV Inspector:** Switch between the 3D viewport and a 2D UV representation of the model.
* **Symmetry:** Mirror 3D brush strokes and projections across the X axis.
* **Unpainted Area Detection:** Highlight or automatically select areas that have not yet been painted.
* **Multiple Projection Layers:** Build up your texture using separate transparent projection layers.
* **Compare Mode:** Quickly compare the original base texture with the painted result.
* **Undo / Redo:** Multi-step history for painting, projection, erasing, and texture changes.
* **Texture Export:** Export the resulting diffuse texture as `.png`, `.tga`, or `.jpg`.
* **Reference Capture:** Capture the current 3D view or cylindrical panorama into Krita.
* **Dockable & Expandable:** Keep the viewport inside Krita or expand it into a larger standalone window.

---

## Screenshots & Video

Add screenshots and demonstration videos here as the project develops.

Recommended examples:

* Main 3D viewport
* 3D painting workflow
* Extract Texture / Import Texture workflow
* UV projection
* Cylindrical projection
* 2D UV Inspector
* Unpainted-area visualization

---

## Installation

There are several ways to install the plugin.

### Method 1 — Import the Plugin ZIP

This is the recommended method for most users.

1. Download the latest plugin package from the [**Krita 3D Mesh Painter itch.io page**](https://tijerinart.itch.io/krita-3d-projection-painter).
2. Open **Krita**.
3. Go to **Tools → Scripts → Import Python Plugin...**
4. Select the plugin `.zip` file.
5. Restart Krita.
6. Open **Settings → Configure Krita... → Python Plugin Manager**.
7. Enable **Krita 3D Mesh Painter**.
8. Restart Krita again if requested.

> Depending on your Krita version, the exact wording of the Python plugin importer may differ.

For the official Krita installation documentation, see the [**Krita Python Plugin Installation Guide**](https://docs.krita.org/en/user_manual/python_scripting/install_custom_python_plugin.html).

---

### Method 2 — Manual Installation

Krita Python plugins consist of a plugin directory and a `.desktop` descriptor. Both need to be placed inside Krita's `pykrita` resource directory.

The easiest way to find your resource folder is:

**Settings → Manage Resources → Open Resource Folder**

Then open the `pykrita` directory.

Typical locations are:

**Windows**

```text
%APPDATA%\krita\pykrita\
```

**Linux**

```text
~/.local/share/krita/pykrita/
```

**Linux Flatpak**

```text
~/.var/app/org.kde.krita/data/krita/pykrita/
```

**macOS**

```text
~/Library/Application Support/Krita/pykrita/
```

Copy the following into `pykrita/`:

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

Then:

1. Restart Krita.
2. Open **Settings → Configure Krita...**
3. Select **Python Plugin Manager**.
4. Enable **Krita 3D Mesh Painter**.
5. Restart Krita.

Krita's official documentation describes the same `.desktop` + plugin-folder structure for Python plugins. ([Krita Python Plugin Documentation](https://docs.krita.org/en/user_manual/python_scripting/krita_python_plugin_howto.html))

---

## Opening the Plugin

Once installed and enabled:

**Settings → Dockers → Krita 3D Mesh Painter**

The plugin opens as a dockable panel inside your Krita workspace.

A default **Logo Krita 3D Painter** model and texture are provided so you can immediately test the viewport and painting tools.

---

# Quick Start

### 1. Load a 3D Model

Click **New 3D** and select an `.obj` or `.fbx` model.

The plugin will load the mesh into the 3D viewport.

### 2. Frame the Model

Use **Frame** to automatically center the camera on the active mesh.

You can orbit around the model and adjust the camera position and zoom.

### 3. Paint Directly on the Model

Select the **3D Brush** and paint directly in the viewport.

Digital tablet pressure controls:

* Brush size
* Opacity / flow

The **3D Eraser** can be used at any time to remove projected paint and reveal the underlying texture.

### 4. Project a Krita Image

Use **Set Image** to load the current Krita artwork as a stencil overlay.

Choose the desired resolution and click **Project**.

The artwork will be projected onto the model and converted into UV texture data.

### 5. Export the Texture

When your texture is ready, use **Export** to save the resulting texture as:

* PNG
* TGA
* JPG

---

# Base Texture Workflow

Krita 3D Mesh Painter also supports direct editing of the original base texture.

### Extract Texture

Click **Extract Tex** to bring the active material's original texture into Krita at its native pixel resolution.

The plugin can also create a:

```text
UV_Guide_<Material>
```

wireframe layer to help with texture placement.

### Edit in Krita

Paint and edit the extracted texture using Krita's normal tools.

### Import Texture

Click **Import Tex** to replace the model's original base texture with the edited version.

This workflow does not require creating a projection layer.

---

# Projection Modes

## Planar Projection

The standard projection mode projects artwork from the current orthographic camera view.

This is useful for painting visible areas of a model from a controlled camera angle.

---

## Cylindrical Projection

Enable **Cylindrical** to work with a 360° cylindrical projection.

The projection cylinder is centered at:

```text
(0, 0, 0)
```

Its dimensions are dynamically controlled by the camera position and zoom.

This allows you to capture a cylindrical panorama and reproject it onto the model.

---

# 3D / 2D UV View

The **3D / 2D** toggle switches between:

### 3D View

The interactive 3D model and painting viewport.

### 2D UV View

A 2D representation of the model's UV texture layout.

This makes it easier to inspect how projected artwork is distributed across the texture.

---

# Texture Resolution

The resolution selector supports:

| Resolution           | Use                                  |
| -------------------- | ------------------------------------ |
| **512 × 512**        | Low resolution / quick testing       |
| **1024 × 1024**      | 1K                                   |
| **2048 × 2048**      | 2K — Default                         |
| **4096 × 4096**      | 4K — Ultra                           |
| **3D Viewport Size** | Uses the current viewport dimensions |

---

# Projection Layers

Multiple transparent projection layers can be created inside the plugin.

Use the layer controls to:

* Add projection layers
* Delete or clear layers
* Toggle layer visibility
* Select the active projection layer

This makes it possible to build up complex textures without immediately merging everything into the base texture.

---

# Seam Bleeding

Enable **Seams** to extend pixels around UV island borders.

This helps reduce dark or empty lines appearing along UV seams after texture projection and filtering.

---

# Unpainted Areas

The **Unpainted** tool highlights regions of the model that have not yet received painted information.

The plugin also provides:

* **Only Empty** — restrict projection to unpainted areas.
* **Select Empty** — automatically select unpainted areas in Krita.

This can be useful when progressively painting a texture without accidentally covering previously painted regions.

---

# Tools Reference

| Tool             | Description                                 |
| ---------------- | ------------------------------------------- |
| **New 3D**       | Load an OBJ or FBX model                    |
| **Frame**        | Center the camera on the active model       |
| **Reset**        | Restore the default model and texture       |
| **Export**       | Export the resulting texture                |
| **Unpainted**    | Highlight unpainted regions                 |
| **Info**         | Open the plugin documentation and shortcuts |
| **Home**         | Open the official project page              |
| **Extract Tex**  | Extract the original material texture       |
| **Import Tex**   | Replace the model's base texture            |
| **3D / 2D**      | Switch between 3D and UV views              |
| **Symmetry**     | Mirror strokes and projections              |
| **Mesh**         | Display the triangle wireframe              |
| **Compare**      | Compare original and painted textures       |
| **Undo / Redo**  | Navigate editing history                    |
| **Capture**      | Capture the current 3D view                 |
| **Set Image**    | Load Krita artwork as an overlay            |
| **Brush**        | Paint directly on the model                 |
| **Eraser**       | Erase projected paint                       |
| **Project**      | Project artwork onto the model              |
| **Cylindrical**  | Enable cylindrical projection               |
| **Only Empty**   | Project only onto empty areas               |
| **Select Empty** | Select unpainted areas                      |
| **Seams**        | Enable UV seam bleeding                     |
| **Prev Cam**     | Restore a previous camera position          |

---

# Requirements

* **Krita 5.x or later**
* Windows, Linux, or macOS
* Python 3
* PyQt5
* OpenGL 3.3 Core or OpenGL ES 3.0 compatible hardware

No separate Python installation is required when using the Python environment bundled with Krita.

---

# Compatibility

| Platform | Support   |
| -------- | --------- |
| Windows  | Supported |
| Linux    | Supported |
| macOS    | Supported |

The plugin is designed for Krita 5.x and later.

---

# Buy / Support the Project

Krita 3D Mesh Painter is available through itch.io.

[**Get Krita 3D Mesh Painter on itch.io**](https://tijerinart.itch.io/krita-3d-projection-painter)

The itch.io page contains the latest available release and additional information about the plugin.

---

# Feedback & Bug Reports

If you encounter a problem, please open a GitHub Issue.

When reporting a problem, please include:

* Krita version
* Operating system
* Plugin version
* 3D file format (`.obj`, `.fbx`, etc.)
* Steps to reproduce the problem
* Error message, if available
* Screenshots or a short video when useful

Please check existing issues before creating a new one.

---

# Development

Krita 3D Mesh Painter is a Python plugin for Krita.

The project uses:

* Python 3
* PyQt5
* OpenGL
* Krita's Python API

The plugin is organized as a standard Krita Python plugin, consisting of a `.desktop` descriptor and a Python package.

For information about developing Python plugins for Krita, see the official [**Krita Python Plugin Documentation**](https://docs.krita.org/en/user_manual/python_scripting/krita_python_plugin_howto.html).

---

# License

Krita 3D Mesh Painter is distributed as a Krita Python plugin.

See the `LICENSE` file in this repository for the complete license terms.

> **Note:** Krita's official licensing information states that distributed Python plugins must be shared under the GNU GPL. If you intend to distribute this plugin publicly, make sure the repository contains the appropriate GPL license and that the distributed plugin complies with those requirements.

See the [**Krita License information**](https://krita.org/en/about/license/) for more details.

---

# Credits

**Krita 3D Mesh Painter**

Created by **José Tijerín**
GitHub: `tijegoblinight`

Official project page:

[**tijerinart.itch.io/krita-3d-projection-painter**](https://tijerinart.itch.io/krita-3d-projection-painter)

Created: September 26, 2026

---

## About Krita

Krita is a free and open-source digital painting application developed by the Krita community.

[**Visit Krita's official website**](https://krita.org/)

[**Krita Documentation**](https://docs.krita.org/)

[**Krita Python Scripting Documentation**](https://docs.krita.org/en/user_manual/python_scripting.html)

---

## Useful Links

* [**Buy / Download the Plugin**](https://tijerinart.itch.io/krita-3d-projection-painter)
* [**Krita Official Website**](https://krita.org/)
* [**Krita Documentation**](https://docs.krita.org/)
* [**Krita Python Plugin Documentation**](https://docs.krita.org/en/user_manual/python_scripting/krita_python_plugin_howto.html)
* [**Krita Python Plugin Installation Guide**](https://docs.krita.org/en/user_manual/python_scripting/install_custom_python_plugin.html)
* [**Krita Scripting School**](https://scripting.krita.org/)
* [**Report a Bug / Request a Feature**](../../issues)
* [**Releases**](../../releases)
* [**Source Code**](../../)


