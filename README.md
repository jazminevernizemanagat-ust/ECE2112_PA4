# ECE2112_PA4

# Experiment 4: Data Wrangling and Data Visualization

**Name:** MANAGAT, JAZMINE VERNIZE S.  
**Section:** 2ECE-A  

---

## Intended Learning Outcomes

* Filter tabular data using several categorical and numerical conditions.
* Construct focused DataFrames by selecting relevant features.
* Summarize the relationship between categorical features and a numerical variable.
* Communicate a data comparison using clear and correctly labeled plots.

```python
import matplotlib.pyplot as plt
import pandas as pd

board_exam = pd.read_excel(r"C:\Users\Jaz\Downloads\board2.xlsx")
```

---

## Detailed Discussion of Problems and Solutions

### A. VISAYAS COMMUNICATION DATAFRAME

* **Problem Statement:** Create a DataFrame named `VisComm` containing students whose `Hometown` is Visayas and whose `Track` is Communication. Retain only these columns, in the stated order: `Name, Gender, Math, Electronics, Average`
* Display the resulting DataFrame and its number of rows. Both filtering conditions must be applied to
the source dataset before the columns are selected.
* **Implementation:**

```python
board_exam["Average"] = board_exam[
    ["Math", "Electronics", "GEAS", "Communication"]
].mean(axis=1)

vis_comm_filter = (board_exam["Hometown"] == "Visayas") & (
    board_exam["Track"] == "Communication")

target_columns = ["Name", "Gender", "Math", "Electronics", "Average"]
VisComm = board_exam[vis_comm_filter][target_columns]

print("VisComm DataFrame:")
print(VisComm)
print("\nNumber of rows:", len(VisComm))
```

### B. VISAYAS FEMALE DATAFRAME

* **Problem Statement:** Create a second DataFrame named `VisFemale` containing students whose `Hometown` is Visayas and whose `Gender` is Female. Retain only: `Name, Track, GEAS, Electronics, Average`. Display `VisFemale`. Then display only the rows of `VisFemale` whose `Average` is at least 60. Do not overwrite `VisFemale` when performing this second filter.
* **Implementation:**

```python
vis_female_filter = (board_exam["Hometown"] == "Visayas") & (
    board_exam["Gender"] == "Female")

target_cols_b = ["Name", "Track", "GEAS", "Electronics", "Average"]
VisFemale = board_exam[vis_female_filter][target_cols_b]

print("VisFemale DataFrame: ")
print(VisFemale)

print("\n", "Female Students with and Average atleast 60: ")
print(VisFemale[VisFemale["Average"] >= 60])
```

### C. CATEGORY-AVERAGE VISUALIZATION

* **Problem Statement:** Examine how the recorded `Average` differs across the three categorical features `Track`, `Gender`, and `Hometown`.
a. For each feature, compute the mean of `Average` for every category using Pandas.  
b. Display the three summary tables. 
c. Create one figure containing three bar charts: mean `Average` by `Track`, by `Gender`, and by `Hometown`.
d. Below the figure, write three concise statements identifying the category with the highest sample mean for each feature
* **Implementation:**

```python
board_exam["Average"] = board_exam[
    ["Math", "Electronics", "GEAS", "Communication"]
].mean(axis=1)

track_mean = board_exam.groupby("Track")["Average"].mean().reset_index()
gender_mean = board_exam.groupby("Gender")["Average"].mean().reset_index()
hometown_mean = board_exam.groupby("Hometown")["Average"].mean().reset_index()

print("=== Summary Table: Mean Average by Track ===")
print(track_mean.to_string(index=False))

print("\n=== Summary Table: Mean Average by Gender ===")
print(gender_mean.to_string(index=False))

print("\n=== Summary Table: Mean Average by Hometown ===")
print(hometown_mean.to_string(index=False))

fig, axes = plt.subplots(1, 3, figsize=(15, 5))

axes[0].bar(track_mean["Track"], track_mean["Average"], color="skyblue")
axes[0].set_title("Mean Average by Track")
axes[0].set_xlabel("Track")
axes[0].set_ylabel("Mean Average")
axes[0].set_ylim(0, 100)

axes[1].bar(gender_mean["Gender"], gender_mean["Average"], color="salmon")
axes[1].set_title("Mean Average by Gender")
axes[1].set_xlabel("Gender")
axes[1].set_ylabel("Mean Average")
axes[1].set_ylim(0, 100)

axes[2].bar(
    hometown_mean["Hometown"], hometown_mean["Average"], color="lightgreen"
)
axes[2].set_title("Mean Average by Hometown")
axes[2].set_xlabel("Hometown")
axes[2].set_ylabel("Mean Average")
axes[2].set_ylim(0, 100)

plt.tight_layout()
plt.show()
```

