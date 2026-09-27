---
name: audit-notebook
description: Execute a Jupyter notebook and verify it runs without errors
disable-model-invocation: true
argument-hint: [notebook-path]
---

Audit the notebook at $ARGUMENTS[0]:

1. Execute the notebook:
   ```
   conda run -n crypto-course jupyter nbconvert --to notebook --execute $ARGUMENTS[0] --output /tmp/audit-output.ipynb
   ```

2. If execution fails:
   - Read the error output
   - Identify the failing cell
   - Read the notebook to understand context
   - Fix the issue
   - Re-run to verify

3. Check quality:
   - Has title cell with overview, prerequisites, learning objectives, estimated time?
   - Alternates between markdown explanation and code cells?
   - Has exercises section?
   - Has summary/next steps?
   - All functions have docstrings?
   - Cross-references relevant section files?

4. Report: pass/fail, any issues found, any fixes applied
