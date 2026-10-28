# Hydraulic Workbench Studio · 2.4.5

**Pressurized pipe-network transient hydraulic simulation platform**

A one-dimensional transient hydraulic analysis platform for urban water supply and irrigation conveyance and distribution, supporting the method of characteristics (MOC) and the finite volume method (FVM) for single pipes, branched networks and looped networks. From parameter modeling, drawing recognition and topology review, through transient computation, real longitudinal profiles and pressure checks, valve-closure scenario comparison, rotational irrigation planning and sensor placement, to result reports, the whole analysis is done in a single workbench. **Starting with 2.4.5, the platform's hydraulic computation can also be called directly by research code outside the GUI, through a headless research API and command-line tools.**

This repository provides the **Windows client, Research CLI, release notes and user documentation; the software source code is not published**. Regular users do not need to install Python or set up a development environment.

**[Download the 2.4.5 installer](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.5/HydraulicWorkbenchStudio-2.4.5-Windows-x64-Setup.exe)** · **[Download the full portable ZIP](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.5/HydraulicWorkbenchStudio-2.4.5-Windows-x64.zip)** · **[Download the Research CLI](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.5/HydraulicWorkbenchResearchCLI-2.4.5-Windows-x64.zip)** · **[View this release](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/tag/v2.4.5)**

Chinese documentation and a mirror for users in mainland China: [Gitee](https://gitee.com/Iqx-nwafu/hydraulic-workbench-studio)

Users with an earlier version installed can also upgrade to 2.4.5 directly via **Settings → Software Update…**; for the highlights of this release, see below and the [2.4.5 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.5%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt).

Getting started: download and install → submit a registration request → sign in once approved. **Existing accounts keep working; there is no need to register again.**

## Features

| Capability | What you can do |
| --- | --- |
| Transient analysis of single pipes and networks | Analyze pressure, flow and wave propagation with MOC or FVM; supports branched and looped networks, mechanical valve operations and varying pipe parameters |
| Modeling and computation checks | Set geometry, materials, wave speed, friction, boundaries and initial states; initialize from steady state and check parameters, grid, time step and resource requirements |
| Drawing recognition and topology editing | Recognize PNG/JPG/JPEG/DXF drawings locally; review nodes, connections, sources and scale, fill in real hydraulic parameters, then import into the model |
| Real longitudinal profiles and pressure checks | Import or edit pipe centerline elevations; view maximum/minimum HGL envelopes along the line, time histories at monitoring locations, and segments exceeding engineering pressure limits |
| Valve-closure scenario comparison | Compare different valve-operation scenarios, identify segments where exceedances are eliminated, persist or newly appear, and export pressures at the same locations together with valve timing |
| Result analysis and reporting | View time histories, spatial distributions, animations, risk alerts and conservation information; save scenarios, result archives, charts and reports |
| Topology deliverables | Add editable text annotations and export the complete topology as PNG, JPG or DWG |
| Sensor placement and rotational irrigation | Compute rotational irrigation schedules that satisfy velocity, pressure and water-volume constraints; determine the minimum number of pressure gauges and flow meters, and their locations, for a given error tolerance; optionally co-design pipe diameters and the rotation schedule, with an optimality certificate |
| Research interface (new in 2.4.5) | Taking planning JSON as input, batch-evaluate irrigation groups, simulate fault and recovery timelines and run Monte Carlo outside the GUI; results carry version information and input hashes and plug directly into research code and optimizers |
| AI assistant | Ask where features are, how to perform tasks, what parameters mean and how to handle errors; answers can take the current page and scenario parameters into account, and unresolved questions can be turned into user feedback with one click |
| Chinese/English workbench | Work is organized as **Model → Check → Run → Results → Report**; supports Simplified Chinese and English |

Different solution methods support different physical models and boundary conditions. The program checks whether the current combination is within scope; details are available under **Help → Model Scope**.

## New in 2.4.5: research interface

2.4.5 opens up the steady-state hydraulic model that the platform uses for rotational irrigation planning as a programmable interface. Research code no longer needs to click through irrigation groups one by one in the GUI, or reimplement the hydraulic computation itself, to obtain results that follow the same conventions as the platform's planner and are reproducible and archivable.

### Headless research API (`hydraulic_moc.api`, API 1.0)

| Capability | Description |
| --- | --- |
| Steady-state evaluation | Exact matrix solution for tree networks, global gradient algorithm iteration for looped networks; evaluate a single irrigation group or batches of groups; H-W, D-W and fixed Darcy coefficients can be mixed pipe by pipe, with per-pipe minor-loss coefficients and kinematic viscosity |
| Fault isolation | Node, pipe and outlet faults are isolated automatically based on connectivity: in tree networks the subtree downstream of the fault is isolated; in looped networks supply is automatically rerouted through other paths |
| Timeline simulation | Rotation schedules, time-varying source head (step, linear, constant), and fault and recovery events; an external replanning hook is called after each event, so any optimizer, such as NSGA-II, can be plugged in |
| Monte Carlo | Fixed seed, each scenario independently reproducible (scenario i is determined by `[seed, i]`), multi-process parallelism; results do not depend on the number of processes or execution order |
| Exact caching | Exact caching keyed on "set of open outlets + set of closed pipes"; source head is applied only as a uniform offset, significantly speeding up large-scale combinatorial searches without introducing any approximation |
| Result archiving | Every result carries `api_version`, `platform_version`, the SHA-256 of the input JSON, `network_digest`, `model_digest` and all effective options |

Metrics include minimum/mean pressure at open outlets, magnitude and count of each type of constraint violation, and feasibility, plus, for timeline simulation, supply compliance rate, deficit, pressure uniformity, fraction of feasible time and more; objective functions are composed from these metrics by the researcher.

### Research CLI

The release includes a standalone **Research CLI** (`HydraulicWorkbenchResearchCLI-2.4.5-Windows-x64.zip`) that runs right after unzipping: no sign-in, no Python environment, and no dependency on whether the client is installed.

```powershell
hydraulic-moc api describe    --project net.json                                   # network summary and effective model options
hydraulic-moc api evaluate    --project net.json --groups groups.jsonl --output out.jsonl [--detail]
hydraulic-moc api simulate    --project net.json --scenario scenario.json --output sim/
hydraulic-moc api monte-carlo --project net.json --config mc.json --output mc/ --workers auto
hydraulic-moc irrigation-eval --project net.json ...                                # irrigation-group steady-state evaluator
```

| Command | Input | Output |
| --- | --- | --- |
| `evaluate` | JSONL, one `{"id", "open": [...], "source_head_m"?, "unavailable"?, "faults"?}` per line | `.jsonl` (with `--detail`, includes per-node, per-pipe and per-outlet values) or `.csv`, plus `<name>.manifest.json` |
| `simulate` | `{"schedule": [...], "head"?, "events"?, "horizon_h"?}` | `segments.csv`, `outlets.csv`, `summary.json` |
| `monte-carlo` | Scenario specification + `"scenarios"` + `"schedule"` | `scenarios.csv`, `events.csv`, `manifest.json` |

All model options can be overridden with command-line arguments: `--friction --local-loss-ratio --min-pressure --min-velocity --max-velocity --velocity-scope --max-source-flow --outlet-law --outlet-design-pressure`.

### Planning JSON: a common input

The research interface and the desktop **Sensors and irrigation** workspace share the same planning JSON (`hydraulic-irrigation-planning` v1): a file obtained with **Save project** in the GUI can be passed straight to the interface, and files generated by the interface can be opened in the GUI. `topology.nodes / pipes` describes a single-source tree or looped network; each record in `fields` is one outlet (design flow and target volume); `settings` holds the head-end pressure, minimum pressure, velocity range and supply cap. Interface-specific options live under `settings.hydraulic_api` and do not affect the desktop app. Units are fixed: length in m, flow in m³/h (interface results with an `_m3_s` suffix are in m³/s), volume in m³, time in h.

### Research defaults

With no options given, the rotational irrigation research conventions apply, so studies do not need to repeat these values in their files:

| Option | Default |
| --- | --- |
| Friction law | EPANET 2.2, all flow regimes (consistent with the platform's transient solver) |
| Minor losses | Counted as 0.1 × the friction loss (per-pipe K values are added on top) |
| Minimum pressure head / design pressure h_d | 11.4 m |
| Velocity range | 0.3–2.5 m/s, checked only on pipes carrying flow |
| Outflow law | `linear_cap`: q_d·clip(p/h_d, 0, 1) |

If a planning JSON saved from the GUI already contains shared keys such as `min_pressure_m` or `velocity_scope`, the values in the file take precedence; `describe` prints the effective values, and all options go into `model_digest` and are archived with the results.

### Irrigation-group steady-state evaluator (`irrigation_eval`)

A lightweight evaluation interface for rotational irrigation scheduling research (command `hydraulic-moc irrigation-eval`): tree networks, D-W friction; it takes emitter flows and the unavailable set as input and outputs per-node pressures, per-pipe flows and velocities, and feasibility, with exact caching. Under the same conventions it matches the headless API bit for bit on 200 random combinations, so existing research code can keep using it.

### Scope

The interface performs steady-state and piecewise steady-state computation for rotational irrigation planning, scheduling optimization and reliability analysis; for transient (water hammer) checks, use the platform's MOC/FVM solvers. It supports single-source tree and looped networks made up of passive pipes, the same scope as the desktop planner. For the full input format, options, Python usage and stability policy, see the [headless research API guide (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/%E6%97%A0%E7%95%8C%E9%9D%A2%E7%A0%94%E7%A9%B6API%E8%AF%B4%E6%98%8E.md).

## 2.4.4 highlights (compared with 2.4.2)

1. **AI assistant**: use the robot button at the top right to ask where features are, how to perform tasks, what parameters mean and how to handle errors; tick **Include current page and case** to get answers based on your current situation (scenario files, drawings and result data are not sent). Questions it cannot resolve can be turned into user feedback with one click. Each account can ask 10 questions per day; online sign-in is required.
2. **Draggable IDs**: node IDs and pipe IDs can be dragged directly to a suitable position; their positions are kept when they are hidden and shown again, when the scenario is reopened, and on export. Right-click **Restore automatic position** to revert.
3. **Batch style editing**: hold Ctrl/Shift to multi-select text annotations and IDs, then drag them together or right-click **Font, size and colour for all selected** to change them all at once.
4. **Optional IDs on export**: when exporting the topology, you can choose whether to include node IDs and pipe IDs; exported PNG/JPG/DWG files match the on-screen adjustments.
5. **Topology editing shortcuts**: Delete removes the selected nodes, pipes and annotations; Ctrl+Z undoes and Ctrl+Y redoes, with results written back to the scenario automatically. The recognition-result review window supports these shortcuts as well.
6. **Feedback replies**: when the developer replies to your feedback, an unread indicator appears at the top right after sign-in; view replies in **My feedback and replies…**.
7. **Clearer node tee settings**: the node parameter dialog immediately indicates whether the current node can act as a 90° tee, and if **Force enable** is used when the conditions are not met, the reason is shown directly.
8. **Bug fix**: fixed a bug where, if a drawing file name or project directory contained Chinese characters, saving a scenario, editing node parameters or starting a computation reported EPANET 302 “cannot open input file” and work could not continue.

Behavior change: text annotations are now **selected with a single click and edited with a double click**. ID layout is saved in the topology file; older versions still display IDs in their automatic positions. For details, see the [2.4.4 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.4%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) and [2.4.3 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.3%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt).

## What's new in 2.4.2 compared with 2.2.0

Building on the numerical computation and pressure-check capabilities of 2.2.0, this release adds the **Sensors and irrigation** workspace and improves topology deliverables, the sign-in experience and workbench operation.

1. **Topology export**: supports PNG, JPG and AutoCAD 2004 DWG, including the complete topology and text annotations, as well as content outside the viewport. Images support 72–1200 DPI and a sheet-width setting; DWG output is an editable vector drawing.
2. **Topology text annotations**: supports multi-line text, click-to-edit, drag-to-move, and adjustable font, size and color. Annotations are saved with the topology and included in exports; editing annotations alone does not invalidate hydraulic results.
3. **HD animated sign-in**: a high-definition ocean video background with a revised brand and tagline layout. When the video ends it stays on the last frame; scroll the mouse wheel up in the sign-in window to replay it from the start.
4. **Local account avatar**: choose a static PNG, JPG/JPEG, WebP or BMP image from the avatar menu at the top right, or restore the default. Avatars are stored locally per account and are not synced across devices.
5. **Feedback with screenshots**: attach up to 4 static PNG, JPG or WebP images to feedback, with preview and removal before sending. Image feedback requires online sign-in and server-side support; if it is not supported, remove the images and submit text-only feedback.
6. **Rotational irrigation planning and minimum sensor placement**: select **Sensors and irrigation** in the **Project** browser, load the network and field water demands, then compute a rotation schedule that first minimizes total irrigation duration and then optimizes within-group pressure uniformity (mean absolute deviation, variance), checking velocity, pressure and planned water volume in every round. It then determines the minimum number and locations of pressure gauges and flow meters within the specified instrument error and estimation tolerance, checking network-wide pressure and pipe flow errors round by round.
7. **Joint design of pipe diameters and irrigation rotation**: import a topology whose pipe diameters are yet to be determined, or treat the trunk-main diameters of the current network as design variables; design diameters and rotation simultaneously based on the **Diameter / cost catalogue**, view the diameter layout drawing and export it as SVG. After choosing a design, click **Select and freeze**, then proceed to error checks and sensor placement.
8. **Global optimization and optimality certificates**: in **Solver settings**, choose priority mode (shortest total duration first, then the other objectives in turn) or multi-objective Pareto mode. Results come with an optimality certificate recording each objective's lower and upper bounds, gap and proof status, which can be viewed and exported as JSON/CSV. Sensor count or cost can be included in the joint decision; the original sampling-based search method remains available as an option. For large instances where the gap does not close within the time limit, the certificate faithfully reports the current bounds and does not label a feasible solution as proven optimal.
9. **Result export**: rotation schedules, sensor placements, round-by-round error checks and joint design results can be exported as JSON and CSV; planning inputs can be saved and reopened. Example networks and field data are bundled (`data/irrigation_examples/`).
10. **Contact binding**: in **Account information**, request to bind a mobile number and email address; the binding takes effect after administrator approval, and the existing binding remains valid until then.
11. **System Overview and recent project files**: the overview page brings together the analysis domain, model checks, computation results, core validation and current scenario parameters; it provides the five-step analysis workflow, an **Open a calculation module** shortcut and next steps, and lists recently opened or saved scenarios and network configurations.
12. **Computing resources and interface**: native computation can use all available CPU cores, and independent round searches run in parallel; long-running computations are shown in the bottom progress bar and can be canceled at any time. Several layouts were refined, and buttons now share rounded corners, hover transitions and state feedback.

For detailed instructions, see the [2.4.2 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.2%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt). MOC/FVM, real longitudinal profiles, pressure segments and valve-closure scenario comparison are existing platform capabilities; this release does not change their underlying physical models.

## Download, installation and launch

On the [Release page](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/tag/v2.4.5), expand **Assets** and choose from the following files:

| File | Purpose |
| --- | --- |
| [Windows x64 installer](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.5/HydraulicWorkbenchStudio-2.4.5-Windows-x64-Setup.exe) | Recommended for first-time users; installs via a wizard and creates desktop and Start menu shortcuts |
| [Windows x64 full portable ZIP](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.5/HydraulicWorkbenchStudio-2.4.5-Windows-x64.zip) | Extract completely to a folder of your choice, then run |
| [Research CLI ZIP](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.5/HydraulicWorkbenchResearchCLI-2.4.5-Windows-x64.zip) | Command-line tool for the headless research API; runs right after unzipping, with no sign-in or Python environment required |
| [SHA256SUMS.txt](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.5/SHA256SUMS.txt) | SHA-256 checksums for verifying the installer, the ZIP and the command-line tool |

**The `Source code (zip/tar.gz)` archives generated automatically by GitHub and the green `Code → Download ZIP` button contain only this repository's public files; they are neither the software source code nor a runnable client. Please download the installer or full ZIP from the table above.**

Requires Windows 10/11 (64-bit); reserve at least 2 GB of disk space for installation and future updates. The client ships with its runtime dependencies and a local CPU-based drawing-recognition component, so there is no need to install Python, PyTorch or CUDA separately, and no NVIDIA GPU is required. Actual computational resource needs vary with network size, grid and output settings; review the check messages before running.

**Installer**: double-click `HydraulicWorkbenchStudio-2.4.5-Windows-x64-Setup.exe` and follow the wizard to install for the current Windows user, then launch from the desktop or Start menu.

**Portable version**: in File Explorer, choose **Extract All** on the ZIP, open the complete extracted folder and run **`HydraulicWorkbenchStudioSecure.exe`**. Choose a directory you have write access to; do not run the program from inside the archive preview window, and do not move the main executable on its own, because the configuration, updater, drawing-recognition component and runtime libraries must all be kept.

**Research CLI**: after extracting `HydraulicWorkbenchResearchCLI-2.4.5-Windows-x64.zip`, open that directory in PowerShell and run `hydraulic-moc api describe --project <planning JSON>`; the bundled `examples/` folder contains example networks, irrigation groups, timeline scenarios and a Monte Carlo configuration.

The program is not currently signed with a Windows Authenticode code signature, so Windows may display "Unknown publisher". Verify that you downloaded it from this repository and check the SHA-256; if a file's origin is unknown or its checksum does not match, download it again instead of turning off security protection.

To verify, open PowerShell in the folder containing the download, compute the ZIP's checksum and compare it with this release's `SHA256SUMS.txt`:

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath '.\HydraulicWorkbenchStudio-2.4.5-Windows-x64.zip'
```

## Registration and sign-in

First-time registration and first sign-in require an internet connection and an available account service. The software account is independent of your GitHub account.

1. In the sign-in window, click **No account? Register** and enter an account name, a password and the password confirmation.
2. Account names are 3–32 characters and may contain Chinese characters, letters, digits, underscores, periods, hyphens or `@`, but no spaces; passwords are 6–20 characters, must not be a common password rejected by the program, and must not contain the full account name.
3. Read the **User Notice and Privacy Statement**, tick the consent box and submit. The message **Awaiting administrator approval** means your request has been submitted; you can tell the developer the account name you applied for through an existing contact channel, but **do not send your password**.
4. Once approved, enter your account name and password and click **Secure Sign-In**. Existing accounts sign in directly; there is no need to register again after a version update.

**Optional: Save corrections locally (withdrawable)** is off by default and does not affect registration, sign-in or computation; ticking it does not trigger automatic upload of drawings or automatic training.

After a successful online sign-in, the program stores a local offline license for the current Windows user. For later sign-ins, still click **Secure Sign-In**; if the service cannot be reached, the program tries to use a valid local offline license. Offline use is subject to the license validity period; after switching computers or Windows users, or deleting the license cache, you need to sign in online again.

Registration, password changes, account-name changes, sending feedback and license renewal require an internet connection. Password and account-name operations are under **Settings → Password and Security…**; **Remember Username** and **Remember Password** are optional, and remembering the password is not recommended on shared computers.

## Your first computation

We recommend running the bundled examples first to get familiar with **1 Model → 2 Check → 3 Run → 4 Results → 5 Report** before entering real project data.

1. After signing in, click **Model → Network Model**, then **File → Open Current Scenario…** and select the bundled `configs/longitudinal_network_test.json`. Go to the network page first, then open the network scenario.
2. Click **Model → Network profiles and pressure limits…**. Example pipe `P` is 100 m long, with pipe centerline elevations of 0, 20 and 0 m at stations 0, 35 and 100 m; the default pressure-head limits are 12–36 m.
3. Keep the default MOC settings and click **2 Check** at the top. Review blocking issues and warnings; if there are blocking issues, fix them first.
4. Choose **Solve → Run Current Scenario** or press `F5` to start the computation.
5. When it finishes, go to the network **Results** page and select **Profile assessment** in the results workbench to view the pipe centerline, the maximum/minimum HGL envelopes and the segments exceeding limits; click the high point at 35 m to view its pressure time history.
6. Review and export the report; to change the output location, expand **Output & Run…**. Use **File → Save Current Scenario…** to save a scenario JSON that can be edited again.

Single-pipe example: first open **Model → Single-Pipe Model**, then open `configs/longitudinal_single_pipe.json`. For the full steps of CSV import, FVM comparison and scenario comparison, see the [longitudinal profile example (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/LongitudinalReadme.txt).

The examples are synthetic dynamic scenarios. HGL is the hydraulic grade line (piezometric head), and pressure head is `HGL − pipe centerline elevation`. Envelopes record extrema over the internal computation steps, so an extremum may occur between two saved frames.

### Rotational irrigation and sensor placement example

1. In the **Project** browser on the left, click **Sensors and irrigation**, or select it under **Open a calculation module** on the **System Overview** page.
2. Click **Load network** and select a bundled example (e.g. **All-pipe flow example**). In **Inputs** and **Constraints & errors**, verify the head-end pressure, supply cap, minimum pressure, upper and lower velocity limits, and field water demands.
3. Click **1 Plan irrigation** to view the units opened in each round, durations, supply volumes and pressure statistics; you can proceed to the next step only after all hard constraints pass and a certificate has been obtained.
4. Verify the instrument error, estimation tolerance and validation domain, then click **2 Optimize sensors**; view the required instrument locations and per-round error bounds on the **Sensors** and **Round accuracy** tabs. In the diagram, circles are pressure gauges and triangles are flow meters.
5. To design pipe diameters at the same time, enable diameter design on the **Diameter / irrigation design** tab, fill in the **Diameter / cost catalogue**, choose a solution method in **Solver settings**, click **Generate and search designs**, and then **Select and freeze** the chosen design.
6. Use **Save project** to save inputs that can be reopened, and **Export results** to save the schedule, sensor placement, error checks and optimality certificate. A saved planning JSON can be used directly as input to the research interface.

Rotational irrigation results correspond to the network, fields, constraints and error settings you entered; the minimum sensor count applies only to the frozen design and the declared error domain. The sample values in the cost catalog are relative indices; for actual design, enter real price quotes.

### Research interface example

1. Extract the Research CLI and open its directory in PowerShell.
2. Run `hydraulic-moc api describe --project examples\farm_network.json` to view the network summary and the effective model options.
3. Run `hydraulic-moc api evaluate --project examples\farm_network.json --groups examples\groups.jsonl --output out.jsonl --detail` to get the pressure, flow, velocity and feasibility of each irrigation group, plus `out.manifest.json`.
4. Run `hydraulic-moc api simulate --project examples\farm_network.json --scenario examples\scenario.json --output sim\` to get timeline results including faults and recovery (`segments.csv`, `outlets.csv`, `summary.json`).
5. Run `hydraulic-moc api monte-carlo --project examples\farm_network.json --config examples\monte_carlo.json --output mc\ --workers auto` to get reproducible multi-scenario statistics with a fixed seed.

Replace `examples\farm_network.json` with a file saved via **Save project** in the GUI to run the same computations on your own network.

## Using your own project data

| Step | Key actions |
| --- | --- |
| Modeling | Choose single pipe or network; enter geometry, materials, wave speed, friction, initial conditions, boundaries and valve operations |
| Drawing import (optional) | Recognize a drawing via **File → Import Network Drawing (PNG/JPG/JPEG/DXF)…**, manually review connections, sources and scale, then fill in the hydraulic parameters |
| Profiles and limits (optional) | Import a CSV or edit the table in the corresponding editor; make sure pipe centerline elevations and boundary heads use the same elevation datum |
| Check and run | Choose the applicable method (MOC or FVM), verify grid, time step, output interval and resource requirements, resolve blocking issues, then run |
| Results and comparison | View time histories, spatial results, risk alerts, longitudinal profiles and segment differences between valve-closure scenarios |
| Save and deliver | Save the scenario JSON, results and reports; network `.hnr` archives can be reopened with **Load Result Archive…**; use the topology toolbar to add text and export the topology |

The longitudinal profile CSV header is `pipe_id,station_m,centerline_elevation_m`. The single-pipe ID is always `single`; networks use the actual pipe IDs. Templates are in the bundled `data/input/` folder.

After changing a scenario, profile or pressure limits, run the computation again. Not setting pressure limits does not mean the check has passed. The exceedance segment length is the length of the set of locations that exceeded the limit at some point within the statistics window; it is neither the length exceeding the limit simultaneously nor the duration of the exceedance.

The platform is intended for research, teaching and supporting analysis. Drawing recognition does not replace measurement of real parameters; this release does not model pump shutdown, cavitation, column separation or protective devices. Once the vaporization threshold is crossed, single-phase model results can no longer be treated as valid physical conclusions. Real-world engineering applications require verification of input data and of the model's scope.

## Upgrading to 2.4.5

**Releases are published separately on GitHub and through the in-app update service.** 2.4.5 is available both on this page and through the in-app update service: users with an earlier version installed can upgrade directly via **Settings → Software Update…**, or download the installer from this page and install over the existing version (accounts, licenses and output directories are preserved). The version and status shown in the **Software Update** window are authoritative.

- **In-app update**: first save your scenario and back up projects and results, then open **Settings → Software Update…** and click **Check for updates**; if a new version is available, choose **Download and install updates**. Do not force-quit the program or shut down the computer during the update.
- **Full client upgrade**: download this release's installer and follow the wizard; alternatively, extract the full ZIP to a new folder and run it from there. Do not copy only some of the new files over the old program directory. Clean up the old installation only after confirming that the new version opens your scenarios and results.
- **Offline transfer**: download the full ZIP on a computer with internet access, verify its checksum, then transfer it to the target computer; the first sign-in on a new device still requires an internet connection.

Existing software accounts continue to work. Reinstalling or re-extracting is not the same as registering again; on a new device, simply sign in online with your existing account. Back up your projects and results before updating, especially any files of your own stored in the old program directory.

## Data, language and feedback

Scenarios, reports and results are stored wherever you choose when saving or exporting; we recommend keeping them together in a dedicated project directory. Network results go to `outputs/network_runs` in the program folder by default, which can be changed on the **Results** page. Account licenses and remembered sign-in details are stored for the current Windows user; copying the program folder does not transfer the local license.

- **Switch language**: choose Simplified Chinese or English via **Settings → Interface Language**; wait for any computation or export to finish before switching.
- **Open logs**: use **Settings → Open log directory** to locate the runtime logs.
- **Change avatar**: click the avatar menu at the top right and choose **Change avatar…** or **Restore default avatar**.
- **AI assistant**: after signing in online, click the robot button at the top right to ask questions; each account has 10 questions per day. Answers are for reference only; verify anything involving engineering judgment yourself.
- **Report problems**: after signing in online, use **Feedback** at the top right, or open a [GitHub Issue](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/issues); replies from the developer can be viewed in **My feedback and replies…** in the account menu.

| Symptom | Resolution |
| --- | --- |
| Registration is still awaiting review | Contact the developer to confirm your request; submitting again does not activate the account automatically |
| Account disabled, not approved or license expired | Contact the developer to verify the status or request a renewal |
| Cannot connect to the account service | Check your network; the first sign-in requires the service to be reachable, and if a valid local offline license exists, the program will try to sign in offline |
| Missing runtime libraries or drawing-recognition component | Download again, verify the checksum and extract completely, keeping the entire program directory |
| Drawing recognition is slow on first launch | Wait for the components to load, follow the stage and status messages, and cancel the task if necessary |
| Image feedback is not currently supported | Remove the images and submit text-only feedback |
| Assistant reports it is unavailable or the daily quota is used up | Make sure you are signed in online; the limit is 10 per day and resets the next day; you can also use **Feedback** directly |
| Joint design or error-based sensor placement takes a long time | Adjust the time limit in **Solver settings**; the computation can be canceled at any time from the bottom progress bar; check the bounds and proof status in the certificate |
| Research interface reports that the planning JSON cannot be read | Re-export it with **Save project** in the GUI, or run `describe` to inspect the fields named in the message; records must not contain extra keys |
| Contact binding request has not taken effect | Wait for administrator approval; the existing binding remains valid until then |
| Export fails or the file is in use | Close the program that is using the target file, check write permission for the target directory and try again |

When reporting a problem, include the software version, Windows version, steps to reproduce, error messages and a minimal example that can be shared publicly. Do not attach passwords, license caches, session information, or drawings and project materials that you are not permitted to disclose to public Issues; check logs for personal and project information before sharing them.

More documentation: [Installation and uninstallation (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/InstallerReadme.txt) · [Client guide (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/ClientReadme.txt) · [Headless research API guide (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/%E6%97%A0%E7%95%8C%E9%9D%A2%E7%A0%94%E7%A9%B6API%E8%AF%B4%E6%98%8E.md) · [2.4.5 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.5%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [2.4.4 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.4%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [2.4.3 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.3%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [2.4.2 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.2%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [2.2.0 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.2.0%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [Longitudinal profile example (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/LongitudinalReadme.txt) · [License](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/LICENSE)
