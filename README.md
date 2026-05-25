# Basic-Python-Programming-for-Life-science


# Python for Microbiologists - Basic Level

**A gentle, beginner-friendly Python course designed specifically for microbiologists.**

Perfect for researchers working with bacterial growth, antibiotic testing, culture data, CFU counts, and microbial experiments.

**No prior coding experience needed.**

### Why Python for Microbiologists?
- Automate repetitive calculations (CFU/ml, growth rates, OD readings)
- Analyze plate counts, MIC data, and growth curves
- Perform simple statistics on your lab results
- Create publication-ready plots
- Reproducible analysis (important for papers and theses)

**Start → [01_Introduction.md](01_introduction.md)**

# 01 - Welcome to Python for Microbiologists

## Why Learn Python as a Microbiologist?
Microbiology generates lots of numerical data: colony counts, optical density (OD600), zone of inhibition, growth curves, pH readings, antibiotic concentrations, etc.

Python acts like a smart lab assistant that:
- Never makes calculation mistakes
- Can analyze hundreds of samples in seconds
- Helps you make beautiful graphs for papers

**Real-life analogy**: Think of Python as your autoclave for data — it sterilizes errors and standardizes your analysis.

## First Code - Hello Microbiologist!

<details>
<summary>Click to expand - Your first Python code</summary>

```python
# This is a comment. Python ignores lines starting with #
print("Hello, Microbiologist!")
print("Welcome to Python for Microbiology Lab!")
print("Today we will analyze bacterial growth data.")



---

### `02_variables.md`

```markdown
# 02 - Variables and Basic Data Types

Variables are like labeled tubes in your rack — you store important values in them.

## Common Types in Microbiology

```python
# Text (String)
bacteria = "Escherichia coli"
strain = "K-12"
antibiotic = "Ampicillin"

# Whole numbers (Integer)
colony_count = 245
dilution_factor = 10000

# Decimal numbers (Float)
od600 = 0.85          # Optical density reading
growth_rate = 0.62    # per hour
temperature = 37.5

# True/False (Boolean)
is_resistant = True
is_gram_positive = False
