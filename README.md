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

# 📦 STEP 1 — Install and Import Libraries

```python
pip install pandas matplotlib seaborn biopython scikit-learn
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## 📘 What this does

This command installs all required Python packages for your analysis:

- pandas → data handling and tables (DataFrames)  
- matplotlib → basic plotting and graphs  
- seaborn → advanced statistical visualization  
- biopython → phylogenetic tree handling (Bio.Phylo)  
- scikit-learn → machine learning tools + SNP distance calculations  

---

## ❌ Common mistakes beginners make

- Forgetting to activate the correct environment  
- Installing packages in the wrong Python version  
- Missing `!` when running in Jupyter (`!pip install ...`)  
- Not restarting kernel after installation  

---

</details>

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

# METADATA + BASIC VISUALIZATION

---

## 📂 1. Load Your Metadata (Real Dataset)
Before creating plots, data must be properly imported, inspected, and sanitized. Hand-collected clinical datasets often contain hidden issues like trailing spaces or incorrect data types.

```
import pandas as pd

# 1. Load the data into Python
df = pd.read_csv("metadata.csv")

# 2. Show the first few lines to make sure it looks correct
df.head()
```

<details>

<summary>📂 pd.read_csv — What this does</summary>

## 📘 What this does

- `pd.read_csv()` reads your spreadsheet (`metadata.csv`) and turns it into a table in Python called a DataFrame (`df`).  
- `df.head()` displays the top 5 rows of your table to quickly preview the data.  

---

## ❌ Common mistakes beginners make

- **Missing File Error:** Running the code without uploading `metadata.csv` into the Jupyter workspace first.  
- **Wrong spelling:** Typing the filename incorrectly (e.g., `Metadata.csv` instead of `metadata.csv`, or forgetting `.csv`).  

---

## 💡 Tip

Always use `df.head()` as your first step to confirm that Python successfully opened your file.

</details>


## 1.2. Check Table Size and Columns
```python
# 1. See how many rows and columns you have (Rows, Columns)
print(df.shape)

# 2. See a simple list of all your column titles
print(df.columns)
```
<details>

<summary>📊 Dataset Structure — What this does</summary>

## 📘 What this does

- `df.shape` acts like a tape measure. It tells you exactly how many patients (rows) and characteristics (columns) are in your file.  
- `df.columns` prints out a clean list of every column header so you know exactly how they are spelled.  

---

## ❌ Common mistakes beginners make

- Adding parentheses: writing `df.shape()` instead of `df.shape`.  
  - `shape` is a property, not a function, so it does not need `()`.

---

</details>

## 1.3. Remove Hidden Blank Spaces
```python
# 1. Clean the column names (remove hidden spaces at the start/end)
df.columns = df.columns.str.strip()

# 2. Clean the 'Sex' column entries specifically
df["Sex"] = df["Sex"].str.strip()

# 3. Clean the 'CultureResult' column entries specifically
df["CultureResult"] = df["CultureResult"].str.strip()
```

<details>

<summary>🧹 Cleaning Text — What this does</summary>

## 📘 What this does

Human typists often accidentally leave blank spaces in spreadsheets (for example, typing `"Male "` instead of `"Male"` or naming a column `"Sex "` with an invisible trailing space).

`.str.strip()` acts like an eraser that automatically removes invisible spaces from the edges of your text.

---

## ❌ Common mistakes beginners make

- The **KeyError**: Trying to access a column using `df["Sex"]` and getting an error because the spreadsheet actually has it named `"Sex "` with a trailing space.

---

## 💡 Tip

If Python says a column doesn't exist, always check your raw spreadsheet for hidden spaces!

</details>

## 1.4. Convert Age Into Numbers

```python
# 1. Force the 'Age' column to be read as math numbers
df["Age"] = pd.to_numeric(df["Age"], errors="coerce")

# 2. Check the column's average to prove it is numeric now
print(df["Age"].mean())
```
<details>

<summary>🔢 Converting to Numbers — What this does</summary>

## 📘 What this does

Sometimes numbers get imported as "text" instead of real numeric values. This happens if a column contains words like `"Unknown"` or has typing errors.

- `pd.to_numeric()` converts the column into proper numeric values.  
- `errors="coerce"` tells Python: if a value cannot be converted into a number, turn it into `NaN` (empty value) instead of crashing.

---

## ❌ Common mistakes beginners make

- Trying to calculate averages or build plots while the column is still stored as text instead of numbers.  

---

</details>

## 1.5. List Unique Categories
```python
# 1. See all unique categories inside the 'Sex' column
print(df["Sex"].unique())

# 2. See all unique categories inside the 'CultureResult' column
print(df["CultureResult"].unique())
```

<details>

<summary>🔍 Finding Unique Values — What this does</summary>

## 📘 What this does

`.unique()` shows every single unique value present in a column without repeating duplicates.

It is very useful for checking data quality and spotting inconsistencies.

For example, it can reveal if a dataset contains both `"Positive"` and `"positive"` as separate categories.

---

## ❌ Common mistakes beginners make

- Skipping this step and later discovering duplicated categories in plots caused by simple typos or inconsistent labeling.  

---

</details>

---

## 📋 2. Check Dataset Structure

```python
df.shape
df.columns
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Shows number of patients (rows)  
- Shows number of features (columns)  

---

## ❌ Common mistakes beginners make

- Ignoring column names before plotting  
- Assuming column names are standardized  

---

## 💡 Tip

Always inspect columns first in real clinical datasets.

</details>

# 📊 3. AGE DISTRIBUTION (HISTOGRAM)

```python
import matplotlib.pyplot as plt
import seaborn as sns

sns.histplot(df["Age"], bins=20)

plt.title("Age Distribution of TB Patients")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Shows distribution of patient ages  
- Helps understand affected population  

---

## ❌ Common mistakes beginners make

- Age column stored as string instead of numeric  
- Missing values in Age column  
- Too many bins making plot noisy  

---

## 💡 Tip

Convert if needed:

```python
df["Age"] = pd.to_numeric(df["Age"], errors="coerce")
```
</details> 
---

# 📊 4. SEX DISTRIBUTION (BAR CHART)

```python
sns.countplot(data=df, x="Sex")

plt.title("Gender Distribution")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Counts male vs female patients  
- Shows simple distribution  

---

## ❌ Common mistakes beginners make

- Column written as `sex` vs `Sex`  
- Missing values or inconsistent labels (M, Male, male)  

---

## 💡 Tip

Standardize values first:

```python
df["Sex"] = df["Sex"].str.lower()
```
</details> 

---

# 📊 5. REGION DISTRIBUTION

```python
df["region"].value_counts().plot.bar()

plt.title("TB Cases by Region")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Shows number of TB cases per region  

---

## ❌ Common mistakes beginners make

- Spelling inconsistencies in region names  
- Too many categories cluttering plot  

---

## 💡 Tip

Clean spelling before plotting.

</details>

---

# 🥧 6. CULTURE RESULT (PIE CHART)

```python
df["CultureResult"].value_counts().plot.pie(
    autopct="%1.1f%%"
)

plt.title("TB Culture Results")

plt.ylabel("")

plt.show()
```
<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Shows proportion of positive vs negative TB cases  

---

## ❌ Common mistakes beginners make

- Missing `.value_counts()`  
- Too many categories in pie chart  
- Inconsistent labels (Positive vs positive)  

---

## 💡 Tip

Normalize text first:

```python
df["CultureResult"] = df["CultureResult"].str.lower()
```
</details>
---

---

# 🌍 7. REGION vs CULTURE RESULT (HEATMAP)

```python
cross = pd.crosstab(df["region"], df["CultureResult"])

sns.heatmap(cross, annot=True, cmap="Reds")

plt.title("Region vs TB Outcome")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Compares TB positivity across regions  
- Shows patterns in disease distribution  

---

## ❌ Common mistakes beginners make

- Wrong column names  
- Missing values breaking crosstab  
- Too many regions making heatmap crowded  

---

## 💡 Tip

Keep categories limited for teaching.

</details>

---

# 📊 8. SMOKING vs CULTURE RESULT

```python
sns.countplot(data=df, x="Smoking", hue="CultureResult")

plt.title("Smoking vs TB Outcome")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Compares smoking status with TB infection outcome  

---

## ❌ Common mistakes beginners make

- Yes/No not standardized  
- Missing values in categorical columns  

---

## 💡 Tip

Clean values:

```python
df["Smoking"] = df["Smoking"].str.lower()
```
</details>

---

# 9. AGE vs CULTURE RESULT (LINE GRAPH STYLE)

```python
df.groupby("Age")["CultureResult"].value_counts().unstack().plot()

plt.title("Age vs TB Outcome")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Shows how TB positivity varies by age  

---

## ❌ Common mistakes beginners make

- Age treated as categorical instead of numeric  
- No sorting of age values  

---

## 💡 Tip

Sort age if graph looks messy.

</details>

---

# 🌍 10. ORIGIN (ComeFrom) ANALYSIS

```python
df["ComeFrom"].value_counts().plot.bar()

plt.title("Patient Origin Distribution")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Shows where patients came from (migration/travel patterns)  

---

## ❌ Common mistakes beginners make

- Different spelling of same location (Saudi Arabia vs SaudiArabia)  

---

## 💡 Tip

Standardize location names first.

</details>
---


---
