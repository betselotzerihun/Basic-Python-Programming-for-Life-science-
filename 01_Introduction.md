# 01 - Introduction to Python for Microbiologists

**Welcome to Python for Microbiologists!**  
This course is specially designed for researchers and students working in microbiology labs. No prior programming experience is required.

---

## Why Should Microbiologists Learn Python?

Microbiology labs generate large amounts of data every day, including:

- Colony Forming Units (CFU) counts from plate assays  
- Optical Density (OD600) readings from growth experiments  
- Zone of inhibition diameters in antibiotic disk diffusion tests  
- Bacterial growth curves  
- Minimum Inhibitory Concentration (MIC) results  
- pH, temperature, and cell viability counts  

### Benefits of learning Python:

- Fast and accurate calculations  
- Easy analysis of replicate experiments  
- High-quality publication-ready graphs  
- Reproducible research workflows  

Python acts like a smart lab assistant that reduces manual workload.

---

## How to Run Python (Beginner Friendly)

### Google Colab (Recommended)

1. Open https://colab.research.google.com  
2. Click **New Notebook**  
3. Start coding immediately  

---

# Your First Python Codes

---

## Code 1: Hello Microbiologist

```python
print("Hello, Microbiologist!")

<details> <summary><strong>Explanation</strong></summary>

print() is a built-in Python function used to display output.
Parentheses () contain what you want to display.
Quotation marks " " indicate text (string data type).
Python executes code line by line from top to bottom.

Common Error Example
print
(
Hello
, 
Microbiologist
!)

This causes a NameError because Python treats Hello as a variable. Always use quotes for text.

</details>
Code 2: Simulating a Lab Status Log
print
(
"--- LAB PROTOCOL INITIATED ---"
)


print
(
"Incubator Temp: 37°C"
)


print
(
"Shaker Speed: 200 RPM"
)


print
(
"Status: Stable"
)
<details> <summary><strong>Explanation</strong></summary>

Python prints text in exact written order.
Symbols like ---, :, and ° are valid inside strings.
Useful for lab reports and instrument logs.

</details>
Code 3: Python as a Lab Calculator
print
(
"Total Colonies across 3 Replicates:"
)


print
(
45
 
+
 
52
 
+
 
48
)



print
(
"Average CFU per plate:"
)


print
((
45
 
+
 
52
 
+
 
48
) 
/
 
3
)
<details> <summary><strong>Explanation</strong></summary>

Numbers are not written in quotes when calculating.
Operators: +, -, *, /
Python follows PEMDAS/BODMAS rules.
Parentheses control order of calculation.

</details>
Code 4: Comments in Python (Lab Notes)
# Starting antibiotic disk diffusion experiment


print
(
"Setting up Zone of Inhibition measurements..."
)



# This line is disabled and will not run


# print("Hidden step")



print
(
"Measurement complete."
)
<details> <summary><strong>Explanation</strong></summary>
creates comments ignored by Python

Used for lab documentation and notes
Helps explain experiments like a lab notebook
Can disable code temporarily for debugging

</details>
Code 5: Tracking Incubation Progress
print
(
"Incubation Progress:"
)


print
(
"Hour 0: Baseline OD600 = 0.05"
)


print
(
"Hour 4: Mid-log OD600 = 0.45"
)


print
(
"Hour 8: Stationary OD600 = 1.20"
)
<details> <summary><strong>Explanation</strong></summary>

Each line represents an experimental timepoint
Useful for tracking bacterial growth over time
Later modules will introduce variables for automation

</details>
Module 1 Practice Exercises
1. Customize the Log

Modify Code 2:

Add organism name (e.g., Escherichia coli)
Add incubation conditions
2. Do Your Own Lab Math

Compute the average of 3 colony counts from your experiment.

3. Break and Fix (Debugging Practice)
Remove a quotation mark from a print() statement
Run code → observe SyntaxError
Fix it and rerun successfully
print("Welcome to Python for Microbiology Lab!")

