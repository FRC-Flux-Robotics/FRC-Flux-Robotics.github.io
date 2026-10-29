# AGENTS.md — FRC-Flux-Robotics.github.io Standalone Agent Guide (FRC Team 10413)

## 1. Repository Overview & Scope
`FRC-Flux-Robotics.github.io` is the static GitHub Pages documentation and interactive architecture map repository for **FRC Team 10413 (FLUX Robotics)** (`v2027.1.0`):
- [`README.md`](README.md): Overview of hosted pages, interactive class maps, and the unified 5-repo software suite.
- [`class-map-2027.html`](class-map-2027.html): Interactive Class & Relationship Explorer for `Flux-SystemCore-2027` (`53` nodes, `148` edges, `8` test suites).
- [`class-map-roborio-2027.html`](class-map-roborio-2027.html): Interactive Class & Relationship Explorer for `Flux-RoboRIO-2027` (`54` nodes, `157` edges, `8` test suites).
- [`index.html`](index.html): Landing page linking to student guides, robot profiles, and interactive class maps.

## 2. CI/CD & Publishing Standards
- **Zero GitHub Actions Workflows (`.github/workflows/`):** All builds and class-map exports run on GCP Cloud Build (`cloudbuild.yaml` in the source repositories) and publish directly to the `main` branch of `FRC-Flux-Robotics.github.io` and `gs://btvaroska/frc-10413/`. Never add `.github/workflows/` files.
- **Standalone Links Only:** Keep all links in `README.md`, `AGENTS.md`, and HTML pages valid for standalone clones and GitHub Pages visitors.
