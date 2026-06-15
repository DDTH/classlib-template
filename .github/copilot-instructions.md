# Copilot instructions

This repository is **not a library** — it is a NuGet **`dotnet new` template package** (`PackageId` `Ddth.Templates.Classlib`). The root project packs the `content/` directory as a reusable template; the actual C# code that consumers get lives under `content/`.

## Architecture

- **`classlib-template.csproj`** (root) is a packaging-only project: `<PackageType>Template</PackageType>`, `<IncludeBuildOutput>false</IncludeBuildOutput>`, and `<Compile Remove="**\*" />`. It compiles no code — it only bundles `content/**` (excluding `bin`/`obj`) into a template `.nupkg`.
  - `<<VERSION>>` and `<<RELEASE-NOTES>>` in the csproj are **placeholders substituted at release/pack time** (CI). Do not hardcode real values over them.
  - The `CopyPackage` target copies the produced `.nupkg` back into the project directory after `dotnet pack`.
  - `README.md`, `LICENSE.md`, and `RELEASE-NOTES.md` are packed into the package and referenced by the csproj — keep all three present.
- **`content/Ddth/`** is the template payload: a solution (`Ddth.sln`) containing the class library **`Ddth.Mylib`** and the xUnit test project **`Ddth.Mylib.Tests`** (which references the library). This is the structure a consumer materializes via `dotnet new`.
- **`content/Ddth/.template.config/template.json`** defines the `dotnet new` template: `shortName` is **`clt`**, and **`sourceName` is `Ddth`**. The literal token `Ddth` — in folder names, project/file names, and namespaces — is what `dotnet new` replaces with the consumer-supplied name. **Preserve `Ddth` exactly as the substitution token when editing the template; renaming it (e.g. to `DDTH`) breaks name substitution.** `bin`/`obj`/`.template.config`/`.vs` and lock/user files are excluded from generated output.

## Build / pack / test

```sh
# Pack the template package (run at repo root) -> Ddth.Templates.Classlib.<version>.nupkg
dotnet pack

# Build / test the sample content solution
dotnet build content/Ddth/Ddth.sln
dotnet test  content/Ddth/Ddth.sln

# Run a single test
dotnet test content/Ddth/Ddth.sln --filter "FullyQualifiedName~Ddth.Mylib.Tests.UnitTest1.Test1"

# Instantiate the template locally (after install) to smoke-test it
dotnet new clt -n MyLib
```

## Conventions

- **Target framework for content projects is `net6.0`.** Because net6.0 is older than the installed SDK default, the test project pins net6.0-compatible package versions (`Microsoft.NET.Test.Sdk` 17.8.0, `xunit` 2.6.6, `xunit.runner.visualstudio` 2.5.6, `coverlet.collector` 6.0.0). **Do not upgrade these to versions that drop net6.0 support** — the latest releases fail to build on net6.0.
- The content solution uses the classic `.sln` format (not `.slnx`).
- New `dotnet new classlib`/`xunit` templates default to a newer TFM; when adding projects under `content/`, explicitly set `<TargetFramework>net6.0</TargetFramework>` and the matching `<RootNamespace>`.
- `ImplicitUsings` and `Nullable` are **enabled** — avoid redundant common `using`s and write nullable-aware code.
- Code style is enforced by `.editorconfig` (present at root and in `content/Ddth/`, identical), with several rules escalated to **error**/**warning**: required accessibility modifiers; interfaces PascalCase prefixed `I` (error); private instance fields `_camelCase`, private static fields `s_camelCase`; parameters camelCase; `System.*` usings sorted first; enforced `??`/`?.`. Build with `dotnet build` to surface violations as diagnostics.
