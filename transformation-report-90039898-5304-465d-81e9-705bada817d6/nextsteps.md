# Next Steps

## Overview

The transformation appears to be successful with no build errors reported across any of the projects in the solution. All five projects (Bookstore.Data, Bookstore.Domain.Tests, Bookstore.Cdk, Bookstore.Web, and Bookstore.Domain) have compiled without issues.

## Validation Steps

### 1. Verify Target Framework

Confirm that all projects are targeting the appropriate .NET version:

```bash
dotnet list package --framework
```

Review each `.csproj` file to ensure consistent `<TargetFramework>` values across the solution (e.g., `net6.0`, `net7.0`, or `net8.0`).

### 2. Run Unit Tests

Execute the test suite to ensure existing functionality remains intact:

```bash
dotnet test app/Bookstore.Domain.Tests/Bookstore.Domain.Tests.csproj
```

Review test results and investigate any failures that may indicate compatibility issues with the new framework.

### 3. Check Package Dependencies

List all NuGet packages and verify they are compatible with cross-platform .NET:

```bash
dotnet list package --outdated
dotnet list package --deprecated
```

Update any outdated or deprecated packages to their latest stable versions that support the target framework.

### 4. Validate Database Connectivity

Since the solution includes a Data project, test database connections:

- Review connection strings in configuration files (appsettings.json, appsettings.Development.json)
- Ensure database providers (Entity Framework Core, Dapper, etc.) are compatible with cross-platform .NET
- Test database migrations if using Entity Framework Core:

```bash
dotnet ef migrations list --project app/Bookstore.Data/Bookstore.Data.csproj
```

### 5. Test the Web Application

Run the web application locally to verify functionality:

```bash
dotnet run --project app/Bookstore.Web/Bookstore.Web.csproj
```

Perform the following checks:

- Application starts without errors
- All routes and endpoints respond correctly
- Static files are served properly
- Authentication and authorization work as expected
- Forms and data submission function correctly

### 6. Review Configuration Files

Examine configuration files for platform-specific paths or settings:

- Replace backslashes (`\`) with forward slashes (`/`) in file paths
- Verify environment variable usage
- Check for hardcoded Windows-specific paths (e.g., `C:\`)
- Review logging configurations for cross-platform compatibility

### 7. Validate CDK Infrastructure

Since the solution includes a CDK project, verify the infrastructure code:

```bash
dotnet build app/Bookstore.Cdk/Bookstore.Cdk.csproj
```

Review the CDK constructs for any platform-specific assumptions or dependencies.

### 8. Test on Target Platforms

If the goal is true cross-platform support, test the application on different operating systems:

- **Linux**: Test on a Linux distribution (Ubuntu, Debian, etc.)
- **macOS**: Test on macOS if applicable
- **Windows**: Verify continued Windows compatibility

For each platform:

```bash
dotnet build
dotnet test
dotnet run --project app/Bookstore.Web/Bookstore.Web.csproj
```

### 9. Performance Testing

Compare performance metrics between the legacy and migrated versions:

- Application startup time
- Request/response times
- Memory consumption
- Database query performance

### 10. Review Removed or Changed APIs

Check for usage of APIs that may have changed or been removed:

- Review compiler warnings (not just errors)
- Search for deprecated API usage
- Verify third-party library compatibility

```bash
dotnet build /warnaserror
```

## Deployment Preparation

### 1. Update Deployment Scripts

Modify deployment scripts to use cross-platform .NET commands:

- Replace `msbuild` with `dotnet build`
- Replace framework-specific publish commands with `dotnet publish`

### 2. Create Publish Profiles

Generate optimized builds for deployment:

```bash
dotnet publish app/Bookstore.Web/Bookstore.Web.csproj -c Release -o ./publish
```

Consider creating runtime-specific builds if targeting specific platforms:

```bash
dotnet publish -c Release -r linux-x64 --self-contained false
dotnet publish -c Release -r win-x64 --self-contained false
```

### 3. Verify Runtime Dependencies

Ensure the target environment has the required .NET runtime installed, or publish as self-contained:

```bash
dotnet publish -c Release --self-contained true
```

### 4. Update Documentation

Document the migration for team members:

- New build and run commands
- Updated development environment setup
- Changes to deployment procedures
- Any breaking changes or behavioral differences

## Final Checklist

- [ ] All projects build successfully
- [ ] All unit tests pass
- [ ] Integration tests pass (if applicable)
- [ ] Web application runs and functions correctly
- [ ] Database connectivity verified
- [ ] Configuration files reviewed and updated
- [ ] Application tested on target platforms
- [ ] Performance benchmarks meet expectations
- [ ] Deployment process validated
- [ ] Documentation updated