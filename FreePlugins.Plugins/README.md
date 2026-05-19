# CRM.Plugins

The `CRM.Plugins` project is the server-side plugin runtime for FreeCRM. It defines the plugin contract interfaces, the `Plugin` data model, and the `Plugins` loader that discovers, compiles, and executes dynamic C# and Blazor plugin files at startup.

## What the code does

`Plugins.cs` implements `IPlugins` and performs the following at startup when `Load(path)` is called:

1. Scans the `PluginFiles/` directory for `.cs` and `.plugin` files.
2. For each file, uses Roslyn (`Microsoft.CodeAnalysis.CSharp`) to compile the source in-process and calls the `Properties()` method to read plugin metadata (Id, Name, Author, Type, Description, SortOrder, Prompts, etc.).
3. Stores each valid plugin as a `Plugin` object in the internal list. Plugins with `ContainsSensitiveData = true` have their source code AES-encrypted before being sent to clients.
4. Also scans a `BlazorComponents/` subdirectory for `.razor`/`.blazor` files, loading them as `Type = "Blazor"` plugins with optional JSON metadata sidecar files.

`ExecuteDynamicCSharpCode<T>()` compiles and executes arbitrary C# strings at runtime. It loads base .NET 10 references from `Basic.Reference.Assemblies.Net100` plus any server assembly paths registered in `ServerReferences`, then invokes a named method on the compiled type and returns the typed result. Async tasks are awaited synchronously.

Plugin types with built-in invoker mappings:
- `auth` → `Login()`
- `userupdate` → `UpdateUser()`
- all others → `Execute()`

`Encryption.cs` provides the AES encryption used to protect plugin source code in transit.

## Key public classes

| Class | Description |
|-------|-------------|
| `IPlugins` | DI interface; defines `Load`, `ExecuteDynamicCSharpCode<T>`, `AllPlugins`, `ServerReferences`, `UsingStatements` |
| `Plugins` | Concrete implementation of `IPlugins`; loads and executes plugins |
| `Plugin` | Plugin descriptor: Id, Name, Author, Code, Type, Invoker, Prompts, Properties |
| `PluginPrompt` | Definition of a UI prompt presented to the user before plugin execution |
| `PluginPromptOption` | Option item for select/radio/multiselect prompts |
| `PluginPromptValue` | Holds collected prompt values passed to the plugin at execution |
| `PluginExecuteRequest` / `PluginExecuteResult` | Request/response wrapper for plugin execution calls |

## Project references and NuGet packages

**Project references:** none

**Notable NuGet packages:**

| Package | Version |
|---------|---------|
| `Microsoft.CodeAnalysis.CSharp` | 5.3.0 |
| `Basic.Reference.Assemblies.Net100` | 1.8.6 |

## Build details

| Field | Value |
|-------|-------|
| SDK | `Microsoft.NET.Sdk` |
| Target framework | `net10.0` |
| Output type | Class library |
| Nullable | enabled |

## License

Released under the [MIT License](https://opensource.org/licenses/MIT).

## About

Designed, written, and implemented by **Washington State University - Enrollment Information Technology (WSU-EIT)**.

- Website: https://em.wsu.edu/eit/
- GitHub: https://github.com/WSU-EIT

Part of the [FreeCRM](../README.md) solution.
