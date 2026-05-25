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

<summary>📦 pip install — What this does</summary>

## 📘 What this does

This command installs all required Python libraries for data analysis and visualization:

- pandas → data handling and tables (DataFrames)  
- matplotlib → basic plotting and graphing  
- seaborn → advanced statistical visualization  
- biopython → biological data tools (including phylogenetic trees)  
- scikit-learn → machine learning tools and SNP distance calculations  

---

## ❌ Common mistakes beginners make

- Missing `!` when running inside Jupyter (`!pip install ...`)  
- Not restarting the kernel after installation  

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

<summary>📂 Loading CSV — What this does</summary>

## 📘 What this does

- `import pandas as pd` → imports the Pandas library for handling tabular data  
- `pd.read_csv("metadata.csv")` → reads the CSV file and loads it into a DataFrame called `df`  
- `df.head()` → displays the first 5 rows of the dataset to quickly check if the file loaded correctly  

---

## ❌ Common mistakes beginners make

- File not found error because `metadata.csv` is not in the working directory  
- Typing the wrong filename or missing `.csv` extension  
- Using `read_csv()` for Excel files instead of `read_excel()`  
- Forgetting to assign the data to a variable like `df`  

---

## 💡 Tip

Always run:

```python
df.head()
```

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


# 📊 3. AGE DISTRIBUTION 

## 3.1. Draw the Base Histogram
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Draw just the plain bars
sns.histplot(df["Age"])

# Always put this at the very end to show the plot
plt.show()
```
<details>

<summary>📊 Histogram Code — Line-by-Line Explanation</summary>

## 📘 Line-by-line explanation

### `import matplotlib.pyplot as plt`

This imports Matplotlib’s plotting module, which is responsible for displaying figures and graphs in Python.

We shorten it to `plt` so we can easily call plotting functions.

---

### `import seaborn as sns`

This imports Seaborn, a high-level visualization library built on top of Matplotlib.

We shorten it to `sns` so we can quickly call visualization functions like histograms, bar plots, and heatmaps.

---

### `sns.histplot(df["Age"])`

This creates a histogram of the `Age` column from your dataset.

- It groups ages into bins (age ranges)  
- Then counts how many patients fall into each bin  
- Helps you understand the distribution of ages  

---

### `plt.show()`

This command displays the plot on the screen.

Without it, the graph may not appear in some environments (especially scripts or some Jupyter setups).

---

## ❌ Common mistakes beginners make

- Forgetting to import `seaborn` or `matplotlib`  
- Using a column name that does not exist (e.g., `"age"` vs `"Age"`)  
- Not checking if `Age` contains text instead of numbers  
- Forgetting `plt.show()` so the plot does not display  

---

</details>

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
