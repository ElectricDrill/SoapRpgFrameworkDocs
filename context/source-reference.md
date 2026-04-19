# Source Reference — Astra RPG Framework Docs

## Primary paths

| Purpose | Path |
|---|---|
| Docs repository root | `C:\Users\emaci\Documents\AstraRpgFrameworkDocs` |
| DocFX source root | `C:\Users\emaci\Documents\AstraRpgFrameworkDocs\DocFx` |
| Markdown docs | `C:\Users\emaci\Documents\AstraRpgFrameworkDocs\DocFx\MD` |
| Landing page | `C:\Users\emaci\Documents\AstraRpgFrameworkDocs\DocFx\index.md` |
| Images | `C:\Users\emaci\Documents\AstraRpgFrameworkDocs\DocFx\images` |
| Generated site output | `C:\Users\emaci\Documents\AstraRpgFrameworkDocs\docs` |
| DocFX config | `C:\Users\emaci\Documents\AstraRpgFrameworkDocs\DocFx\docfx.json` |
| Astra packages root | `C:\Users\emaci\Documents\AstraPublishing_6_3_8_2D\Packages` |
| Framework package root | `C:\Users\emaci\Documents\AstraPublishing_6_3_8_2D\Packages\com.electricdrill.astra-rpg-framework` |

## Framework package structure

| Folder | Use it for... |
|---|---|
| `Runtime/` | Core gameplay systems, public types, package behavior, API names |
| `Editor/` | Custom inspectors, drawers, generators, validation, editor-only workflows |
| `Samples~/` | Sample assets, sample scene content, user-facing examples |
| `package.json` | Package name, version, display name, Unity version, sample definitions, public URLs |
| `.github/copilot-instructions.md` | Existing package-level conventions and terminology |

## Practical lookup guide

- Check `Runtime/Stats`, `Runtime/Attributes`, `Runtime/Classes`, `Runtime/Experience`, and `Runtime/Scaling` for gameplay concepts
- Check `Runtime/Events` and `Runtime/GameActions` for event-driven and action-driven workflows
- Check `Editor/` when a documentation section describes inspector behavior, buttons, validation, warnings, or generated assets
- Check `Samples~/` when documenting imported samples, example scenes, or prebuilt assets

## API documentation generation

`DocFx/docfx.json` builds API metadata from the framework runtime project under:

- `..\..\AstraPublishing_6_3_8_2D`

Specifically, the metadata source points to:

- `com.electricdrill.astra-rpg-framework.Runtime.csproj`

This means handwritten docs and generated API docs must stay aligned with the same package source.

## Scope reminder

- This repository documents **Astra RPG Framework**
- Companion packages in the same ecosystem may be useful for comparison, but they are not the primary source of truth here
- When a feature belongs to a different package, avoid documenting it here as if it were part of the framework
