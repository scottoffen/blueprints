---
sidebar_position: 1
title: Blueprints
---

Every project starts with the same decisions: folder structure, configuration files,
license files, CI/CD pipelines, documentation scaffolding. Made once, these decisions
reflect hard-won experience. Made repeatedly, they become toil, and an opportunity
for inconsistency to creep in.

Blueprints captures those decisions as `dotnet new` templates. A single command
replaces hours of copying, renaming, and reconfiguring, and ensures that every new
project starts from the same well-considered baseline.

## Why `dotnet new`?

The `dotnet new` command is built into the .NET SDK and available on every platform
where .NET runs. It is not limited to .NET projects. Templates can generate any
file or folder structure, making it a general-purpose scaffolding tool for anything
from GitHub Actions workflows to Docker configurations to documentation sites.

Templates installed via `dotnet new` are:

- **Versioned:** published as NuGet packages, so updates can be distributed and
  installed like any other dependency
- **Parameterized:** values like project names, usernames, and URLs are passed
  as arguments and substituted throughout the generated output
- **Composable:** smaller templates can be used independently or as building
  blocks for larger ones
- **Consistent:** the same template produces the same output every time,
  eliminating the drift that comes from manual setup

## Learning More

The official Microsoft documentation covers everything from creating your first
template to advanced features like conditional content, computed symbols, and
post-generation actions:

- [Custom templates for dotnet new](https://learn.microsoft.com/en-us/dotnet/core/tools/custom-templates)
- [Template authoring guide](https://github.com/dotnet/templating/wiki)
- [dotnet new command reference](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new)