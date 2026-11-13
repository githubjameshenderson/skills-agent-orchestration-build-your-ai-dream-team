# Project Pulse implementation plan

## Summary

This plan sequences the creation of a lightweight static Project Pulse dashboard for Mona's team. The work is split across the repository's custom agent roles: the Orchestrator keeps the build coordinated, the Designer shapes the dashboard experience and styling, and the Coder implements the runnable static app files and launch support. The expected deliverables are the four required files: `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`.

The plan assumes a small static app that loads project data from `app/project-data.json`, renders project cards from `app/index.html`, and uses `app/styles.css` to create a clear, polished dashboard experience. The .vscode launch configuration is added last to support previewing the finished dashboard from the `app/` directory and opening `index.html` instead of a directory listing.

## File ownership and responsibilities

### Designer responsibilities
- Define the dashboard information hierarchy and layout: hero header, project cards, status badges, priority cues, and contributor summary text.
- Decide visual affordances that make the page feel like a polished Project Pulse dashboard rather than a bare HTML scaffold.
- Own the design language for spacing, contrast, typography, card surfaces, shadows, and responsive behavior.
- Ensure accessibility basics such as semantic headings, readable contrast, and keyboard-friendly interactive elements.
- Own the design implementation in `app/styles.css`, including stable CSS hooks such as `.dashboard` and `.project-card`.
- Agree with the Coder on shared class names, status values, and data-driven UI structure before implementation so the visual design matches the HTML and JSON schema.

### Coder responsibilities
- Own `app/project-data.json` and define the static app data contract required by the brief.
- Own `app/index.html` and implement the Project Pulse page so it consumes the `projects` array from `app/project-data.json`, uses the agreed CSS hooks, and renders the dashboard deterministically.
- Create `.vscode/launch.json` with a strict JSON launch configuration for VS Code that sets the working directory to `${workspaceFolder}/app` and opens `index.html`.
- Validate the rendered app against the brief and against the runtime behavior expected from the launch configuration.

## Ordered implementation steps

### 1. Confirm the app contract and data model
- Review the brief and repository guidance to confirm the required app shape.
- Define the expected JSON contract: a top-level `projects` array with project objects containing `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Agree on the allowed status values and their visual variants, plus the CSS hooks for the dashboard shell and project cards (including `.dashboard` and `.project-card`).
- Confirm the render path for the dashboard: static HTML with data binding or a fetch-based render using `app/project-data.json`.

Dependencies:
- Depends on the repository brief and the custom-agent definitions.
- Designer and Coder establish the shared data/status and CSS-hook contract before either implementation workstream begins.

### 2. Build the data source and stylesheet in parallel
- Coder creates `app/project-data.json` with a clean `projects` array that matches the agreed schema.
- Use realistic project names, owners, statuses, activity notes, and priority values that can support a meaningful dashboard.
- Keep the data deterministic and readable so the HTML can render it without additional transformation.
- Designer creates `app/styles.css` with the polished dashboard look described in the brief: visible cards, readable spacing, status badges, priority emphasis, clear typography, and responsive behavior.
- Implement the agreed deterministic class hooks, including `.dashboard` and `.project-card`, and style the agreed status variants.

Dependencies:
- Both workstreams require Step 1's agreed schema, status values, and CSS hooks.
- These tasks can proceed independently in parallel because the shared contract is established first.

### 3. Integrate the dashboard HTML
- Coder creates `app/index.html` with a clear dashboard shell, a meaningful page title, and project card containers aligned with the agreed hierarchy.
- Integrate the finished JSON schema and CSS hooks: load the `projects` data and use the exact class names and status semantics agreed in Step 1 and implemented in `app/styles.css`.
- Keep markup semantic and accessible: use headings, lists, and clearly labeled sections.

Dependencies:
- Requires Step 1's contract and both Step 2 deliverables (`app/project-data.json` and `app/styles.css`) to be finished.
- Integrates the agreed data and styling interfaces; HTML implementation does not run in parallel with final CSS or JSON implementation.

### 4. Add the runnable preview configuration
- Coder creates `.vscode/launch.json` so the project can be launched directly from VS Code.
- Set the launch configuration to run from `${workspaceFolder}/app` and open `index.html` rather than a directory listing.
- Use strict JSON with no comments and deterministic values for the task name, working directory, and browser launch target.

Dependencies:
- Requires Step 3 so `.vscode/launch.json` targets the integrated app file and final file structure.

### 5. Validate end-to-end behavior
- Verify the JSON file is valid and matches the required schema.
- Open the dashboard via the VS Code launch configuration and confirm the browser loads `app/index.html` rather than the file directory.
- Check that project cards render correctly, status badges are visible, and the page resembles a dashboard instead of a bare list.
- Confirm accessibility essentials such as contrast, heading structure, and a readable layout.

Dependencies:
- Requires `app/project-data.json`, `app/styles.css`, `app/index.html`, and `.vscode/launch.json` to be complete and integrated.
- Final checkpoint before handoff to the Orchestrator or learner review.

## Dependencies and sequencing

### Must run sequentially
- Contract first: Designer and Coder agree on the JSON schema, status values, and CSS hooks before implementation starts.
- HTML integration after parallel work: `app/index.html` is implemented after Designer completes `app/styles.css` and Coder completes `app/project-data.json`, so it can integrate the finished schema and CSS hooks.
- Launch configuration after HTML: `.vscode/launch.json` is authored once the final app path and preview target are in place.
- Validation last: end-to-end checks happen after all four files are integrated.

### Can run in parallel
- After Step 1, Designer's `app/styles.css` work and Coder's `app/project-data.json` work can proceed in parallel against the agreed status and CSS-hook contract.
- Coder owns the subsequent `app/index.html` integration, followed by `.vscode/launch.json`; these steps are sequential and do not overlap with the initial parallel work.

## Design and coding coordination

The Designer and Coder should not operate as isolated silos. Before implementation begins, they need a short contract covering:
- the dashboard shell and card component hooks, including `.dashboard` and `.project-card`,
- the JSON schema and allowed status values, including the corresponding badge conventions,
- a common understanding of which sections are informational versus action-oriented,
- evaluation of responsive behavior and readability requirements.

Once the contract is agreed, the Designer can implement `app/styles.css` while the Coder prepares `app/project-data.json`. The Coder then integrates the finished schema and hooks in `app/index.html`. This coordination keeps the UI polished without drifting away from the static-app requirements or the repository's emphasis on a lightweight, contributor-friendly dashboard.

## Edge cases and risks

- Missing or malformed `projects` data could prevent the dashboard from rendering cleanly.
- Inconsistent status names could make badges and styles ambiguous.
- If the launch configuration opens the directory instead of `index.html`, the learner will not see the intended dashboard.
- A bare HTML page without card layout, spacing, or priority cues would fail the design requirement.
- Overly complex JavaScript or library usage would exceed the brief's scope for a simple static app.

Mitigations:
- Keep the JSON schema strictly aligned with the brief and enforce a top-level `projects` array.
- Use a small, fixed set of status strings and apply CSS classes consistently.
- Keep the launch config simple and direct with `cwd` set to the `app` folder.
- Prefer deterministic markup and CSS over decorative complexity.

## Validation expectations

The implementation is considered complete when all of the following are true:
- `app/project-data.json` contains valid JSON with a top-level `projects` array and required project properties: `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `app/index.html` loads the Project Pulse dashboard shell and renders or structures project cards using that data and the agreed CSS hooks.
- `app/styles.css` applies a polished dashboard treatment with readable spacing, card styling, status badges matching the agreed status values, and responsive layout.
- `.vscode/launch.json` is valid strict JSON and launches the dashboard from the `app/` folder so `index.html` is opened directly.
- The dashboard reads as a Project Pulse page, not a generic folder listing or bare HTML placeholder; verify the cards, status badges, and priority cues in the browser.
- The design passes basic accessibility checks: semantic heading structure, comfortable contrast, and clear information hierarchy/readable layout.

## Open questions

- Should the dashboard render from JSON using a small script embedded in `app/index.html`, or should the HTML be pre-rendered with the data already embedded in the page? The brief strongly implies a data-driven static app, so the default plan is JSON-backed rendering.
- Is there an existing preferred browser preview or local server setup beyond the required VS Code launch configuration? At present, the repository guidance only requires the launch file and a direct `index.html` preview.
- Are there any repository-level style or content constraints beyond the brief? No repo-specific app standard is visible in the current files, so the plan intentionally stays within the brief and the agent definitions.

## Planned handoff notes

This plan is intentionally scoped to documentation and implementation sequencing only. It does not implement the application code itself, in line with the Planner agent definition. It is designed to hand off cleanly to the Orchestrator, then to the Designer and Coder, without creating conflicting file ownership or assuming unsupported tooling beyond the brief.
