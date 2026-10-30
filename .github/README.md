# Hydraulic Workbench Studio · 2.4.6

**Pressurized pipe-network transient hydraulic simulation platform**

A one-dimensional transient hydraulic analysis platform for urban water supply and irrigation conveyance and distribution, supporting the method of characteristics (MOC) and the finite volume method (FVM) for single pipes, branched networks and looped networks. From parameter modeling, drawing recognition and topology review, through transient computation, real longitudinal profiles and pressure checks, valve-closure scenario comparison, rotational irrigation planning and sensor placement, to result reports, the whole analysis is done in a single workbench; the platform's steady-state hydraulic model can also be called directly by research code through a headless research API and the Research CLI.

**2.4.6 brings an all-new Qt workbench: modeling, checks, computation and results are linked in one interface, with menus and keyboard shortcuts for everyday actions; a network from the Network workspace goes straight into irrigation planning; and the AI assistant sees the page you are on, your latest check results and the error that just popped up, so its answers address the problem in front of you.**

This repository provides the **Windows client, Research CLI, release notes and user documentation; the software source code is not published**. Regular users do not need to install Python or set up a development environment.

**[Download the 2.4.6 installer](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.6/HydraulicWorkbenchStudio-2.4.6-Windows-x64-Setup.exe)** · **[Download the full portable ZIP](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.6/HydraulicWorkbenchStudio-2.4.6-Windows-x64.zip)** · **[Download the Research CLI](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.6/HydraulicWorkbenchResearchCLI-2.4.6-Windows-x64.zip)** · **[View this release](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/tag/v2.4.6)**

Chinese documentation and a mirror for users in mainland China: [Gitee](https://gitee.com/Iqx-nwafu/hydraulic-workbench-studio)

Users with 2.4.5 installed can upgrade to 2.4.6 directly from the avatar menu at the top right via **Settings → Software Update…**; for the highlights of this release, see below and the [2.4.6 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.6%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt).

Getting started: download and install → submit a registration request → sign in once approved. **Existing accounts keep working; there is no need to register again.**

## Features

| Capability | What you can do |
| --- | --- |
| Transient analysis of single pipes and networks | Analyze pressure, flow and wave propagation with MOC or FVM; supports branched and looped networks, mechanical valve operations and varying pipe parameters |
| Modeling and computation checks | Set geometry, materials, wave speed, friction, boundaries and initial states; the single-pipe form shows a live preview of pipe geometry, boundary processes and valve curves; the Calculation Check Center checks parameters, grid, time step and resource requirements and takes you to each issue |
| Drawing recognition and topology editing | Recognize PNG/JPG/JPEG/DXF drawings locally and review nodes, connections, sources and scale; the topology canvas supports box-select batch editing, undo/redo and coloring by risk results |
| Real longitudinal profiles and pressure checks | Import or edit pipe centerline elevations; view maximum/minimum HGL envelopes along the line, time histories at monitoring locations, and segments exceeding engineering pressure limits |
| Valve-closure scenario comparison | Compare different valve-operation scenarios, identify segments where exceedances are eliminated, persist or newly appear, and export pressures at the same locations together with valve timing |
| Result analysis and reporting | Browse a linked view of network-map animation, node and pipe result tables, probe time histories, shortest hydraulic-path profiles, events and alerts and longitudinal profiles, with hover readouts and zoom on every chart; save scenarios, result archives, charts and reports |
| Topology deliverables | Add editable text and line annotations and export the complete topology as PNG, JPG or DWG |
| Sensor placement and rotational irrigation | Compute rotational irrigation schedules that satisfy velocity, pressure and water-volume constraints; determine the minimum number of pressure gauges and flow meters, and their locations, for a given error tolerance; optionally co-design pipe diameters and the rotation schedule, with an optimality certificate; take the network straight from the Network workspace and keep it in sync |
| Research interface | Taking planning JSON as input, batch-evaluate irrigation groups, simulate fault and recovery timelines and run Monte Carlo outside the GUI; results carry version information and input hashes and plug directly into research code and optimizers |
| AI assistant | Ask where features are, how to perform tasks, what parameters mean and how to handle errors; answers automatically take the current page, scenario, latest check results and error text into account, and unresolved questions can be turned into user feedback with one click |
| Chinese/English workbench | Four workspaces in the **Project** browser, a **File / Solve / Results / Interface / Help** menu bar and keyboard shortcuts for everyday actions; supports Simplified Chinese and English |

Different solution methods support different physical models and boundary conditions. The program checks whether the current combination is within scope; details are available under **Help → Model Scope**.

## New in 2.4.6

1. **All-new Qt workbench**: the **Project** browser is streamlined to four workspaces (System Overview, Single-Pipe Model, Network, Sensors and irrigation), and the menu bar is **File / Solve / Results / Interface / Help**. The "current scenario" follows the page you are on: `Ctrl+O` opens, `Ctrl+S` saves, `F5` runs, `Esc` cancels, and `Ctrl+1`/`Ctrl+2` jump to the single-pipe model and its results. Every long-running task shows its progress in the status bar and can be canceled.
2. **Live System Overview cards**: analysis domain and scenario name, model check (blocking issues and warnings, with the first issue), whether the results match the current parameters, and the number of passed core validations, so the next step is clear at a glance.
3. **Linked results browser**: network results bring together the network-map animation, node and pipe result tables, probe time histories, the shortest hydraulic-path profile, events and alerts, report files and real longitudinal profile checks, all linked to the selected object. Single-pipe results offer six views (water-hammer verdict and section pressure, section flow, wave-propagation heat map, animation with GIF export, diagnostics along the pipe, profile assessment), and parameter calibration writes the best wave speed and friction back with one click.
4. **From network computation straight to irrigation planning**: click **Send to sensors / irrigation** on the **Topology** toolbar, or **Import from network model** on the planning page, to bring node elevations, coordinates, pipe lengths, diameters, roughness and source head into the plan; nodes with a base demand above zero automatically become outlets. Later edits to the network are synced when you return to the planning page, keeping the water demands and constraints you entered.
5. **An AI assistant that knows the software**: the knowledge base was rewritten page by page for the new interface, with a new task quick reference covering complete paths from single-pipe valve-closure water hammer, modeling from drawings, risk localization and valve-closure comparison to irrigation sensor placement and the Research CLI. Each question automatically includes the current page, the current scenario, the latest Calculation Check Center result and the text of any error or warning that just appeared, so asking right after an error gets you targeted steps.
6. **Self-service password recovery by email**: click **Forgot password?** on the sign-in page, enter your account name and registered email to receive a 6-digit code, then set a new password directly without waiting for approval. A mobile number and email can be bound at registration.
7. **Paste screenshots into feedback**: after taking a screenshot with QQ, WeChat, Snipaste or `Win+Shift+S`, press `Ctrl+V` or click **Paste screenshot** in the feedback window.
8. **Research CLI sign-in**: sign in once with `hydraulic-moc login` using your platform account, then run `api` and `irrigation-eval` computations without opening the GUI and without a network connection while the license is valid; `status` shows when the license expires, and `logout` removes the local sign-in record.

After upgrading, the first sign-in asks you to read and accept the updated **User Notice and Privacy Statement** (a new section on account contact details and password recovery). For full details, see the [2.4.6 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.6%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt).

## Research interface

The steady-state hydraulic model that the platform uses for rotational irrigation planning is available as a programmable interface (since 2.4.5). Research code no longer needs to click through irrigation groups one by one in the GUI, or reimplement the hydraulic computation itself, to obtain results that follow the same conventions as the platform's planner and are reproducible and archivable.

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

The release includes a standalone **Research CLI** (`HydraulicWorkbenchResearchCLI-2.4.6-Windows-x64.zip`) that runs right after unzipping, with no Python environment and no dependency on whether the client is installed. Sign in once with an approved platform account; while the license is valid (up to 14 days, and never beyond the account's validity period), computations do not need a network connection.

```powershell
hydraulic-moc login                                                                 # sign in with your platform account (scripts can use --password-stdin)
hydraulic-moc status                                                                # signed-in account and license expiry
hydraulic-moc api describe    --project net.json                                   # network summary and effective model options
hydraulic-moc api evaluate    --project net.json --groups groups.jsonl --output out.jsonl [--detail]
hydraulic-moc api simulate    --project net.json --scenario scenario.json --output sim/
hydraulic-moc api monte-carlo --project net.json --config mc.json --output mc/ --workers auto
hydraulic-moc irrigation-eval --project net.json ...                                # irrigation-group steady-state evaluator
hydraulic-moc logout                                                                # remove the local sign-in record
```

| Command | Input | Output |
| --- | --- | --- |
| `evaluate` | JSONL, one `{"id", "open": [...], "source_head_m"?, "unavailable"?, "faults"?}` per line | `.jsonl` (with `--detail`, includes per-node, per-pipe and per-outlet values) or `.csv`, plus `<name>.manifest.json` |
| `simulate` | `{"schedule": [...], "head"?, "events"?, "horizon_h"?}` | `segments.csv`, `outlets.csv`, `summary.json` |
| `monte-carlo` | Scenario specification + `"scenarios"` + `"schedule"` | `scenarios.csv`, `events.csv`, `manifest.json` |

All model options can be overridden with command-line arguments: `--friction --local-loss-ratio --min-pressure --min-velocity --max-velocity --velocity-scope --max-source-flow --outlet-law --outlet-design-pressure`. Without a valid sign-in, computation commands exit with code 3 and state the reason, so batch scripts can detect it.

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

## Earlier releases

- **2.4.5**: headless research API, Research CLI and irrigation-group steady-state evaluator; see the [2.4.5 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.5%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt).
- **2.4.4 / 2.4.3**: AI assistant; draggable node and pipe IDs saved with the scenario; batch annotation styling; optional IDs on topology export; undo/redo in topology editing; feedback reply notifications. See the [2.4.4](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.4%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) and [2.4.3](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.3%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) release notes (Chinese).
- **2.4.2**: the **Sensors and irrigation** workspace (rotational irrigation planning, minimum sensor placement, joint diameter and irrigation design, optimality certificates); PNG/JPG/DWG topology export with text annotations; feedback with screenshots; contact binding; computation on all CPU cores. See the [2.4.2 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.2%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt).
- **2.2.0**: real longitudinal profiles with HGL envelopes, engineering pressure limits and exceedance segments, batch valve-closure scenario comparison, `.hnr` v3 result archives. See the [2.2.0 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.2.0%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt).

## Download, installation and launch

On the [Release page](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/tag/v2.4.6), expand **Assets** and choose from the following files:

| File | Purpose |
| --- | --- |
| [Windows x64 installer](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.6/HydraulicWorkbenchStudio-2.4.6-Windows-x64-Setup.exe) | Recommended for first-time users; installs via a wizard and creates desktop and Start menu shortcuts |
| [Windows x64 full portable ZIP](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.6/HydraulicWorkbenchStudio-2.4.6-Windows-x64.zip) | Extract completely to a folder of your choice, then run |
| [Research CLI ZIP](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.6/HydraulicWorkbenchResearchCLI-2.4.6-Windows-x64.zip) | Command-line tool for the headless research API; runs right after unzipping, with no Python environment required; run `login` once before first use |
| [SHA256SUMS.txt](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/releases/download/v2.4.6/SHA256SUMS.txt) | SHA-256 checksums for verifying the installer, the ZIP and the command-line tool |

**The `Source code (zip/tar.gz)` archives generated automatically by GitHub and the green `Code → Download ZIP` button contain only this repository's public files; they are neither the software source code nor a runnable client. Please download the installer or full ZIP from the table above.**

Requires Windows 10/11 (64-bit); reserve at least 2 GB of disk space for installation and future updates. The client ships with its runtime dependencies and a local CPU-based drawing-recognition component, so there is no need to install Python, PyTorch or CUDA separately, and no NVIDIA GPU is required. Actual computational resource needs vary with network size, grid and output settings; review the Calculation Check Center messages before running.

**Installer**: double-click `HydraulicWorkbenchStudio-2.4.6-Windows-x64-Setup.exe` and follow the wizard to install for the current Windows user, then launch from the desktop or Start menu.

**Portable version**: in File Explorer, choose **Extract All** on the ZIP, open the complete extracted folder and run **`HydraulicWorkbenchStudioSecure.exe`**. Choose a directory you have write access to; do not run the program from inside the archive preview window, and do not move the main executable on its own, because the configuration, updater, drawing-recognition component and runtime libraries must all be kept.

**Research CLI**: after extracting `HydraulicWorkbenchResearchCLI-2.4.6-Windows-x64.zip`, open that directory in PowerShell, run `.\hydraulic-moc.exe login` and enter your platform account name and password, then run `.\hydraulic-moc.exe api describe --project <planning JSON>`; the bundled `examples/` folder contains example networks, irrigation groups, timeline scenarios and a Monte Carlo configuration. Keep `auth_distribution.json` next to the exe.

The program is not currently signed with a Windows Authenticode code signature, so Windows may display "Unknown publisher". Verify that you downloaded it from this repository and check the SHA-256; if a file's origin is unknown or its checksum does not match, download it again instead of turning off security protection.

To verify, open PowerShell in the folder containing the download, compute the file's checksum and compare it with this release's `SHA256SUMS.txt`:

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath '.\HydraulicWorkbenchStudio-2.4.6-Windows-x64-Setup.exe'
```

## Registration and sign-in

First-time registration and first sign-in require an internet connection and an available account service. The software account is independent of your GitHub account.

1. In the sign-in window, click **No account? Register** and enter an account name, a password and the password confirmation.
2. Account names are 3–32 characters and may contain Chinese characters, letters, digits, underscores, periods, hyphens or `@`, but no spaces; passwords are 6–20 characters, must not be a common password rejected by the program, and must not contain the full account name. You are told right away if the account name is already taken.
3. Enter a mobile number or email address (at least one; **an email is recommended**, as it enables self-service password recovery); it is approved together with the registration request.
4. Read the **User Notice and Privacy Statement**, tick the consent box and submit. The message **Awaiting administrator approval** means your request has been submitted; you can tell the developer the account name you applied for through an existing contact channel, but **do not send your password**.
5. Once approved, enter your account name and password and click **Secure Sign-In**. Existing accounts sign in directly; there is no need to register again after a version update.

**Allow recognition corrections as local model samples** is off by default and does not affect registration, sign-in or computation; ticking it does not trigger automatic upload of drawings or automatic training, and you can manage the consent or delete local samples later under **Help → About**.

After a successful online sign-in, the program stores a local offline license for the current Windows user. For later sign-ins, still click **Secure Sign-In**; if the service cannot be reached, the program tries to use a valid local offline license. Offline use is subject to the license validity period; after switching computers or Windows users, or deleting the license cache, you need to sign in online again.

**Forgot your password?** Click **Forgot password?** on the sign-in page, enter your account name and the email bound at registration, receive a 6-digit code (valid for 10 minutes), and set a new password once it is verified. After a reset, the account's other sessions and any locally remembered old password stop working.

Registration, password recovery, password changes, account-name changes, sending feedback and license renewal require an internet connection. Password and account-name changes are under **Change Password…** in the avatar menu at the top right and take effect after administrator approval; **Remember Username** and **Remember Password** are optional, and remembering the password is not recommended on shared computers.

## Your first computation

We recommend running the bundled examples first to get familiar with **build the model → check parameters → run → analyze results** before entering real project data.

1. After signing in, choose **File → Open case…** and select the bundled `configs/longitudinal_network_test.json`. The program recognizes it as a network scenario and opens **Network / Topology**.
2. On **Network / Scenario**, click **Profile and pressure limits…** to view the example: pipe `P` is 100 m long, with pipe centerline elevations of 0, 20 and 0 m at stations 0, 35 and 100 m; the default pressure-head limits are 12–36 m.
3. Go to **Solver settings**, keep the default MOC and click **Open Compute Check Center**. Review blocking issues and warnings; if there are blocking issues, use **locate and repair** to fix them first.
4. Click **Run the MOC network simulation**, or press `F5` on a Network page. The computation runs in the background with progress in the status bar; click **Cancel** or press `Esc` to stop it.
5. When it finishes, the **Results** page opens automatically. On the **Profile assessment** tab, view the pipe centerline, the maximum/minimum HGL envelopes and the segments exceeding limits; click the high point at 35 m to view its pressure time history, or click **Locate worst monitor**.
6. To change the output location, click **Change…** next to **Output directory** on the **Results** page. Use **File → Save Network Run** to save a scenario JSON that can be edited again.

Single-pipe example: open `configs/longitudinal_single_pipe.json` the same way with **File → Open case…**; the program opens the single-pipe model. Click **Computing Check Center**, then **Start MOC calculation** (or press `F5`); the results open automatically when it finishes. For the full steps of CSV import, FVM comparison and scenario comparison, see the [longitudinal profile example (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/LongitudinalReadme.txt).

The examples are synthetic dynamic scenarios. HGL is the hydraulic grade line (piezometric head), and pressure head is `HGL − pipe centerline elevation`. Envelopes record extrema over the internal computation steps, so an extremum may occur between two saved frames.

### Rotational irrigation and sensor placement example

1. In the **Project** browser on the left, click **Sensors and irrigation**.
2. Click **Load network ▾** and select a bundled example (e.g. **All-pipe flow example**), or click **Import from network model** to use the network open in the Network workspace. In **Inputs** and **Constraints & errors** on the toolbar, verify the head-end pressure, supply cap, minimum pressure, upper and lower velocity limits, and field water demands.
3. Click **1 Plan irrigation** to view the units opened in each round, durations, supply volumes and pressure statistics; you can proceed to the next step only after all hard constraints pass and a certificate has been obtained.
4. Verify the instrument error, estimation tolerance and validation domain, then click **2 Optimize sensors**; view the required instrument locations and per-round error bounds on the **Sensors** and **Round accuracy** tabs. In the diagram, circles are pressure gauges and triangles are flow meters.
5. To design pipe diameters at the same time, enable diameter design on the **Joint diameter and irrigation design** tab, fill in the **Diameter / cost catalogue**, choose a solution method in **Solver settings**, click **Generate and search designs**, and then **Select and freeze** the chosen design.
6. Use **Save project** to save inputs that can be reopened, and **Export results** to save the schedule, sensor placement, error checks and optimality certificate. A saved planning JSON can be used directly as input to the research interface.

Rotational irrigation results correspond to the network, fields, constraints and error settings you entered; the minimum sensor count applies only to the frozen design and the declared error domain. The sample values in the cost catalog are relative indices; for actual design, enter real price quotes.

### Research interface example

1. Extract the Research CLI, open its directory in PowerShell and run `.\hydraulic-moc.exe login` to sign in with your platform account.
2. Run `hydraulic-moc api describe --project examples\farm_network.json` to view the network summary and the effective model options.
3. Run `hydraulic-moc api evaluate --project examples\farm_network.json --groups examples\groups.jsonl --output out.jsonl --detail` to get the pressure, flow, velocity and feasibility of each irrigation group, plus `out.manifest.json`.
4. Run `hydraulic-moc api simulate --project examples\farm_network.json --scenario examples\scenario.json --output sim\` to get timeline results including faults and recovery (`segments.csv`, `outlets.csv`, `summary.json`).
5. Run `hydraulic-moc api monte-carlo --project examples\farm_network.json --config examples\monte_carlo.json --output mc\ --workers auto` to get reproducible multi-scenario statistics with a fixed seed.

Replace `examples\farm_network.json` with a file saved via **Save project** in the GUI to run the same computations on your own network.

## Using your own project data

| Step | Key actions |
| --- | --- |
| Modeling | Choose single pipe or network; enter geometry, materials, wave speed, friction, initial conditions, boundaries and valve operations |
| Drawing import (optional) | Recognize a drawing via **File → Import Network Drawing (PNG/JPG/JPEG/DXF)…**, review connections, sources and scale in the **Recognized Topology Review** window, then fill in the hydraulic parameters on **Network / Topology** |
| Profiles and limits (optional) | Click **Profile and pressure limits…** (on the single-pipe page, or on **Network / Scenario**), then import a CSV or edit the table; make sure pipe centerline elevations and boundary heads use the same elevation datum |
| Check and run | Choose the applicable method (MOC or FVM), verify grid, time step, output interval and resource requirements in the Calculation Check Center, resolve blocking issues one by one, then press `F5` |
| Results and comparison | Browse the linked network map animation, node and pipe results, probe time histories, path profiles, events and alerts and longitudinal profiles; open **Results → Valve scenario comparison…** for segment differences |
| Save and deliver | Save the scenario JSON, results and reports; network `.hnr` archives can be reopened with **Load Result Archive…** on the **Results** page; use the topology toolbar to add text and line annotations and export the topology |

The longitudinal profile CSV header is `pipe_id,station_m,centerline_elevation_m`. The single-pipe ID is always `single`; networks use the actual pipe IDs. Templates are in the bundled `data/input/` folder.

After changing a scenario, profile or pressure limits, run the computation again. Not setting pressure limits does not mean the check has passed. The exceedance segment length is the length of the set of locations that exceeded the limit at some point within the statistics window; it is neither the length exceeding the limit simultaneously nor the duration of the exceedance.

The platform is intended for research, teaching and supporting analysis. Drawing recognition does not replace measurement of real parameters; this release does not model pump shutdown, cavitation, column separation or protective devices. Once the vaporization threshold is crossed, single-phase model results can no longer be treated as valid physical conclusions. Real-world engineering applications require verification of input data and of the model's scope.

## Upgrading to 2.4.6

**Releases are published separately on GitHub and through the in-app update service.** 2.4.6 is available both on this page and through the in-app update service: users with an earlier version installed can upgrade in the app, or download the installer from this page and install over the existing version (accounts, licenses and output directories are preserved). The version and status shown in the **Software Update** window are authoritative.

- **In-app update**: first save your scenario and back up projects and results. In 2.4.5 and earlier, click the avatar at the top right and choose **Settings → Software Update…**; from 2.4.6 on, choose **Software Update…** directly in the avatar menu. Click **Check for updates**; if a new version is available, choose **Download and install updates**. Do not force-quit the program or shut down the computer during the update.
- **Full client upgrade**: download this release's installer and follow the wizard; alternatively, extract the full ZIP to a new folder and run it from there. Do not copy only some of the new files over the old program directory. Clean up the old installation only after confirming that the new version opens your scenarios and results.
- **Offline transfer**: download the installer or full ZIP on a computer with internet access, verify its checksum, then transfer it to the target computer; the first sign-in on a new device still requires an internet connection.
- **Research CLI**: download the 2.4.6 Research CLI, extract it and run `login` once; your existing scripts keep working.

Existing software accounts continue to work; the first sign-in after upgrading asks you to accept the updated User Notice and Privacy Statement. Reinstalling or re-extracting is not the same as registering again; on a new device, simply sign in online with your existing account. Back up your projects and results before updating, especially any files of your own stored in the old program directory.

## Data, language and feedback

Scenarios, reports and results are stored wherever you choose when saving or exporting; we recommend keeping them together in a dedicated project directory. Network results go to `outputs/network_runs` in the program folder by default, which can be changed on the **Results** page. Account licenses and remembered sign-in details are stored for the current Windows user; copying the program folder does not transfer the local license.

- **Switch language**: on the **Settings** page (gear icon in the top bar, or **⚙ Settings** at the bottom of the **Project** browser), choose Simplified Chinese or English under **Interface Language**; the interface reopens and you stay signed in.
- **Open logs**: click **▤ Logs** at the bottom of the **Project** browser, or **Open log directory** on the **Settings** page.
- **Change avatar**: click the avatar menu at the top right and choose **Change avatar…** or **Restore default avatar**.
- **AI assistant**: after signing in online, click the robot icon in the top bar; tick **Include current page and case** to get answers based on your current situation (scenario files, drawings and result data are not sent). Each account has 10 questions per day. Answers are for reference only; verify anything involving engineering judgment yourself.
- **Report problems**: after signing in online, click the ✉ icon in the top bar (screenshots can be pasted with `Ctrl+V`), or open a [GitHub Issue](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/issues); replies from the developer can be viewed in **My feedback and replies…** in the avatar menu.

| Symptom | Resolution |
| --- | --- |
| Registration is still awaiting review | Contact the developer to confirm your request; submitting again does not activate the account automatically |
| Account disabled, not approved or license expired | Contact the developer to verify the status or request a renewal |
| Cannot connect to the account service | Check your network; the first sign-in requires the service to be reachable, and if a valid local offline license exists, the program will try to sign in offline |
| Forgot password | Use **Forgot password?** on the sign-in page to receive a code at your registered email; if no email is bound, contact the developer |
| Password recovery code not received | Check your spam folder; you can request a new code after 60 seconds, up to 5 times per account per day |
| Asked to accept the notice again after upgrading | The User Notice and Privacy Statement has been updated; read it and tick the consent box; existing licenses and data are unaffected |
| Missing runtime libraries or drawing-recognition component | Download again, verify the checksum and extract completely, keeping the entire program directory; if the recognition component reports a problem, click **Recheck Component** in **Component details** |
| Drawing recognition is slow on first launch | Wait for the components to load, follow the stage and status messages, and cancel the task if necessary |
| Assistant reports it is unavailable or the daily quota is used up | Make sure you are signed in online; the limit is 10 per day and resets the next day; you can also send feedback directly |
| Joint design or error-based sensor placement takes a long time | Adjust the time limit in **Solver settings**; the computation can be canceled at any time from the status bar; check the bounds and proof status in the certificate |
| The CLI says you are not signed in, or the license has expired | Run `hydraulic-moc login` again; `hydraulic-moc status` shows when the license expires |
| Research interface reports that the planning JSON cannot be read | Re-export it with **Save project** in the GUI, or run `describe` to inspect the fields named in the message; records must not contain extra keys |
| Contact binding request has not taken effect | Wait for administrator approval; the existing binding remains valid until then |
| Export fails or the file is in use | Close the program that is using the target file, check write permission for the target directory and try again |

When reporting a problem, include the software version, Windows version, steps to reproduce, error messages and a minimal example that can be shared publicly. Do not attach passwords, license caches, session information, or drawings and project materials that you are not permitted to disclose to public Issues; check logs for personal and project information before sharing them.

More documentation: [2.4.6 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.6%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [Installation and uninstallation (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/InstallerReadme.txt) · [Client guide (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/ClientReadme.txt) · [Headless research API guide (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/%E6%97%A0%E7%95%8C%E9%9D%A2%E7%A0%94%E7%A9%B6API%E8%AF%B4%E6%98%8E.md) · [2.4.5 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.5%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [2.4.4 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.4%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [2.4.3 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.3%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [2.4.2 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.4.2%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [2.2.0 release notes (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/2.2.0%E6%9B%B4%E6%96%B0%E8%AF%B4%E6%98%8E.txt) · [Longitudinal profile example (Chinese)](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/docs/LongitudinalReadme.txt) · [License](https://github.com/Iqx-nwafu/hydraulic-workbench-studio/blob/main/LICENSE)
