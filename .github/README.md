# Hydraulic Workbench Studio · 2.5.0

A Windows workbench for transient hydraulics in pressurized water-supply and irrigation networks. It combines MOC/FVM simulation with drawing-based modeling, elevation and pressure checks, irrigation planning, monitoring design, and canal data analysis. Hydraulic computation and drawing recognition run on the local machine.

**20 November 2026 · Version 2.5.0.** Save open projects, then choose **Software Update…** from the account menu at the top right. Existing accounts remain valid. For a new installation on Windows 10 or 11 x64, use the 2.5.0 installer or portable ZIP below.

## What's new in 2.5.0

- **Share an engineering project.** Workgroups supports one-to-one chats between confirmed contacts. Send project files, parameter snapshots or computed results, up to 32 MiB per transfer. Recipients open independent copies in a new workbench. Include inputs and dependencies when the recipient needs to edit and rerun a calculation.
- **Ask the assistant about a screenshot.** Attach or paste up to four still images and review them before sending. Separate settings control page context, personalized answers and cloud memory. A solution confirmed and submitted by a user can be reviewed by an administrator for use in answers to similar questions.
- **Manage an account.** Change the username from the avatar menu by confirming the current password. Projects and contacts remain associated with the same account. Open Settings at the lower left of the project browser. Workgroups, image analysis and cloud memory management require a compatible approval service.
- **Verify application files and authorization.** Studio checks its application modules, updater and uninstaller before sign-in. Updates verify the files they install. If authorization expires during a session, sign in online with the same account to continue with the open project. Saved credentials for external irrigation portals are filled only on a confirmed HTTPS site.

See the [full 2.5.0 update notes (Chinese)](../docs/2.5.0更新说明.txt).

## Previous release · 2.4.12

Other Irrigation Districts added five external platform shortcuts. Users can store authorized credentials for each portal and complete CAPTCHA or additional verification in the browser. See the [2.4.12 update notes (Chinese)](../docs/2.4.12更新说明.txt).

## Windows downloads · 2.5.0

Download the 2.5.0 package for the tool you use.

- [Windows installer](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.5.0/HydraulicWorkbenchStudio-2.5.0-Windows-x64-Setup.exe)
- [Full portable ZIP](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.5.0/HydraulicWorkbenchStudio-2.5.0-Windows-x64.zip)
- [Research CLI ZIP](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.5.0/HydraulicWorkbenchResearchCLI-2.5.0-Windows-x64.zip)
- [SHA-256 checksums](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.5.0/SHA256SUMS.txt)

For the portable client, extract the full ZIP to a new directory and run HydraulicWorkbenchStudioSecure.exe. Keep the executable, signed configuration and libraries together.

## Previous release: 2.4.11

Field-style electric gates retain selection and opening animation. This release also improved small-screen gate controls, sidebar recovery, unread notifications, and strict file-delta updates. See the [2.4.11 update notes (Chinese)](../docs/2.4.11更新说明.txt).

## Previous release: what's new in 2.4.10

- **Canal sharing:** use **Canal Site Monitoring (trial) → Share canals…** and enter an HWS username. Sharing covers the canals, gates, and sensors in the current site configuration. Viewers can monitor and export data. Operators can request a fresh gate reading, stop a gate, or adjust its opening or position when a vendor connection is configured. The server checks permissions on every request.
- **Automatic timestamp pairing:** use the latest stored gate sample as the reference and select each sensor's nearest record at or before it. The default tolerance is 300 seconds, adjustable from 0 to 900 seconds. Missing or out-of-tolerance readings remain blank, without interpolation, time shifts, or fallback to independent latest readings. Details and control confirmations show the reference time, data age, and pairing status; users can inspect source times and export a snapshot CSV.
- **Sharing maintenance:** vendor credentials are sealed on the platform server and are not sent to recipients. Shared windows keep separate local settings and caches. Sharing reuses an existing field station and relay package without rotating its upload key. Owners publish configuration changes; recipients refresh shared configuration. Revocation blocks new access and control but does not erase previously downloaded history.
- **Compact dialogs:** connection and field-relay dialogs are resizable, with scrollable forms and accessible save, cancel, close, and relay-package actions.

Both users need 2.4.10 or later and an online HWS session. Pairing does not change device clocks or establish simultaneous sampling; a complete match can still use stale gate data. The historical interpolated aligned-table export remains available separately. A command being accepted does not prove physical execution; an unknown outcome requires checking gate feedback and command history, without automatic resubmission.

See the [2.4.10 update notes (Chinese)](../docs/2.4.10更新说明.txt) and [step-by-step guide (Chinese)](../docs/渠道共享与时间配对使用说明.md).

## Previous release: what's new in 2.4.9

The following is the historical 2.4.9 feature summary. These capabilities remain available in this release.

- **Image-to-irrigation workflow:** import multispectral reflectance and temperature imagery, define field polygons and grids, estimate and calibrate soil moisture, calculate crop water demand, and allocate irrigation volumes to network nodes.
- **Multi-objective rotation planning:** search for feasible groups and compare the non-dominated solutions found across irrigation time, pressure uniformity, over-irrigation, and deficit. Adopt a plan after hydraulic and volume checks.
- **Pressure maps and rotation transients:** inspect supply paths and pressure risks, then replay valve transitions with MOC and a consistent color scale.
- **Canal site monitoring (trial):** define canals, gates, and sensors in Excel, generate a 3D layout, import measurement files or acquisition databases, align timestamps, and analyze reach water balance.
- **Field-data synchronization and optional gate control:** upload new readings from a field PC and retrieve them through the service. Gate control is disabled by default; supported actions and status feedback are available when enabled.
- **Drawing feedback:** graphical login verification and optional contribution of manually reviewed drawings. Permission to use a contributed drawing for other users' login questions is requested separately.
- **Research API 1.3:** script and CLI access to rotation, optimization, sensor placement, transient hydraulics, soil-moisture workflows, and canal analysis, with version, input-hash, and parameter records.
- **Maintenance:** a standalone uninstaller based on the installation manifest, plus improvements to background synchronization, window shutdown, historical imports, and compact layouts.

## Upgrade and service migration

Use the account menu's software-update command to install 2.5.0; older clients place it under Settings. File-level updates are available from 2.4.7 onward. The following compatibility and migration notes were introduced with 2.4.9: version 2.4.8 planning files remain readable, rotation transient analysis requires node outlet-valve parameters, and the first login after upgrading to 2.4.9 requires renewed acceptance of the user notice and a fresh choice for model-training consent.

**Historical 2.4.9 validation:** the **2.4.8 → 2.4.9** incremental update contained **128 changed files**, approximately **31.25 MiB** compressed, **93.07% less** than the full 2.4.9 update package. These are not current-release download figures; actual downloads depend on the starting version and local file state.

**Service migration introduced in 2.4.9:** clients connect directly to hws.aulos.cn without falling back to ngrok. The **7-day** transition for the previous endpoint has a scheduled cut-off of **2026-11-20 12:20:30 Beijing time (UTC+8), equivalent to 2026-11-20 04:20:30 UTC**. After shutdown, older clients cannot renew authorization or update through the previous endpoint; an existing offline authorization remains usable until its original expiry. Field PCs still using the old service need a migrated relay package. Stations already using a 2.4.9 package for the new service keep that package when upgrading to or sharing through 2.4.10.

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

For current Chinese documentation, see the [main README](../README.md), [2.5.0 update notes](../docs/2.5.0更新说明.txt), and [sharing and pairing guide](../docs/渠道共享与时间配对使用说明.md). The [2.5.0 client guide](../docs/ClientReadme.txt) and [installation, update and uninstall guide](../docs/InstallerReadme.txt) are also available in Chinese. For historical changes, see the [2.4.9 release notes](../docs/2.4.9更新说明.txt).
