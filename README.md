# 🧬 Python for Visualization

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

- `df.columns.str.strip()` → removes hidden spaces from all column names (e.g., `"Sex "` → `"Sex"`)  
- `df["Sex"].str.strip()` → removes extra spaces from values in the `Sex` column (e.g., `"Male "` → `"Male"`)  
- `df["CultureResult"].str.strip()` → removes extra spaces from values in the `CultureResult` column  

---

This helps ensure that Python treats values consistently without errors caused by invisible spaces.


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
💡 Tip

Always verify conversion using:
```python
df["Age"].dtype
```

before doing statistical analysis.
</details>

## 1.5. List Unique Categories
```python
# 1. See all unique categories inside the 'Sex' column
print(df["Sex"].unique())

# 2. See all unique categories inside the 'CultureResult' column
print(df["CultureResult"].unique())
```

<details>

<summary>🔍 Unique Values Check — Data Consistency</summary>

## 📘 What this does

- `df["Sex"].unique()` → shows all distinct values in the `Sex` column (e.g., Male, Female, etc.)  
- `df["CultureResult"].unique()` → shows all distinct values in the `CultureResult` column (e.g., Positive, Negative, etc.)  

This helps you detect inconsistencies like:

- `"Male"` vs `"male"`  
- `"Positive"` vs `"positive"`  

---

## ❌ Common mistakes beginners make

- Skipping this check and later getting duplicate categories in plots  
- Assuming data is clean without verifying spelling and formatting  
- Not noticing case differences (upper vs lower case)  

---

## 💡 Tip

Always run `.unique()` before visualization to ensure your categories are clean and consistent.

</details>

---


# 📊 3. AGE DISTRIBUTION 
## Approach A: The Quick & Simple "Pandas-Only" Way
### 3.1. The Built-in Pandas Histogram Shortcut
```python
# Generate a quick histogram using ONLY pandas
df["Age"].plot(kind="hist", bins=10, color="orange", edgecolor="black")

plt.title("Quick Age Distribution Chart")
plt.show()
```
<details>

<summary>📊 Pandas Histogram — What this does</summary>

## 📘 What this does

- `df["Age"].plot(kind="hist")` → creates a histogram directly using Pandas without Seaborn  
- `bins=10` → splits the data into 10 age ranges  
- `color="orange"` → changes the bar color to orange  
- `edgecolor="black"` → adds black borders to bars for better visibility  
- `plt.title()` → adds a title to the chart  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting that `Age` must be numeric before plotting  
- Confusing Pandas plotting with Seaborn plotting  
- Missing `plt.show()` so nothing appears  
- Not adding labels or title, making the chart unclear  

---

## 💡 Tip

Pandas plotting is useful for quick and simple visualizations, but Seaborn is better for publication-quality plots.

</details>

## Approach B: The Step-by-Step Breakdown matplotlib and seaborn
### 3.1. Draw the Base Histogram
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

### 3.2. Adding Titles and Axis Labels
```python
# 1. Draw the histogram bars
sns.histplot(df["Age"])

# 2. Add titles and labels so humans can read it
plt.title("Age Distribution of TB Patients")
plt.xlabel("Age of Patients (Years)")
plt.ylabel("Number of Patients")

plt.show()
```

<details>

<summary>📊 Age Histogram — What this does</summary>

## 📘 What this does

- `sns.histplot(df["Age"])` → draws a histogram showing how patient ages are distributed  
- `plt.title()` → adds a clear title to the graph  
- `plt.xlabel()` → labels the X-axis (Age in years)  
- `plt.ylabel()` → labels the Y-axis (number of patients)  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting to check if `Age` is numeric before plotting  
- Missing `plt.show()` so the graph does not appear  
- Using wrong column names (`age` vs `Age`)  
- Forgetting labels, making graphs hard to understand  

---

## 💡 Tip

Always label your plots clearly so others can understand your results instantly.

</details>

### 🎨 3.3. Customizing Bins, Colors, and Trend Lines
```python
# Make the chart larger, change color, and add a smooth trend line (kde)
sns.histplot(df["Age"], bins=10, color="teal", kde=True)

plt.title("Age Distribution of TB Patients")
plt.xlabel("Age (Years)")
plt.ylabel("Patient Count")

plt.show()
```

<details>

<summary>📊 Enhanced Age Histogram — What this does</summary>

## 📘 What this does

- `sns.histplot(df["Age"], bins=10, color="teal", kde=True)`  
  → Creates a histogram of patient ages  
  → `bins=10` groups ages into 10 ranges  
  → `color="teal"` changes the bar color  
  → `kde=True` adds a smooth curve showing the overall distribution trend  

- `plt.title()` → adds a title to the plot  
- `plt.xlabel()` → labels the X-axis (Age in years)  
- `plt.ylabel()` → labels the Y-axis (number of patients)  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting to convert `Age` to numeric before plotting  
- Using too many bins, making the plot noisy  
- Forgetting `kde=True` effect is optional and may confuse beginners  
- Missing labels, making interpretation difficult  

---

## 💡 Tip

Use `kde=True` when you want to see the **overall trend shape** of your data, not just raw counts.

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

## 📊 5.1. Basic Count Plot with Label Rotation
```python
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Create the vertical bar chart
sns.countplot(data=df, x="region", palette="pastel")

# 2. Rotate the region names so they don't overlap!
plt.xticks(rotation=45)

# 3. Add titles
plt.title("Number of TB Cases by Region")
plt.xlabel("Region of Origin")
plt.ylabel("Patient Count")
plt.show()
```

<details>

<summary>📊 Region Bar Chart — What this does</summary>

## 📘 What this does

- `sns.countplot(data=df, x="region", palette="pastel")` → creates a bar chart that counts how many patients come from each region  
- `plt.xticks(rotation=45)` → rotates region labels so they don’t overlap and become unreadable  
- `plt.title()` → adds a clear title to the chart  
- `plt.xlabel()` → labels the X-axis (region names)  
- `plt.ylabel()` → labels the Y-axis (number of patients)  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Misspelled column name (`region` vs `Region`)  
- Overlapping x-axis labels (forgetting `rotation=45`)  
- Using palette without understanding color grouping  
- Forgetting `plt.show()` so the plot does not display  

---

## 💡 Tip

Use `countplot()` when you want to quickly compare category counts across groups.

</details>

## 5.2. Sorting the Bars from Highest to Lowest
```python
# 1. Find the order of regions from most frequent to least frequent
highest_to_lowest = df["region"].value_counts().index

# 2. Tell Seaborn to plot the bars in that exact order
sns.countplot(data=df, x="region", order=highest_to_lowest, palette="muted")

plt.xticks(rotation=45)
plt.title("Ordered TB Cases by Region")

plt.show()
```
<details>

<summary>📊 Ordered Region Bar Chart — What this does</summary>

## 📘 What this does

- `df["region"].value_counts().index` → finds regions sorted from most common to least common  
- `order=highest_to_lowest` → forces Seaborn to display bars in that sorted order  
- `sns.countplot(...)` → creates a bar chart showing number of TB cases per region  
- `plt.xticks(rotation=45)` → rotates region names so they don’t overlap  
- `plt.title()` → adds a clear title to the plot  
- `plt.show()` → displays the final graph  

---

## ❌ Common mistakes beginners make

- Forgetting to define `order` so bars appear in random order  
- Misunderstanding `.value_counts().index`  
- Misspelled column name (`region` vs `Region`)  
- Overlapping x-axis labels without rotation  

---

## 💡 Tip

Always sort categorical data when order matters — it makes patterns much easier to see.

</details>



## 🗺️ 5.3. The Horizontal Bar Chart (Best for Long Names)
```python
# Switch x and y! Put region on the y-axis to lay the bars flat
sns.countplot(data=df, y="region", palette="Set3")

plt.title("TB Cases by Region (Horizontal View)")
plt.xlabel("Number of Patients")
plt.ylabel("Region")

plt.show()
```

<details>

<summary>📊 Horizontal Region Bar Chart — What this does</summary>

## 📘 What this does

- `sns.countplot(data=df, y="region")` → creates a horizontal bar chart showing counts of TB cases per region  
- `palette="Set3"` → applies a color palette to make bars visually distinct  
- `plt.title()` → adds a title to the chart  
- `plt.xlabel()` → labels the X-axis (number of patients)  
- `plt.ylabel()` → labels the Y-axis (region names)  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Confusing `x` and `y` in Seaborn countplot  
- Missing column name consistency (`region` vs `Region`)  
- Forgetting axis labels after switching orientation  
- Using too many categories, making the chart long  

---

## 💡 Tip

Horizontal bar charts are very useful when category names are long or many.

</details>

## 📈 5.4.  The Simplified "Pandas-Only" Way Sorted Pandas Bar Chart Shortcut
```python
# 1. Count the regions (Pandas automatically sorts them highest to lowest!)
region_counts = df["region"].value_counts()

# 2. Plot as a bar chart with custom colors and clean black edges
region_counts.plot(kind="bar", color="cornflowerblue", edgecolor="black")

plt.title("TB Cases by Region")
plt.xlabel("Region")
plt.ylabel("Count")
plt.xticks(rotation=45)

plt.show()
```
<details>

<summary>📊 Region Bar Chart (Pandas Version) — What this does</summary>

## 📘 What this does

- `df["region"].value_counts()` → counts how many TB cases exist in each region and sorts them from highest to lowest  
- `region_counts.plot(kind="bar")` → creates a bar chart using Pandas plotting  
- `color="cornflowerblue"` → sets bar color for better visual clarity  
- `edgecolor="black"` → adds borders around bars for better separation  
- `plt.title()` → adds a chart title  
- `plt.xlabel()` → labels the X-axis (region names)  
- `plt.ylabel()` → labels the Y-axis (number of cases)  
- `plt.xticks(rotation=45)` → rotates region names to prevent overlap  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting that `value_counts()` already sorts data automatically  
- Confusing Pandas plotting with Seaborn plotting  
- Misspelling column names (`region` vs `Region`)  
- Forgetting `plt.show()` so the chart does not appear  
- Overlapping labels when not rotating x-axis text  

---

## 💡 Tip

Pandas plotting is faster for quick analysis, while Seaborn is better for polished visualizations.

</details>

## 5.5 If you want both the Count AND the Percentage: Count (Percentage%)
```python
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Create the chart
ax = sns.countplot(data=df, x="region", palette="pastel")
total = len(df)

# 2. Add percentages on top of each bar (compact look)
for p in ax.patches:
    if p.get_height() > 0:
        ax.annotate(f'{100 * p.get_height() / total:.1f}%', 
                    (p.get_x() + p.get_width() / 2, p.get_height()), 
                    ha='center', va='bottom', xytext=(0, 4), textcoords='offset points')

# 3. Titles and clean layout
plt.xticks(rotation=45)
plt.title("Number of TB Cases by Region")
plt.xlabel("Region of Origin")
plt.ylabel("Patient Count")
plt.show()
```
<details>

<summary>📊 Region Bar Chart with Percentages — What this does</summary>

## 📘 What this does

- `sns.countplot(...)` → creates a bar chart showing number of TB cases per region  
- `total = len(df)` → calculates total number of patients in the dataset  
- `ax.patches` → loops through each bar in the chart  
- `ax.annotate(...)` → writes percentage values on top of each bar  
- `100 * p.get_height() / total` → converts counts into percentages  
- `plt.xticks(rotation=45)` → rotates region labels to avoid overlap  
- `plt.title()` → adds chart title  
- `plt.xlabel()` → labels X-axis (region names)  
- `plt.ylabel()` → labels Y-axis (patient count)  
- `plt.show()` → displays final plot  

---

## ❌ Common mistakes beginners make

- Forgetting to calculate `total = len(df)` before percentage calculation  
- Using wrong column name (`region` vs `Region`)  
- Not understanding `ax.patches` loop structure  
- Overlapping labels when not rotating x-axis  
- Incorrect percentage formula or integer division mistakes  

---

## 💡 Tip

Adding percentages makes bar charts more informative and publication-ready, especially for epidemiology data.

</details>

---

# 🥧 6. CULTURE RESULT (PIE CHART)
## Option A: The Enhanced Pie Chart (Custom Colors & Edge Borders)
This option upgrades the standard pie chart by adding custom colors and distinct borders between slices, which makes it much easier to read.

```python
import matplotlib.pyplot as plt

# 1. Count the values
culture_counts = df["CultureResult"].value_counts()

# 2. Draw the pie chart (Fixed by placing edgecolor inside wedgeprops)
culture_counts.plot(
    kind="pie", 
    autopct="%1.1f%%", 
    colors=["tomato", "lightskyblue"], 
    wedgeprops=dict(edgecolor="white", linewidth=2), # Fixed here!
    startangle=140
)

# 3. Add title and clear out the default vertical label
plt.title("TB Culture Results Breakdown", pad=20)
plt.ylabel("")

plt.show()
```
<details>

<summary>🥧 Pie Chart — What this does</summary>

## 📘 What this does

- `df["CultureResult"].value_counts()` → counts how many positive and negative TB results exist and sorts them automatically  
- `culture_counts.plot(kind="pie")` → creates a pie chart from the counted values  
- `autopct="%1.1f%%"` → shows percentage values on each slice  
- `colors=["tomato", "lightskyblue"]` → sets custom colors for the slices  
- `wedgeprops=dict(edgecolor="white", linewidth=2)` → adds white borders between slices for clarity  
- `startangle=140` → rotates the pie chart for better visual orientation  
- `plt.title()` → adds a title to the chart  
- `plt.ylabel("")` → removes the default y-axis label (which is unnecessary in pie charts)  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting that `value_counts()` automatically sorts values  
- Not setting `autopct`, making percentages invisible  
- Overcrowding pie charts with too many categories  
- Forgetting to remove the default y-label (`plt.ylabel("")`)  
- Using unclear or similar colors for slices  

---

## 💡 Tip

Pie charts work best when you have **only a few categories (2–5 max)** and want to show proportions clearly.

</details>

## 🍩 Option B: The Modern Donut Chart (Highly Recommended)
A donut chart is simply a pie chart with a hollow center. It looks highly modern and professional, and it is incredibly easy to create in a single line using wedgeprops.
```python
import matplotlib.pyplot as plt

# 1. Count the values
culture_counts = df["CultureResult"].value_counts()

# 2. Use 'wedgeprops' to hollow out the center and turn it into a donut
culture_counts.plot(
    kind="pie", 
    autopct="%1.1f%%", 
    colors=["mediumseagreen", "gold"],
    wedgeprops=dict(width=0.4, edgecolor="white", linewidth=2),
    startangle=90
)

plt.title("TB Culture Results (Donut View)")
plt.ylabel("")

plt.show()
```
<details>

<summary>🍩 Donut Chart — What this does</summary>

## 📘 What this does

- `df["CultureResult"].value_counts()` → counts how many TB positive and negative results exist in the dataset  
- `culture_counts.plot(kind="pie")` → creates a pie chart from the counts  
- `autopct="%1.1f%%"` → displays percentage values on each slice  
- `colors=["mediumseagreen", "gold"]` → assigns custom colors to each category  
- `wedgeprops=dict(width=0.4, edgecolor="white", linewidth=2)` → transforms the pie chart into a donut chart by creating a hole in the center  
- `startangle=90` → rotates the chart for better visual alignment  
- `plt.title()` → adds a title to the chart  
- `plt.ylabel("")` → removes the default y-axis label  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting that `width` in `wedgeprops` controls donut hole size  
- Using too many categories (donut charts become unclear)  
- Not setting `startangle`, making slices look misaligned  
- Forgetting to remove y-label (`plt.ylabel("")`)  
- Using poor color contrast, making slices hard to distinguish  

---

## 💡 Tip

Donut charts are a cleaner version of pie charts and are better for presentations when you want a modern visual style.

</details>


## 🗂️ Option C: Pie Chart with a Side Legend (Best for Long Text Labels)
When category names are long, printing them directly on top of the pie slices makes a huge mess. This approach hides the text labels on the slices and places them into a clean side legend box instead.

```python
import matplotlib.pyplot as plt

# 1. Count the values
culture_counts = df["CultureResult"].value_counts()

# 2. Draw the pie chart but turn labels OFF on the slices
culture_counts.plot(
    kind="pie", 
    labels=None, 
    autopct="%1.1f%%", 
    colors=["coral", "silver"],
    startangle=90
)

# 3. Add a separate, clean legend box on the side
plt.legend(
    labels=culture_counts.index, 
    loc="center left", 
    bbox_to_anchor=(1, 0.5)
)

plt.title("TB Culture Results Summary")
plt.ylabel("")

plt.show()
```
<details>

<summary>📊 Pie Chart with Legend — What this does</summary>

## 📘 What this does

- `df["CultureResult"].value_counts()` → counts how many TB positive and negative results exist in the dataset  
- `culture_counts.plot(kind="pie")` → creates a pie chart from the counts  
- `labels=None` → removes text labels directly from the pie slices for a cleaner look  
- `autopct="%1.1f%%"` → shows percentage values on each slice  
- `colors=["coral", "silver"]` → assigns custom colors to the slices  
- `startangle=90` → rotates the chart for better visual alignment  
- `plt.legend(...)` → adds a separate legend box showing category names  
  - `labels=culture_counts.index` → uses category names for the legend  
  - `loc="center left"` → positions legend on the left side  
  - `bbox_to_anchor=(1, 0.5)` → moves legend outside the chart area  
- `plt.title()` → adds a title to the chart  
- `plt.ylabel("")` → removes the default y-axis label  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting to set `labels=None` and getting cluttered slices  
- Misplacing legend outside the figure (bad `bbox_to_anchor` values)  
- Using mismatched number of colors vs categories  
- Forgetting `plt.ylabel("")` leading to unnecessary labels  
- Overcrowding pie charts with many categories  

---

## 💡 Tip

Using a legend instead of slice labels makes pie charts **much cleaner and publication-ready**, especially when category names are long.

</details>
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
