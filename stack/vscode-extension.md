# Tech stack: VS Code extensions

- Build: TypeScript bundled into one file with Vite; Biome; `tsc --noEmit`.
- Package manager: npm may stay (`vsce` works best with it).
- Tests: Vitest for logic; `@vscode/test-electron` inside the editor; CI matrix Linux, Windows, macOS.
- Release: tag `v<version>` → CI builds the `.vsix` → GitHub release with the `.vsix` → publish to Visual Studio Marketplace (`vsce`) and Open VSX (`ovsx`).
