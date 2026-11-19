# LUCI on IBM — circuits and figures

## `stim_circuits/`

Stim circuits for the hardware experiments (dataset 2026-05-15, 10000 shots), grouped by scheme: `Standard/`, `ACID/`, `ACID-variant/`.
These circuits include noise channels (`X_ERROR`, `DEPOLARIZE1`, `DEPOLARIZE2`).

File name:

```
<scheme>_case<c>_dr<d_r>_dc<d_c>_L<L>_basis<X|Z>_shots10000_r<rounds>.stim
```

| Field | Meaning |
|---|---|
| `case` | defect case (0, 1, 2) |
| `dr`, `dc` | code distances (d_r, d_c) |
| `L` | L parameter (Standard = 1, ACID / ACID-variant = 2) |
| `basis` | memory basis (X or Z) |
| `r` | number of syndrome-extraction rounds (Standard: r1–r7, ACID / ACID-variant: r1–r3) |

## `figures/`

| Folder | Contents |
|---|---|
| `main_text/` | Fig. 1–4 of the manuscript |
| `supplementary/` | figures used in the supplementary material (active-qubit layouts, detection-event plots, detector diagrams, logical error rates) |
