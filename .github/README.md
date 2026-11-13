# Hydraulic Workbench Studio · 2.4.9

A Windows workbench for transient hydraulics in pressurized water-supply and irrigation networks. It combines MOC/FVM simulation, drawing-based modeling, elevation and pressure checks, irrigation scheduling, monitoring design, and canal field-data analysis.

**Version 2.4.9 connects aerial imagery, field water demand, node irrigation volumes, and rotation planning.** It also supports user-defined irrigation sites with canal, gate, and sensor data. Hydraulic computation and drawing inference run locally; account, update, assistant, and field-data relay services use the Aliyun deployment at [hws.aulos.cn](https://hws.aulos.cn).

This repository distributes **Windows binaries and documentation; it does not publish the software source code**. End users do not need Python or CUDA.

Released **2026-11-13** for **Windows 10/11 x64**. Existing accounts remain valid.

## Downloads

- [Windows installer](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.9/HydraulicWorkbenchStudio-2.4.9-Windows-x64-Setup.exe)
- [Full portable ZIP](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.9/HydraulicWorkbenchStudio-2.4.9-Windows-x64.zip)
- [Research CLI ZIP](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.9/HydraulicWorkbenchResearchCLI-2.4.9-Windows-x64.zip)
- [SHA-256 checksums](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.9/SHA256SUMS.txt)
- [All releases](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases)

Install with Setup.exe or extract the full ZIP into a new directory and run `HydraulicWorkbenchStudioSecure.exe`. Keep all files in the distribution together. GitHub's automatically generated **Source code (zip/tar.gz)** and **Code → Download ZIP** contain the public documentation snapshot, not the application.

## What's new

- **Image-to-irrigation workflow:** import multispectral reflectance and temperature imagery, define field polygons and grids, estimate and calibrate soil moisture, calculate crop water demand, and allocate irrigation volumes to network nodes.
- **Multi-objective rotation planning:** search for feasible groups and compare the non-dominated solutions found across irrigation time, pressure uniformity, over-irrigation, and deficit. Adopt a plan after hydraulic and volume checks.
- **Pressure maps and rotation transients:** inspect supply paths and pressure risks, then replay valve transitions with MOC and a consistent color scale.
- **Canal site monitoring (trial):** define canals, gates, and sensors in Excel, generate a 3D layout, import measurement files or acquisition databases, align timestamps, and analyze reach water balance.
- **Field-data synchronization and optional gate control:** upload new readings from a field PC and retrieve them through the service. Gate control is disabled by default; supported actions and status feedback are available when enabled.
- **Drawing feedback:** graphical login verification and optional contribution of manually reviewed drawings. Permission to use a contributed drawing for other users' login questions is requested separately.
- **Research API 1.3:** script and CLI access to rotation, optimization, sensor placement, transient hydraulics, soil-moisture workflows, and canal analysis, with version, input-hash, and parameter records.
- **Maintenance:** a standalone uninstaller based on the installation manifest, plus improvements to background synchronization, window shutdown, historical imports, and compact layouts.

## Upgrade and service migration

Use the account menu's software-update command; older clients place it under Settings. File-level updates are available from 2.4.7 onward. Version 2.4.8 planning files remain readable. Rotation transient analysis requires node outlet-valve parameters. The first login after upgrading requires renewed acceptance of the user notice and a fresh choice for model-training consent.

The verified **2.4.8 → 2.4.9** incremental update contains **128 changed files**, with approximately **31.25 MiB** of compressed download data, **93.07% less** than the full update package. These figures apply to the verified 2.4.8 installation state; other starting versions or locally modified files may require a different download.

**Version 2.4.9 connects directly to hws.aulos.cn and does not fall back to ngrok.** The **7-day** transition for the previous endpoint ends at **2026-11-20 12:20:30 Beijing time (UTC+8), equivalent to 2026-11-20 04:20:30 UTC**. Upgrade before this deadline. After shutdown, older clients cannot renew authorization or update through the previous endpoint; an existing offline authorization remains usable until its original expiry. Field PCs should use a newly generated 2.4.9 relay package.

## Research CLI

Keep `auth_distribution.json` beside the executable. The distributed CLI uses a platform account and supports offline calculation while its authorization is valid.

```powershell
hydraulic-moc login
hydraulic-moc status
hydraulic-moc api --help
```

See the [headless research API guide](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/无界面研究API说明.md) for workflow-specific input formats and commands.

## Model and data scope

Drawing recognition proposes geometry, connectivity, and component types. Users must verify scale, sources, elevations, demands, diameters, roughness, and wave speeds before calculation. Drawing contribution is voluntary and local inference remains available without contributing a drawing.

The grouping search reports the non-dominated solutions it finds; it does not establish a complete Pareto front or global optimality. Exact-optimization certificates apply to their stated problem, bounds, and gaps.

Soil-moisture estimates require appropriate imagery and field calibration. Bundled field imagery and sampling points are synthetic demonstrations. Recorded validation for the KarLy dataset does not establish accuracy for an arbitrary field. Remote field-data synchronization is optional; uploaded readings are relayed and retained for 90 days, and accounts retrieve them according to authorization.

For full Chinese documentation, see the [main README](https://github.com/Iqx-nwafu/hydraulic-workbench-studio#readme) and [2.4.9 release details](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.9更新说明.txt).
