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
<Runner Size="32" RunSpeed="7" />
<Celebration Size="48" />
<WaitingMan Size="32" Speed="5" />
<LoveMan Size="32" Text="❤" />
<Cloner Size="32" CloneSpeed="6" />
<Locker Size="32" KnockSpeed="5" />
<Labman Size="32" Speed="5" />
<QueryMan Size="32" Speed="5" />
<Statsman Size="32" Speed="5" />
```

Color follows `currentColor` — set CSS `color` on a parent.

## Components

| Component      | Notable parameters                          |
|----------------|---------------------------------------------|
| `Celebration`  | `Size`                                      |
| `Cloner`       | `Size`, `CloneSpeed` (1–10)                 |
| `Labman`       | `Size`, `Speed` (1–10)                      |
| `Locker`       | `Size`, `KnockSpeed` (1–10)                 |
| `LoveMan`      | `Size`, `Speed` (1–10), `Text`              |
| `QueryMan`     | `Size`, `Speed` (1–10)                      |
| `Runner`       | `Size`, `RunSpeed` (1–10), `JackSparrow`    |
| `Statsman`     | `Size`, `Speed` (1–10)                      |
| `WaitingMan`   | `Size`, `Speed` (1–10)                      |

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
