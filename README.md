# Garv's CGPA Calculator — Integrated B.Sc. & M.Sc. Physics

A beautiful, feature-rich CGPA calculator built for the 5-year Integrated B.Sc. & M.Sc. (Physics) program.

## Features

- **Semester selector** (Sem 1–4 with animated transitions)
- **Previous SGPA input** for cumulative CGPA calculation
- **Grade input per subject** (O/A+/A/B+/B/C/F/NE — grade points, not marks)
- **Mark subjects as failing** to see impact on CGPA
- **CGPA Targeter** — calculates required SGPA to hit a desired CGPA
- **Grade distribution bar** visualization
- No relative grading — absolute grade points only

## Grade Scale (Absolute)

| Grade | Marks | Points |
|-------|-------|--------|
| O     | 91–100 | 10 |
| A+    | 81–90 | 9  |
| A     | 71–80 | 8  |
| B+    | 61–70 | 7  |
| B     | 51–60 | 6  |
| C     | 40–50 | 5  |
| F     | <40   | 0  |

## Deploy on GitHub Pages

1. Create a new repo (e.g. `cgpa-calc`)
2. Upload `index.html` to the root
3. Go to **Settings → Pages → Source → main branch → / (root)**
4. Your site will be live at `https://yourusername.github.io/cgpa-calc`

> No build step, no dependencies, no Node.js needed — pure HTML/CSS/JS.

## Adding More Semesters

In `index.html`, find the `SEMESTERS` object and add entries for semesters 5–10 following the same format.
