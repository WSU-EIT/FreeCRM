# FreeCRM Base Application

FreeCRM is a fully functioning CRM starter kit built with ASP.NET Core 9 and Blazor WebAssembly.  It is designed to be the "base app" that you can merge into your own products with a minimal amount of conflict resolution.  The project makes heavy use of **partial classes**, **partial Razor components**, and companion `*.App.*` files so that the core platform can receive updates while your team keeps custom business logic in a clearly separated layer.

## Why the project is structured this way

| Concept | Purpose |
| --- | --- |
| Partial classes / Razor files | Core logic lives in the default files, while anything in the companion `.App.` files is reserved for your custom behaviour.  You rarely touch the base file, so pulling upstream updates is safer. |
| `*.App.*` naming | Anything that ends in `.App.cs`, `.App.razor`, `.appsettings`, etc. is **your customization surface**.  The defaults ship with stubbed methods, properties, or markup to help you understand where to plug in. |
| App-specific methods | Methods such as `Program.AppModifyStart`, `ConfigurationHelpersLoadApp`, and `LoadData` in the `.App.` components are intentionally empty extension points ready for your code. |
| Optional modules | CRM features such as Appointments, Email Templates, Invoices, Locations, Payments, Services, and Tags can be removed with the dedicated tooling so you only ship what you need. |
| Rename utilities | The repository includes utilities to rename the solution, regenerate GUIDs, and update namespaces so the base app can become *your* branded app with a single command. |

Because the customization surface is explicitly marked, merging upstream changes generally only requires resolving a small handful of conflicts inside files that contain `.App.` in their name.

## Solution layout

```
FreeCRM.sln
├── CRM/                  # ASP.NET Core host for Blazor WebAssembly + REST APIs
├── CRM.Client/           # Blazor WebAssembly UI
├── CRM.DataAccess/       # Data access layer, repositories, external service helpers
├── CRM.DataObjects/      # Shared DTOs and configuration models
├── CRM.EFModels/         # Entity Framework Core models & migrations
├── CRM.Plugins/          # Plugin runtime and sample plugins
├── Rename FreeCRM.exe    # Utility to rename the solution, projects, and namespaces
└── Remove Modules...     # Utility to strip optional modules from the base app
```

Each project mirrors the same customization approach: the base implementation ships in files such as `DataAccess.cs` or `Index.razor`, and your overrides live beside them in `DataAccess.App.cs` or `Index.App.razor`.

## Getting started

1. **Install prerequisites**
   * [.NET SDK 9.0](https://dotnet.microsoft.com/download/dotnet/9.0)
   * A SQL database connection string (localdb, SQL Server, or another provider configured in `appsettings.json`)
2. **Restore packages and build**
   ```bash
   dotnet restore
   dotnet build
   ```
3. **Run the development server**
   ```bash
   dotnet run --project CRM/CRM.csproj
   ```
   The Blazor WebAssembly front end will be hosted by the ASP.NET Core backend.  Update `appsettings.Development.json` for local secrets such as OAuth keys or connection strings.

## Customizing the base app

1. **Use the `.App.` files first**  – add your application-specific logic, services, and UI elements to the companion files.  For example:
   * `CRM/Program.App.cs` exposes hooks (`AppModifyBuilderStart`, `AppModifyBuilderEnd`, `AppModifyStart`, `AppModifyEnd`) for configuring the ASP.NET Core pipeline.
   * `CRM.Client/Shared/AppComponents/Index.App.razor` contains empty methods such as `LoadData` where you can fetch and display additional information on the home view.
   * `CRM.DataAccess/DataAccess.App.cs` lets you extend the repository without editing the generated base class.

2. **Share code across layers**  – use the shared projects (`CRM.DataObjects`, `CRM.Client`, `CRM.DataAccess`) to move common DTOs and helpers so both the client and server stay in sync.

3. **Configuration helpers**  – extend `ConfigurationHelper.App.cs` to surface configuration values that the Blazor client can consume without modifying the core helper implementation.

4. **Styling** – update `CRM.Client/wwwroot/css/site.App.css` to brand the application.  The core stylesheet stays untouched so you can accept upstream design tweaks without losing your custom theme.

5. **Extend authentication & authorization** – populate `AuthenticationPoliciesApp` or inject services inside `Program.AppModifyBuilderStart` to hook into the built-in cookie/JWT pipeline.

6. **Plugins** – drop compiled assemblies or `.plugin` manifests into the `CRM/Plugins` directory.  The server loads them at startup (`Program` configures the plugin loader automatically) and exposes the DI container to your plugin code.

> ⚠️ If you must touch a non-`.App.` file, consider creating a partial class or overriding method inside an `.App.` file instead.  That way future merges remain painless.

## Utilities included in the repository

### Rename tool

Run the `Rename FreeCRM.exe` console application to rename the solution, regenerate project GUIDs, and update namespaces.  You can also pass the new name as a command-line argument:

```bash
"Rename FreeCRM.exe" MyNewProduct
```

### Module removal tool

The `Remove Modules from FreeCRM.exe` utility trims optional modules from the codebase.  Use the command-line switches to pick which features stay:

```bash
"Remove Modules from FreeCRM.exe" remove:Appointments,Payments
"Remove Modules from FreeCRM.exe" keep:Tags
"Remove Modules from FreeCRM.exe" remove:all
```

If you discover code that should have been removed, please open an issue and include the file path plus the line number.

## Configuration

Key settings live in `CRM/appsettings.json` and `CRM/appsettings.Development.json`.  Notable values include:

* `ConnectionStrings:AppData` – database connection string used by `CRM.DataAccess`.
* `AzureSignalRurl` – enables Azure SignalR integration when populated.  Otherwise the app uses the built-in SignalR server.
* `LocalModeUrl`, `DatabaseType`, `AllowApplicationEmbedding`, `AnalyticsCode` – consumed inside `Program` and exposed to components through configuration helpers.
* `GloballyDisabledModules` / `GloballyEnabledModules` – toggle modules without recompiling custom code.

## Contributing & staying up to date

1. Keep your custom changes inside the `.App.` files whenever possible.
2. When upstream changes are published, merge them into your branch.  You will typically only have to reconcile the handful of `.App.` files you have edited.
3. Submit issues or PRs if you find bugs in the base implementation or gaps in the module removal process.

This approach lets you treat FreeCRM as a vendor-supplied core platform while tailoring it to your business with predictable merge behaviour.
