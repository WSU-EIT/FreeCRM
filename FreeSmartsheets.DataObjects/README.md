# CRM.DataObjects

The `CRM.DataObjects` project defines all shared data transfer objects (DTOs), enumerations, interface contracts, configuration helpers, and caching utilities used by both the server-side `CRM.DataAccess` project and the browser-side `CRM.Client` project.

## What the code does

`DataObjects` is a partial class containing the complete set of strongly-typed DTOs that flow across the HTTP API boundary. Because the class is shared between server and client, it has no dependencies on ASP.NET Core or EF Core — only `System.Runtime.Caching`.

Key source files:

| File | Contents |
|------|----------|
| `DataObjects.cs` | Core enumerations (`DeletePreference`, `SettingType`, `UserLookupType`), `ActiveUser`, `ApplicationSettings`, `BooleanResponse`, `SensitiveAttribute` |
| `DataObjects.App.cs` | Application-specific DTO extensions |
| `DataObjects.Appointments.cs` | `Appointment`, `AppointmentNote`, `AppointmentService`, `AppointmentAttendanceUpdate`, `AppoinmentLoader` |
| `DataObjects.Departments.cs` | `Department`, `DepartmentGroup` |
| `DataObjects.EmailTemplates.cs` | `EmailTemplate` |
| `DataObjects.Invoices.cs` | `Invoice`, invoice line items |
| `DataObjects.Locations.cs` | `Location` |
| `DataObjects.Payments.cs` | `Payment` |
| `DataObjects.Services.cs` | `Service` |
| `DataObjects.SignalR.cs` | SignalR message envelope types |
| `DataObjects.Tags.cs` | `Tag`, `TagItem` |
| `DataObjects.UDFLabels.cs` | `UDFLabel` (user-defined field labels) |
| `DataObjects.UserGroups.cs` | `UserGroup`, `UserInGroup` |
| `DataObjects.ActiveDirectory.cs` | AD/LDAP lookup result objects |
| `DataObjects.Ajax.cs` | AJAX request/response wrappers |
| `ConfigurationHelper.cs` / `ConfigurationHelper.App.cs` | `IConfigurationHelper`, `ConfigurationHelperLoader`, `ConfigurationHelperConnectionStrings` |
| `GlobalSettings.cs` / `GlobalSettings.App.cs` | Application-wide static settings and constants |
| `Caching.cs` | Shared in-memory cache wrapper using `System.Runtime.Caching` |

## Key public classes

| Class | Description |
|-------|-------------|
| `DataObjects` | Root partial class containing all DTOs and enumerations |
| `DataObjects.ActiveUser` | Authenticated user context passed to every data operation |
| `DataObjects.ApplicationSettings` | Global application configuration DTO |
| `DataObjects.BooleanResponse` | Standard success/failure response with optional message |
| `ConfigurationHelper` / `IConfigurationHelper` | DI interface for reading app configuration at runtime |
| `GlobalSettings` | Static class holding app-wide constants and defaults |
| `Caching` | Thread-safe in-memory object cache |

## Project references and NuGet packages

**Project references:**
- `CRM.Plugins`

**Notable NuGet packages:**

| Package | Version |
|---------|---------|
| `System.Runtime.Caching` | 10.0.7 |

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
