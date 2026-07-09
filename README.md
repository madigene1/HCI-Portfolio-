# HCI Portfolio

A static website — plain HTML/CSS, **no build step and nothing to compile**. The design system lives in `DESIGN.md` and `design-system/tokens.css`, the home page is `index.html`, and each project is its own folder (e.g. `Scout/`).

## Preview the site locally

Open `index.html` directly in a browser, or (better, so links and images resolve) run a tiny local server from this folder:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`. Stop it with `Ctrl+C`. That's the only command you ever need to run.

## How to add a new project

Let Cursor do it with the **`portfolio-project-page`** skill. In Cursor, just ask, for example:

> Add a new project called "Booking Redesign".

The skill scaffolds the content file, generates the project page, and adds the card to the home page's Work section for you. It keeps everything on the design system automatically.

Typical flow:

1. Ask Cursor to add the project (give it a title).
2. The skill creates a `content.md` for the project and asks you to fill in the sections (Overview, Problem, Approach, Process, Solution, Outcome, Reflection). Add any images to the project's `assets/` folder and reference them in `content.md`.
3. Tell Cursor it's ready, and it generates the page and links it from the home page.
4. Refresh the browser (see **Preview** above) to see the new project.

> **If the skill is ever missing** (e.g. on a new account), ask Cursor to recreate it: "Recreate the `portfolio-project-page` skill for adding portfolio project pages to this site." It can rebuild the skill from the existing `Scout/` project and `DESIGN.md` as the pattern.

## Tips

- Keep one accent color per page (blue by default) and use the tokens in `design-system/tokens.css` rather than hardcoding values.
- Placeholders to replace on the home page: portrait images in the hero and About sections, the résumé at `assets/resume.pdf`, and the sample work-experience rows in the About section.
