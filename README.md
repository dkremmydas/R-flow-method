# The R Flow Method  
*A Text-Based Workflow Approach to R Scripting*  

The **R Flow Method** is a structured way of writing R scripts that combines the flexibility of R with the clarity of workflow tools like **KNIME**.  
Instead of dragging and dropping nodes, each script is written as a **sequence of text-based blocks**, where each block represents a task and produces one clear deliverable.  

This repository documents the method, provides examples, and offers templates to help you adopt it in your own projects.  

---

## ✨ Core Principles  

1. **Task-Based Blocks**  
   - Organize your code into blocks, each performing one well-defined task.  
   - Use RStudio section markers for clarity:  
     ```r
     ## Task Name ----
     ```

2. **Capitalized Output Variables**  
   - Each block produces one main deliverable stored in an ALL CAPS variable.  
   - Example:  
     ```r
     FDALL = read_from_gdx("FDALL", file)
     ```

3. **Temporary Variables**  
   - Use temporary objects (`tmp1`, `tmp2`, …) inside blocks.  
   - These are overwritten later, keeping memory usage clean.  

4. **Deliverables Beyond Objects**  
   - A block can end with:  
     - a capitalized R object,  
     - a table (`knitr::kable`),  
     - a plot (`ggplot2` into `PLOT.list`), or  
     - an external file (Excel, GDX, CSV).  

5. **Configuration and Metadata**  
   - Store global parameters in a `CONFIG` list.  
   - Always include a `METADATA` block at the end with date, time, script name, and config values.  

---

## 📂 Typical Script Structure  

1. **Header** – Purpose, description, author.  
2. **Setup** – Libraries, sourced functions, CONFIG.  
3. **Create Directories** – Ensure output paths exist.  
4. **Load Data** – Read input data.  
5. **Processing Blocks** – Each produces a capitalized output.  
6. **Scenario / Analysis Blocks** – Specific calculations or models.  
7. **Deliverables** – Save Excel, GDX, plots, or tables.  
8. **Metadata** – Record runtime information.  

---

## 🚀 Example Mini-Workflow  

```r
# Setup ----
library(data.table)
CONFIG = list(database_dir = "E:/Database")

# Load Data ----
tmp1 = fread("farms.csv")
FDALL = unique(tmp1$FARM_ID)

# Process Outliers ----
tmp1 = tmp1[VALUE > 0]
CLEANED_DATA = tmp1

# Analysis ----
SUMMARY = CLEANED_DATA[, .(mean_val = mean(VALUE)), by=FARM_ID]

# Deliverable: Excel ----
EXCEL.list[["Summary"]] = copy(SUMMARY)

# Metadata ----
METADATA = list(
  DATE = Sys.Date(),
  SCRIPT = "03.correct_outliers.R"
)
