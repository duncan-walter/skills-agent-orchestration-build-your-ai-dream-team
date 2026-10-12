# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona’s Project Pulse as a lightweight static dashboard for contributors. It should show multiple projects, their owners, status, recent activity, priority or risk, and a short contributor-friendly summary in a polished, accessible, responsive card layout.

Follow `.github/project-pulse-brief.md` and the repository’s existing custom-agent workflow. Keep the app to the requested HTML, CSS, and JSON files; introduce no framework or package dependency. Serve the app from `app/` and open `index.html` through the VS Code **Run Project Pulse Dashboard** configuration.

## Agent responsibilities

- **Orchestrator:** Coordinate Planner, Designer, and Coder using the phases below. Confirm file ownership and the HTML/CSS/data contract before implementation; integrate and review the result. Do not implement the dashboard itself.
- **Planner:** Provide this plan, including file ownership, dependencies, ordering, risks, and validation.
- **Designer:** Define information hierarchy, responsive behavior, accessibility, visual styling, and the CSS hooks used by the page. Own `app/styles.css`; do not edit Coder-owned files.
- **Coder:** Implement `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`, then validate the app behavior and launch configuration. Do not edit Designer-owned `app/styles.css`.

## Ordered implementation steps and file assignments

### 1. Agree on the implementation contract

**Owner:** Orchestrator, with input from Designer and Coder
**Files:** No implementation files; this agreement informs the files in the next step.

Before parallel implementation, agree on:

- Dashboard sections, information hierarchy, responsive layout, accessibility expectations, and the class names HTML will use.
- The top-level JSON shape: a `projects` array whose entries include `name`, `owner`, `status`, `recentActivity`, and `priority`. Agree on how to represent the requested contributor-friendly summary; it can be an additional field or concise text derived from the required fields.
- The elements and class hooks Coder will include in `app/index.html`, including `.dashboard` and `.project-card`.
- The expected behavior for empty or unreadable data.
- The launch requirements: serve from `app/`, use `python3 -m http.server 5500`, and open `index.html` rather than the directory root.

**Dependency:** This contract is a prerequisite for parallel implementation because the HTML depends on both the data shape and Designer’s CSS hooks.

### 2. Implement the dashboard files

Once the contract is agreed, these file assignments can proceed in parallel:

| Owner | Assigned file | Scope |
|---|---|---|
| **Designer** | `app/styles.css` | Style the dashboard using the agreed hooks. Provide project cards, clear status and priority treatments, readable spacing and typography, responsive behavior, visible focus states, and sufficient contrast. Include `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`. |
| **Coder** | `app/project-data.json` | Create valid JSON with a top-level `projects` array. Include multiple clearly illustrative sample projects, each with `name`, `owner`, `status`, `recentActivity`, and `priority`. Include the agreed summary representation if applicable. |
| **Coder** | `app/index.html` | Create the page titled **Project Pulse**. Link `styles.css` and load or reference `project-data.json`. Render visible cards with the agreed classes and show each project’s name, owner, status, recent activity, priority, and summary. Include accessible page structure and loading, empty, and error states. |
| **Coder** | `.vscode/launch.json` | Add strict JSON defining **Run Project Pulse Dashboard**. Configure it to serve from `${workspaceFolder}/app` using `python3 -m http.server 5500`, and open `http://localhost:%s/index.html` when the server is ready. Confirm the chosen launch type and readiness pattern are supported in the Codespace. |

**Dependencies:** The CSS and Coder’s three files can be developed in parallel after Step 1, provided the contract is stable. The HTML/data integration depends on the agreed JSON shape; HTML styling depends on the agreed selector hooks. If either agreement changes, coordinate the changes before integration rather than having both agents edit the same file.

### 3. Integrate and validate

**Owner:** Coder validates implementation; Orchestrator reviews integration.
**Files:** Review all four implementation files; make corrections through their assigned owners.

Confirm that the page, data, styles, and launch configuration work together. The Orchestrator should identify failures and assign each correction to the existing file owner; keep Designer and Coder scopes separate.

## Dependencies and ordering

- `app/project-data.json` must follow the agreed schema before the HTML data-rendering behavior can be considered complete.
- `app/index.html` and `app/styles.css` depend on an agreed set of CSS hooks. Designer and Coder should agree on that contract before parallel work.
- `.vscode/launch.json` is independent of the UI and data files, but the live launch test depends on all app files being present.
- Integration review and the live preview must happen after implementation.
- No application framework, package manager, or new runtime dependency is required by the brief.

## Parallel work

After the contract in Step 1 is agreed:

- Designer can implement `app/styles.css` while Coder implements the HTML, JSON data, and launch configuration.
- No two agents should edit the same file.
- Keep any changes to the shared HTML/CSS/data contract coordinated; if the agreed hooks or schema changes, update the affected files through their assigned owners.

## Work that must run sequentially

1. Agree on the data schema and HTML/CSS hooks.
2. Implement in parallel using those stable agreements.
3. Integrate and inspect the assembled files.
4. Run static checks and the **Run Project Pulse Dashboard** preview.
5. Fix any issues, then have the Orchestrator confirm the final result.

## Edge cases and risks

- **Data loading failure:** Show a readable error if `project-data.json` cannot be fetched or parsed; do not leave a blank dashboard.
- **Empty data:** Show an intentional empty state when `projects` is an empty array.
- **Missing or unexpected fields:** Avoid broken cards if values are absent; handle unknown status or priority values without relying on them as valid CSS identifiers.
- **Unsafe project text:** Insert JSON-provided text as text rather than interpreting it as HTML.
- **Local-file restrictions:** Test through the HTTP server, not by opening `index.html` directly with a `file://` URL; browser fetch restrictions can prevent loading the JSON file.
- **Launch compatibility:** The repository’s devcontainer explicitly lists Copilot and GitHub Actions extensions, but does not list the Python VS Code extension. Verify the chosen launch configuration’s provider and `serverReadyAction` behavior in the Codespace. A server process that starts without opening the browser does not satisfy the brief.
- **Port collision:** If port 5500 is already occupied, report the issue and verify that the configured port, ready pattern, and browser URL remain consistent if a different port is necessary.
- **Representative data:** The brief supplies no real project records. Use clearly illustrative samples and do not present them as verified team data.

## Validation expectations

1. **File and content checks**
   - Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
   - Confirm the HTML title is **Project Pulse** and it references both `styles.css` and `project-data.json`.
   - Confirm project cards render with the `project-card` class and display status, recent activity, and priority.
   - Confirm CSS includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.

2. **Data and configuration checks**
   - Run `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`; both must succeed.
   - Confirm the data contains a top-level `projects` array and each project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
   - Confirm launch configuration name **Run Project Pulse Dashboard**, working directory `${workspaceFolder}/app`, command `python3 -m http.server 5500`, and a ready action that opens `http://localhost:%s/index.html`.

3. **Behavior and accessibility checks**
   - Run the launch configuration in VS Code. Confirm it serves the dashboard rather than a directory listing and opens `index.html`.
   - Confirm multiple cards display all required information at desktop and narrow viewport widths.
   - Confirm loading, empty, and fetch/parse error states are understandable.
   - Inspect keyboard focus visibility, semantic structure, text contrast, and behavior when project values are long.

4. **Repository workflow alignment**
   - Ensure the plan contains the required file assignments, agent responsibilities, dependencies, parallel/sequential decisions, and validation expectations. These are also checked by `.github/workflows/2-step.yml`.
   - Ensure the implementation meets the content and JSON checks in `.github/workflows/3-step.yml`. The workflow validates file presence, required phrases, and JSON parsing; it does **not** run the app or prove the launch configuration works. The live preview remains necessary.

## Open questions

- What real project names, owners, activity, status, priorities, and contributor summaries should replace the illustrative sample data?
- Should summary be an explicit optional JSON field or concise text derived from the required fields?
- Which launch configuration provider is available in the learner’s Codespace, and does it support the specified server-ready browser action without additional extension setup?
