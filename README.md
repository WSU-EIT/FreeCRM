# FreeCRM Base Application Template

FreeCRM is a Blazor WebAssembly CRM starter kit built on .NET 9 that is designed to be renamed, branded, and extended into bespoke line-of-business applications. It combines a feature-rich baseline with a carefully curated set of customization points so you can merge upstream updates without constantly re-applying local patches.

## Key ideas

- **Modular solution structure** – Server, client, data access, data objects, and plugin projects are kept separate so you can replace or extend them independently.
- **Partial class customizations** – Every project ships with matching `*.App.*` partial files that are intended to contain your overrides and app-specific logic. Keep your edits in these files and merging new releases becomes a matter of resolving the few intentional touchpoints.
- **Upgrade-friendly pages** – Place custom Razor pages inside `CRM.Client/Pages/App` (and subfolders) so future migration tooling can pick them up automatically.
- **Extensible plugin model** – Drop compiled or source-based plugins into the `CRM/Plugins` folder and they will be loaded at runtime.

## Solution layout

| Project | Purpose | Customization points |
| --- | --- | --- |
| `CRM` | ASP.NET Core host that serves the Blazor WebAssembly client, configures dependency injection, and exposes API controllers. | `Program.App.cs`, `Controllers/*.App.cs`, `Classes/ConfigurationHelper.App.cs` for server-side hooks and configuration helpers. |
| `CRM.Client` | WebAssembly front end that contains layouts, pages, and UI helpers. | `Helpers.App.cs`, `DataModel.App.cs`, and pages inside `Pages/App` for UI-specific behavior and data shaping. |
| `CRM.DataAccess` | Data access layer that coordinates EF Core contexts and business logic. | `DataAccess.App.cs`, `RandomPasswordGenerator.App.cs`, `Utilities.App.cs`, etc. for data-layer extensions and overrides. |
| `CRM.DataObjects` | Shared DTOs, enums, and helper models referenced by both client and server. | `DataObjects.App.cs`, `GlobalSettings.App.cs` for app-specific fields and configuration defaults. |
| `CRM.Plugins` | Runtime plugin loader and helpers. | Implement `IPlugin` and drop assemblies or source files in `CRM/Plugins`; they are discovered during startup. |

## Getting started

1. **Install prerequisites**
   - .NET 9 SDK
   - (Optional) Visual Studio 2022 17.10+ or VS Code with C# Dev Kit for an IDE experience
2. **Clone and restore**
   ```bash
   git clone https://github.com/your-org/FreeCRM.git
   cd FreeCRM
   dotnet restore CRM.sln
   ```
3. **Run the app**
   ```bash
   dotnet run --project CRM
   ```
   The server project hosts the WebAssembly client; browse to the indicated URL once the application starts.

## Customization workflow

1. **Keep local code in `*.App.*` files** – Each partial file is empty (or filled with templates) so you can add overrides without touching the base implementation. Examples include startup hooks in `Program.App.cs`, custom SignalR handling in `Helpers.App.cs`, and bespoke data queries in `DataAccess.App.cs`.
2. **Add UI to `Pages/App`** – Organize custom Razor components beneath `CRM.Client/Pages/App` so they are easy to track during upgrades.
3. **Extend shared models** – Use `CRM.DataObjects/DataObjects.App.cs` to add DTO properties or helper methods that the client and server understand.
4. **Augment configuration helpers** – `CRM/Classes/ConfigurationHelper.App.cs` lets you register additional configuration values and computed defaults without editing the base helper.
5. **Deliver dynamic behavior with plugins** – Implement `IPlugin`, compile to DLL, or provide source snippets and place them in the `CRM/Plugins` folder. Startup logic registers each plugin and makes it available through dependency injection.

## Built-in tooling

- **Rename utility** – Run the `Rename FreeCRM.exe` console application to rename projects, regenerate GUIDs, and update namespaces to match your brand. It also accepts a command-line argument for the new name (`"Rename FreeCRM.exe" MyNewProjectName`).
- **Module trimmer** – Use `Remove Modules from FreeCRM.exe` to remove optional features (Appointments, EmailTemplates, Invoices, Locations, Payments, Services, Tags). Specify `remove:Module1,Module2` to drop specific modules, `keep:Module1` to retain only certain modules, or `remove:all` to strip every optional module.

## Configuration highlights

- Environment settings live in `appsettings.json` and `appsettings.Development.json`, including module toggles, SignalR mode, analytics codes, and feature flags used during startup.
- The application registers standard and app-specific authorization policies. Add new policy names by returning them from `AuthenticationPoliciesApp` in `Program.App.cs`.
- Plugin namespaces can be expanded through the `PluginUsingStatements` section in configuration; they are loaded into the dynamic compilation context on startup.

## Upgrading to new releases

1. Pull the latest upstream changes.
2. Resolve merge conflicts, focusing primarily on your curated `*.App.*` and `Pages/App` files.
3. Run the rename/module trimming utilities again if project naming or module selection changes.
4. Rebuild and smoke-test your instance to verify integrations and plugins still load.

Because your custom logic is isolated to dedicated partials and app folders, upgrades typically require reconciling only a handful of files instead of the entire solution.

