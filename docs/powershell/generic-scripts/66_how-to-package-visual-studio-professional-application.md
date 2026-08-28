
# How to Package Visual Studio Professional

This guide describes how to prepare an offline installation layout and silent installation command for **Visual Studio Professional**.

The process covers downloading the Visual Studio bootstrapper, identifying the required workloads, creating a `vsconfig.vsconfig` file, generating an offline layout, and creating the installation command line.

## Step 1: Download the Bootstrapper

Download the **Visual Studio Professional bootstrapper** required for the version you intend to package.

The bootstrapper is typically named:


vs_professional.exe


Place the bootstrapper in a suitable working directory.

Example:

```text
D:\VS2022\
```

## Step 2: Analyze and Select the Required Workloads

Visual Studio workloads determine which development tools and components are installed.

Select only the workloads required for your environment. This helps reduce the installation footprint and keeps the package focused on the required functionality.

### 2.1 .NET Development Workloads

| Workload                                      | Workload ID                                      |
| --------------------------------------------- | ------------------------------------------------ |
| .NET desktop development                      | `Microsoft.VisualStudio.Workload.ManagedDesktop` |
| ASP.NET and web development                   | `Microsoft.VisualStudio.Workload.NetWeb`         |
| Azure development                             | `Microsoft.VisualStudio.Workload.Azure`          |
| Data storage and processing                   | `Microsoft.VisualStudio.Workload.Data`           |
| .NET Multi-platform App UI development (MAUI) | `Microsoft.VisualStudio.Workload.NetCrossPlat`   |

### 2.2 C++ Development Workloads

| Workload                                     | Workload ID                                       |
| -------------------------------------------- | ------------------------------------------------- |
| Desktop development with C++                 | `Microsoft.VisualStudio.Workload.NativeDesktop`   |
| Universal Windows Platform (UWP) development | `Microsoft.VisualStudio.Workload.Universal`       |
| Mobile development with C++                  | `Microsoft.VisualStudio.Workload.NativeMobile`    |
| C++ Linux development                        | `Microsoft.VisualStudio.Workload.NativeCrossPlat` |
| Game development with C++                    | `Microsoft.VisualStudio.Workload.NativeGame`      |

!!! note
C++ CMake tools for Windows are generally provided as a component associated with the relevant C++ workload rather than as a separate top-level workload.

### 2.3 Mobile and Cross-Platform Workloads

| Workload                                      | Workload ID                                    |
| --------------------------------------------- | ---------------------------------------------- |
| Mobile development with .NET (Xamarin)        | `Microsoft.VisualStudio.Workload.Xamarin`      |
| .NET Multi-platform App UI development (MAUI) | `Microsoft.VisualStudio.Workload.NetCrossPlat` |
| Mobile development with C++                   | `Microsoft.VisualStudio.Workload.NativeMobile` |

### 2.4 Game Development Workloads

| Workload                    | Workload ID                                  |
| --------------------------- | -------------------------------------------- |
| Game Development with Unity | `Microsoft.VisualStudio.Workload.Unity`      |
| Game Development with C++   | `Microsoft.VisualStudio.Workload.NativeGame` |

### 2.5 Web and Cloud Workloads

| Workload                    | Workload ID                              |
| --------------------------- | ---------------------------------------- |
| ASP.NET and Web Development | `Microsoft.VisualStudio.Workload.NetWeb` |
| Node.js development         | `Microsoft.VisualStudio.Workload.Node`   |
| Azure development           | `Microsoft.VisualStudio.Workload.Azure`  |

### 2.6 Other Development Workloads

| Workload                            | Workload ID                                             |
| ----------------------------------- | ------------------------------------------------------- |
| Python development                  | `Microsoft.VisualStudio.Workload.Python`                |
| Visual Studio extension development | `Microsoft.VisualStudio.Workload.VisualStudioExtension` |
| Office/SharePoint development       | `Microsoft.VisualStudio.Workload.Office`                |

## Step 3: Create the `vsconfig.vsconfig` File

Create a configuration file named:

```text
vsconfig.vsconfig
```

The file contains the workload IDs that should be included in the Visual Studio installation.

### Example Configuration

The following example includes:

* .NET desktop development
* ASP.NET and web development

```json
{
  "version": "1.0",
  "components": [
    "Microsoft.VisualStudio.Workload.ManagedDesktop",
    "Microsoft.VisualStudio.Workload.NetWeb"
  ]
}
```

Save the configuration file in a known location.

Example:

```text
D:\vsconfig.vsconfig
```

### Why Use a `vsconfig` File?

Using a configuration file makes the package easier to reproduce and maintain because the required workloads are explicitly defined.

When additional workloads are required, add their workload IDs to the `components` array.

## Step 4: Create an Offline Layout

Use the Visual Studio bootstrapper to download the required installation files and create an offline layout.

Example:

```powershell
.\vs_professional.exe --layout "D:\VS2022_Layout" --lang en-US --config "D:\vsconfig.vsconfig"
```

### Command Parameters

| Parameter  | Description                                                              |
| ---------- | ------------------------------------------------------------------------ |
| `--layout` | Specifies the destination directory for the offline installation layout. |
| `--lang`   | Specifies the Visual Studio language to download.                        |
| `--config` | Specifies the `vsconfig` file containing the selected workloads.         |

In this example, the offline layout is created in:

```text
D:\VS2022_Layout
```

The generated layout contains the installation files required to install Visual Studio from a local source.

## Step 5: Installation Command Line

After the offline layout has been created and copied to the target computer, Visual Studio can be installed silently.

Example:

```powershell
Set-Location "C:\Windows\Temp\VS2022Layout"

.\vs_professional.exe `
    --quiet `
    --wait `
    --norestart `
    --installPath "C:\Program Files\Microsoft Visual Studio\2022\Professional" `
    --add Microsoft.VisualStudio.Workload.ManagedDesktop `
    --add Microsoft.VisualStudio.Workload.NetWeb `
    --includeOptional

exit $LASTEXITCODE
```

### Installation Parameters

| Parameter           | Description                                                          |
| ------------------- | -------------------------------------------------------------------- |
| `--quiet`           | Runs the installation without user interaction.                      |
| `--wait`            | Waits for the installation process to complete.                      |
| `--norestart`       | Prevents the installer from automatically restarting the system.     |
| `--installPath`     | Specifies the Visual Studio installation directory.                  |
| `--add`             | Adds the specified Visual Studio workload.                           |
| `--includeOptional` | Includes optional components associated with the selected workloads. |

## Workload Configuration vs Installation Command

There are two places where workloads appear in this process.

The `vsconfig.vsconfig` file defines the workloads used when creating the offline layout:

```json
{
  "version": "1.0",
  "components": [
    "Microsoft.VisualStudio.Workload.ManagedDesktop",
    "Microsoft.VisualStudio.Workload.NetWeb"
  ]
}
```

The installation command explicitly specifies the workloads to install:

```powershell
--add Microsoft.VisualStudio.Workload.ManagedDesktop `
--add Microsoft.VisualStudio.Workload.NetWeb
```

Keep these configurations aligned with the requirements of the package.

## Example Directory Structure

A working package might be organized as:

```text
VS2022Package/
├── vs_professional.exe
├── vsconfig.vsconfig
└── VS2022_Layout/
    ├── ...
    └── ...
```

The exact contents of the offline layout depend on the Visual Studio version, language, and selected workloads.

## Recommended Packaging Workflow

```text
Download Bootstrapper
        ↓
Analyze Required Workloads
        ↓
Create vsconfig.vsconfig
        ↓
Create Offline Layout
        ↓
Validate Installation
        ↓
Build Deployment Package
        ↓
Deploy to Target Systems
```

## Validation

Before deploying to production systems, test the package on a representative test computer.

Verify that:

* Visual Studio installs without user interaction.
* The expected installation path is used.
* Required workloads are installed.
* Required optional components are available.
* The system is not unexpectedly restarted.
* The installation returns the expected exit code.
* Visual Studio launches successfully after installation.
* Applications or development tools that depend on Visual Studio work correctly.

## Troubleshooting

| Issue                                 | Possible resolution                                                    |
| ------------------------------------- | ---------------------------------------------------------------------- |
| Bootstrapper not found                | Verify the bootstrapper filename and working directory.                |
| Offline layout creation fails         | Verify available disk space and the `--layout` path.                   |
| Configuration file not found          | Verify the path supplied to `--config`.                                |
| Required workload missing             | Verify the workload ID in `vsconfig.vsconfig`.                         |
| Installation does not run silently    | Verify the command-line parameters and test the bootstrapper manually. |
| Installation fails from offline media | Verify that the offline layout contains all required components.       |

## Best Practices

* Use a version-controlled `vsconfig.vsconfig` file.
* Select only the workloads actually required.
* Keep the bootstrapper and offline layout associated with the intended Visual Studio release.
* Test the offline installation before packaging for production.
* Validate the installation exit code.
* Keep the package source organized and documented.
* Rebuild the offline layout when the Visual Studio version or required workloads change.

## Notes

The workload selection in this document is provided as a reference. The actual workloads required for an installation depend on the development environment and application requirements.

The bootstrapper version should match the Visual Studio release being packaged.

The installation path can be customized to meet your organization's packaging and deployment standards.

## Conclusion

Packaging Visual Studio Professional using the bootstrapper, a `vsconfig` file, and an offline layout provides a repeatable approach for controlled deployment.

The basic process is:

1. Download the bootstrapper.
2. Identify the required workloads.
3. Create the `vsconfig.vsconfig` file.
4. Generate the offline layout.
5. Test the installation.
6. Deploy using the silent installation command.

````

One point to be aware of: this is currently written as a **generic Visual Studio packaging procedure**, so it belongs under:


docs/powershell/generic-scripts/


It should not go under your `PSADT/4.17` section unless you later turn this into an actual PSADT deployment package.
