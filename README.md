# BHARAT: Open World — Sundarpur

**A web-first, open-source 3D village exploration game built with Godot 4.4.1.**

> **Release:** `0.1.0-prealpha` — a playable *vertical slice*, not a completed commercial-scale open-world game. All environment models are original procedural primitive geometry. Visuals are intentionally stylized rather than photorealistic, to keep the browser build lightweight.

## What you can play now

- Explore a handcrafted fictional Indian village named **Sundarpur** in third person.
- Follow a four-part story: chai stall → post office → clinic → village elder.
- Meet **six NPCs**, including two villagers who wander through the town.
- Discover **four landmarks**: Shanti Mandir, Lotus Pond, Village School, and Village Hall.
- Watch the **day/night cycle**, village streetlights, and **toggle monsoon rain**.
- Use a **minimap** with player and mission markers.
- Save progress locally and continue a journey later.
- Hear original synthesized village ambience and interaction effects.
- Use keyboard/mouse or basic on-screen controls on touch devices.

### Controls

| Action | Control |
| --- | --- |
| Move | `W A S D` or arrow keys |
| Run | `Shift` |
| Jump | `Space` |
| Talk to nearby villager | `E` or `Enter` |
| Rotate camera | Hold right mouse button and move mouse, or `Q` / `R` |
| Toggle monsoon rain | `T` |
| Open menu | `Esc` or on-screen menu button |
| Touchscreens | On-screen direction, camera, interact, jump and run buttons |

**Quest tip:** Follow the golden marker on the minimap. Stand near a villager and press `E`.

## Requirements

- Godot **4.4.1** standard edition (not .NET), with Web export templates.
- A browser supporting WebGL 2 for exported Web builds.
- No paid assets, accounts, cloud services or external plugins required to run the project.

## Run locally in Godot

1. Download or clone this repository.
2. Open **Godot 4.4.1** and choose **Import**, then select `project.godot`.
3. Press **F6** / **F5** (Run Project) to start.
4. Choose **NEW JOURNEY**.

CLI, if Godot is on your `PATH`:

```bash
godot --headless --path . --import
godot --path .
```

## Export for web

Install the matching Godot 4.4.1 export templates first, then run:

```bash
mkdir -p build/web
godot --headless --path . --import
godot --headless --path . --export-release "Web" build/web/index.html
```

Serve the output through HTTP rather than opening `index.html` via `file://`:

```bash
python -m http.server 8000 --directory build/web
```

Visit `http://localhost:8000`. The project uses **GL Compatibility** to target WebGL 2. Some older devices/browsers may not support it. Godot Web exports can require a secure context on deployed hosts; GitHub Pages uses HTTPS.

## GitHub Pages deployment

1. Create a **public** GitHub repository and upload all files, keeping the folder structure.
2. Ensure the default branch is named `main` (or update both workflow files to your branch).
3. Open **Settings → Pages → Build and deployment → Source → GitHub Actions**.
4. Push to `main` or run **Publish game to GitHub Pages** from the Actions tab.
5. After a successful workflow, your game will be available at `https://YOUR_USERNAME.github.io/YOUR_REPOSITORY/`.

The `ci.yml` workflow runs offline structural checks, Godot smoke tests, and creates a downloadable Web build. `pages.yml` builds and publishes the Web version.

> **Verification note:** The development environment used to author this initial repository did not have a Godot executable or internet access to install it. Python static checks ran locally; **Godot parsing, runtime, export and browser behavior must be verified through GitHub Actions or a local Godot installation before calling this a tested release**. If CI finds an engine-specific issue, use its log as the source of truth.

## Project structure

```text
bharat-open-world/
├── project.godot                 # Godot project / WebGL-compatible renderer
├── export_presets.cfg           # Web export preset
├── scenes/main.tscn             # Main game scene
├── scripts/
│   ├── main.gd                  # Gameplay orchestration, UI, saves, weather
│   ├── world_builder.gd         # Procedural village geometry, NPCs, landmarks
│   ├── player_controller.gd     # Third-person movement and camera
│   ├── quest_manager.gd         # Pure quest state machine and discovery logic
│   └── minimap.gd               # Minimap drawing
├── assets/
│   ├── audio/                   # Original synthesized WAV audio
│   └── icon.png                 # Original icon
├── tests/
│   ├── smoke_test.gd            # Godot integration smoke tests
│   └── static_check.py         # Offline checks
├── docs/
│   ├── ARCHITECTURE.md
│   ├── ROADMAP.md
│   ├── QA_STATUS.md
│   └── plans/2026-11-13-bharat-open-world.md
└── .github/workflows/          # CI + Pages automation
```

## Test

```bash
python tests/static_check.py
godot --headless --path . --import
godot --headless --path . --script res://tests/smoke_test.gd
```

Expected messages: `STATIC_CHECKS_PASSED` and `BHARAT_SMOKE_TEST_PASSED`. CI will fail if Godot cannot parse, run or export the project.

## Performance strategy

- GL Compatibility renderer, low-poly original meshes, simple StandardMaterial3D materials.
- Bounded world of about 136 × 136 Godot units; no texture downloads.
- One directional shadow caster, modest street lighting, a bounded CPU rain emitter.
- Gameplay should target 30 FPS on capable mobile browsers and 60 FPS on desktop, **but these are design targets, not benchmarked results**.
- For future large worlds: chunk streaming, LOD, occlusion, instancing, baking and profiling before adding content.

## Scope and constraints

This version is a vertical slice. It does **not** yet include driving, a large seamless world, interiors, NPC dialogue choices, traffic, sophisticated simulation AI, photorealistic assets or multiplayer. Those are planned milestones in [`docs/ROADMAP.md`](docs/ROADMAP.md). There are no claims of production-grade visual realism or cross-device performance without testing.

## License and attribution

- Source code, icon, original synthesized audio and procedural meshes: **MIT License**, see [`LICENSE`](LICENSE).
- No third-party game art or copyrighted audio is included.
- Godot is an independently licensed open-source engine, not bundled here. See [Godot license](https://godotengine.org/license/).

## Contributing

Please open an issue describing the expected behavior, observed behavior, reproduction steps, browser/OS, and Godot version. For new features, link the relevant milestone in the roadmap. Use descriptive pull requests and run the tests above.
