# CLAUDE.md

Guide Claude Code (claude.ai/code) for this repo.

## Commands

```bash
# Build the whole solution (lib + demo)
dotnet build

# Run the Blazor WebAssembly demo locally (hot reload)
dotnet run --project samples/StickFigurez.Demo

# Pack the NuGet package
dotnet pack src/StickFigurez -c Release -o artifacts

# Resolve the version Nerdbank.GitVersioning will assign
dotnet tool install --tool-path .tools nbgv   # one-time
.tools/nbgv get-version
```

No test project. CI run `restore → build -c Release → pack`, upload `.nupkg` as artifact.

Target `net10.0` (SDK pin via `global.json` to `10.0.x`, `allowPrerelease: true`). `Directory.Build.props` set `TreatWarningsAsErrors=true` + `EnforceCodeStyleInBuild=true` every project — warning fail build.

## Architecture

Two projects in `StickFigurez.sln`:

- `src/StickFigurez` — published Razor class library (`Microsoft.NET.Sdk.Razor`). Ship as `StickFigurez` NuGet package.
- `samples/StickFigurez.Demo` — Blazor WebAssembly app (`Microsoft.NET.Sdk.BlazorWebAssembly`), reference library via `ProjectReference`. Deploy to GitHub Pages (+ `stickfigurez.com`) by `.github/workflows/deploy-pages.yml`.

### Component pattern

Every figure under `src/StickFigurez/Components/` same shape:

- `Foo.razor` — inline SVG, `[Parameter] int Size`, `[Parameter] int Speed` (clamp 1–10), computed CSS-variable string via `style="--foo-duration: …"` on root `<svg>`.
- `Foo.razor.css` — Blazor-scoped CSS, keyframes consume those custom properties.

Two conventions for new figure:

1. **Color follows `currentColor`.** SVG stroke/fill use `currentColor`; consumer set `color:` on parent. No hard-code colors.
2. **Speed → duration mapping in C#**, not CSS. Razor file format duration with `CultureInfo.InvariantCulture` (comma-decimal locales break inline style otherwise), expose as CSS custom property.
3. **Off-origin rotations use translate–rotate–translate trick** (see `Locksmith.razor.css` `key-turn`, `Runner.razor` nested joint groups). SVG no `transform-origin` for inline `<g>` in older Safari; portable workaround.

### Adding a new figure

Add `Foo`:

1. Create `src/StickFigurez/Components/Foo.razor` (+ `.razor.css`), follow pattern above.
2. Register in `figures` array in `samples/StickFigurez.Demo/Pages/Home.razor` — demo grid, search, live-snippet render all read from this one array (`Name`, keyword list, `RenderFragment`, snippet string).
3. Add row to component table + usage example in `README.md`.

Rename component = update all three spots + both `Foo.razor` and `Foo.razor.css` files (filename = component name in Razor).

## Versioning and release

Version driven by [Nerdbank.GitVersioning](https://github.com/dotnet/Nerdbank.GitVersioning) (`version.json`). Public release only from `master` or tags match `v\d+\.\d+(\.\d+)?(-.+)?`.

Release flow:

1. `nbgv prepare-release` create `release/v0.x` branch, bump minor on `master`.
2. Tag release commit `v0.x.y`, push tag.
3. Manual trigger `Publish NuGet` (`.github/workflows/publish-nuget.yml`, `workflow_dispatch`). Use NuGet.org **trusted publishing** (OIDC, `id-token: write`) under user `mahlberg` — no API-key secret to rotate.

`Directory.Build.props` opt into deterministic, source-linked builds (`EmbedUntrackedSources`, `IncludeSymbols`, snupkg). No disable on library project — these make published package debuggable.

## Demo deployment

`deploy-pages.yml` run every push to `master`. Publish demo, copy `index.html → 404.html` (SPA fallback for GitHub Pages), `touch` `.nojekyll`. Add route = SPA fallback cover it; add static assets Jekyll mangle (filename start `_`) = `.nojekyll` keep them.