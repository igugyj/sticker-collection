# AGENTS.md

Personal sticker library. The actual content is image files under `lib/`; there is no application code to run.

Repo: https://github.com/igugyj/sticker-collection · Gallery: https://sticker-collection.pages.dev

## Structure

- `lib/<category>/` — the sticker library. Each subfolder is a category; drop image files here. This is the primary "content" of the repo.
- `emoCut.py` — interactive image-splitting tool (uses `pillow`; `pip install -r requirements.txt`). Run `python emoCut.py`, prompts for an image, rows/cols, optional overlap/aspect-ratio. Outputs tiles + `info.txt` to `<name>_split_{rows}x{cols}/`.
- `scripts/update_stats.py` — regenerates the folder-count table in `README.md` between `<!-- stats_start -->` / `<!-- stats_end -->`.
- `scripts/generate_gallery.py` + `scripts/template.html` — builds a static `index.html` gallery in the repo root (placeholder `{{ DATA }}`).
- `docs/.ai/` — extended Chinese docs for this repo; protocol: after each task, update the affected doc and add an entry on top of `docs/.ai/changelog.md`.

## Gotchas

- **Do NOT hand-edit the stats block in `README.md`.** `.github/workflows/update_stats.yml` (GitHub Actions, runs on push to `master`) regenerates and auto-commits it with `[skip ci]`.
- **`.github/deploy-gallery.yml` is misplaced** at `.github/deploy-gallery.yml` instead of `.github/workflows/`, so the gallery deploy workflow never runs. Keep new workflows in `.github/workflows/`.
- **Default branch is `master`**, not `main`.
- **Filenames are non-ASCII and punctuation-heavy** (Chinese/Japanese, spaces, `！？（）`). Always quote paths in shell commands. `emoCut.py` uses CJK output messages.
- `.gitignore` keeps generated files out of git: `index.html` (gallery), `info.txt` (emoCut output), `.venv`.
- README stats count only direct files under `lib/` and skip `.preview*` prefix + `.html/.psd/.txt/.md/.url` extensions — they can differ from a naive file count.
- `scripts/*.py` are stdlib-only (no deps). Only `emoCut.py` needs `pillow`.

## Verify locally

- `python scripts/update_stats.py` — refresh README stats (prints "No changes in statistics, skipping update." when already current).
- `python scripts/generate_gallery.py` — (re)generate `index.html`, then open it in a browser.

No tests, lint, or typecheck config exists.
