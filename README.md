# AspNetCore Project Startup

> Based on [Milan Jovanović video](https://www.youtube.com/watch?v=QRgtcbxJlo0&t=109s&ab_channel=MilanJovanovi%C4%87)

This repository provides a **boilerplate setup for ASP.NET Core solutions**, designed to simplify configuration and enforce consistency across multiple projects in the same solution. It follows modern practices using global `.editorconfig`, `Directory.Build.*` files, and centralized package versioning via `Directory.Packages.props`.

---

## 📁 Configuration Structure

### ✅ Global `.editorconfig`

Includes a `.editorconfig` file copied from the official [.NET Runtime](https://github.com/dotnet/runtime) repository, ensuring consistent formatting and style rules aligned with Microsoft’s own standards.

This file acts as a **source code formatting standard** for the entire solution and is recognized by IDEs and code analysis tools.


### ✅ `Directory.Build.props`

Loaded **before** project `.csproj` files, this file contains global build configuration shared across all projects in the solution.

**Included settings:**

```xml
<Project>
  <PropertyGroup>
    <TargetFramework>net9.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>true</Nullable>
    <AnalysisLevel>latest</AnalysisLevel>
    <AnalysisMode>all</AnalysisMode>
    <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
    <CodeAnalysisTreatWarningsAsErrors>true</CodeAnalysisTreatWarningsAsErrors>
    <EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>
  </PropertyGroup>

  <!-- If not a Docker Compose project -->
  <ItemGroup Condition="'$(MSBuildProjectExtension)' != '.dcproj'">
    <PackageReference Include="SonarAnalyzer.CSharp">
      <PrivateAssets>all</PrivateAssets>
      <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
    </PackageReference>
  </ItemGroup>
</Project>
```

### ✅ Directory.Packages.props
Centralizes NuGet package versioning by enabling ManagePackageVersionsCentrally.

Example:

```xml
Copiar
Editar
<Project>
  <PropertyGroup>
    <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
  </PropertyGroup>

  <ItemGroup>
    <PackageVersion Include="Microsoft.AspNetCore.OpenApi" Version="9.0.2" />
    <PackageVersion Include="SonarAnalyzer.CSharp" Version="10.12.0.118525" />
  </ItemGroup>
</Project>
```

### ✅ Directory.Build.targets
Loaded after .csproj files, this file allows customization of the build process.

Example included:

```xml
Copiar
Editar
<Project>
  <ItemGroup>
    <None Update="Resources\**">
      <CopyToOutputDirectory>Never</CopyToOutputDirectory>
    </None>
  </ItemGroup>
</Project>
```
This prevents automatic copying of files inside Resources\** to the output directory.

## 🧪 Requirements
 - [.NET SDK 9.0 (preview or later)](https://dotnet.microsoft.com/)
 - Visual Studio 2022+ or JetBrains Rider
 - MSBuild 17.0+

## 🚀 How to Use
1. Clone this repository.
2. Add your projects inside the solution.
3. The global configuration files will be applied automatically.
4. Customize as needed while preserving the modular structure.

## 📦 Benefits
 - Avoid duplication in .csproj files
 - Centralized NuGet version management
 - Consistent code style across the solution
 - Build validation with strict rules and analyzers

## 📄 License
This project is licensed under the MIT License.
