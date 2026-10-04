---
name: validate-structure
description: Validate project structure - check all notebooks/sections exist, have valid JavaScript Object Notation (JSON), correct structure, and consistent cross-references
disable-model-invocation: true
---

Validate the entire project structure without executing notebooks.

## Checks

1. **File existence**: Verify all 16 notebooks and 9 sections listed in CLAUDE.md exist on disk
2. **Valid JSON**: Parse each `.ipynb` file and confirm valid JSON with `nbformat` >= 4
3. **Cell structure**: For each notebook verify:
   - First cell is markdown with title (`# Computational Notebook`)
   - Has at least one code cell with imports
   - Has a markdown cell containing `## Exercises`
   - Last cell is markdown containing `## Summary`
   - Total cells >= 20
4. **Cross-references**: Scan all notebooks and sections for links to other files:
   - Check that referenced notebook files exist (e.g., `06-market-analysis.ipynb`)
   - Check that referenced section files exist (e.g., `../sections/04-blockchain-economics.md`)
   - Flag any broken links
5. **CLAUDE.md consistency**: Verify the status table in CLAUDE.md matches what's on disk

## Output

Print a summary:
```
Files: 16/16 notebooks, 9/9 sections
JSON valid: 16/16
Structure valid: X/16 (list any failures)
Broken references: X found (list them)
CLAUDE.md: in sync / out of sync
```
