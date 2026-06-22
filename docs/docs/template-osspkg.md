---
sidebar_position: 3
title: Open Source Package Templates
---

The templates in this section are designed to be used independently or together.
They are presented in a progressive order, where each template may build on files
introduced by previous ones. The final template, `osspkg`, combines all of them
into a complete repository structure for an open source C# package.

| Short Name | Description |
|------------|-------------|
| `strongname` | Adds a strong naming guide to the current directory. |
| `docs` | Adds a Docusaurus setup guide to the current directory. |
| `props` | Adds Directory.Build.props and version.json to the src directory. |
| `coverage` | Adds test coverage tooling to the current directory. |
| `github` | Adds GitHub community files to the current directory. |
| `repofiles` | Adds common OSS project files to the current directory. |
| `osspkg` | Repository structure for an open source C# package with docs, versioning, and CI/CD. |

---

## Strong Naming Guide (`strongname`)

Strong naming gives an assembly a unique identity by signing it with a cryptographic
key pair. It is required for assemblies that need to be installed in the Global
Assembly Cache (GAC), and is a common practice for public NuGet packages so consumers
can verify they are referencing the correct binary. If strong naming is not a
requirement for your project, remove the `StrongNaming` property group from
`src/Directory.Build.props` and skip this guide entirely.

Adds a strong naming guide to the current directory. Once you have completed the
strong naming setup for your project, this file can and should be removed.

### Parameters

None. This template copies files without substitution.

### Usage

```bash
dotnet new strongname
```

### Output

| File | Description |
|------|-------------|
| `STRONG_NAMING.md` | A guide to strong naming your .NET assembly. |

---

## Docusaurus Setup Guide (`docs`)

Good documentation is one of the most important things an open source project can
have. It reduces the support burden, lowers the barrier to adoption, and signals
that the project is well-maintained. Docusaurus publishes your documentation as a
static site hosted on GitHub Pages, meaning it is versioned alongside your code,
freely hosted, and always in sync with your repository. If you do not plan to
publish documentation for your project, this file can be removed without affecting
anything else.

Adds a Docusaurus setup guide to the current directory. Once you have completed
the Docusaurus setup for your project, this file can and should be removed.

### Parameters

None. This template copies files without substitution.

### Usage

```bash
dotnet new docs
```

### Output

| File | Description |
|------|-------------|
| `DOCUSAURUS.md` | A guide to setting up and configuring a Docusaurus documentation site. |

---

## Directory Build Props (`props`)

Adds `Directory.Build.props` and `version.json` to the `src` directory.

`Directory.Build.props` is an MSBuild file that automatically applies shared
configuration to every project in the `src` directory, so you only have to define
it once. This template's version configures:

- **Target frameworks:** multi-targets `netstandard2.0`, `netstandard2.1`, and
  `net6.0` through `net10.0` out of the box
- **Strong naming:** signs the assembly using a `.snk` key file
- **Repository metadata:** authors, copyright, repository URL, and GitHub Pages
  documentation URL, all populated from template parameters
- **Packaging:** dual MIT/Apache-2.0 license expression, symbol packages,
  package validation, and README inclusion
- **Source Link:** embeds source file information into the package so debuggers
  can step into your library's source code
- **Nerdbank.GitVersioning:** version is derived automatically from git history
- **Code style:** nullable reference types, implicit usings, latest language
  version, and code style enforced at build time

`version.json` configures the initial version for Nerdbank.GitVersioning.

### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `--authorName` | No | `Scott Offen` | The name of the package author. |
| `--version` | No | `1.0.0-preview.{height}` | The initial version for Nerdbank.GitVersioning. |

### Usage

```bash
dotnet new props
```

```bash
dotnet new props --authorName "Jane Smith" --version "1.0.0"
```

### Output

| File | Description |
|------|-------------|
| `src/Directory.Build.props` | Shared MSBuild properties applied to all projects in the `src` directory. |
| `src/version.json` | Nerdbank.GitVersioning configuration file. |

```
src/
├── Directory.Build.props
└── version.json
```

---

## Test Coverage (`coverage`)

Adds test coverage tooling to the current directory. Running `test-coverage.ps1`
from the repository root automatically discovers every solution file under `src/`,
builds and tests each one, merges the coverage output from all test projects into
a single combined report, and opens it in your browser. Pass `-NoOpen` to skip
opening the browser, which makes the script safe to run in CI.

ReportGenerator is used to produce the HTML report. The script prefers the local
tool manifest at `src/.config/dotnet-tools.json` when present, and falls back to
a globally installed `reportgenerator` if not. Coverage collection is handled by
Coverlet, configured via `src/coverlet.runsettings`.

### Parameters

None. This template copies files without substitution.

### Usage

```bash
dotnet new coverage
```

### Output

| File | Description |
|------|-------------|
| `test-coverage.ps1` | PowerShell script for generating and opening a test coverage report. |
| `src/coverlet.runsettings` | Coverlet configuration for controlling coverage collection. |
| `src/.config/dotnet-tools.json` | Local .NET tools manifest, including the ReportGenerator tool. |

```
src/
├── .config/
│   └── dotnet-tools.json
└── coverlet.runsettings
test-coverage.ps1
```

---

## GitHub Community Files (`github`)

A healthy open source project needs more than just code. GitHub community files
set expectations for contributors, establish a code of conduct, provide structured
ways to report bugs and request features, and automate the most important parts of
the release pipeline. Without them, contributors have no guidance, issues become
noise, and releases are manual and error-prone. This template adds the full set of
community health files that GitHub recognizes, along with issue templates and the
GitHub Actions workflows for building, testing, publishing, and deploying
documentation.

### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `--name` | No | `Blueprint` | Used to populate placeholders in the generated files. |
| `--org` | No | `scottoffen` | The GitHub organization or username that owns the repository. |
| `--repo` | Yes | none | The GitHub repository name. |

### Usage

```bash
dotnet new github --name MyPackage --repo my-repo
```

```bash
dotnet new github --name MyPackage --org my-org --repo my-repo
```

### Output

All files are added under the `.github/` folder.

| File | Description |
|------|-------------|
| `CODEOWNERS` | Defines code owners for automatic review assignment. |
| `code_of_conduct.md` | Community code of conduct. |
| `contributing.md` | Guidelines for contributing to the project. |
| `PULL_REQUEST_TEMPLATE.md` | Default pull request template. |
| `security.md` | Security policy and vulnerability reporting instructions. |
| `ISSUE_TEMPLATE/bug_report.yml` | Issue template for bug reports. |
| `ISSUE_TEMPLATE/config.yml` | Issue template chooser configuration. |
| `ISSUE_TEMPLATE/documentation.yml` | Issue template for documentation issues. |
| `ISSUE_TEMPLATE/feature_request.yml` | Issue template for feature requests. |
| `workflows/cleanup.yml` | Workflow for cleaning up stale packages. |
| `workflows/deploy-docs.yml` | Workflow for deploying the Docusaurus documentation site to GitHub Pages. |
| `workflows/pr-build.yml` | Workflow for building and testing pull requests. |
| `workflows/publish.yml` | Workflow for publishing the package to NuGet and GitHub Packages. |

```
.github/
├── ISSUE_TEMPLATE/
│   ├── bug_report.yml
│   ├── config.yml
│   ├── documentation.yml
│   └── feature_request.yml
├── workflows/
│   ├── cleanup.yml
│   ├── deploy-docs.yml
│   ├── pr-build.yml
│   └── publish.yml
├── CODEOWNERS
├── code_of_conduct.md
├── contributing.md
├── PULL_REQUEST_TEMPLATE.md
└── security.md
```

---

## OSS Project Files (`repofiles`)

A well-structured open source repository communicates professionalism and
intentionality from the first glance. This template adds the foundational files
that every open source project should have:

- **`.editorconfig`:** Enforces consistent code style across editors and IDEs,
  ensuring that contributors are not introducing whitespace or formatting noise
  regardless of what tools they use.
- **`.gitignore`:** Prevents build artifacts, user-specific IDE files, and other
  generated content from being accidentally committed to the repository.
- **`LICENSE-MIT` and `LICENSE-APACHE`:** Dual licensing under MIT and Apache 2.0
  is a common and well-regarded practice in the .NET open source ecosystem. MIT is
  permissive and widely understood. Apache 2.0 adds explicit patent protection,
  which matters to organizations with legal teams that review dependencies. Offering
  both lets consumers choose the license that best fits their needs, which lowers
  the barrier to adoption. If you prefer a single license, remove the file you do
  not want and update the `PackageLicenseExpression` in `src/Directory.Build.props`
  from `MIT OR Apache-2.0` to either `MIT` or `Apache-2.0`.

### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `--name` | No | `Blueprint` | Used to populate placeholders in the generated files. |
| `--org` | No | `scottoffen` | The GitHub organization or username that owns the repository. |
| `--repo` | Yes | none | The GitHub repository name. |
| `--authorName` | No | `Scott Offen` | The name of the package author. |

### Usage

```bash
dotnet new repofiles --name MyPackage --repo my-repo
```

```bash
dotnet new repofiles --name MyPackage --org my-org --repo my-repo --authorName "Jane Smith"
```

### Output

Includes everything from [`github`](#github-community-files-github), plus:

| File | Description |
|------|-------------|
| `.editorconfig` | Editor configuration for consistent code style across editors and IDEs. |
| `.gitignore` | Git ignore rules for .NET projects. |
| `LICENSE-MIT` | MIT license file. |
| `LICENSE-APACHE` | Apache 2.0 license file. |

```
.github/
├── ISSUE_TEMPLATE/
│   ├── bug_report.yml
│   ├── config.yml
│   ├── documentation.yml
│   └── feature_request.yml
├── workflows/
│   ├── cleanup.yml
│   ├── deploy-docs.yml
│   ├── pr-build.yml
│   └── publish.yml
├── CODEOWNERS
├── code_of_conduct.md
├── contributing.md
├── PULL_REQUEST_TEMPLATE.md
└── security.md
.editorconfig
.gitignore
LICENSE-APACHE
LICENSE-MIT
```

---

## Open Source .NET Package (`osspkg`)

A complete repository structure for an open source C# package, including
documentation, strong naming, versioning, test coverage, and GitHub Actions for
publishing to NuGet and GitHub Packages.

The generated solution contains two projects: the main package project and a test
project. The test project already references the main project and includes
xUnit, Shouldly, Moq, and Coverlet as dependencies. xUnit, Shouldly, and Moq are
globally imported, so test classes do not need explicit `using` statements for any
of them. The main project already has an `InternalsVisibleTo` declaration wired up
for the test project, so internal members are accessible in tests without any
additional configuration.

### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `--name` | No | `Blueprint` | The name of the package. |
| `--org` | No | `scottoffen` | The GitHub organization or username that owns the repository. |
| `--repo` | Yes | none | The GitHub repository name. |
| `--authorName` | No | `Scott Offen` | The name of the package author. |
| `--version` | No | `1.0.0-preview.{height}` | The initial version for Nerdbank.GitVersioning. |

### Usage

```bash
dotnet new osspkg --name MyPackage --repo my-repo
```

```bash
dotnet new osspkg --name MyPackage --org my-org --repo my-repo --authorName "Jane Smith" --version "1.0.0-preview.{height}"
```

### Output

Includes everything from [`repofiles`](#oss-project-files-repofiles),
[`props`](#directory-build-props-props), and [`coverage`](#test-coverage-coverage), plus:

| File | Description |
|------|-------------|
| `README.md` | Starter documentation for the new repository. |
| `src/Blueprint.slnx` | The solution file for the package. |
| `src/Blueprint/Blueprint.csproj` | The main package project file. |
| `src/Blueprint.Tests/Blueprint.Tests.csproj` | The test project file. |

```
MyPackage/
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.yml
│   │   ├── config.yml
│   │   ├── documentation.yml
│   │   └── feature_request.yml
│   ├── workflows/
│   │   ├── cleanup.yml
│   │   ├── deploy-docs.yml
│   │   ├── pr-build.yml
│   │   └── publish.yml
│   ├── CODEOWNERS
│   ├── code_of_conduct.md
│   ├── contributing.md
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── security.md
├── src/
│   ├── .config/
│   │   └── dotnet-tools.json
│   ├── MyPackage/
│   │   └── MyPackage.csproj
│   ├── MyPackage.Tests/
│   │   └── MyPackage.Tests.csproj
│   ├── coverlet.runsettings
│   ├── Directory.Build.props
│   ├── MyPackage.slnx
│   └── version.json
├── .editorconfig
├── .gitignore
├── DOCUSAURUS.md
├── LICENSE-APACHE
├── LICENSE-MIT
├── README.md
├── STRONG_NAMING.md
└── test-coverage.ps1
```