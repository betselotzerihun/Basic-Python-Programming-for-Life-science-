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

<details> <summary>📘 Explanation + Common Mistakes</summary>
What this does

Loads all tools needed for:

data handling (pandas)
visualization (matplotlib, seaborn)
SNP analysis (scikit-learn)
phylogenetic trees (Biopython)
❌ Common mistakes beginners make
Forgetting to run the import cell first
Typing wrong package names (panda instead of pandas)
Missing Biopython installation in notebook kernel
Running notebook cells out of order
💡 Tip

Always run this cell before anything else.

</details>
