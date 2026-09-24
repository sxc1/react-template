# React template implementation brief

## Objective and context

Build a barebones, single-page React app in this repository, styled from the `sxc1/design` Git submodule and deployed to GitHub Pages. This brief is self-contained: implementation must not depend on access to the original conversation or screenshot.

- Repository: https://github.com/sxc1/react-template
- Expected production URL: https://sxc1.github.io/react-template/
- Initial state: README, license, and this plan; no app implementation.

Before starting, inspect the current files and working tree, preserve user changes, and account for work already completed. When asked to execute this plan, implement the requirements below and update the five checkboxes as their completion criteria are met. Record unfinished work and verification blockers at the end of this file.

## Scope and visual direction

The page contains only a header and a central hero with the content specified below. Do not add a footer, extra sections, cards, authentication, routing, a theme switcher, or additional marketing copy.

Use dark mode by default. Source colors, font stacks, spacing, and radii from the imported theme wherever practical; local CSS should primarily handle layout and decorative gradients. Center header navigation independently of the different widths of the brand and info button. On narrow screens, allow a compact second navigation row if needed to keep all controls visible without horizontal overflow.

## Implementation checklist

### 1. App foundation

- [x] Create the minimal React app.

Use Vite, React, TypeScript, npm, and Tailwind CSS v4 with its Vite integration. Tailwind is needed to process the upstream theme's Tailwind-specific directives. Choose mutually compatible supported versions when implementing and commit the npm lockfile.

Keep the implementation small: an entry point, an App component containing the header and hero, and an app stylesheet are sufficient. Use an inline SVG for the circle-i icon instead of adding an icon library. No router, backend, or state management library is needed.

Provide `dev`, `build`, and `preview` scripts, with TypeScript checking included in the build. Add the Vite/TypeScript configuration, HTML entry point, and ignore rules for dependencies and build output. Preserve the existing license.

**Complete when:** `npm ci` installs dependencies reproducibly, the development server starts, and `npm run build` produces `dist` without TypeScript errors.

### 2. Shared design theme

- [x] Integrate the design repository as a Git submodule.

Add https://github.com/sxc1/design.git at `vendor/design`, committing both `.gitmodules` and the submodule gitlink. Local builds and CI must use the committed submodule revision, not automatically fetch the latest upstream branch.

Import `vendor/design/sxc1-design-tokens.css` through the app's CSS entry point and process it with Tailwind v4. The inspected upstream file includes `@import "tailwindcss"`, semantic variables in `:root` and `.dark`, and typography, spacing, radius, and shadow tokens. Apply `.dark` at the document root. Confirm the actual file and token names when implementing. Do not copy tokens into the app or edit the submodule to customize this page.

Theme reference: https://github.com/sxc1/design/blob/main/sxc1-design-tokens.css

**Complete when:** a fresh clone with submodules can build, the theme import is processed successfully, and the rendered page uses the imported dark theme.

### 3. Page content and behavior

- [x] Implement the exact header and hero.

| Location | Content | Behavior |
| --- | --- | --- |
| Header, left | `sxc1` | Plain brand text |
| Header, center | `About` | Placeholder button |
| Header, center | `GitHub` | Link to https://github.com/sxc1 |
| Header, center | `FAQ` | Placeholder button |
| Header, right | Circle-i icon | Placeholder button with accessible name `Information` |
| Hero heading | `Template for sxc1` | The page's single h1 |
| Hero subtext | `Theme derived from https://github.com/sxc1/design` | Make the URL a link to https://github.com/sxc1/design |
| Hero action | `Explore` | Pill-shaped link to https://sxc1.github.io/design/ |

Render center navigation in the listed order. Use semantic header, nav, and main elements, real anchors for destinations, and `type="button"` for placeholders. Placeholder buttons do not navigate, open dialogs, or add content; their future behavior remains unimplemented. This placeholder treatment for info is a planning assumption because no action was specified. Keep external links in the same tab by default. Hide decorative SVG content from assistive technology.

Follow the visual direction above. Provide readable contrast, visible keyboard focus, usable pointer targets, and responsive wrapping, including the long theme URL. No animation dependencies are necessary.

**Complete when:** the page contains exactly the requested visible content, all three external links have the correct destinations, placeholders cause no navigation or errors, and the layout works at mobile and desktop widths.

### 4. GitHub Pages deployment

- [ ] Configure and verify GitHub Pages deployment.

Set Vite's `base` to `/react-template/` and add `public/.nojekyll`. Use asset imports that work under the project subpath.

Create `.github/workflows/deploy.yml`, adapting these references to this repository:

- Initial Pages configuration: https://github.com/sxc1/fh6/commit/ab1dcf39d8b63581d6fd950a06ab16d025280b90
- Deployment diagnostics: https://github.com/sxc1/fh6/commit/31ed1e3aa8d9fe24949ff6ee6f422181003f48fb

The initial reference uses separate build and deployment jobs, uploads `dist` as a Pages artifact, sets permissions and deployment concurrency, and configures a Vite base path. The second adds failure-only reporting of Pages configuration and deployment API state. Preserve these useful behaviors while adapting repository-specific values.

Workflow requirements:

- Validate pull requests to `main`; build and deploy pushes to `main`. Support manual dispatch, with deployment restricted to `main`.
- Check out recursive submodules at their pinned revisions.
- Use a supported Node version compatible with the selected Vite version, npm caching, `npm ci`, and `npm run build`.
- Pull requests only validate the build and must not deploy or require Pages write permissions.
- For production, configure Pages, upload `dist`, and deploy using official GitHub Pages actions. Verify supported action versions during implementation.
- Grant `contents: read`, `pages: write`, and `id-token: write` where needed; use the `github-pages` environment and deployment URL output.
- Serialize production deployments without canceling an in-progress deployment.
- Include failure-only Pages diagnostics following the second reference. Diagnostic failures must not obscure the original deployment error.

Set the repository's Pages source to **GitHub Actions** when repository access and the execution request permit it. If publishing or remote configuration is unavailable, finish the local configuration and record the exact remaining action. A configured workflow alone does not demonstrate a successful live deployment.

**Complete when:** the workflow is committed and deployment succeeds at the expected URL with assets loading correctly. If only configuration is complete, leave this checkbox open and record that distinction below.

### 5. Verification and handoff

- [ ] Verify the result and document how to use the template.

Run the production build and inspect its preview under `/react-template/`. Check representative narrow mobile and desktop widths for header alignment, readable text, wrapping, and overflow. Verify keyboard focus, all link destinations, and placeholder behavior. Check for console errors and failed asset requests. After publishing, load and refresh the live project URL and confirm styles and scripts load successfully.

Keep validation proportional to this small static page; an elaborate test framework is unnecessary. Report actual checks performed and any checks that could not be completed.

Update README.md with:

- Project purpose and production URL.
- Node/npm prerequisites and installation, development, build, and preview commands.
- Cloning with `git clone --recurse-submodules` and initializing an existing clone with `git submodule update --init --recursive`.
- How to intentionally update the design submodule and commit its new revision.
- Pages workflow behavior, the required repository setting, and deployment troubleshooting.
- The Vite base path and what to change when copying this template to a differently named repository.

**Complete when:** local verification passes, README instructions match the implementation, and live verification results or remaining blockers are recorded accurately.

## Progress notes and blockers

- Steps 1–3 implemented: Vite 8, React 19, TypeScript, Tailwind CSS v4 with its Vite plugin, npm lockfile, and the requested header and hero.
- The unchanged design submodule is pinned to `9f5c276f1c1a18bb2fa3067d9e3cab1611253f39`. Its stylesheet is imported directly; `.dark` is applied to the document root.
- Verification: `npm ci` and `npm run build` pass; `npm run dev` starts successfully. Browser checks at 320px and 1440px confirmed no horizontal document overflow, centered desktop navigation, the imported dark background, all three exact link destinations, inert placeholder buttons, and visible keyboard focus. No browser console warnings or errors were reported during these checks.
- No commits were created, as requested. `git submodule add` staged `.gitmodules` and the pinned gitlink; application files and this brief remain uncommitted. A fresh clone of the final implementation cannot be verified until it is committed; the current pinned checkout builds successfully.
- Steps 4–5 remain open and outside this request: Pages base path, `.nojekyll`, deployment workflow/settings, README expansion, production preview and live deployment verification. The app currently uses Vite's default `/` base.
