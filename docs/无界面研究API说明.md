# 无界面研究 API 说明（API 1.2，平台 2.4.7）

平台 2.4.5 起提供面向科研的无界面接口：以平台规划 JSON（`hydraulic-irrigation-planning` v1）为唯一输入，在界面之外直接调用平台用于轮灌规划的稳态水力模型，输出带版本与模型摘要的结构化结果。2.4.6 起命令行工具用平台账户登录一次即可使用，授权有效期内（最长 14 天，且不晚于账户使用期限）计算不需要联网。2.4.7 起桌面客户端菜单栏的“API”按钮可在界面里直接调用同一套接口（见第 1 节）；API 1.1 新增与科研三目标同一口径的取样选项、分段违反指标和 `plan_objectives`（第 5 节），API 1.2 新增精确最优轮灌 `optimize-exact`（第 5 节）。

| 能力 | 命令 |
| --- | --- |
| 管网概况与最终生效的模型选项 | `hydraulic-moc api describe` |
| 单组 / 批量稳态评估 | `hydraulic-moc api evaluate` |
| 时间轴仿真：时变水头、故障 / 恢复、重规划钩子 | `hydraulic-moc api simulate` |
| 蒙特卡洛：固定种子场景、多进程 | `hydraulic-moc api monte-carlo` |
| 精确最优轮灌：已证明最优的压力均匀度方案与最优性上界 | `hydraulic-moc api optimize-exact` |
| 轮灌组稳态评估器（树状管网） | `hydraulic-moc irrigation-eval` |
| 登录 / 查看授权 / 退出登录 | `hydraulic-moc login` / `status` / `logout` |

## 1. 获取与运行

从 Release 下载 `HydraulicWorkbenchResearchCLI-2.4.7-Windows-x64.zip`，完整解压后在 PowerShell 中进入该目录，先用已获批的平台账户登录一次（需要审批中心在线）：

```powershell
.\hydraulic-moc.exe --version
.\hydraulic-moc.exe login                 # 输入平台账户名和密码；脚本可用 --password-stdin 从标准输入读密码
.\hydraulic-moc.exe status                # 查看登录账户与授权到期时间
.\hydraulic-moc.exe api describe --project examples\farm_network.json
```

工具自带运行环境，不需要安装 Python，也不依赖桌面客户端；请保留与 exe 同目录的 `auth_distribution.json`。未登录或授权无效时，`api` 与 `irrigation-eval` 计算命令以退出码 3 结束并说明原因；`logout` 删除本机登录记录。随包 `examples/` 含示例管网 `farm_network.json`、轮灌组 `groups.jsonl`、时间轴场景 `scenario.json` 与蒙特卡洛配置 `monte_carlo.json`，`examples/README.md` 给出对应命令（含 `optimize-exact`）。

**客户端入口（2.4.7 起）**：桌面客户端菜单栏“帮助”右侧的“API”按钮打开“科研接口（API）”窗口，不需要另装命令行工具。

- “本机计算”：选择规划 JSON（文件、“监测布设与轮灌”页当前规划或示例管网）和计算类型（第 4 节的五个命令），缺少轮灌组、情景或蒙特卡洛配置时可按规划中的出水口“生成模板…”，计算在状态栏任务中运行。窗口把各字段拼成 `hydraulic-moc api` 的参数，用同一个解析器解析、调用同一个命令函数，输出文件和 manifest 与命令行逐项相同；客户端内计算使用当前登录。
- “模型设置”：第 3 节九个选项的覆盖值（留空不覆盖）、蒙特卡洛进程数、缓存条目和输出根目录，保存后长期生效。
- “命令行与脚本”：与当前选择完全等价的命令行和 Python 代码，可直接放进批处理；“为本机授权…”等同于 `hydraulic-moc login`。

## 2. 输入：规划 JSON

与桌面端“监测布设与轮灌”共用同一种文件：在界面中“保存规划”得到的 JSON 可直接交给接口。单位固定为 m、m³、m³/h、h，记录不能带多余键。

- `topology.nodes` / `topology.pipes`：单水源，树状或环状。每根管自己选择 `headloss_formula`：`"D-W"`（需要 `roughness_m`；或给 `darcy_factor` 固定摩阻系数）或 `"H-W"`（`roughness_hw`）。`minor_loss_coefficient` 是逐管 K 值，`kinematic_viscosity_m2_s` 是逐管运动黏度。
- `fields`：**每条记录是一个出水口**（灌水器、给水栓或田块阀门）。`field_id` 为出水口 ID，`node_id` 为所在节点，`design_flow_m3_h` 为设计流量，`required_volume_m3` 为目标水量（用于水量指标）。
- `settings`：与平台规划器共用的键会被读取：`source_pressure_m`、`min_pressure_m`、`min_velocity_m_s`、`max_velocity_m_s`、`velocity_scope`、`max_source_flow_m3_h`。只属于接口的选项放在嵌套的 `settings.hydraulic_api` 里（键名见第 3 节），桌面端不受影响。

## 3. 模型与选项

需求驱动稳态模型（SI 单位）：

```
出水口 o 开启 → 在其节点注入设计流量 q_o
h_e = (1 + r) · h_f(Q_e) + K_e · v|v| / (2g)
H_n = H_s − Σ 沿程有向损失,   p_n = H_n − z_n（压力水头）
```

树状管网用矩阵乘法精确求解；环状管网从满足质量守恒的生成树流量出发做 Newton（全局梯度法）迭代，收敛判据为节点质量残差 < 1e-10 m³/s、管段能量残差 < 1e-9 m。

| 选项 | 默认值 | 含义 |
| --- | --- | --- |
| `friction` | `"epanet"` | 未指定 `darcy_factor` 的 D-W 管所用摩阻律：`epanet`（EPANET 2.2 全流态，和平台瞬态求解器一致）或 `colebrook`（迭代 Colebrook-White） |
| `local_loss_ratio` | `0.1` | 局部损失按沿程损失的比例 r 计入（总损失 1.1 × 沿程）；逐管 K 另外累加 |
| `min_pressure_m` | `11.4` | 开启出水口所在节点的最低压力水头 |
| `min_velocity_m_s` / `max_velocity_m_s` | `0.3` / `2.5` | 流速约束 |
| `velocity_scope` | `"active_only"` | `active_only` 只检查有流量的管；`all_pipes` 检查所有连通管（平台规划器口径） |
| `max_source_flow_m3_h` | `None` | 组内设计流量之和的上限 |
| `outlet_law` | `"linear_cap"` | 出流上报规则：`linear_cap` = q_d·clip(p/h_d,0,1)；`orifice_cap` = q_d·√clip(p/h_d,0,1)；`design` = q_d |
| `outlet_design_pressure_m` | `11.4` | 出流规则中的 h_d；设为 `null` 时取 `min_pressure_m` |

默认值即轮灌研究口径，研究不需要在 JSON 里重复写这些值；需要别的口径时再覆盖。生效顺序：规划器共用键 → `settings.hydraulic_api` → 命令行参数。`describe` 会打印最终生效的值；全部选项进入 `model_digest`，每份结果都带这个摘要。

**水源水头**：`source_head_m` 一律指水源处的测压管水头（压力水头 + 水源高程，与节点高程同一基准），不是水源压力。省略时取 `settings.source_pressure_m + z_source`。

**单位**：规划 JSON 里的流量是 m³/h，加载时换算成 m³/s。结果中带 `_m3_s` 后缀的为 m³/s；只有 `source_flow_m3_h`、`source_flow_violation_m3_h` 和上限 `max_source_flow_m3_h` 保持 m³/h，与规划器一致。水量为 m³，时间为 h，压力水头和水头损失为 m，流速为 m/s。

## 4. 命令行

公共参数：`--project`，以及与第 3 节一一对应的覆盖项 `--friction --local-loss-ratio --min-pressure --min-velocity --max-velocity --velocity-scope --max-source-flow --outlet-law --outlet-design-pressure --cache-items`。

```powershell
hydraulic-moc api describe    --project net.json
hydraulic-moc api evaluate    --project net.json --groups groups.jsonl --output out.jsonl [--detail]
hydraulic-moc api simulate    --project net.json --scenario scenario.json --output sim\
hydraulic-moc api monte-carlo --project net.json --config mc.json --output mc\ --workers auto
hydraulic-moc api optimize-exact --project net.json --group-size 4 --step-h 4 --time-limit 600 --output exact\ [--source-head 25]
```

| 命令 | 输入 | 输出 |
| --- | --- | --- |
| `evaluate` | JSONL，每行 `{"id", "open": [...], "source_head_m"?, "unavailable"?: [...], "faults"?: [{"target", "id"}]}` | `.jsonl`（`--detail` 时含逐节点、逐管段和逐出水口的值）或 `.csv`（只含标量）；另写 `<名>.manifest.json` |
| `simulate` | `{"schedule": [{"outlets", "duration_h", "label"?}], "head"?, "events"?: [{"time_h", "kind", "target", "id"}], "horizon_h"?, "start_time_h"?, "start_volumes_m3"?: {id: m3}}` | `segments.csv`、`outlets.csv`、`summary.json` |
| `monte-carlo` | 第 6 节的场景规格加上 `"scenarios"` 和 `"schedule"` | `scenarios.csv`、`events.csv`、`manifest.json` |
| `optimize-exact` | 只需 `--project`；`--group-size`、`--step-h`、`--time-limit`（秒）、`--source-head`（省略取项目水头）、`--seed` 对应第 5 节的参数 | `plan.json`（完整结果）、`schedule.csv`（每组一行：`step`、以分号分隔的 `outlets`、`duration_h`、`start_h`）、`manifest.json`（含最优性上界与间隙） |

每份 manifest 都记录：`api_version`、`platform_version`、输入 JSON 的 SHA-256、`network_digest`、`model_digest`、全部生效选项和 UTC 时间。NaN 在 JSON 中写为 `null`，在 CSV 中写为空。

### 稳态评估结果

每组结果主要字段：逐节点压力水头、逐管流量（沿管段起点→终点为正）、流速、水头损失、逐出水口出流、`min_pressure_m` / `mean_pressure_m`（只统计开启出水口所在节点）、各类违反量与违反计数、`converged`、`feasible`。

- **故障**：节点故障会关闭与它相连的所有管段，管段故障关闭该管，出水口故障只停用该出水口。与水源断开的节点压力为空，节点上的出水口不能开启。树状管网中，这就是故障点下游的整棵子树；环状管网会经其他路径重新分配流量。
- **缓存**：按（开启出水口集合，关闭管段集合）精确缓存；水源水头只是最后一步的整体平移，所以缓存不引入任何近似。

## 5. 时间轴仿真

- 时间轴在每个步结束、每个事件和水头曲线的每个折点处切分为 *segment*。每段内开启出水口、关闭管段和水头都不变，供水量 = q_o × 时长。
- `head` 支持常量、逐时阶梯（`regular`）和线性曲线；线性曲线取每段中点的水头，段长不超过 `linear_substep_h`。
- 故障发生在某一步中间时，这一步按剩余时长继续运行，只是被隔离的出水口停止出水；恢复后这些出水口重新出水。
- 指标：`achievement_pct` = Σmin(V,T)/ΣT，`target_met_pct`，`mean_deficit_m3` / `max_deficit_m3`，`pressure_uniformity_pct` = 100·(1 − MAD/|均值|)（开启出水口压力），`feasible_time_pct`，`pressure_violation_segment_pct` / `velocity_violation_segment_pct` / `infeasible_segment_pct`（有出水的段中违反的比例），`constraint_violation` = 三者之和 / 100（没有任何段出水时记 1），`min_pressure_m`，`max_velocity_m_s`。`summary.json` 同时给出受故障影响出水口子集的指标。
- Python 中 `api.plan_objectives(metrics)` 直接返回轮灌研究的三目标（均为最小化）：100 − `achievement_pct`、`mean_deficit_m3`、100 − `pressure_uniformity_pct`（没有压力样本时记 100）；其他目标函数由研究自己用上述指标组合。
- 重规划钩子（在每次事件之后、以及计划执行完毕时调用，可接入 NSGA-II 等任意优化器）通过 Python 接口使用；命令行执行固定计划。

### 研究口径（API 1.1）

`simulate` 有两个只影响取样方式、不影响水力模型的选项，可写在场景 JSON 和蒙特卡洛配置里；不写时与 API 1.0 结果完全相同：

| 选项 | 默认 | 研究口径 | 含义 |
| --- | --- | --- | --- |
| `head_sampling` | `"segment"`：水头曲线每个折点都切段 | `"step_start"` | 每个灌水时段开始时取一次水头并保持到时段结束，事件之后重新取一次 |
| `pressure_weighting` | `"duration"`：均匀度按段时长加权 | `"segment"` | 每段压力样本等权，`pressure_std_m` 用 ddof = 1 |

两个选项都取研究口径、无故障时，三目标和 `constraint_violation` 与科研代码的三目标和约束违反度一致到浮点舍入（100 组水头 × 原方案与随机方案，最大差 3e-14）。两种口径下供水量指标相同，差别只在压力均匀度：逐时切段会让同一方案的均匀度相差约 2 个百分点，跨平台与科研代码比较均匀度时请统一取研究口径。

### 精确最优轮灌（`optimize-exact`，API 1.2）

- 问题：每个出水口只开一次，`group_size` 个一组，每组 `step_h` 小时，水头恒定；每组都满足压力和流速约束；目标是最大化压力均匀度 PUI = 100·(1 − MAD/均值)（每个出水口一个压力样本，等于研究口径下的 `pressure_uniformity_pct`）。各组时长相同，可行方案的供水指标都一样，因此只比均匀度。出水口数须是 `group_size` 的整数倍。
- 方法：先批量评估枚举全部可行组，方案即一个集合划分。对均值落在区间 [a, b] 内的方案，Σ|P − m| ≥ max(2Σ(P − b)⁺, 2Σ(a − P)⁺)；以此建集合划分模型，对均值区间分支定界：宽区间用 LP 松弛定界并二分，区间窄到 0.1 m 后解 MILP（HiGHS），直到间隙小于 0.001 个百分点。所有界都来自已求解的松弛，证明不依赖启发式解。可行方案来自固定种子的随机精确覆盖构造和松弛的整数解，再做组间交换的局部搜索，结果可复现。
- 结果：`status` 为 `optimal`（已证明最优）、`time_limit`（返回当前最好方案和有效上界）或 `infeasible`（已证明不存在由可行组构成的划分）；`pressure_uniformity_pct`、`uniformity_upper_bound_pct` 和 `gap_pct_points` 给出方案、上界与间隙，`gap_pct_points` 即与真正最优之间差距的上限。候选组数超过 `max_candidates`（默认 200 万）时直接报错。
- 规模（4 核，科研网络按支管缩减，水头 25 m）：24 个灌水器约 40 s 证明最优，36 个约 12–18 min 证明最优；48 个（约 8 万个可行组）6 s 内得到 98.90 % 的方案，1 h 时上界 99.01 %，间隙 0.11 个百分点。随包示例 8 个出水口两两一组不到 1 s 证明最优。
- 结果可直接交给 `simulate`（`head_sampling: "step_start"`、`pressure_weighting: "segment"`）复核。它与桌面端“管径与轮灌设计”的全局最优是不同的问题：后者允许一个出水口分到多轮、轮次时长连续，目标是组内压力 MAD。

## 6. 蒙特卡洛

```json
{"horizon_h": 120, "seed": 2026, "scenarios": 500,
 "head": {"kind": "uniform", "low_m": 20, "high_m": 30, "interval_h": 1},
 "faults": {"target": "node", "candidates": "outlet_nodes", "count": [1, 3],
            "time_h": [0, 110], "recovery_probability": 0.75, "repair_h": null},
 "schedule": [{"outlets": ["H1", "H2"], "duration_h": 4.0}]}
```

- 第 i 个场景使用随机种子 `[seed, i]`，因此每个场景单独可复现，与进程数、执行顺序、生成的场景总数都无关。
- `head`：除第 5 节的曲线写法外，还支持 `uniform`（每个时段独立均匀抽样）。`faults.target` 可取 `node` / `pipe` / `outlet`；`candidates` 可取 `outlet_nodes` / `all_nodes` / `outlets` 或显式 ID 列表；`count` 是闭区间 [lo, hi]；`repair_h` 给出修复时长区间，取 `null` 时恢复时刻在（故障时刻, 视界）内均匀抽取。
- 每个场景输出一行，含全网指标、受影响出水口指标和缓存命中数；`--workers auto` 使用全部 CPU 核心。

## 7. 轮灌组稳态评估器（`irrigation-eval`）

面向轮灌调度研究的轻量评估接口：树状管网、D-W 摩阻；输入规划 JSON（或节点/管段表）、灌水器流量表与轮灌组 JSONL，输出每组压力、流速、可行性 CSV，可另出逐节点压力长表。默认口径（EPANET 摩阻、r = 0.1、11.4 m）下与无界面 API 在 200 组随机组合上逐位一致（压力、流量、出流、可行性和违反计数）。

```powershell
hydraulic-moc irrigation-eval --project net.json --emitters emitters.csv --groups groups.jsonl --output groups.csv [--nodes-output nodes.csv]
```

## 8. 适用范围

- 稳态和准稳态（逐段稳态）计算，面向轮灌规划、调度优化与可靠性分析；瞬态（水锤）校核请使用平台的 MOC/FVM 求解器。
- 需求驱动：降压时按 `outlet_law` 折减的出流用于统计供水量。
- 单水源、被动管道组成的树状与环状管网，与桌面端规划器的支持范围一致。

## 9. 稳定性约定

本文档描述的 JSON 键和 CSV 列名构成 API 1.x（1.1 新增 `head_sampling`、`pressure_weighting`、`plan_objectives` 和分段违反指标，默认行为与 1.0 相同；1.2 新增 `optimize-exact`），已有脚本、场景 JSON 和 CSV 列名都不用改动。新增选项或输出字段只会以向后兼容的方式加入；需要改变已有含义时提升 `api_version` 的主版本号。树状 H-W/D-W 结果与平台规划求解器逐位或在 1e-10 m 内一致，环状管网与平台规划器一致到 1e-5 m 且自身残差满足第 3 节判据；多进程与串行结果完全相同；`optimize-exact` 在 12 个出水口的全部 5775 种划分上与穷举结果一致。
