# Tech stack: size budgets and bundle reports

## Budget: opt-in

A budget exists only when the project's `AGENTS.md` records it. Size is a design constraint only then.

- No recorded budget: ship the stack and its libraries; size is no reason for own code or a trimmed component.
- Budget wanted (device UI, extension popup or content script): ask the user for the real headroom. Record each limit as `limit: reason` in `AGENTS.md`.
- Two measures, each its own limit; record only the ones that apply:
  - **flash**: every file the device stores, in its stored form (the `.gz` in place of its original, images as they are). Remedies: raise the limit, drop the library.
  - **first load**: every response the browser fetches for the start page (HTML, CSS, entry and preloaded chunks), gzip. Remedies: raise the limit, lazy-load the route, drop the library.
- Limit: set from the headroom with room for growth, not from a default.
- Output dir: `build/` for SvelteKit `adapter-static` (or its `pages`), `dist/` for plain Vite. Both checks run in `mise run check`.
- Flash check: `size-limit` + `@size-limit/file`, config `.size-limit.json`. List what ships; exclude originals the device replaces by `.gz`:
  ```json
  [{ "name": "flash", "path": ["build/**", "!build/**/*.{html,js,css,json,svg}"], "limit": "<recorded> KB", "brotli": false }]
  ```
  Device stores no `.gz`: path `build/**`. Size-limit compresses with brotli unless `brotli: false` is set.
- First-load check: a Playwright test against `vite preview`. A file glob misses preloaded chunks and route CSS; the browser does not.
  ```ts
  import { gzipSync } from 'node:zlib';
  import { expect, test } from '@playwright/test';

  const LIMIT_KB = 0; // recorded limit

  test('first load fits the budget', async ({ page }) => {
    const sizes: Promise<number>[] = [];
    page.on('response', (response) => {
      sizes.push(response.body().then((body) => gzipSync(body).length, () => 0));
    });
    await page.goto('/', { waitUntil: 'networkidle' });
    const total = (await Promise.all(sizes)).reduce((sum, bytes) => sum + bytes, 0);
    expect(total).toBeLessThanOrEqual(LIMIT_KB * 1024);
  });
  ```
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
