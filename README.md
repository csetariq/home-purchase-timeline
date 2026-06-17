# Resale Home Purchase Tracker — setup

You edit `home.mw` (plain text). GitHub renders it to an interactive timeline/Gantt
page and serves it read-only on GitHub Pages. Your seller just opens a URL.

## Repo layout

```
your-repo/
├─ home.mw                      # the timeline source (edit this)
└─ .github/
   └─ workflows/
      └─ deploy.yml             # renders + deploys on every push
```

## 1. Test locally first (2 minutes, optional but recommended)

```bash
npm install -g @markwhen/mw
mw home.mw index.html
open index.html        # or just double-click it
```

If `index.html` shows the timeline, the CLI works on your machine and CI will too.
(`@markwhen/mw` is a little old — if the global install errors, try Node 18, or
`npx @markwhen/mw home.mw index.html`.)

## 2. Push to GitHub

1. Create a new **public** repo (see privacy note below).
2. Add `home.mw` at the root and `deploy.yml` at `.github/workflows/deploy.yml`.
3. Commit and push to the `main` branch.

## 3. Turn on Pages

Repo → **Settings → Pages → Build and deployment → Source: GitHub Actions**.

That's it — no branch to pick. The workflow you pushed handles the build and deploy.

## 4. Get the link

After the first push, open the **Actions** tab and wait for the run to finish
(~1 min). The deploy step prints the live URL, which will look like:

```
https://<your-username>.github.io/<repo-name>/
```

Send that to your seller. They can pan, zoom, expand sections, and tick through
the view — but they're hitting a static page, so they can't edit anything.

## 5. Updating it later

Edit `home.mw`, commit, push. The site rebuilds itself in about a minute. Nothing
to re-send — the URL stays the same.

For comfortable editing with live preview, use the **VS Code extension**
(`Markwhen.markwhen`) or the editor at https://meridiem.markwhen.com, then keep
`home.mw` in the repo as the source of truth.

## Notes / gotchas

- **Privacy:** a public repo's Pages site is world-readable by anyone with the URL.
  That's fine for a task list, but **don't put sensitive financial or personal data**
  in `home.mw`. Truly private Pages requires a paid GitHub plan.
- **Dates are optional in practice, not literally.** Markwhen is a timeline, so every
  task occupies *some* range. The starter uses **relative dates** (`after !id 2 weeks`)
  so you only commit to dates you actually know — change any one to an absolute date
  (e.g. `15/8/2026 - 1 week:`) when it firms up, and dependent tasks shift automatically.
- **Dependencies** are real: `after`, `before`/`by`, and `!id.start` / `!id.end` let one
  task key off another. Move an early task and everything downstream recalculates.
- **Status** is just the `#tag` on each line — flip `#todo` → `#active` → `#done`.
- **Per-item updates** live in the `- [ ]` / `- [x]` checklist lines under each task.
