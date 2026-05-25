# 🧬 Python for Visualization
## 2-Day Beginner Notebook Training (Focused Edition)

---

# 📌 What This Training Focuses On

We focus on:

- 📂 Metadata exploration
- 📊 Visualization (bar chart, pie chart, heatmap, line graph, tables)
- 🧬 SNP analysis
- 🌳 Phylogenetic tree visualization

All work is done in Jupyter Notebooks only.

---

# 📦 STEP 1 — Import Libraries

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

from Bio import Phylo
from sklearn.metrics import pairwise_distances
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

Loads all tools needed for:

- data handling (`pandas`)
- visualization (`matplotlib`, `seaborn`)
- SNP analysis (`scikit-learn`)
- phylogenetic trees (`Biopython`)

---

## ❌ Common mistakes beginners make

- Forgetting to run the import cell first
- Typing wrong package names (`panda` instead of `pandas`)
- Missing Biopython installation in notebook kernel
- Running notebook cells out of order

---

## 💡 Tip

Always run this cell before anything else.

</details>

---

# 📅 DAY 1 — METADATA + BASIC VISUALIZATION

---

# 📂 1. Load Metadata Table

```python
df = pd.read_csv("metadata.csv")

df.head()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Loads TB isolate metadata
- Reads CSV file into a DataFrame
- Displays first rows of dataset

---

## ❌ Common mistakes beginners make

- File not located in notebook directory
- Wrong filename spelling
- Forgetting quotation marks around filename
- Trying to load Excel file using `read_csv()`

---

## 💡 Tip

Use `df.head()` to preview data quickly.

</details>

---

# 📋 2. View Table Summary

```python
df.shape

df.columns
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Shows dataset dimensions
- Displays column names

---

## ❌ Common mistakes beginners make

- Thinking `.columns()` is a function
- Skipping dataset inspection before plotting
- Ignoring missing values

---

## 💡 Tip

Always inspect dataset structure first.

</details>

---

# 📊 3. Bar Chart — Lineage Distribution

```python
sns.countplot(data=df, x="lineage")

plt.title("TB Lineage Distribution")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Counts isolates per lineage
- Creates bar chart visualization

---

## ❌ Common mistakes beginners make

- Wrong column names
- Forgetting `plt.show()`
- Missing lineage values

---

## 💡 Tip

Check column names using:

```python
df.columns
```

</details>

---

# 🥧 4. Pie Chart — Lineage Proportion

```python
df["lineage"].value_counts().plot.pie(
    autopct="%1.1f%%"
)

plt.title("Lineage Proportion")

plt.ylabel("")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Converts lineage counts into percentages
- Displays lineage proportion using pie chart

---

## ❌ Common mistakes beginners make

- Forgetting `.value_counts()`
- Using too many categories
- Forgetting `plt.ylabel("")`

---

## 💡 Tip

Pie charts work best with few categories.

</details>

---

# 📈 5. Line Graph — Coverage Trend

```python
cov = pd.read_csv("coverage.csv")

plt.plot(cov["coverage_depth"])

plt.title("Sequencing Coverage Trend")

plt.xlabel("Samples")

plt.ylabel("Depth")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Plots sequencing depth across samples
- Shows sequencing quality trend

---

## ❌ Common mistakes beginners make

- Wrong column name
- Non-numeric values
- Assuming data is sorted

---

## 💡 Tip

Inspect coverage data before plotting.

</details>

---

# 📋 6. CSV Table Preview

```python
df.head(10)
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Displays first 10 rows of dataset

---

## ❌ Common mistakes beginners make

- Using `print(df)` for large tables
- Displaying full dataset accidentally

---

## 💡 Tip

Use `.head()` for quick previews.

</details>

---

# 📅 DAY 2 — SNP ANALYSIS + PHYLOGENY

---

# 🧬 7. Load SNP Matrix

```python
snp = pd.read_csv(
    "snps.csv",
    index_col=0
)

snp.head()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Loads SNP matrix
- Uses isolate IDs as row index

---

## ❌ Common mistakes beginners make

- Forgetting `index_col=0`
- SNP values not numeric
- Missing values in SNP matrix

---

## 💡 Tip

SNP matrices should usually contain only:
- 0 = absence
- 1 = presence

</details>

---

# 🧮 8. SNP Distance Calculation

```python
distance = pairwise_distances(
    snp,
    metric="hamming"
)
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Computes genetic distance between isolates
- Uses Hamming distance metric

---

## ❌ Common mistakes beginners make

- Missing values (`NaN`)
- Non-numeric SNP matrix
- Wrong distance metric

---

## 💡 Tip

Clean SNP data before analysis.

</details>

---

# 🌡️ 9. SNP Heatmap

```python
sns.heatmap(distance)

plt.title("SNP Distance Matrix")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Visualizes genetic similarity between isolates
- Displays SNP distance patterns

---

## ❌ Common mistakes beginners make

- Large datasets slowing notebooks
- Missing labels
- Overcrowded heatmaps

---

## 💡 Tip

Use smaller datasets during beginner training.

</details>

---

# 🌳 10. Phylogenetic Tree Visualization

```python
tree = Phylo.read(
    "tree.nwk",
    "newick"
)

Phylo.draw(tree)
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Reads phylogenetic tree
- Displays evolutionary relationships between TB isolates

---

## ❌ Common mistakes beginners make

- Wrong tree format
- Broken `.nwk` file
- Missing Biopython installation

---

## 💡 Tip

Validate tree file before visualization.

</details>

---

# 📊 FINAL SUMMARY — WHAT YOU LEARNED

After this training, you can:

---

## 📂 Metadata Exploration

- Load TB datasets
- Explore metadata tables
- Inspect isolate information

---

## 📊 Visualization

- Bar charts
- Pie charts
- Line graphs
- Heatmaps
- Table previews

---

## 🧬 SNP Analysis

- Load SNP matrices
- Compute genetic distances
- Visualize SNP similarity

---

## 🌳 Phylogenetic Trees

- Read Newick trees
- Visualize evolutionary relationships

---

# 📌 FINAL NOTE

This training is designed to be:

- ✔ beginner-friendly
- ✔ notebook-based
- ✔ visual
- ✔ practical
- ✔ TB genomics focused
- ✔ easy to reproduce

---
