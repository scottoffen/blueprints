---
sidebar_position: 2
title: Getting Started
---

# Installation

Blueprints is published to GitHub Packages. To install it, you will need a GitHub
personal access token (PAT) with at least `read:packages` scope.

## Install

### 1. Create a Personal Access Token (PAT)

Go to [GitHub Settings → Developer Settings → Personal Access Tokens](https://github.com/settings/tokens)
and create a token with the `read:packages` scope.

### 2. Store the Personal Access Token as an Environment Variable

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
configuration. It can be anything you like. The value below is just a suggestion.
The URL must reference `scottoffen` as the GitHub username, as that is where the
package is published.

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

## Update

To update to the latest version:

```bash
dotnet new update ScottOffen.Blueprints
```

## Uninstall

To remove the templates:

```bash
dotnet new uninstall ScottOffen.Blueprints
```

## Troubleshooting

### Personal Access Token (PAT) Expiration

When creating your PAT, consider setting it to never expire. A `read:packages`
token is limited in scope. It can only read packages, not write to repositories,
access private data, or perform any other actions. The risk of a long-lived token
with this scope is low, and it saves you from having to rotate it every time it
expires and update your NuGet source configuration.

### NuGet Source Already Exists

If you have previously added the source under a different key name, or if the add
command reports that the source already exists, you can either remove and re-add it:

```bash
dotnet nuget remove source "scottoffen"
```

Or update it in place:

**macOS/Linux**
```bash
dotnet nuget update source "scottoffen" \
  --username YOUR_GITHUB_USERNAME \
  --password $GITHUB_PAT
```

**PowerShell**
```powershell
dotnet nuget update source "scottoffen" `
  --username YOUR_GITHUB_USERNAME `
  --password $env:GITHUB_PAT
```

**Windows (Command Prompt)**
```cmd
dotnet nuget update source "scottoffen" ^
  --username YOUR_GITHUB_USERNAME ^
  --password %GITHUB_PAT%
```

### Template Not Showing Up in `dotnet new list`

If installation succeeds but the template does not appear in `dotnet new list`, the
template engine cache may need to be refreshed. Run:

```bash
dotnet new update
```

If it still does not appear, uninstall and reinstall the package:

```bash
dotnet new uninstall ScottOffen.Blueprints
dotnet new install ScottOffen.Blueprints
```

### Placeholders Not Being Replaced

If the generated output contains literal placeholder values like `$(GitHubOrg)` or
`$(RepoName)` instead of the values you provided, check the following:

- Ensure you passed the parameter using the correct flag name (e.g. `--org`, `--repo`)
- Ensure the placeholder in the template file exactly matches the `replaces` value
  defined in `template.json`
- Run `dotnet new <shortname> --help` to see the full list of available parameters
  for that template

### Source Not Trusted

GitHub Packages requires authentication. If the source is not trusted or returns an
authentication error on install, ensure that:

- Your PAT has the `read:packages` scope
- The `--username` value in the `dotnet nuget add source` command is your own GitHub
  username, not the package owner's username
- Your PAT has not expired (see [Personal Access Token Expiration](#personal-access-token-pat-expiration) above)

For further troubleshooting, refer to the following resources:

- [GitHub Packages: Working with the NuGet registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-nuget-registry)
- [dotnet nuget add source](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-nuget-add-source)
- [dotnet new install](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new-install)