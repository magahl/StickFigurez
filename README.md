# StickFigurez

<img src="assets/logo.svg" alt="StickFigurez" width="320" />

Animated SVG stick-figure components for Blazor.

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
<Celebrator Size="48" Speed="5" />
<Chef Size="32" Speed="5" />
<Cloner Size="32" Speed="6" />
<Coder Size="32" Speed="6" />
<Githubber Size="32" Speed="5" />
<Investigator Size="32" Speed="5" />
<JackSparrow Size="32" Speed="7" />
<Locksmith Size="32" Speed="5" />
<Lover Size="32" Text="❤" />
<Runner Size="32" Speed="7" />
<Scientist Size="32" Speed="5" />
<Teacher Size="32" Speed="5" />
<VibeCoder Size="32" Speed="4" />
<Waiting Size="32" Speed="5" />
```

Color follows `currentColor` — set CSS `color` on a parent.

## Components

| Component      | Notable parameters                          |
|----------------|---------------------------------------------|
| `Celebrator`   | `Size`, `Speed` (1–10)                      |
| `Chef`         | `Size`, `Speed` (1–10)                      |
| `Cloner`       | `Size`, `Speed` (1–10)                      |
| `Coder`        | `Size`, `Speed` (1–10)                      |
| `Githubber`    | `Size`, `Speed` (1–10)                      |
| `Investigator` | `Size`, `Speed` (1–10)                      |
| `JackSparrow`  | `Size`, `Speed` (1–10)                      |
| `Locksmith`    | `Size`, `Speed` (1–10)                      |
| `Lover`        | `Size`, `Speed` (1–10), `Text`              |
| `Runner`       | `Size`, `Speed` (1–10)                      |
| `Scientist`    | `Size`, `Speed` (1–10)                      |
| `Teacher`      | `Size`, `Speed` (1–10)                      |
| `VibeCoder`    | `Size`, `Speed` (1–10)                      |
| `Waiting`      | `Size`, `Speed` (1–10)                      |

## Demo

Live demo: <https://stickfigurez.com/>

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

## Star History

[![Star History Chart](https://api.star-history.com/chart?repos=magahl/StickFigurez&type=date&legend=top-left)](https://www.star-history.com/?repos=magahl%2FStickFigurez&type=date&legend=top-left)

## License

MIT — see [LICENSE](LICENSE).
