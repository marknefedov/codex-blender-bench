# Codex Blender Reconstruction Benchmark

This repository is a visual benchmark for agent-driven Blender reconstruction. It compares Codex-generated Blender scenes against source objects ranging from simple utensils to a stylized character.

The GitHub Pages site is in `index.html` with styling in `styles.css`. It reviews each reconstruction, shows the input references, and links to the generated `.blend` files.

## Repository Layout

- `source/`: ground-truth source geometry (`.blend`, `.obj`, or `.glb`)
- `oneshot/`: single material-preview input images
- `muti-view-6-ortho/`: six orthographic input views per object
- `ai-oneshot/`: agent outputs generated from the single-image prompt
- `ai-mv6o/`: agent outputs generated from six orthographic views
- `session logs/`: saved run logs from the reconstruction sessions
- `index.html`, `styles.css`: static GitHub Pages report

## Requirements

The report site itself has no external Python package dependencies. To reproduce the modeling benchmark, install:

- Git LFS, because `.obj`, `.blend`, and `.glb` assets are tracked through LFS
- Blender with the Blender MCP add-on enabled
- An agent that can use Blender MCP tools, such as Codex Desktop with Blender MCP configured
- Python 3 if you want to preview the static site locally

`requirements.txt` is intentionally dependency-free because local preview uses Python's standard library.

## Preview The Site Locally

```powershell
python -m http.server 8765 --bind 127.0.0.1
```

Open:

```text
http://127.0.0.1:8765/
```

## Reproduce The Benchmark With An Agent

Use any agent that can inspect images and operate Blender through MCP. The important constraint is that the source assets and input images should be treated as read-only.

For each object, run two reconstruction tasks:

1. One-shot reconstruction
   - Input: `oneshot/<object>_material_preview.png`
   - Output target: `ai-oneshot/`
   - Ask the agent to recreate the visible object in Blender and save a `.blend` plus final render.

2. Six-view orthographic reconstruction
   - Input: all six files in `muti-view-6-ortho/<object>/`
   - Output target: `ai-mv6o/<object>/`
   - Ask the agent to use the front, back, left, right, top, and bottom views to build one coherent Blender model, then save a `.blend` plus final render.

Suggested agent instruction:

```text
Recreate this object in Blender using Blender MCP. Treat all source meshes and input images as read-only. Save the generated Blender scene and a final rendered PNG in the requested output folder. Prefer editable procedural geometry where practical, but do not modify any existing benchmark assets.
```

After generation, review the result using:

- visual match to the source render
- silhouette and proportions
- part completeness
- material fidelity
- whether multi-view inputs were fused into one coherent object
- Blender scene metadata such as object count, mesh count, material count, and total vertices/faces

The current report was written from the provided renders plus read-only Blender metadata inspection. No mesh or image data was edited during the review.

## Asset Attribution

- `source/anime_girl_casual_outfit__stylized_3d_character.glb`: [Anime Girl Casual Outfit - Stylized 3D Character](https://sketchfab.com/3d-models/anime-girl-casual-outfit-stylized-3d-character-c9e4a8b3ebdc4677b8ef25a7d59cf038) on Sketchfab.

## Publish With GitHub Pages

This repository is ready for GitHub Pages as a static site. In the GitHub repository settings, set Pages to serve from the repository root on the default branch.
