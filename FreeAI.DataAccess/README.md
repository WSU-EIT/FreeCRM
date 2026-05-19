# CRM.DataAccess

The `CRM.DataAccess` project is the server-side data access layer for FreeCRM. It contains all business logic, EF Core repository operations, authentication helpers, Microsoft Graph integration, LDAP/Active Directory support, PDF generation, and other server-only integrations.

## What the code does

`DataAccess` is a partial class (split across ~30 files) that implements `IDataAccess` and `IDisposable`. It receives a connection string, database type, and service provider at construction time, then wires up an `EFDataModel` DbContext and initializes the plugin system.

The class is organized into focused partial files:

| File | Responsibility |
|------|---------------|
| `DataAccess.cs` | Constructor, EF context setup, core initialization |
| `DataAccess.App.cs` | Application-specific overrides and init hooks |
| `DataAccess.Appointments.cs` | Appointment CRUD, notes, services, attendance |
| `DataAccess.Authenticate.cs` | Local, JWT, OpenID, and OAuth login flows |
| `DataAccess.ActiveDirectory.cs` | LDAP/AD lookups via `Novell.Directory.Ldap` |
| `DataAccess.ApplicationSettings.cs` | Reading/writing global application settings |
| `DataAccess.CSharpCode.cs` | Dynamic C# code execution via Roslyn |
| `DataAccess.Departments.cs` | Department and department group management |
| `DataAccess.EmailTemplates.cs` | Email template CRUD |
| `DataAccess.Encryption.cs` | AES encryption/decryption helpers |
| `DataAccess.FileStorage.cs` | Binary file upload, retrieval, and deletion |
| `DataAccess.Invoices.cs` | Invoice CRUD and appointment invoice linking |
| `DataAccess.JWT.cs` | JWT encode/decode using `JWTHelpers` |
| `DataAccess.Language.cs` | Locale/language management |
| `DataAccess.Locations.cs` | Location CRUD |
| `DataAccess.Migrations.cs` | EF migration execution |
| `DataAccess.Payments.cs` | Payment record management |
| `DataAccess.PDF.cs` | PDF generation via QuestPDF |
| `DataAccess.Plugins.cs` | Plugin cache load/save |
| `DataAccess.Services.cs` | Service record CRUD |
| `DataAccess.Settings.cs` | Per-tenant settings storage |
| `DataAccess.SignalR.cs` | SignalR active-user tracking |
| `DataAccess.Tags.cs` | Tag and tag-item management |
| `DataAccess.Tenants.cs` | Tenant CRUD and tenant-context resolution |
| `DataAccess.UDFLabels.cs` | User-defined field label management |
| `DataAccess.UserGroups.cs` | User group CRUD |
| `DataAccess.Users.cs` | User CRUD, password hashing, lockout |
| `DataAccess.Utilities.cs` | Shared utility methods |
| `GraphAPI.cs` / `GraphAPI.App.cs` | Microsoft Graph API calls (user lookup, profile photos) |
| `RandomPasswordGenerator.cs` | Cryptographically secure password generation |
| `DataMigrations.*.cs` | Per-provider SQL migration scripts (SQLite, SQL Server, MySQL, PostgreSQL) |

## Key public classes

| Class | Description |
|-------|-------------|
| `DataAccess` | Main partial class; implements `IDataAccess` for all data operations |
| `IDataAccess` | Interface contract consumed by controllers and DI |
| `GraphAPI` | Microsoft Graph API helper for Azure AD user operations |
| `RandomPasswordGenerator` | Generates secure random passwords |
| `Utilities` | General-purpose server-side helpers |

## Project references and NuGet packages

**Project references:**
- `CRM.DataObjects`
- `CRM.EFModels`
- `CRM.Plugins`

**Notable NuGet packages:**

| Package | Version |
|---------|---------|
| `Microsoft.Graph` | 5.105.0 |
| `Azure.Identity` | 1.21.0 |
| `Novell.Directory.Ldap.NETStandard` | 4.0.0 |
| `QuestPDF` | 2026.2.4 |
| `JWTHelpers` | 1.0.1 |
| `Brad.Wickett_Sql2LINQ` | 3.0.1 |
| `CsvHelper` | 33.1.0 |
| `Microsoft.Kiota.Abstractions` | 2.0.0 |
| `Microsoft.AspNetCore.Components.WebAssembly.Server` | 10.0.7 |

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
