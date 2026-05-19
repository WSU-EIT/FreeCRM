# CRM.EFModels

The `CRM.EFModels` project contains the Entity Framework Core `DbContext`, all entity model classes, and the fluent model configuration for the FreeCRM database schema. It targets multiple database providers and is consumed exclusively by `CRM.DataAccess`.

## What the code does

`EFDataModel` (in `EFModels/EFDataModel.cs`) inherits from `DbContext` and exposes `DbSet<T>` properties for every table in the schema. Provider selection (SQL Server, SQLite, MySQL via `MySql.EntityFrameworkCore`, PostgreSQL via `Npgsql`, or InMemory) happens in `DataAccess.cs` at startup — the `OnConfiguring` override in `EFDataModel` is left commented out and is only activated temporarily when generating provider-specific migration scripts.

`EFModelOverrides.cs` contains any `OnModelCreating` fluent API overrides that cannot be expressed through data annotations alone.

### Entity models

| Entity | Table |
|--------|-------|
| `Appointment` | Appointments |
| `AppointmentNote` | AppointmentNotes |
| `AppointmentService` | AppointmentServices |
| `AppointmentUser` | AppointmentUsers |
| `Department` | Departments |
| `DepartmentGroup` | DepartmentGroups |
| `EmailTemplate` | EmailTemplates |
| `FileStorage` | FileStorages |
| `Invoice` | Invoices |
| `Location` | Locations |
| `Payment` | Payments |
| `PluginCache` | PluginCaches |
| `Service` | Services |
| `Setting` | Settings |
| `Tag` | Tags |
| `TagItem` | TagItems |
| `Tenant` | Tenants |
| `UDFLabel` | UDFLabels |
| `User` | Users |
| `UserGroup` | UserGroups |
| `UserInGroup` | UserInGroups |

Optional modules (Appointments, Invoices, Locations, Payments, Services, Tags) are conditionally included in the schema using `{{ModuleItemStart/End}}` template markers.

## Key public classes

| Class | Description |
|-------|-------------|
| `EFDataModel` | Main `DbContext`; configures all entity sets and relationships |
| `EFModelOverrides` | Fluent model configuration overrides |
| `User` | User entity with authentication and profile fields |
| `Tenant` | Multi-tenant root entity |
| `Appointment` | Core scheduling entity |
| `Invoice` | Financial invoice entity |
| `FileStorage` | Binary file metadata and content storage |
| `PluginCache` | Persisted cache for compiled plugin assemblies |

## Project references and NuGet packages

**Project references:** none (this project has no project references)

**Notable NuGet packages:**

| Package | Version |
|---------|---------|
| `Microsoft.EntityFrameworkCore` | 10.0.7 |
| `Microsoft.EntityFrameworkCore.SqlServer` | 10.0.7 |
| `Microsoft.EntityFrameworkCore.Sqlite` | 10.0.7 |
| `Microsoft.EntityFrameworkCore.InMemory` | 10.0.7 |
| `MySql.EntityFrameworkCore` | 10.0.7 |
| `Npgsql.EntityFrameworkCore.PostgreSQL` | 10.0.1 |
| `Microsoft.EntityFrameworkCore.Tools` | 10.0.7 |

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
