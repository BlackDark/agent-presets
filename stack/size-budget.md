# Tech stack: size budgets and bundle reports

## Budget: opt-in

A budget exists only when the project's `AGENTS.md` records it. Size is a design constraint only then.

- No recorded budget: ship the stack and its libraries; size is no reason for own code or a trimmed component.
- Budget wanted (device UI, extension popup or content script): ask the user for the real headroom (free flash/overlay, typical link, first-load target). Record `limit: reason` in `AGENTS.md`.
- Limit: set from that headroom with room for growth, not from a default.
- Check: `size-limit` + `@size-limit/file` in `mise run check`, config `.size-limit.json`:
  ```json
  [{ "path": "dist/**/*.{html,js,css,svg}", "limit": "<recorded> KB", "gzip": true }]
  ```
- Budget hit: run the bundle report, then propose to the user: raise the limit, lazy-load the route, or drop the library. Forking, trimming or rewriting a library to fit needs the user's yes, recorded in an ADR or `AGENTS.md`.

## Bundle report

Shows which module lands in which chunk, raw and gzip. Use it before arguing about size, and attach the Markdown to PRs that add a dependency to a budgeted UI.

- `rollup-plugin-visualizer` (Vite and Rolldown), devDependency, only when `ANALYZE=1`; `stats/` gitignored.
- `mise run analyze` = `ANALYZE=1 pnpm build`; open `stats/treemap.html`.

```ts
import { visualizer } from 'rollup-plugin-visualizer';
import { defineConfig, type PluginOption } from 'vite';

const analyze = process.env.ANALYZE === '1';

export default defineConfig({
  plugins: [
    // ...
    analyze && (visualizer({ filename: 'stats/treemap.html', template: 'treemap', gzipSize: true }) as PluginOption),
    analyze && (visualizer({ filename: 'stats/report.md', template: 'markdown', gzipSize: true }) as PluginOption),
  ],
});
```

ESM only: the package needs `"type": "module"` or a `vite.config.mts`.
