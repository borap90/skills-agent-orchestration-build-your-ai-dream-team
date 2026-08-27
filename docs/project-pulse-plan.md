# Project Pulse Dashboard Implementation Plan

## Summary

Mona's Project Pulse dashboard will be implemented as a lightweight static app for contributors. It will display multiple project cards with project name, owner, status, recent activity, priority/risk, and a contributor-friendly summary.

Repository findings:

- `.github/project-pulse-brief.md` defines the dashboard requirements.
- `.github/agents/` contains the Orchestrator, Planner, Designer, and Coder definitions.
- `app/` is currently empty; there is no existing frontend implementation or framework to extend.
- `.vscode/launch.json` does not yet exist.
- `.vscode/tasks.json` only configures the Copilot CLI exercise terminal.
- The repository uses GitHub Actions checks that validate required files, JSON syntax, selectors, and key content.
- The app should use browser-native HTML, CSS, JavaScript, JSON, and Python's standard HTTP server without adding dependencies.

The Orchestrator should coordinate the work, keep file ownership separate, and perform the final integration review without implementing source files directly.

## Ordered implementation steps

### 1. Confirm requirements and establish the implementation contract

**Owner:** Planner, coordinated by Orchestrator
**Files:** `docs/project-pulse-plan.md`; read-only references to `.github/project-pulse-brief.md`, `.github/agents/*.agent.md`, and `.github/workflows/3-step.yml`

- Confirm the required dashboard content and launch behavior.
- Define the shared HTML/CSS/data contract before parallel implementation begins.
- Use semantic page structure with a visible `Project Pulse` heading and a dashboard container.
- Reserve `.dashboard` for the main dashboard layout and `.project-card` for every rendered project card.
- Define required project fields: `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Include `summary` as an additional field because the brief requests contributor-friendly summaries.
- Use a top-level `projects` array in the JSON file.
- Decide that rendering will use an inline script in `app/index.html`; no additional JavaScript file is required by the brief.
- Document all ownership, dependencies, parallel work decisions, and validation expectations in this plan.

### 2. Create the visual and accessibility foundation

**Owner:** Designer
**File:** `app/styles.css`

- Create a polished card-based dashboard rather than a bare HTML page.
- Style the `.dashboard` layout with readable spacing, a clear header area, and a responsive project grid.
- Style `.project-card` with visible hierarchy, rounded corners using `border-radius`, and depth using `box-shadow`.
- Define visual treatments for status and priority/risk badges, including neutral handling for unknown values.
- Use sufficient color contrast and ensure meaning is not conveyed by color alone.
- Support narrow screens with responsive layout rules and readable long text.
- Use a system font stack and local CSS only so the dashboard works without network access.
- Keep selectors and class names aligned with the implementation contract.
- Do not modify `app/index.html` or `app/project-data.json`.

### 3. Create deterministic project data

**Owner:** Coder
**File:** `app/project-data.json`

- Create valid strict JSON with a top-level `projects` array.
- Include multiple representative projects.
- Every project must include:
  - `name`
  - `owner`
  - `status`
  - `recentActivity`
  - `priority`
  - `summary`
- Use realistic, contributor-friendly values covering different statuses and priority levels.
- Keep values as plain strings so they can be safely rendered as text.
- Avoid trailing commas, comments, duplicate keys, or schema inconsistencies.
- Do not modify `app/styles.css`.

### 4. Build the dashboard document and rendering behavior

**Owner:** Coder
**File:** `app/index.html`
**Depends on:** Steps 2 and 3

- Use the exact document title `Project Pulse` and a visible `Project Pulse` heading.
- Reference `styles.css` through a stylesheet link.
- Reference `project-data.json` through an inline script using a relative `fetch()` call.
- Render one `.project-card` per project from the JSON `projects` array.
- Display each project's `name`, `owner`, `status`, `recentActivity`, `priority`, and `summary`.
- Use semantic elements, accessible labels, heading hierarchy, and meaningful text alternatives.
- Use DOM APIs such as `textContent` rather than injecting untrusted JSON values as raw HTML.
- Include loading, empty, and error states so the page does not remain blank if data loading fails.
- Apply safe, normalized classes or data attributes for status and priority styling.
- Keep the implementation self-contained and free of external libraries.
- Do not modify `app/styles.css` or `app/project-data.json`.

### 5. Add the VS Code preview configuration

**Owner:** Coder
**File:** `.vscode/launch.json`
**Depends on:** Step 4

- Create strict JSON with no comments.
- Add a configuration named exactly `Run Project Pulse Dashboard`.
- Use the standard `node-terminal` launch approach with:
  - command: `python3 -m http.server 5500`
  - working directory: `${workspaceFolder}/app`
- Configure `serverReadyAction` to open `http://localhost:%s/index.html`.
- Ensure the launch target opens `index.html`, not the app directory root.
- Use deterministic configuration fields and avoid requiring additional extensions.
- Do not modify the existing `.vscode/tasks.json`.

### 6. Perform integrated validation and handoff

**Owner:** Orchestrator, with Designer and Coder reviewing their owned files
**Files:** read-only review of `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`

- Verify that the HTML, CSS hooks, JSON schema, and launch configuration agree.
- Route any required fixes back to the original file owner rather than having multiple agents edit the same file.
- Confirm the launch configuration serves the `app` directory and opens the dashboard frontend.
- Record validation results for the eventual final handoff document in the later exercise step.

## File assignments for each step

| Step | Owner | Assigned files | Scope |
|---|---|---|---|
| 1 | Planner / Orchestrator | `docs/project-pulse-plan.md` | Requirements, phases, contracts, ownership, dependencies, and validation plan |
| 2 | Designer | `app/styles.css` | Visual hierarchy, responsive layout, accessibility, badges, cards, and dashboard styling |
| 3 | Coder | `app/project-data.json` | Deterministic project fixture data and required schema |
| 4 | Coder | `app/index.html` | Semantic markup, data loading, project-card rendering, and UI states |
| 5 | Coder | `.vscode/launch.json` | Strict JSON launch configuration for the static app |
| 6 | Orchestrator / assigned owners | All four implementation files | Read-only integration review and owner-directed fixes |

The Designer must not edit the Coder-owned files. The Coder must not redesign `app/styles.css` unless the Orchestrator explicitly reassigns that file.

## Dependencies

1. The Project Pulse brief and agent definitions must be reviewed before implementation.
2. The implementation contract in `docs/project-pulse-plan.md` must be complete before Designer and Coder work begins.
3. Designer styling and Coder data creation can begin after the shared class and field contract is agreed.
4. `app/index.html` depends on the JSON field names and CSS hooks from Steps 2 and 3.
5. `.vscode/launch.json` depends on the known app directory and `index.html` target.
6. Browser validation depends on all four implementation files existing.
7. Any final handoff documentation is downstream of integrated validation and is outside this plan's file scope.

## Parallel work

The following work can run in parallel after Step 1:

- Designer creates `app/styles.css`.
- Coder creates `app/project-data.json`.

These tasks have separate file ownership and can use the agreed `.dashboard`, `.project-card`, status, priority, and data-field contracts.

Validation tasks that do not require a browser can also run in parallel:

- Parse `app/project-data.json`.
- Parse `.vscode/launch.json`.
- Inspect required selectors and references.
- Check the required project fields.

## Sequential work

The following work must remain sequential:

1. Requirements research before implementation planning.
2. Plan completion before specialist delegation.
3. JSON and CSS contract completion before Coder builds the rendering logic.
4. `app/index.html` creation after the data schema and CSS hooks are known.
5. Launch configuration review after the target HTML file exists.
6. Browser smoke testing after all files are integrated.
7. Owner-directed fixes before the final validation report.

Although the launch configuration is technically independent of page content, it is intentionally reviewed after `index.html` so the Orchestrator can verify that the complete launch path opens the dashboard rather than a directory listing.

## Edge cases

- `project-data.json` cannot be fetched when opening `index.html` directly with `file://`; validation must use the HTTP server launch configuration.
- Network or server errors must produce a visible error message.
- Invalid JSON or a missing/non-array `projects` value must not cause an unhandled blank page.
- An empty projects array should display an explicit empty state.
- Missing optional summaries should receive a readable fallback.
- Missing required values should display a safe fallback such as “Not specified.”
- Unknown status or priority values should use a neutral visual treatment.
- Long project names, owner names, activity descriptions, and summaries must wrap without breaking the layout.
- Status and priority must remain understandable without color alone.
- Narrow mobile viewports must avoid horizontal scrolling.
- JSON values must be rendered as text to avoid accidental markup injection.
- Port `5500` may already be in use; the launch failure should be reported clearly and the configured port should not be silently changed.
- The server-ready pattern must match Python HTTP server output reliably enough for VS Code to open the configured URL.
- The dashboard should remain usable with keyboard navigation and reduced-motion preferences.

## Validation expectations

### Static validation

Run:

- `python3 -m json.tool app/project-data.json`
- `python3 -m json.tool .vscode/launch.json`

Inspect that:

- `app/index.html` contains the exact `Project Pulse` title and visible heading.
- `app/index.html` references `styles.css` and `project-data.json`.
- Each rendered card uses `project-card`.
- The HTML references `status`, `recentActivity`, and `priority`.
- `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- The JSON contains a top-level `projects` array and all required fields.
- `.vscode/launch.json` contains `Run Project Pulse Dashboard`, `python3 -m http.server 5500`, `${workspaceFolder}/app`, and `http://localhost:%s/index.html`.
- `launch.json` contains no comments and parses as strict JSON.

### Runtime validation

Using VS Code Run and Debug:

1. Select `Run Project Pulse Dashboard`.
2. Start the configuration.
3. Confirm the server runs from `app/`.
4. Confirm the browser opens `index.html`.
5. Confirm visible Project Pulse project cards appear.
6. Confirm owner, status, recent activity, priority, and summary are visible.
7. Test a narrow viewport and keyboard navigation.
8. Confirm there are no browser console errors.
9. Stop the preview server after testing.

The repository's Step 3 workflow provides automated checks for file existence, required key phrases, JSON parsing, required selectors, and launch configuration content. It does not replace the manual browser smoke test. `scripts/validate-exercise.sh` is intended for the complete exercise and also checks later learner files, so it may not pass until the final handoff step is complete.

## Open questions

- No Mona-specific brand palette is provided. Use an accessible neutral palette with distinct status and priority treatments unless branding guidance is supplied later.
- The brief requires contributor-friendly summaries but does not list `summary` among the mandatory JSON keys. This plan resolves that ambiguity by adding `summary`.
- No filtering, sorting, persistence, or backend API is requested; those features should remain out of scope.
- The expected file list does not include a separate JavaScript file, so rendering should remain in an inline script in `app/index.html`. A future enhancement could extract it if the repository scope expands.
- The launch configuration assumes the Codespace has the standard VS Code `node-terminal` support and Python 3 available, consistent with the repository's Codespaces-based workflow.
