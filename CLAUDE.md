# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

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

There is no test project. CI runs `restore → build -c Release → pack` and uploads the `.nupkg` as an artifact.

Targets `net10.0` (SDK pinned via `global.json` to `10.0.x`, `allowPrerelease: true`). `Directory.Build.props` enables `TreatWarningsAsErrors=true` and `EnforceCodeStyleInBuild=true` for every project — warnings will fail the build.

## Architecture

Two projects in `StickFigurez.sln`:

- `src/StickFigurez` — the published Razor class library (`Microsoft.NET.Sdk.Razor`). Ships as the `StickFigurez` NuGet package.
- `samples/StickFigurez.Demo` — Blazor WebAssembly app (`Microsoft.NET.Sdk.BlazorWebAssembly`) that references the library by `ProjectReference`. Deployed to GitHub Pages (and to `stickfigurez.com`) by `.github/workflows/deploy-pages.yml`.

### Component pattern

Every figure under `src/StickFigurez/Components/` is the same shape:

- `Foo.razor` — inline SVG, `[Parameter] int Size`, `[Parameter] int Speed` (clamped 1–10), and a computed CSS-variable string passed via `style="--foo-duration: …"` on the root `<svg>`.
- `Foo.razor.css` — Blazor-scoped CSS containing the keyframes that consume those custom properties.

Two conventions matter for any new figure:

1. **Color follows `currentColor`.** SVG strokes/fills use `currentColor` so the consumer sets `color:` on a parent. Don't hard-code colors.
2. **Speed → duration mapping is done in C#**, not CSS. The Razor file formats the duration with `CultureInfo.InvariantCulture` (commas-as-decimals locales would otherwise break the inline style) and exposes it as a CSS custom property.
3. **Off-origin rotations use the translate–rotate–translate trick** (see `Locksmith.razor.css` `key-turn`, `Runner.razor` nested joint groups). SVG has no `transform-origin` for inline `<g>` in older Safari; this is the portable workaround.

### Adding a new figure

To add `Foo`:

1. Create `src/StickFigurez/Components/Foo.razor` (+ `.razor.css`) following the pattern above.
2. Register it in the `figures` array in `samples/StickFigurez.Demo/Pages/Home.razor` — the demo's grid, search, and live-snippet rendering all read from this single array (`Name`, keyword list, `RenderFragment`, snippet string).
3. Add a row to the component table and the usage example in `README.md`.

Renaming a component requires updating all three locations plus both `Foo.razor` and `Foo.razor.css` files (filename = component name in Razor).

## Versioning and release

Versioning is driven by [Nerdbank.GitVersioning](https://github.com/dotnet/Nerdbank.GitVersioning) (`version.json`). Public releases come only from `master` or tags matching `v\d+\.\d+(\.\d+)?(-.+)?`.

Release flow:

1. `nbgv prepare-release` creates a `release/v0.x` branch and bumps the minor version on `master`.
2. Tag the release commit `v0.x.y` and push the tag.
3. Manually trigger `Publish NuGet` (`.github/workflows/publish-nuget.yml`, `workflow_dispatch`). It uses NuGet.org **trusted publishing** (OIDC, `id-token: write`) under user `mahlberg` — there is no API-key secret to rotate.

`Directory.Build.props` opts into deterministic, source-linked builds (`EmbedUntrackedSources`, `IncludeSymbols`, snupkg). Don't disable these on the library project — they're what makes the published package debuggable.

## Demo deployment

`deploy-pages.yml` runs on every push to `master`. It publishes the demo, copies `index.html → 404.html` (SPA fallback for GitHub Pages), and `touch`es `.nojekyll`. If you add a route, the SPA fallback already covers it; if you add static assets that Jekyll would mangle (filenames starting with `_`), the `.nojekyll` is what keeps them.
