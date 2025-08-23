# Seattle Pets Analysis

This repository contains exploratory analysis of the **Seattle Pets** dataset from the [openintro](https://cran.r-project.org/package=openintro) R package. The project focuses on cleaning pet name data, counting popularity, and comparing naming patterns between cats and dogs.

## Dataset

- **Source:** `openintro` package  
- **Dataset name:** `seattlepets`  
- **Key variables:**
  - `animal_name` – registered name of the pet  
  - `species` – type of animal (e.g., Dog, Cat, etc.)  
  - additional metadata: license issue dates, zip codes, etc.

## Analysis Workflow

### 1. Data Cleaning
- Convert pet names to lowercase with `tolower()`  
- Trim whitespace with `trimws()`  
- Remove missing (`NA`) or blank names  

```r
seattlepets <- seattlepets %>%
  mutate(animal_name = animal_name |>
           tolower() |>
           trimws()) %>%
  filter(!is.na(animal_name), animal_name != "")
````

### 2. Counting Popular Names

* Count frequency of each pet name, sorted in descending order
* Use `count()` with the `name = "count"` argument for clarity

```r
popular_names <- seattlepets %>%
  count(animal_name, sort = TRUE, name = "count")
```

### 3. Popular Names by Species

* Break down counts by both `species` and `animal_name`

```r
popular_names <- seattlepets %>%
  count(species, animal_name, sort = TRUE, name = "count")
```

### 4. Top Names per Species

* Select the top 10 names within each species

```r
top_names <- popular_names %>%
  group_by(species) %>%
  slice_max(order_by = count, n = 10, with_ties = FALSE)
```

### 5. Contingency Table

* Build a species × name table of counts
* Replace 0s with `NA` for readability

```r
contingency_tbl <- top_names %>%
  pivot_wider(
    names_from = species,
    values_from = count,
    values_fill = NA
  )
```

### 6. Visualization

* Compare proportions of cats vs. dogs for popular names
* Names **above the diagonal line** are more common among dogs;
  names **below the line** are more common among cats.

Example interpretation:

* **Lucy** and **Charlie** appear more often among dogs.
* **Pepper** and **Chloe** appear more often among cats.
* Overall relationship is **positive** — popular cat names tend also to be popular dog names.


## Key Insights

* The most common names (e.g., *Luna, Lucy, Bella, Max*) are popular across both cats and dogs.
* Certain names lean species-specific (*Pepper* → cats, *Charlie* → dogs).
* A simple correlation of name frequencies between species is **positive**, highlighting shared trends in naming culture.
* Visualization reveals important nuances not visible in raw counts alone.


## Requirements

* R (≥ 4.0)
* Packages: `openintro`, `tidyverse` (dplyr, tidyr, ggplot2), `knitr`


## Repository Structure

```
.
├── pet-analysis.Rmd      # Main R Markdown document
├── pet-analysis.html     # Knitted output (rendered report)
├── README.md             # Project overview (this file)
└── figures/              # Saved plots
```



## Next Steps

* Extend analysis by zip code or year.
* Visualize geographic distribution of popular names.
* Explore whether licensing dates reveal name trends over time.


**Seattle’s pets tell us as much about their humans as about themselves!**
