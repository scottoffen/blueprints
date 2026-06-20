# Blueprints

A collection of `dotnet new` templates for common project types and workflows.

📖 [Documentation](https://scottoffen.github.io/blueprints)

## Installation

Blueprints is published to GitHub Packages. To install it, you'll need a GitHub
personal access token (PAT) with at least `read:packages` scope.

### 1. Create a PAT

Go to [GitHub Settings → Developer Settings → Personal Access Tokens](https://github.com/settings/tokens)
and create a token with the `read:packages` scope.

### 2. Store the PAT as an Environment Variable

Store your PAT as an environment variable rather than entering it in plain text on
the command line.

**macOS/Linux**
```bash
export GITHUB_PAT=your_pat_here
```

**PowerShell**
```powershell
$env:GITHUB_PAT = "your_pat_here"
```

**Windows (Command Prompt)**
```cmd
setx GITHUB_PAT "your_pat_here"
```

### 3. Add the NuGet Source

The `--name` flag sets the key used to identify this source in your local NuGet
configuration. It can be anything you like -- the value below is just a suggestion.

**macOS/Linux**
```bash
dotnet nuget add source "https://nuget.pkg.github.com/scottoffen/index.json" \
  --name "scottoffen" \
  --username YOUR_GITHUB_USERNAME \
  --password $GITHUB_PAT
```

**PowerShell**
```powershell
dotnet nuget add source "https://nuget.pkg.github.com/scottoffen/index.json" `
  --name "scottoffen" `
  --username YOUR_GITHUB_USERNAME `
  --password $env:GITHUB_PAT
```

**Windows (Command Prompt)**
```cmd
dotnet nuget add source "https://nuget.pkg.github.com/scottoffen/index.json" ^
  --name "scottoffen" ^
  --username YOUR_GITHUB_USERNAME ^
  --password %GITHUB_PAT%
```

### 4. Install the Package

```bash
dotnet new install ScottOffen.Blueprints
```

## Updating

To update to the latest version:

```bash
dotnet new update ScottOffen.Blueprints
```

## Uninstalling

To remove the templates:

```bash
dotnet new uninstall ScottOffen.Blueprints
```

## Templates

| Short Name | Description |
|------------|-------------|
| _coming soon_ | _coming soon_ |

## Template Naming Convention

Template folder names follow the convention `template-[scope]-[ecosystem]-[descriptor]`.

**Scope** describes what the template creates:

| Value | Description |
|-------|-------------|
| `repo` | An entire repository structure |
| `sln` | A solution structure (multiple projects) |
| `project` | A single project |
| `partial` | A partial project addition (e.g., a Docker container, a GitHub Actions workflow) |

**Ecosystem** identifies the technology:

| Value | Description |
|-------|-------------|
| `dotnet` | C# / .NET |
| `php` | PHP |
| `js` | JavaScript |
| `ts` | TypeScript |
| `docker` | Docker |
| `gh` | GitHub (Actions, workflows, etc.) |

**Descriptor** is a short name for what the template specifically creates, such as
`osspkg`, `webapi`, `console`, `mysql`, or `postgres`.

## License

This project is dual licensed under the [MIT License](./LICENSE-MIT) and the
[Apache License 2.0](./LICENSE-APACHE). You may choose either license at your option.