# CRM.Client

The `CRM.Client` project is the Blazor WebAssembly client for the FreeCRM application. It runs entirely in the browser, communicates with the server via HTTP API calls and SignalR, and provides all user-facing pages and shared components.

## What the code does

`Program.cs` bootstraps the WebAssembly host and registers:
- `HttpClient` pointed at the host base address
- `BlazoredLocalStorage` for client-side persistence
- `BlazorDataModel` as a singleton shared data model used across all pages
- Blazor Bootstrap, MudBlazor, and Radzen component services
- A `CompilerService` (from `DynamicBlazorSupport/`) that compiles and renders Razor components at runtime using Roslyn in the browser

`BlazorDataModel` (in `DataModel.cs`) is the central singleton state object; every page injects it to share tenant context, current user, application settings, and cached list data.

The `DynamicBlazorSupport/` folder contains a full Roslyn-based in-browser compilation pipeline (`CompilationService`, `VirtualProjectFileSystem`, `CompileToAssemblyResult`) allowing runtime loading and rendering of plugin-supplied `.razor` files.

`Helpers.cs` and `Helpers.App.cs` provide shared utility methods used across pages and components.

## @page routes

All routes support an optional `/{TenantCode}` prefix for multi-tenancy.

| Route | Component |
|-------|-----------|
| `/` | `Index.razor` |
| `/About` | `About.razor` |
| `/ChangePassword` | `ChangePassword.razor` |
| `/PasswordChanged` | `PasswordChanged.razor` |
| `/DatabaseOffline` | `DatabaseOffline.razor` |
| `/not-found` | `NotFound.razor` |
| `/ServerUpdated` | `ServerUpdated.razor` |
| `/Profile` | `Profile.razor` |
| `/Login` | `Authorization/Login.razor` |
| `/Logout` | `Authorization/Logout.razor` |
| `/ProcessLogin` | `Authorization/ProcessLogin.razor` |
| `/Authorization/AccessDenied` | `Authorization/AccessDenied.razor` |
| `/Authorization/InvalidUser` | `Authorization/InvalidUser.razor` |
| `/Authorization/NoLocalAccount` | `Authorization/NoLocalAccount.razor` |
| `/InvalidTenantCode` | `Settings/Misc/InvalidTenantCode.razor` |
| `/MissingTenantCode` | `Settings/Misc/MissingTenantCode.razor` |
| `/Setup` | `Settings/Misc/Setup.razor` |
| `/Settings` | `Settings/Misc/Settings.razor` |
| `/Settings/AppSettings` | `Settings/Misc/AppSettings.razor` |
| `/Settings/DeletedRecords` | `Settings/Misc/DeletedRecords.razor` |
| `/Settings/Language` | `Settings/Misc/Languages.razor` |
| `/Settings/UDF` | `Settings/Misc/UDF.razor` |
| `/Settings/Departments` | `Settings/Departments/Departments.razor` |
| `/Settings/DepartmentGroups` | `Settings/Departments/DepartmentGroups.razor` |
| `/Settings/EditDepartment/{departmentid}` | `Settings/Departments/EditDepartment.razor` |
| `/Settings/AddDepartment` | `Settings/Departments/EditDepartment.razor` |
| `/Settings/EditDepartmentGroup/{departmentgroupid}` | `Settings/Departments/EditDepartmentGroup.razor` |
| `/Settings/AddDepartmentGroup` | `Settings/Departments/EditDepartmentGroup.razor` |
| `/Settings/Users` | `Settings/Users/Users.razor` |
| `/Settings/UserGroups` | `Settings/Users/UserGroups.razor` |
| `/Settings/EditUser/{userid}` | `Settings/Users/EditUser.razor` |
| `/Settings/AddUser` | `Settings/Users/EditUser.razor` |
| `/Settings/EditUserGroup/{groupid}` | `Settings/Users/EditUserGroup.razor` |
| `/Settings/AddUserGroup` | `Settings/Users/EditUserGroup.razor` |
| `/Settings/Tenants` | `Settings/Tenants/Tenants.razor` |
| `/Settings/EditTenant/{tenantid}` | `Settings/Tenants/EditTenant.razor` |
| `/Settings/AddTenant` | `Settings/Tenants/EditTenant.razor` |
| `/Settings/Tags` | `Settings/Tags/Tags.razor` |
| `/Settings/EditTag/{id}` | `Settings/Tags/EditTag.razor` |
| `/Settings/AddTag` | `Settings/Tags/EditTag.razor` |
| `/Settings/Services` | `Settings/Services/Services.razor` |
| `/Settings/EditService/{id}` | `Settings/Services/EditService.razor` |
| `/Settings/AddService` | `Settings/Services/EditService.razor` |
| `/Settings/Locations` | `Settings/Locations/Locations.razor` |
| `/Settings/EditLocation/{id}` | `Settings/Locations/EditLocation.razor` |
| `/Settings/AddLocation` | `Settings/Locations/EditLocation.razor` |
| `/Settings/EmailTemplates` | `Settings/Email/EmailTemplates.razor` |
| `/Settings/EditEmailTemplate/{id}` | `Settings/Email/EditEmailTemplate.razor` |
| `/Settings/AddEmailTemplate` | `Settings/Email/EditEmailTemplate.razor` |
| `/Settings/Files` | `Settings/Files/Files.razor` |
| `/Schedule` | `Scheduling/Schedule.razor` |
| `/Invoices` | `Invoices/Invoices.razor` |
| `/Invoices/{userid}` | `Invoices/Invoices.razor` |
| `/CreateInvoice` | `Invoices/EditInvoice.razor` |
| `/CreateInvoice/{userid}` | `Invoices/EditInvoice.razor` |
| `/EditInvoice/{id}` | `Invoices/EditInvoice.razor` |
| `/AppointmentInvoices/{AppointmentId}` | `Invoices/AppointmentInvoices.razor` |
| `/ViewInvoice/{id}` | `Invoices/ViewInvoice.razor` |
| `/Payments` | `Payments/Payments.razor` |
| `/Plugins` | `TestPages/PluginTesting.razor` |
| `/DynamicComponent` | `TestPages/DynamicComponent.razor` |
| `/HtmlEditor` | `TestPages/HtmlEditor.razor` |
| `/Monaco` | `TestPages/Monaco.razor` |
| `/TimerTest` | `TestPages/Test.razor` |
| `/DoubleClick` | `TestPages/DoubleClick.razor` |
| `/SortTest` | `TestPages/Sort.razor` |

## Key public classes

| Class | Description |
|-------|-------------|
| `BlazorDataModel` | Singleton client-side state model shared across all pages |
| `CompilationService` | Roslyn-based in-browser Razor/C# compiler for dynamic plugin components |
| `VirtualProjectFileSystem` | In-memory virtual file system that backs the runtime compiler |
| `CompileToAssemblyResult` | Return value from the in-browser compilation pipeline |
| `Program` | WebAssembly entry point; registers all DI services |
| `Message` / `NewMessage` | Toast/notification message objects used across the UI |
| `SnippetsOptions` | Options model for Monaco editor snippet configuration |

## Project references and NuGet packages

**Project references:**
- `CRM.DataObjects`

**Notable NuGet packages:**

| Package | Version |
|---------|---------|
| `Microsoft.AspNetCore.Components.WebAssembly` | 10.0.7 |
| `Microsoft.AspNetCore.SignalR.Client` | 10.0.7 |
| `MudBlazor` | 9.4.0 |
| `Blazor.Bootstrap` | 3.5.0 |
| `Radzen.Blazor` | 10.3.2 |
| `Blazored.LocalStorage` | 4.5.0 |
| `BlazorMonaco` | 3.4.0 |
| `BlazorSortableList` | 2.1.2 |
| `FreeBlazor` | 2.0.5 |
| `Microsoft.CodeAnalysis.CSharp` | 5.3.0 |
| `FluentValidation` | 12.1.1 |
| `CsvHelper` | 33.1.0 |
| `HtmlAgilityPack` | 1.12.4 |
| `Humanizer` | 3.0.10 |

## Build details

| Field | Value |
|-------|-------|
| SDK | `Microsoft.NET.Sdk.BlazorWebAssembly` |
| Target framework | `net10.0` |
| Output type | Blazor WebAssembly client |
| Nullable | enabled |

## License

Released under the [MIT License](https://opensource.org/licenses/MIT).

## About

Designed, written, and implemented by **Washington State University - Enrollment Information Technology (WSU-EIT)**.

- Website: https://em.wsu.edu/eit/
- GitHub: https://github.com/WSU-EIT

Part of the [FreeCRM](../README.md) solution.
