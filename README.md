# StickFigurez

Animated SVG stick-figure components for Blazor.

> Placeholder logo — drop a real one at `assets/icon.png` and update the package.

[![CI](https://github.com/magahl/StickFigurez/actions/workflows/ci.yml/badge.svg)](https://github.com/magahl/StickFigurez/actions/workflows/ci.yml)
[![NuGet](https://img.shields.io/nuget/v/StickFigurez.svg)](https://www.nuget.org/packages/StickFigurez)

## Install

```bash
dotnet add package StickFigurez
```

## Use

In `_Imports.razor`:

```razor
@using StickFigurez.Components
```

In a page or component:

```razor
<Runner Size="32" Speed="7" />
<JackSparrow Size="32" Speed="7" />
<Celebrator Size="48" Speed="5" />
<Waiting Size="32" Speed="5" />
<Lover Size="32" Text="❤" />
<Cloner Size="32" Speed="6" />
<Locksmith Size="32" Speed="5" />
<Scientist Size="32" Speed="5" />
<Searcher Size="32" Speed="5" />
<Teacher Size="32" Speed="5" />
```

Color follows `currentColor` — set CSS `color` on a parent.

## Components

| Component      | Notable parameters                          |
|----------------|---------------------------------------------|
| `Celebrator`   | `Size`, `Speed` (1–10)                      |
| `Cloner`       | `Size`, `Speed` (1–10)                      |
| `Scientist`    | `Size`, `Speed` (1–10)                      |
| `Locksmith`    | `Size`, `Speed` (1–10)                      |
| `Lover`        | `Size`, `Speed` (1–10), `Text`              |
| `Searcher`     | `Size`, `Speed` (1–10)                      |
| `JackSparrow`  | `Size`, `Speed` (1–10)                      |
| `Runner`       | `Size`, `Speed` (1–10)                      |
| `Teacher`      | `Size`, `Speed` (1–10)                      |
| `Waiting`      | `Size`, `Speed` (1–10)                      |

## Demo

Live demo: <https://magahl.github.io/StickFigurez/>

Run locally:

```bash
dotnet run --project samples/StickFigurez.Demo
```

## Build

```bash
dotnet build
dotnet pack src/StickFigurez -c Release -o artifacts
```

Versioning via [Nerdbank.GitVersioning](https://github.com/dotnet/Nerdbank.GitVersioning) — see `version.json`.

## Release

1. `nbgv prepare-release` (creates a `release/v0.x` branch).
2. Tag the release commit `v0.x.y` and push the tag.
3. CI publishes the package to NuGet.org.

## License

MIT — see [LICENSE](LICENSE).
