# Development and Deployment Guide

## Branch strategy

Suggested:

```text
main        production
staging     pre-production validation
feature/*   individual changes
fix/*       targeted fixes
```

Do not develop directly in production without retaining a known-good tag or commit.

## Local development

The app fetches JSON, so opening `index.html` through a `file://` URL can fail. Use a static local web server or a preview environment.

Keep development, test, and production data separate. Do not commit confidential operational data to a public repository.

## File ownership

- UI structure: `index.html`
- Styling/responsive behavior: `css/app.css`
- Client state and workflows: `js/app.js`
- Station source data: `data/stations.json`
- Device source data: `data/devices.json`
- Lookups: `data/lookups.json`

After backend migration, add modules such as:

```text
js/api.js
js/state.js
js/filters.js
js/audit.js
js/render.js
```

Avoid allowing one `app.js` file to grow indefinitely.

## Change process

1. Write the problem and acceptance criteria.
2. Identify data-model impact.
3. Identify desktop, iPad, and phone behavior.
4. Make the smallest coherent change.
5. Run syntax and data checks.
6. Run targeted workflow tests.
7. Run responsive checks.
8. Verify export/recovery.
9. Commit with a descriptive message.
10. Validate the Cloudflare preview/deployment.
11. Run production smoke test.
12. Update handoff/changelog.

## Suggested commit messages

```text
feat: add multi-select data health filters
fix: prevent phone audit horizontal overflow
refactor: split API and rendering modules
chore: validate station and device JSON
 docs: update database migration plan
```

## Cloudflare deployment checks

- Confirm the Worker/Pages project points to the intended repository and branch.
- Confirm root directory is correct.
- Confirm `index.html` is at the served root.
- Confirm static assets and data paths are case-correct.
- Review deployment logs.
- Confirm the production URL serves the new commit.
- If a stale version appears, verify cache headers and use versioned asset URLs rather than asking users to repeatedly clear all browser data.

## Environment configuration after database migration

Do not store secrets in GitHub source files or browser JavaScript.

Use deployment environment variables for:

- API base URL.
- Public client configuration when appropriate.
- Server-side database credentials.
- Storage configuration.
- Logging keys.

Only public, restricted client keys may be sent to the browser. Privileged keys must remain server-side.

## Changelog format

```markdown
## YYYY-MM-DD

### Added
- ...

### Changed
- ...

### Fixed
- ...

### Data migrations
- ...

### Validation performed
- ...

### Known issues
- ...
```
