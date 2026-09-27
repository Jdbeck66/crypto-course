---
name: run-notebook
description: Execute a single notebook and show output - quick smoke test without quality audit
disable-model-invocation: true
argument-hint: [notebook-path]
---

Execute a notebook and display results without full quality audit.

## Process

1. Execute the notebook:
   ```
   mamba run -n crypto-course jupyter nbconvert --to notebook --execute $ARGUMENTS[0] --output /tmp/run-output.ipynb --ExecutePreprocessor.timeout=300 2>&1
   ```

2. If successful:
   - Report "PASS" with execution time
   - Show count of cells executed

3. If failed:
   - Report "FAIL"
   - Show the error traceback
   - Identify the failing cell number and its first line of code

## Notes
- This is a quick smoke test — no quality checks, no fixes
- Use `/audit-notebook` for full quality review
- Use `/audit-all` to batch-test all notebooks
