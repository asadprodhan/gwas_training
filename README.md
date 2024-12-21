# **3-Day Beginner Training Course: R for Rice Genome-Wide Association Mapping**

## 📚 **Overview**  
This repository contains materials for a **3-day beginner-level training course** on performing **Genome-Wide Association Studies (GWAS)** in rice using **R**. The course covers essential data preprocessing, visualization, and analysis workflows, utilizing publicly available datasets.

---

## 📅 **Course Outline**

### **Day 1: Introduction to R and GWAS Datasets**  
- **Objective:** Familiarize with R basics, loading datasets, and understanding GWAS datasets.  
- **Topics Covered:**  
   - R basics and syntax  
   - Loading and exploring phenotype and genotype datasets  
- **Files Used:**  
   - `phenotype_data.csv`  
   - `genotype_data.csv`  
   - `day1_intro.R`  

**Script Highlights:**  
- Load and explore phenotype data.  
- Load and explore genotype data.  
- Understand data structure and contents.

---

### **Day 2: Data Analysis and Visualization**  
- **Objective:** Learn data visualization techniques with R and create a **Manhattan Plot**.  
- **Topics Covered:**  
   - Data exploration and preprocessing  
   - Visualization using `ggplot2`  
- **Files Used:**  
   - `manhattan_data.csv`  
   - `day2_visualization.R`  

**Script Highlights:**  
- Create a **Manhattan Plot** to visualize SNP associations.  
- Interpret GWAS visualization outputs.

---

### **Day 3: Performing GWAS with GAPIT**  
- **Objective:** Conduct GWAS using the **GAPIT (Genome Association and Prediction Integrated Tool)** package.  
- **Topics Covered:**  
   - Introduction to GAPIT  
   - Running GWAS analysis  
   - Interpreting GWAS outputs  
- **Files Used:**  
   - `phenotype_data.csv`  
   - `genotype_data.csv`  
   - `day3_gwas_pipeline.R`  

**Script Highlights:**  
- Run a GWAS pipeline using GAPIT.  
- Generate and interpret GWAS summary statistics.  

---

## 🛠️ **Installation and Setup**

### **Prerequisites**
Ensure you have the following installed:
- **R** (≥4.0)
- **RStudio** (Recommended)
- Required R Packages:  
   ```r
   install.packages(c("ggplot2", "GAPIT"))   
   ```
# Automated Sample Scripts and Datasets for Rice GWAS Training

This repository contains automated sample scripts and dataset recommendations for a 3-day training program on rice Genome-Wide Association Studies (GWAS) using R.

## 📅 **Day 1: Introduction to R and GWAS Concepts**

### **Session 1: Getting Started with R**
**Script 1: R Basics & Data Import**

```r
# Basic R Operations
x <- 5
y <- 10
sum <- x + y
print(sum)

# Install necessary packages
install.packages(c("readr", "dplyr", "ggplot2"))

# Load dataset (example dataset from Rice SNP-Seek Database)
library(readr)
phenotype_data <- read_csv("phenotype_data.csv")
head(phenotype_data)
```

**Dataset:** Example phenotype data (e.g., plant height, grain yield) in CSV format.

---

### **Session 2: Introduction to GWAS**
**Script 2: Understanding Phenotypic and Genotypic Data**

```r
# Explore phenotype data
summary(phenotype_data)
str(phenotype_data)

# Sample Genotypic Data (Example SNP data)
genotype_data <- read_csv("genotype_data.csv")
head(genotype_data)
```

**Datasets:**
- `phenotype_data.csv`: Sample phenotype data (e.g., Plant Height, Yield).
- `genotype_data.csv`: Example SNP dataset (e.g., marker names, genotypes).

---

### **Session 3: Exploring Rice GWAS Data**
**Script 3: Data Cleaning and Preprocessing**

```r
# Clean Phenotype Data
library(dplyr)
phenotype_data <- phenotype_data %>%
  filter(!is.na(Trait1)) %>%
  mutate(Group = as.factor(Group))

# Clean Genotype Data
genotype_data <- genotype_data %>%
  filter(!is.na(SNP1))
```

---

## 📅 **Day 2: Data Analysis and Visualization in R**

### **Session 1: Data Manipulation in R**
**Script 4: Data Wrangling**

```r
# Summarize phenotype data
library(dplyr)
summary_stats <- phenotype_data %>%
  group_by(Group) %>%
  summarise(mean_height = mean(Height, na.rm = TRUE))

print(summary_stats)
```

---

### **Session 2: Phenotypic Data Analysis**
**Script 5: Descriptive Statistics and Correlation**

```r
# Basic Statistics
summary(phenotype_data$Height)

# Correlation
cor_matrix <- cor(phenotype_data[, c("Height", "Yield")], use = "complete.obs")
print(cor_matrix)
```

---

### **Session 3: Data Visualization**
**Script 6: Manhattan and QQ Plots**

```r
library(ggplot2)

# Sample Manhattan Plot Data
manhattan_data <- data.frame(
  SNP = paste0("SNP", 1:1000),
  Chromosome = sample(1:12, 1000, replace = TRUE),
  Position = runif(1000, 1, 1e6),
  P_value = runif(1000, 0, 0.05)
)

# Manhattan Plot
ggplot(manhattan_data, aes(x = Position, y = -log10(P_value), color = as.factor(Chromosome))) +
  geom_point() +
  theme_minimal() +
  labs(title = "Manhattan Plot", x = "Genomic Position", y = "-log10(P-value)")
```

**Dataset:** `manhattan_data.csv` (or generated dynamically in the script).

---

## 📅 **Day 3: Performing GWAS in R**

### **Session 1: GWAS Analysis Using R Packages**
**Script 7: GWAS with GAPIT**

```r
# Install GAPIT
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("GAPIT")

library(GAPIT)

# Example GWAS Run
gwas_result <- GAPIT(
  Y = phenotype_data,   # Phenotype data
  G = genotype_data,    # Genotype data
  PCA.total = 3         # Number of principal components
)
```

---

### **Session 2: Interpreting GWAS Results**
**Script 8: SNP Annotation and Result Interpretation**

```r
# Filter significant SNPs
significant_snps <- gwas_result$GWAS %>%
  filter(P.value < 0.05)

# Display top SNPs
head(significant_snps)
```

---

### **Session 3: Practical GWAS Workflow**
**Script 9: Full GWAS Pipeline Automation**

```r
# Load necessary libraries
library(GAPIT)
library(dplyr)

# Run GWAS
gwas_result <- GAPIT(
  Y = phenotype_data,
  G = genotype_data,
  PCA.total = 3
)

# Visualize results
library(qqman)
manhattan(gwas_result$GWAS, col = c("blue4", "orange3"))
qq(gwas_result$GWAS$P.value)
```

**Datasets:**
- `phenotype_data.csv`
- `genotype_data.csv`

---

## 📂 **Dataset Files**
- `phenotype_data.csv`: Example phenotype data.
- `genotype_data.csv`: Example genotype SNP data.
- `manhattan_data.csv`: Example GWAS plot data.

## 🛠️ **Dependencies**
- R version >= 4.0
- R Packages: `readr`, `dplyr`, `ggplot2`, `GAPIT`, `qqman`

## 📖 **References**
- GAPIT documentation: [https://zzlab.net/GAPIT/](https://zzlab.net/GAPIT/)
- SNP-Seek Database: [https://snp-seek.irri.org/](https://snp-seek.irri.org/)

---

## 🤝 **Contributing**
Contributions are welcome! Please open an issue or pull request.

## 📜 **License**
This project is licensed under the MIT License.

Happy analyzing! 🌾📊✨
