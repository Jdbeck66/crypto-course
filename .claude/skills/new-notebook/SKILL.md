---
name: new-notebook
description: Create a new Jupyter notebook following project educational standards
disable-model-invocation: true
argument-hint: [notebook-number] [topic-name]
---

Create a new Jupyter notebook at `notebooks/$ARGUMENTS[0]-$ARGUMENTS[1].ipynb`.

## Required Structure

1. **Title cell (markdown):** `# Computational Notebook NN: Topic Name`
   - Overview paragraph
   - Prerequisites list
   - Learning Objectives (numbered)
   - Estimated Time: 4-6 hours
   - Links to relevant resources

2. **Setup cell (code):** Import all libraries with try/except fallback installs

3. **Content cells:** Alternate between markdown explanation and code
   - Each major topic gets a markdown header cell explaining the concept
   - Code cells follow with implementation and demonstrations
   - Include output that shows results
   - Use concrete examples with real-world data where possible

4. **Exercises section (markdown + code):** 3-5 exercises with:
   - Clear problem statement
   - Hints
   - Starter code with `# YOUR CODE HERE` placeholder

5. **Summary cell (markdown):**
   - What You Learned checklist
   - Next Steps with links to the next notebook and relevant sections

## Standards
- All code must run in the `crypto-course` conda environment
- Use type hints in function signatures
- Include docstrings for all functions
- Print clear, formatted output with labels
- Use the existing notebook 01 as the quality reference
- Target ~40-60 code/markdown cells total
- Cross-reference the relevant section(s) from `sections/` directory
