# CRM

The `CRM` project is the ASP.NET Core server host for the FreeCRM application. It wires together the Blazor WebAssembly client, REST API controllers, a SignalR real-time hub, authentication middleware, a plugin system, and an optional background processor.

## What the code does

`Program.cs` builds the ASP.NET Core host using `Microsoft.NET.Sdk.Web`. On startup it:

- Registers Radzen component services and scoped dialog/notification/theme services.
- Configures SignalR — falls back to local SignalR unless `AzureSignalRurl` is set in `appsettings.json`, in which case it connects to Azure SignalR Service.
- Loads compiled plugin assemblies from the `PluginFiles/` directory using the `Plugins.Plugins` loader; plugin server references and `using` statements are supplied from configuration.
- Resolves the database connection string and type (`DatabaseType` can be `InMemory`, SQLite, SQL Server, MySQL, or PostgreSQL) and registers `IDataAccess` as transient DI.
- Conditionally starts a `BackgroundProcessor` hosted service (configurable interval, load-balancing filter by machine name).
- Registers a `CustomAuthentication` service supporting local accounts, OpenID Connect, and social OAuth providers (Google, Facebook, Microsoft, Apple).
- Configures claim-based authorization policies including `AppAdmin`, `Admin`, `CanBeScheduled`, `ManageAppointments`, `ManageFiles`, and `PreventPasswordChange`.
- Maps the `crmHub` SignalR endpoint at `/crmHub` with stateful reconnects enabled.
- Maps Razor Components with interactive WebAssembly render mode, serving the `CRM.Client` assembly.

Controllers cover: AJAX, app-specific endpoints, application settings, appointments, authentication, departments, email templates, encryption, file storage, invoices, languages, locations, payments, plugins, services, tags, tenants, UDF labels, user groups, users, and utilities.

The `BackgroundProcessor` is an `IHostedService` that runs timed jobs and plugin-driven tasks at a configurable interval.

## Key public classes

| Class | Description |
|-------|-------------|
| `Program` | Application entry point; configures all services and middleware |
| `BackgroundProcessor` | Hosted background service for scheduled and plugin-driven tasks |
| `CustomAuthenticationHandler` | JWT/cookie authentication middleware |
| `CustomAuthIdentity` | Identity helper for building ClaimsPrincipal from tokens |
| `RouteHelper` | Route configuration utilities |
| `crmHub` (SignalR) | Real-time hub mapped at `/crmHub` |
| `DataController` | Base REST API controller with DI wiring |
| `SetupController` | Handles initial application setup flow |

## Blazor server components

Razor components are in `Components/`. The application root is `App.razor`. Server-side components complement the WebAssembly client pages listed in the `CRM.Client` README.

## Project references and NuGet packages

**Project references:**
- `CRM.DataAccess`
- `CRM.Plugins`
- `CRM.Client`

**Notable NuGet packages:**

| Package | Version |
|---------|---------|
| `Microsoft.AspNetCore.Components.WebAssembly.Server` | 10.0.7 |
| `Microsoft.Azure.SignalR` | 1.33.0 |
| `Microsoft.AspNetCore.Authentication.OpenIdConnect` | 10.0.7 |
| `Microsoft.AspNetCore.Authentication.Google` | 10.0.7 |
| `Microsoft.AspNetCore.Authentication.Facebook` | 10.0.7 |
| `Microsoft.AspNetCore.Authentication.MicrosoftAccount` | 10.0.7 |
| `AspNet.Security.OAuth.Apple` | 10.0.0 |
| `Serilog.Extensions.Logging.File` | 3.0.0 |

## Build details

| Field | Value |
|-------|-------|
| SDK | `Microsoft.NET.Sdk.Web` |
| Target framework | `net10.0` |
| Output type | Web application (executable) |
| Nullable | enabled |

## License

Released under the [MIT License](https://opensource.org/licenses/MIT).

## About

Designed, written, and implemented by **Washington State University - Enrollment Information Technology (WSU-EIT)**.

- Website: https://em.wsu.edu/eit/
- GitHub: https://github.com/WSU-EIT

Part of the [FreeCRM](../README.md) solution.
