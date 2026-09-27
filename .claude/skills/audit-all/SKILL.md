---
name: audit-all
description: Execute all notebooks in sequence and report pass/fail status for each
disable-model-invocation: true
---

Batch-audit all notebooks in the `notebooks/` directory.

## Process

1. Get the list of all `.ipynb` files in `notebooks/`, sorted by number
2. For each notebook, execute it:
   ```
   mamba run -n crypto-course jupyter nbconvert --to notebook --execute "notebooks/$FILE" --output /tmp/audit-$(basename $FILE) --ExecutePreprocessor.timeout=300
   ```
3. Track results: pass or fail with error summary
4. After all notebooks run, print a summary table:
   ```
   | # | Notebook | Status | Error (if any) |
   ```
5. Report total: X/16 passed

## Notes
- Use `--ExecutePreprocessor.timeout=300` (5 min per notebook) to prevent hangs
- If a notebook fails, capture the error but continue to the next one
- Do NOT fix failures automatically — just report them
- This is a read-only diagnostic skill
