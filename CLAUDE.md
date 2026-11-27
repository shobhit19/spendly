# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Spendly: a personal expense tracker (amounts in ₹) built with Flask, Jinja2 and SQLite. It is a step-by-step teaching project: the public pages are built, but the app logic is mostly unwritten stubs marked "Step N" in the code.

## Commands

Windows, PowerShell. `python3` does not work on this machine (it opens the Microsoft Store), so use `python`.

```powershell
python -m venv venv
venv\Scripts\python.exe -m pip install -r requirements.txt
venv\Scripts\python.exe app.py        # dev server, debug mode, http://127.0.0.1:5001
venv\Scripts\python.exe -m pytest                       # all tests
venv\Scripts\python.exe -m pytest path/to/test_file.py::test_name   # single test
```

pytest and pytest-flask are installed but there is no `tests/` directory yet. There is no linter or build step.

## Architecture

- [app.py](app.py) is the whole backend: one Flask `app` with every route. Pages that exist (`/`, `/login`, `/register`, `/terms`, `/privacy`) just call `render_template`. The login and register routes accept GET only, and their forms POST to URLs that have no handler yet. The remaining routes (`/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete`) are placeholders returning plain text.
- [database/db.py](database/db.py) is a comment-only stub. It is meant to provide `get_db()` (SQLite connection with `row_factory` and foreign keys on), `init_db()` (`CREATE TABLE IF NOT EXISTS`) and `seed_db()`. The SQLite file `expense_tracker.db` is git-ignored and does not exist until that is built.
- Templates all extend [templates/base.html](templates/base.html), which owns the navbar and the footer (including the Terms and Privacy links) and exposes `title`, `head`, `content` and `scripts` blocks. Use `url_for('<endpoint>')` for links, as the existing templates do.

## Styling conventions

CSS is split across two files, which is easy to miss:

- [static/css/style.css](static/css/style.css) is loaded on every page. It defines the theme variables in `:root` (`--ink`, `--paper`, `--accent`, `--font-display`, and so on), the navbar, footer, buttons, auth and form styles, and the shared `legal-*` classes used by `terms.html` and `privacy.html`. Reuse these variables instead of adding hex values.
- [static/css/landing.css](static/css/landing.css) is loaded only by `landing.html`, through its `head` block, so it comes after `style.css`. It holds the hero and the video modal, all under the `lp-` prefix.
- `style.css` still contains the old `.hero` and `.mock-card` rules from before the hero redesign. They are no longer used by `landing.html`.

The project uses no JavaScript framework or libraries. [static/js/main.js](static/js/main.js) is an empty placeholder, and page-specific scripts are inline in the template's `scripts` block (see the video modal in `landing.html`).
