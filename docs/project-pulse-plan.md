# Mona's Project Pulse — Implementation Plan

Note: Save verbatim as docs/project-pulse-plan.md

---

## Summary

"Mona's Project Pulse" is a small, static dashboard that visualizes team project health (status, progress, owners, due dates, quick metrics). The goal is a runnable static web app inside Codespaces with no required backend server. The app will live under app/ and must be openable from Codespaces. To make it robust against file:// fetch restrictions we will implement a fetch-with-inline-fallback pattern (details below). This plan breaks work into small, assignable steps with explicit file ownership, parallelizable tasks, risks, tests, and time estimates.

---

## Goals

- Deliver a polished static dashboard (HTML/CSS/JS + JSON) that runs in Codespaces without a server.
- Provide readable, maintainable code and clear ownership for key files.
- Support: list of projects, per-project progress, filters/search, simple visual summary (bars or sparklines), responsive layout, and accessibility basics (keyboard, ARIA).
- Make it easy for reviewers to run locally in Codespaces: include `.vscode/launch.json` with "cwd": "${workspaceFolder}/app" and a launch that opens index.html (with fallback instructions if file:// is blocked).
- Include a small sample `app/project-data.json` file that drives the UI.

---

## File assignments (required files + owners)

Required files (explicit owners listed):

- app/index.html — Owner: Coder (primary). Designer provides final markup guidance / assets.
- app/styles.css — Owner: Designer (primary). Coder refactors/implements responsive tweaks as needed.
- app/project-data.json — Owner: Designer (primary). Designer defines sample data, realistic field names and sample entries. Coder validates schema with code.
- .vscode/launch.json — Owner: Coder (primary). Designer reviews behavior for preview.

Additional recommended files (owners for consistency):

- app/scripts.js — Owner: Coder (primary). Implements data loading, DOM updates, filtering, fallback logic.
- app/assets/* (icons, logos) — Owner: Designer.
- docs/run-in-codespaces.md — Owner: Coder.
- docs/test-plan.md — Owner: Coder (author), Designer (reviewer).

Note: Each step below lists file-level assignments for small tasks.

---

## Ordered Steps (numbered, small, assignable)

Estimated hours per step are included in parentheses. Steps are intentionally granular so the Orchestrator can assign them to different workers.

1. Repo & Requirements Review (0.5h)
   - Task: Read README and repo root to confirm current structure and target branch.
   - Files: none (review).
   - Owner: Coder.

2. Design: Define data model & sample dataset (1.5h)
   - Task: Designer drafts `project-data.json` schema and 6–10 sample projects (fields: id, name, owner, status, progress (0–100), due_date ISO, tags[], description, tasks_completed, tasks_total).
   - Files: app/project-data.json (create).
   - Owner: Designer.

3. Design: Low-fi mockups & layout spec (2.5h)
   - Task: Designer creates 2 responsive mockups (desktop/tablet/mobile) and documents layout, colors, spacing, typography, and interactions (hover, focus states).
   - Files: docs/mockups.md (or Figma links), app/styles.css (initial notes as comments).
   - Owner: Designer.

4. Coder: Project scaffold & index.html skeleton (1.5h)
   - Task: Create `app/index.html` skeleton with semantic containers (header, main, filters, project-list, footer) and references to `styles.css` and `scripts.js`. Include an empty inline data fallback container (e.g., a <script> tag for embedded JSON) — document but do not write actual code here now; plan for it.
   - Files: app/index.html.
   - Owner: Coder.

5. Designer: Base CSS (desktop) — first pass (2.5h)
   - Task: Implement core visual styles in `app/styles.css` to match mockups: color palette, typography, card styles, spacing, progress bar base styles.
   - Files: app/styles.css.
   - Owner: Designer.

6. Coder: Static sample JSON validation + small test harness (1.0h)
   - Task: Coder reviews `app/project-data.json` and writes a minimal `app/scripts.js` stub that can load the JSON via fetch; include logic plan for fallback to inline JSON if fetch fails (no server). (This step creates the stub scaffolding and test harness, not full features.)
   - Files: app/scripts.js, app/project-data.json (validation).
   - Owner: Coder.

7. Coder: Implement data-loading with robust fallback (2.5h)
   - Task: Implement fetch of `project-data.json` relative path; if fetch fails (e.g., file:// restrictions), load inline JSON embedded in a <script id="embedded-data" type="application/json"> element in index.html. Also implement graceful error UI for read failure.
   - Files: app/scripts.js, app/index.html.
   - Owner: Coder.

8. Coder: Render project list/cards from data (3.5h)
   - Task: Render project cards into DOM, showing project title, owner, status badge, progress bar, due date. Keep markup accessible (use role attributes, headings, aria-labels). Implement minimal DOM update functions.
   - Files: app/scripts.js, app/index.html, app/styles.css (small CSS tweaks).
   - Owner: Coder.

9. Designer: Responsive tweaks & mobile styles (1.5h)
   - Task: Finalize mobile/tablet CSS breakpoints, adjust card layout and typography for small screens, ensure touch target sizes, update styles.css.
   - Files: app/styles.css.
   - Owner: Designer.

10. Coder: Implement search & filter controls (2.5h)
    - Task: Implement UI controls (search box, status filter dropdown, owner filter, tag filter) and wire them to the data rendering logic. Add debounce to search.
    - Files: app/index.html (controls markup), app/scripts.js, app/styles.css (control styles).
    - Owner: Coder.

11. Coder: Add sorting and "show overdue" logic (1.5h)
    - Task: Add sort options (by due date, progress, name) and quick toggle to filter overdue projects (due_date < today).
    - Files: app/scripts.js, app/index.html.
    - Owner: Coder.

12. Coder: Small visual summaries (progress bars + mini charts) (3.0h)
    - Task: Implement per-project progress bars and a top-level summary (total projects, average progress). For a simple chart, implement a small inline SVG bar or use CSS-based sparkline to avoid third-party libs.
    - Files: app/scripts.js, app/styles.css, app/index.html.
    - Owner: Coder.

13. Accessibility & keyboard support pass (2.0h)
    - Task: Keyboard navigation, focus states, ARIA labels, color contrast checks. Designer and Coder review together.
    - Files: app/styles.css, app/index.html, app/scripts.js.
    - Owners: Designer (visual/a11y checks), Coder (implementation).

14. .vscode/launch.json creation & Codespaces run instructions (0.75h)
    - Task: Create `.vscode/launch.json` with "cwd": "${workspaceFolder}/app" and a launch configuration to open index.html. Add notes about file:// issues and fallback instructions (Live Server / Simple Browser / inline-data fallback).
    - Files: .vscode/launch.json, docs/run-in-codespaces.md.
    - Owner: Coder.

15. Documentation: Running & testing guide (1.25h)
    - Task: Create docs/run-in-codespaces.md and docs/test-plan.md with exact steps to open in Codespaces, test fallback, and known limitations.
    - Files: docs/run-in-codespaces.md, docs/test-plan.md.
    - Owner: Coder.

16. Manual QA & polish pass (2.0h)
    - Task: Run manual tests, fix layout bugs, tune spacing, fix small accessibility issues, test in Chrome/Firefox and mobile sizes.
    - Files: app/styles.css, app/index.html, app/scripts.js.
    - Owners: Coder (fixes), Designer (visual checks).

17. Final Acceptance & Sign-off (0.5h)
    - Task: Verify acceptance criteria, update docs and PR description.
    - Files: docs/project-pulse-plan.md (this file), docs/test-plan.md.
    - Owners: Project owner / stakeholder.

Total estimated time: ~28.5 hours (sum of above).

---

## Parallelism — what can run in parallel and why

- Designer (steps 2 & 3) can run concurrently with Coder (step 4). Reason: mockups and JSON schema don't depend on scaffolded HTML.
- Designer implementing base CSS (step 5) can start while Coder sets up the scripts stub (step 6). CSS and JS wiring are largely independent.
- Implementing search/filter UI markup (index.html) can be done in parallel with scripts.js logic for data-loading if the markup contract (IDs/classes) is agreed upfront.
- Accessibility reviews and adjustments (step 13) should be done in parallel with bug fixes in step 16 once features exist.
- Overall parallel tasks: steps 2–6, 9–12 (some work overlap). Be careful to coordinate on class names and data attributes to avoid conflicts.

---

## Work that must run sequentially

- Data model definition (step 2) must finish before reliable rendering logic is implemented (steps 6–8).
- HTML skeleton (step 4) must exist before most DOM-targeting JS is implemented (step 8).
- Data loading and fallback (step 7) must be in place before final rendering and filtering work (steps 8–11).
- Accessibility pass (step 13) should follow basic UI implementation (steps 8–12).

---

## Designer responsibilities (detailed)

Primary goals: visual clarity, accessibility, and realistic sample data.

- Provide app/project-data.json (owner).
  - Define schema: id, name, owner, status, progress (0–100), due_date (ISO), tags[], description, tasks_completed, tasks_total.
  - Provide 6–10 varied examples: overdue, completed, near-complete, long names, multi-tag.
- Provide mockups and style guidance.
  - Low-fi and final mockups for desktop, tablet, and mobile.
  - Color palette, typography, spacing, focus/hover states, and accessible contrast choices.
  - Export required assets (icons, logos) optimized for web.
- Provide CSS base for visual system (owner of app/styles.css).
  - Core variables, layout rules, card and control styles; mark extension points for coder (class names, data-attrs).
- Accessibility & content guidance.
  - Specify ARIA roles, expected headings order, focus behavior, and labels for controls.
  - Provide sample long-form text and localized examples for truncation tests.
- Review & sign-off.
  - Review code PRs for visual and data fidelity; approve when mockups match implementation.

## Coder responsibilities (detailed)

Primary goals: robust, accessible, and easy-to-run static implementation.

- Implement app/index.html (owner).
  - Create semantic markup, include <script id="embedded-data" type="application/json"> fallback, and control elements with agreed IDs/classes.
- Implement app/scripts.js (owner).
  - Load project-data.json with fetch; on failure, parse embedded JSON fallback.
  - Render project cards, filters, search, sorting, and "show overdue".
  - Keep DOM updates accessible (focus management, live regions if needed).
- Implement run & launch files (.vscode/launch.json).
  - Ensure Codespaces can open index.html; document file:// fallback steps in docs/run-in-codespaces.md.
- Implement small visuals (progress bars, summary) without external libs.
- Write minimal tests/manual checks in docs/test-plan.md to validate data-loading and UI behaviors.
- Address Designer review comments and iterate until sign-off.

## Dependencies & Ordering

- Data model (Designer) → HTML skeleton (Coder) → Data-loading fallback (Coder) → Rendering & controls (Coder) → Visual polish & responsive tweaks (Designer + Coder) → Accessibility pass → Docs & launch config → Final QA/sign-off.
- Key constraints:
  - Rendering requires a stable data schema; do not implement final render until schema is provided.
  - CSS class names and data-attributes must be agreed before heavy DOM wiring to avoid rework.
- Parallelizable items:
  - Designer mockups & sample data can be produced while coder scaffolds HTML and script stubs.
  - CSS base and JS stubs can be developed in parallel if interface contract (IDs/classes) is shared.

## Validation & Acceptance Criteria

- App runs in Codespaces by opening app/index.html (with documented fallback); instructions verified in docs/run-in-codespaces.md.
- Data-loading:
  - Fetching app/project-data.json succeeds in normal environments.
  - When fetch fails, embedded JSON fallback loads and renders identical UI.
- UI correctness:
  - Project list renders all sample projects with title, owner, status badge, progress, and due date.
  - Search, filters, sorting, and "show overdue" work and update the DOM correctly.
- Accessibility:
  - Keyboard navigation works for controls and project cards.
  - Focus states visible; ARIA labels present for interactive controls.
  - Color contrast meets WCAG AA for normal text.
- Responsiveness:
  - Layout adapts at desktop/tablet/mobile breakpoints; no horizontal scroll on mobile viewport.
- Documentation & tests:
  - docs/run-in-codespaces.md and docs/test-plan.md include steps to reproduce failures and acceptance checks.
- Review:
  - Designer approves visual match to mockups; Coder confirms behavior.
  - Stakeholder signs off in final acceptance step.
