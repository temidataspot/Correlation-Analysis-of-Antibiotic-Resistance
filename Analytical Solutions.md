# Analysis 

The purpose of this analysis is to investigate patterns of antibiotic co-resistance among the ten antibiotics. 
By examining correlations between resistance levels, the analysis aims to identify if resistance to one antibiotic is 
statistically associated with resistance to others. This can reveal patterns of multidrug resistance, which occur when 
bacteria are resistant to multiple antibiotics due to shared mechanisms or selective pressures.

**Steps:**
- Install relevant packages and load their libraries
- Load the uploaded excel file and assign to a dataframe
- Subset the columns related to antibiotic resistance
- Calculate correlation matrix. Method used in this analysis is **pearson correlation**
- Plot correlation heatmap

# Pearson correlation

```r
# Install the necessary packages
install.packages("readxl")
install.packages("tidyverse")
install.packages("dplyr")
install.packages("ggplot2")
install.packages("corrplot")
install.packages("reshape2")
install.packages("pheatmap")
install.packages("RColorBrewer")
install.packages("ggcorrplot")

#Load libraries
library(readxl)
library(tidyverse)
library(dplyr)
library(ggplot2)
library(corrplot)
library(reshape2)
library(pheatmap)
library(RColorBrewer)
library(ggcorrplot)

# Load the Excel file
aeromonas <- read_excel("aeromonas_dataset.xlsx", sheet = "Sheet4", range = "A1:AF6")
View(aeromonas)

# Sum the values for each antibiotic across sensitive, resistant, and intermediate
antibiotic_combined <- aeromonas %>%
  mutate(
    AML = rowSums(select(., starts_with("AML"))),
    CN  = rowSums(select(., starts_with("CN"))),
    SXT = rowSums(select(., starts_with("SXT"))),
    CT  = rowSums(select(., starts_with("CT"))),
    N   = rowSums(select(., starts_with("N"))),
    OT  = rowSums(select(., starts_with("OT"))),
    CIP = rowSums(select(., starts_with("CIP"))),
    C   = rowSums(select(., starts_with("C"))),
    P   = rowSums(select(., starts_with("P"))),
    AMP = rowSums(select(., starts_with("AMP")))
  ) %>%
  # Select only the summed columns
  select(AML, CN, SXT, CT, N, OT, CIP, C, P, AMP)

# View the new simplified data
View(antibiotic_combined)

# Calculate the correlation matrix for combined antibiotics
correlation_matrix_combined <- cor(antibiotic_combined, method = "pearson")

# Plot the correlation heatmap for combined antibiotics
ggcorrplot(correlation_matrix_combined, method = "circle", colors = c("purple", "white", "red"), lab = TRUE) +
  ggtitle("Correlation of Antibiotic Resistance") +
  theme(plot.title = element_text(hjust = 0.5))  # Center the title
```
![image](https://github.com/temidataspot/Correlation-Analysis-of-Antibiotic-Resistance/blob/main/Screenshot%202024-10-18%20113043.png)
