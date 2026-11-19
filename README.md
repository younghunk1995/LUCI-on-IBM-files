# LUCI on IBM — circuits and figures

Circuits, data and figures for the experiments on `ibm_miami` (15 May 2026, $10^4$ shots per circuit).

## Naming

Files follow the notation of the paper:

```
<scheme>_basis<X|Z>_dX<d_X>_dZ<d_Z>[_config<1|2|3>][_R<R>]
```

| Field | Meaning |
|---|---|
| scheme | `surface_code` (standard surface code), `LUCI_baseline`, `LUCI_variant` |
| `basis` | basis in which the logical state is prepared and measured (X or Z) |
| `dX`, `dZ` | code distances $d_X$ and $d_Z$ |
| `config` | first, second and third distance-3 qubit configuration; the grown patches ($d_X=3, d_Z=5$ in the X basis and $d_X=5, d_Z=3$ in the Z basis) use a single configuration and have no `config` tag |
| `R` | number of rounds, counting the final measurement as one round: $R\in\{1,\dots,7\}$ for the surface code, $R\in\{3,5,7\}$ for LUCI |

Each scheme has 8 configurations: in each basis, three distance-3 configurations and one grown patch.

| Basis | Configurations |
|---|---|
| X | `dX3_dZ3_config1`, `dX3_dZ3_config2`, `dX3_dZ3_config3`, `dX3_dZ5` |
| Z | `dX3_dZ3_config1`, `dX3_dZ3_config2`, `dX3_dZ3_config3`, `dX5_dZ3` |

## `stim_circuits/`

Stim circuits executed on hardware, one file per configuration and number of rounds, grouped by scheme. The circuits include the noise channels used in our simulations and decoding, set from the `ibm_miami` calibration data of 15 May 2026: `DEPOLARIZE2` for CZ gates, `DEPOLARIZE1` for single-qubit gates and for qubits idling in a layer (identity-gate error), and `X_ERROR` for measurement and initialization.

The idle noise in these files is corrected with respect to the circuits recorded at submission (`noisy_stim` in the `_job.json` files), where the idle error of each qubit was taken from the wrong physical qubit. The two versions differ only in the `DEPOLARIZE1` values on idle qubits.

## `experiment_data/`

Hardware data, grouped by scheme, with two JSON files per configuration:

- `<name>_measurements.json`: measurement results retrieved from IBM Quantum for the job
- `<name>_job.json`: job record saved when the circuits were submitted

### `<name>_measurements.json`

Job metadata (`backend`, `job_id`, `timestamp`, `shots`, `physical_qubits`) and a `results` list with one entry per number of rounds:

| Field | Meaning |
|---|---|
| `R` | number of rounds |
| `run_time_s` | circuit duration in seconds |
| `stim_circuit` | path of the corresponding Stim circuit in this repository |
| `counts` | measurement records and how often each occurred |
| `qasm_circuit`, `qasm_transpiled_circuit` | OpenQASM 3 circuit before and after transpilation to the device |

In each `counts` key, character *k* is Stim measurement *k* of the corresponding circuit.

### `<name>_job.json`

The contents are unchanged from submission and use our internal labels: `Standard` = surface code, `ACID` = LUCI baseline, `ACID-variant` = LUCI variant, and `case0`–`case2` = `config1`–`config3`.

| Field | Meaning |
|---|---|
| `backend`, `job_id`, `timestamp` | IBM backend, job ID and submission time |
| `counts_samples` | number of shots |
| `Experiment_info`, `distances` | scheme and code distances |
| `rounds_list`, `run_time` | circuits in the job and the duration (s) of each; for LUCI, entries 1, 2, 3 correspond to $R = 3, 5, 7$ |
| `qubits` | physical qubits used |
| `qiskit_circ`, `transpile_qiskit_circ` | OpenQASM 3 circuits before and after transpilation, one per number of rounds |
| `noisy_stim` | Stim circuits with noise as recorded at submission, one per number of rounds (identical to the files in `stim_circuits/` except for the corrected idle noise, see above) |
| `bestbits` | as recorded at submission |

`qubit_layout_X.json` and `qubit_layout_Z.json` list the physical qubits of each patch, keyed as `<scheme>_<d_X>_<d_Z>_case<c>` in the internal labels above.

## `figures/`

| Folder | Contents |
|---|---|
| `main_text/` | Fig. 1–4 of the manuscript |
| `supplementary/` | figures of the supplementary material |

Supplementary figures use the naming above with a suffix for the figure type. The detection-probability figures of the distance-3 patches combine the three qubit configurations and therefore have no `config` tag.

| Suffix | Contents |
|---|---|
| `_qubit_configuration` | physical qubits of the patch on `ibm_miami` with two-qubit gate and measurement error rates |
| `_detection_probability` | detection probabilities of individual stabilizers over rounds for $R = 7$, measured on hardware and simulated with the circuits in `stim_circuits/` (orange) |
| `_detection_probability_heatmap` | heatmap of the measured detection probabilities over rounds for $R = 7$ |

`logical_error_rate_basis<X|Z>_BPOSD.pdf` shows the experimental logical error rates decoded with BP-OSD, and `logical_error_rate_basis<X|Z>_BPOSD_simulation.pdf` the corresponding circuit-level simulations ($10^4$ samples per circuit).
