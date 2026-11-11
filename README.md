# Embedded Gauge Documentation

This directory is the root of the MkDocs documentation project. It is structured so it can later become a standalone repository and be included by firmware repositories as a Git submodule at the same `Documentation/` path.

- `mkdocs.yml` contains site configuration and navigation.
- `docs/index.md` contains all project-specific overviews and comparison matrices.
- Topic folders under `docs/` contain reusable, cross-project guidance.
- `docs/assets/images/` contains shared and project-specific media.

Keep links inside the documentation relative to their Markdown or media files. Avoid links that depend on a particular checkout location so the docs continue to work as a standalone repository or submodule.
