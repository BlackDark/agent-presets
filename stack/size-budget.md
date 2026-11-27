# Tech stack: size budgets and bundle reports

## Budget: opt-in

A budget exists only when the project's `AGENTS.md` records it. Size is a design constraint only then.

- No recorded budget: ship the stack and its libraries; size is no reason for own code or a trimmed component.
- Budget wanted (device UI, extension popup or content script): ask the user for the real headroom. Record each limit as `limit: reason` in `AGENTS.md`.
- Two measures, each its own limit; record only the ones that apply:
  - **flash**: every file in the deployed UI directory as stored on the device (the `.gz` when the device stores those). Remedies: raise the limit, drop the library.
  - **first load**: files the start page loads, gzip. Remedies: raise the limit, lazy-load the route, drop the library.
- Limit: set from the headroom with room for growth, not from a default.
- Check: `size-limit` + `@size-limit/file` in `mise run check`, config `.size-limit.json`. Output dir: `build/` for SvelteKit `adapter-static` (or its `pages`), `dist/` for plain Vite.
  ```json
  [
    { "name": "flash", "path": "build/**/*.gz", "limit": "<recorded> KB", "brotli": false },
    { "name": "first load", "path": ["build/index.html", "build/_app/immutable/entry/*.js"], "limit": "<recorded> KB", "gzip": true }
  ]
  ```
  Device without `.gz` files: flash path `build/**`. Size-limit compresses with brotli unless `brotli: false` or `gzip: true` is set.
- Budget hit: run the bundle report, then propose a remedy for the measure that failed. Forking, trimming or rewriting a library to fit needs the user's yes, recorded in an ADR or `AGENTS.md`.

## Bundle report

Shows which module lands in which chunk, raw and gzip. Use it before arguing about size, and attach the Markdown to PRs that add a dependency to a budgeted UI.

- `rollup-plugin-visualizer` (Vite and Rolldown), devDependency, only when `ANALYZE=1`; `stats/` gitignored.
- `mise run analyze` = `ANALYZE=1 pnpm build`; open `stats/treemap.html`.
- `applyToEnvironment` keeps the report on the client build; without it SvelteKit's server build writes the same files.

```ts
import { visualizer } from 'rollup-plugin-visualizer';
import { defineConfig, type Plugin } from 'vite';

const analyze = process.env.ANALYZE === '1';

function bundleReport(): Plugin[] {
  const reports = [
    { filename: 'stats/treemap.html', template: 'treemap' },
    { filename: 'stats/report.md', template: 'markdown' },
  ] as const;
  return reports.map((report) => ({
    ...(visualizer({ ...report, gzipSize: true }) as Plugin),
    applyToEnvironment: (env) => env.name === 'client',
  }));
}

export default defineConfig({
  plugins: [
    // ...
    ...(analyze ? bundleReport() : []),
  ],
});
```

ESM only: the package needs `"type": "module"` or a `vite.config.mts`. `process` needs `@types/node`.
