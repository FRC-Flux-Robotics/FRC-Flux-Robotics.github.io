# AGENTS.md — FRC-Flux-Robotics.github.io Repository Guide (FRC Team 10413)

## 1. Repository Overview & Scope
`FRC-Flux-Robotics.github.io` is the static GitHub Pages documentation and interactive architecture map repository for **FRC Team 10413 (FLUX Robotics)**:
- [`README.md`](README.md): Overview of hosted pages and interactive class maps.
- [`index.html`](index.html): Landing page linking to student guides, robot profiles, and interactive class maps.
- [`class-map-2027.html`](class-map-2027.html), [`class-map-roborio-2027.html`](class-map-roborio-2027.html), & [`class-map-new-robot.html`](class-map-new-robot.html): Self-contained interactive class and dependency explorers.

## 2. Publishing Standards
- **Zero GitHub Actions Workflows (`.github/workflows/`):** Static HTML/CSS/SVG assets are committed directly to `main`; never add `.github/workflows/` files.
- **Standalone Links Only:** Keep all links in `README.md`, `AGENTS.md`, and HTML pages valid for standalone clones and GitHub Pages visitors.
