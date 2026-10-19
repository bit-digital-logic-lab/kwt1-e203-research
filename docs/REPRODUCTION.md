# Reproduction scope

The reported checks were run in the project's local Ubuntu environment. The public snapshot intentionally contains no audio archive, training cache, official model checkpoint or weight package. Reproduction therefore requires obtaining each input from its authoritative source under its applicable terms.

The private engineering checkout contains the scripts and per-run logs for these checks. The corresponding public summary names the tested shapes, sample counts, output metrics and proof limits so readers can distinguish software reference results from RTL subset results and full SoC acceptance.

Key commands used in the private checkout:

```bash
bash graduate/ds_tasks/r2_preprocess/repro.sh
bash graduate/ds_tasks/encoder_gate/run_gate.sh
bash graduate/ds_tasks/r2_pack_format/run.sh
bash graduate/ds_tasks/r2_scale_contract/repro.sh
bash graduate/ds_tasks/r2_full_numeric/repro.sh
```

Some commands read gitignored local files from the engineering checkout. A zero exit from a task's own script is not sufficient by itself: review its recorded phase exit codes, sample partition labels, and the scope in `docs/STATUS.md`.
