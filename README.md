## Overview
This is an individual mini data analysis project for UBC course *[STAT 545A](https://stat545.stat.ubc.ca/syllabus-545a/)* in the Winter term 2021. The project aims to use various `Tidyverse` packages in a real-world dataset. The dataset I chose to work on was about Vancouver trees available under the name `vancouver_trees` in the `datateachr` package in `R`. Below is the exhaustive list of tasks that have been done during each milestone.

- [x] Milestone 1
  * Exploring datasets
  * Get insight about variables and plotting different quantities
  * Asking 4 research questions about the data
  * Release a milestone 1 version 
- [x] Milestone 2
  * Created summarizing tibbles for each research question
  * Plotted graphs to get better insights into our data
  * Recognized pitfalls of each question and refined them
  * Selected the two research questions to continue for the next milestone
  * Reformat the dataset (removing irrelevant columns, etc.) for Milestone 3
- [x] Milestone 3
  * Used `forcats` package to reorder a factor
  * Manipulated date format in R with `lubridate` package
  * Fit linear models to see whether there is any correlation between age and diameter of different types of trees
  * Writing and reading `.csv` and `.rds` files
 
 ## Project Files
 ### Milestone Folders
 Files are separated into three folders based on the corresponding milestone. In each milestone folder, you can find below files:
1. *Github markdown (.md)*: [Milestone 1](/Milestone%201/MD1-M1.md), [Milestone 2](/Milestone%202/MDA-M2.md), [Milestone 3](/Milestone%203/mini-project-3.md)
2. *R markdown file (.rmd)*: [Milestone 1](/Milestone%201/MD1-M1.Rmd), [Milestone 2](/Milestone%202/MDA-M2.Rmd), [Milestone 3](/Milestone%203/mini-project-3.rmd)
3. *Figure files*: Each Milestone file contains a folder to save plots produced by the `.rmd` file to render them in the GitHub `.md` file


### Output Folder
This folder contains output files of Milestone 3, which are listed as follows:
* [Summary table (csv file)](/Output/num_tree_nghbr.csv): The number of trees planted from 2016 to 2019 in each neighborhood of Vancouver, which was obtained as a part of Milestone 2
* [Linear model (rds file)](/Output/diameter_age_model.rds): The fitted model on (age, diameter) variables of trees to explore whether there is any correlation between them (should be loaded with ``loadRDS()`` function).


 ## Code Execution
 For running the `.rmd` files the following dependencies are needed:
 * `R` version: 4.1.1
 * `tidyverse` package version: 1.3.1
 * `datateachr` package version: 0.2.1
 
 These packages are loaded at the beginning of each `.rmd` file. Using IDEs such as `RStudio` is recommended to execute R markdown files by simply knit each `.rmd` and reproducing Github markdown (`.md`) output. The output could be easily changed to the desired format by changing the YAML header in the `.rmd` files.

## Contributers
[@Hossein](https://github.com/HosseinEbrahimiK)

