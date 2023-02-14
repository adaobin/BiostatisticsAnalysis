---
title: "Lemurs"
author: "Adaobi Nwankwo"
format: html
editor: visual
execute:
  keep-md: true
---



## Lemurs Analysis

I'm choosing to work on this data set from the Duke Lemur Center.

## Loading Packages and Data sets

Reading in the data. Here we will load the tidyverse package and Lemurs data.


::: {.cell}

```{.r .cell-code}
#Load the tidyverse
library(tidyverse)
```

::: {.cell-output .cell-output-stderr}
```
── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
✔ tibble  3.1.8      ✔ dplyr   1.0.10
✔ tidyr   1.2.1      ✔ stringr 1.5.0 
✔ readr   2.1.3      ✔ forcats 0.5.2 
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
```
:::

```{.r .cell-code}
library(kableExtra)
```

::: {.cell-output .cell-output-stderr}
```

Attaching package: 'kableExtra'

The following object is masked from 'package:dplyr':

    group_rows
```
:::
:::

::: {.cell}

```{.r .cell-code}
lemurs <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-08-24/lemur_data.csv')
```

::: {.cell-output .cell-output-stderr}
```
Rows: 82609 Columns: 54
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr  (19): taxon, dlc_id, hybrid, sex, name, current_resident, stud_book, es...
dbl  (27): birth_month, litter_size, expected_gestation, concep_month, dam_a...
date  (8): dob, estimated_concep, dam_dob, sire_dob, dod, weight_date, conce...

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
:::
:::

::: {.cell}

```{.r .cell-code}
#install.packages("tidymodels")
library(tidymodels)
```

::: {.cell-output .cell-output-stderr}
```
── Attaching packages ────────────────────────────────────── tidymodels 1.0.0 ──
```
:::

::: {.cell-output .cell-output-stderr}
```
✔ broom        1.0.2     ✔ rsample      1.1.1
✔ dials        1.1.0     ✔ tune         1.0.1
✔ infer        1.0.4     ✔ workflows    1.1.2
✔ modeldata    1.1.0     ✔ workflowsets 1.0.0
✔ parsnip      1.0.3     ✔ yardstick    1.1.0
✔ recipes      1.0.4     
```
:::

::: {.cell-output .cell-output-stderr}
```
── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
✖ scales::discard()        masks purrr::discard()
✖ dplyr::filter()          masks stats::filter()
✖ recipes::fixed()         masks stringr::fixed()
✖ kableExtra::group_rows() masks dplyr::group_rows()
✖ dplyr::lag()             masks stats::lag()
✖ yardstick::spec()        masks readr::spec()
✖ recipes::step()          masks stats::step()
• Dig deeper into tidy modeling with R at https://www.tmwr.org
```
:::

```{.r .cell-code}
my_data_splits <- initial_split(lemurs, prop = 0.5)

exploratory_data <- training(my_data_splits)
test_data <- testing(my_data_splits)
```
:::

::: {.cell}

```{.r .cell-code}
#install.packages("skimr")
library(skimr)
exploratory_data%>%
  skim()
```

::: {.cell-output-display}
<table style='width: auto;'
      class='table table-condensed'>
<caption>Data summary</caption>
<tbody>
  <tr>
   <td style="text-align:left;"> Name </td>
   <td style="text-align:left;"> Piped data </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Number of rows </td>
   <td style="text-align:left;"> 41304 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Number of columns </td>
   <td style="text-align:left;"> 54 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> _______________________ </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Column type frequency: </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> character </td>
   <td style="text-align:left;"> 19 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Date </td>
   <td style="text-align:left;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> numeric </td>
   <td style="text-align:left;"> 27 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ________________________ </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Group variables </td>
   <td style="text-align:left;"> None </td>
  </tr>
</tbody>
</table>


**Variable type: character**

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> skim_variable </th>
   <th style="text-align:right;"> n_missing </th>
   <th style="text-align:right;"> complete_rate </th>
   <th style="text-align:right;"> min </th>
   <th style="text-align:right;"> max </th>
   <th style="text-align:right;"> empty </th>
   <th style="text-align:right;"> n_unique </th>
   <th style="text-align:right;"> whitespace </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> taxon </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dlc_id </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hybrid </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sex </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> name </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> current_resident </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> stud_book </td>
   <td style="text-align:right;"> 1358 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1160 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> estimated_dob </td>
   <td style="text-align:right;"> 36084 </td>
   <td style="text-align:right;"> 0.13 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> birth_type </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> birth_institution </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 86 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_id </td>
   <td style="text-align:right;"> 502 </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 548 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_name </td>
   <td style="text-align:right;"> 7581 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 471 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_taxon </td>
   <td style="text-align:right;"> 7581 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_id </td>
   <td style="text-align:right;"> 855 </td>
   <td style="text-align:right;"> 0.98 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 471 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_name </td>
   <td style="text-align:right;"> 12426 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 396 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_taxon </td>
   <td style="text-align:right;"> 12426 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dob_estimated </td>
   <td style="text-align:right;"> 36084 </td>
   <td style="text-align:right;"> 0.13 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_category </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> preg_status </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
</tbody>
</table>


**Variable type: Date**

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> skim_variable </th>
   <th style="text-align:right;"> n_missing </th>
   <th style="text-align:right;"> complete_rate </th>
   <th style="text-align:left;"> min </th>
   <th style="text-align:left;"> max </th>
   <th style="text-align:left;"> median </th>
   <th style="text-align:right;"> n_unique </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> dob </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 1946-10-01 </td>
   <td style="text-align:left;"> 2018-07-24 </td>
   <td style="text-align:left;"> 1994-04-10 </td>
   <td style="text-align:right;"> 1405 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> estimated_concep </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 1946-06-03 </td>
   <td style="text-align:left;"> 2018-05-22 </td>
   <td style="text-align:left;"> 1993-11-11 </td>
   <td style="text-align:right;"> 1442 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_dob </td>
   <td style="text-align:right;"> 7581 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:left;"> 1958-10-01 </td>
   <td style="text-align:left;"> 2015-01-08 </td>
   <td style="text-align:left;"> 1986-06-26 </td>
   <td style="text-align:right;"> 408 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_dob </td>
   <td style="text-align:right;"> 12426 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:left;"> 1946-10-01 </td>
   <td style="text-align:left;"> 2014-08-07 </td>
   <td style="text-align:left;"> 1985-11-29 </td>
   <td style="text-align:right;"> 357 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dod </td>
   <td style="text-align:right;"> 18746 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:left;"> 1969-06-02 </td>
   <td style="text-align:left;"> 2019-01-15 </td>
   <td style="text-align:left;"> 2008-09-27 </td>
   <td style="text-align:right;"> 1198 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> weight_date </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 1968-09-16 </td>
   <td style="text-align:left;"> 2019-02-05 </td>
   <td style="text-align:left;"> 2006-01-10 </td>
   <td style="text-align:right;"> 8768 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> concep_date_if_preg </td>
   <td style="text-align:right;"> 40336 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:left;"> 1971-11-09 </td>
   <td style="text-align:left;"> 2018-05-22 </td>
   <td style="text-align:left;"> 2003-08-20 </td>
   <td style="text-align:right;"> 498 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> infant_dob_if_preg </td>
   <td style="text-align:right;"> 40336 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:left;"> 1972-03-08 </td>
   <td style="text-align:left;"> 2018-07-24 </td>
   <td style="text-align:left;"> 2004-01-27 </td>
   <td style="text-align:right;"> 493 </td>
  </tr>
</tbody>
</table>


**Variable type: numeric**

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> skim_variable </th>
   <th style="text-align:right;"> n_missing </th>
   <th style="text-align:right;"> complete_rate </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> sd </th>
   <th style="text-align:right;"> p0 </th>
   <th style="text-align:right;"> p25 </th>
   <th style="text-align:right;"> p50 </th>
   <th style="text-align:right;"> p75 </th>
   <th style="text-align:right;"> p100 </th>
   <th style="text-align:left;"> hist </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> birth_month </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 5.58 </td>
   <td style="text-align:right;"> 2.71 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:right;"> 5.00 </td>
   <td style="text-align:right;"> 7.00 </td>
   <td style="text-align:right;"> 12.00 </td>
   <td style="text-align:left;"> ▇▇▇▂▃ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> litter_size </td>
   <td style="text-align:right;"> 9059 </td>
   <td style="text-align:right;"> 0.78 </td>
   <td style="text-align:right;"> 1.65 </td>
   <td style="text-align:right;"> 0.81 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:left;"> ▇▅▁▂▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> expected_gestation </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 119.39 </td>
   <td style="text-align:right;"> 39.72 </td>
   <td style="text-align:right;"> 62.00 </td>
   <td style="text-align:right;"> 90.00 </td>
   <td style="text-align:right;"> 124.00 </td>
   <td style="text-align:right;"> 160.00 </td>
   <td style="text-align:right;"> 193.00 </td>
   <td style="text-align:left;"> ▆▃▇▅▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> concep_month </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6.62 </td>
   <td style="text-align:right;"> 3.69 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:right;"> 6.00 </td>
   <td style="text-align:right;"> 11.00 </td>
   <td style="text-align:right;"> 12.00 </td>
   <td style="text-align:left;"> ▆▆▅▂▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_age_at_concep_y </td>
   <td style="text-align:right;"> 7585 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:right;"> 7.20 </td>
   <td style="text-align:right;"> 4.64 </td>
   <td style="text-align:right;"> 0.59 </td>
   <td style="text-align:right;"> 3.64 </td>
   <td style="text-align:right;"> 6.44 </td>
   <td style="text-align:right;"> 10.19 </td>
   <td style="text-align:right;"> 26.03 </td>
   <td style="text-align:left;"> ▇▆▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_age_at_concep_y </td>
   <td style="text-align:right;"> 12430 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 9.14 </td>
   <td style="text-align:right;"> 6.26 </td>
   <td style="text-align:right;"> 0.61 </td>
   <td style="text-align:right;"> 4.64 </td>
   <td style="text-align:right;"> 7.49 </td>
   <td style="text-align:right;"> 12.44 </td>
   <td style="text-align:right;"> 33.36 </td>
   <td style="text-align:left;"> ▇▅▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_death_y </td>
   <td style="text-align:right;"> 18750 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 17.44 </td>
   <td style="text-align:right;"> 8.45 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 11.06 </td>
   <td style="text-align:right;"> 17.21 </td>
   <td style="text-align:right;"> 23.40 </td>
   <td style="text-align:right;"> 39.39 </td>
   <td style="text-align:left;"> ▃▇▇▅▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_of_living_y </td>
   <td style="text-align:right;"> 26638 </td>
   <td style="text-align:right;"> 0.36 </td>
   <td style="text-align:right;"> 13.50 </td>
   <td style="text-align:right;"> 8.86 </td>
   <td style="text-align:right;"> 0.54 </td>
   <td style="text-align:right;"> 6.62 </td>
   <td style="text-align:right;"> 10.65 </td>
   <td style="text-align:right;"> 19.81 </td>
   <td style="text-align:right;"> 35.20 </td>
   <td style="text-align:left;"> ▇▇▃▃▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_last_verified_y </td>
   <td style="text-align:right;"> 37224 </td>
   <td style="text-align:right;"> 0.10 </td>
   <td style="text-align:right;"> 12.90 </td>
   <td style="text-align:right;"> 8.19 </td>
   <td style="text-align:right;"> 0.36 </td>
   <td style="text-align:right;"> 6.82 </td>
   <td style="text-align:right;"> 10.40 </td>
   <td style="text-align:right;"> 19.14 </td>
   <td style="text-align:right;"> 34.26 </td>
   <td style="text-align:left;"> ▆▇▃▃▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_max_live_or_dead_y </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 15.59 </td>
   <td style="text-align:right;"> 8.81 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 7.80 </td>
   <td style="text-align:right;"> 14.56 </td>
   <td style="text-align:right;"> 22.54 </td>
   <td style="text-align:right;"> 39.39 </td>
   <td style="text-align:left;"> ▇▇▆▅▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> n_known_offspring </td>
   <td style="text-align:right;"> 18479 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 5.60 </td>
   <td style="text-align:right;"> 4.77 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:right;"> 7.00 </td>
   <td style="text-align:right;"> 36.00 </td>
   <td style="text-align:left;"> ▇▂▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> weight_g </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1483.56 </td>
   <td style="text-align:right;"> 1317.63 </td>
   <td style="text-align:right;"> 4.74 </td>
   <td style="text-align:right;"> 196.00 </td>
   <td style="text-align:right;"> 1300.00 </td>
   <td style="text-align:right;"> 2480.00 </td>
   <td style="text-align:right;"> 10337.00 </td>
   <td style="text-align:left;"> ▇▅▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> month_of_weight </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6.50 </td>
   <td style="text-align:right;"> 3.40 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:right;"> 7.00 </td>
   <td style="text-align:right;"> 9.00 </td>
   <td style="text-align:right;"> 12.00 </td>
   <td style="text-align:left;"> ▇▆▆▆▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_d </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3101.69 </td>
   <td style="text-align:right;"> 2822.08 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 765.00 </td>
   <td style="text-align:right;"> 2305.00 </td>
   <td style="text-align:right;"> 4799.00 </td>
   <td style="text-align:right;"> 14373.00 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_wk </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 443.10 </td>
   <td style="text-align:right;"> 403.15 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 109.29 </td>
   <td style="text-align:right;"> 329.29 </td>
   <td style="text-align:right;"> 685.57 </td>
   <td style="text-align:right;"> 2053.29 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_mo </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 101.97 </td>
   <td style="text-align:right;"> 92.78 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 25.15 </td>
   <td style="text-align:right;"> 75.78 </td>
   <td style="text-align:right;"> 157.78 </td>
   <td style="text-align:right;"> 472.54 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_mo_no_dec </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 101.48 </td>
   <td style="text-align:right;"> 92.77 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 25.00 </td>
   <td style="text-align:right;"> 75.00 </td>
   <td style="text-align:right;"> 157.00 </td>
   <td style="text-align:right;"> 472.00 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_y </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 8.50 </td>
   <td style="text-align:right;"> 7.73 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 2.10 </td>
   <td style="text-align:right;"> 6.32 </td>
   <td style="text-align:right;"> 13.15 </td>
   <td style="text-align:right;"> 39.38 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> change_since_prev_wt_g </td>
   <td style="text-align:right;"> 1109 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 20.20 </td>
   <td style="text-align:right;"> 176.42 </td>
   <td style="text-align:right;"> -1560.00 </td>
   <td style="text-align:right;"> -20.00 </td>
   <td style="text-align:right;"> 2.00 </td>
   <td style="text-align:right;"> 45.00 </td>
   <td style="text-align:right;"> 3662.00 </td>
   <td style="text-align:left;"> ▁▇▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> days_since_prev_wt </td>
   <td style="text-align:right;"> 1109 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 55.13 </td>
   <td style="text-align:right;"> 151.88 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 13.00 </td>
   <td style="text-align:right;"> 28.00 </td>
   <td style="text-align:right;"> 55.00 </td>
   <td style="text-align:right;"> 6394.00 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> avg_daily_wt_change_g </td>
   <td style="text-align:right;"> 1227 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 0.60 </td>
   <td style="text-align:right;"> 12.14 </td>
   <td style="text-align:right;"> -920.00 </td>
   <td style="text-align:right;"> -0.63 </td>
   <td style="text-align:right;"> 0.13 </td>
   <td style="text-align:right;"> 1.82 </td>
   <td style="text-align:right;"> 300.00 </td>
   <td style="text-align:left;"> ▁▁▁▇▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> days_before_death </td>
   <td style="text-align:right;"> 18746 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 2665.59 </td>
   <td style="text-align:right;"> 2271.44 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 829.25 </td>
   <td style="text-align:right;"> 2064.00 </td>
   <td style="text-align:right;"> 4058.00 </td>
   <td style="text-align:right;"> 13052.00 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> r_min_dam_age_at_concep_y </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.56 </td>
   <td style="text-align:right;"> 0.91 </td>
   <td style="text-align:right;"> 0.40 </td>
   <td style="text-align:right;"> 0.79 </td>
   <td style="text-align:right;"> 1.53 </td>
   <td style="text-align:right;"> 1.76 </td>
   <td style="text-align:right;"> 4.22 </td>
   <td style="text-align:left;"> ▆▇▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> expected_gestation_d </td>
   <td style="text-align:right;"> 40336 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 141.45 </td>
   <td style="text-align:right;"> 33.10 </td>
   <td style="text-align:right;"> 62.00 </td>
   <td style="text-align:right;"> 124.00 </td>
   <td style="text-align:right;"> 160.00 </td>
   <td style="text-align:right;"> 160.00 </td>
   <td style="text-align:right;"> 193.00 </td>
   <td style="text-align:left;"> ▂▂▇▇▃ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> days_before_inf_birth_if_preg </td>
   <td style="text-align:right;"> 40336 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 70.04 </td>
   <td style="text-align:right;"> 47.03 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 28.00 </td>
   <td style="text-align:right;"> 67.50 </td>
   <td style="text-align:right;"> 106.25 </td>
   <td style="text-align:right;"> 192.00 </td>
   <td style="text-align:left;"> ▇▆▆▃▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pct_preg_remain_if_preg </td>
   <td style="text-align:right;"> 40336 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.22 </td>
   <td style="text-align:right;"> 0.51 </td>
   <td style="text-align:right;"> 0.75 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> ▇▆▆▇▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> infant_lit_sz_if_preg </td>
   <td style="text-align:right;"> 40341 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 1.28 </td>
   <td style="text-align:right;"> 0.56 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:left;"> ▇▂▁▁▁ </td>
  </tr>
</tbody>
</table>
:::
:::


Here, I loaded in a data set for the exploratory data of the Lemur data set. This is half of the data set.


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  head(10) %>%
  kable() %>%
  kable_styling(c("hover", "striped"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> taxon </th>
   <th style="text-align:left;"> dlc_id </th>
   <th style="text-align:left;"> hybrid </th>
   <th style="text-align:left;"> sex </th>
   <th style="text-align:left;"> name </th>
   <th style="text-align:left;"> current_resident </th>
   <th style="text-align:left;"> stud_book </th>
   <th style="text-align:left;"> dob </th>
   <th style="text-align:right;"> birth_month </th>
   <th style="text-align:left;"> estimated_dob </th>
   <th style="text-align:left;"> birth_type </th>
   <th style="text-align:left;"> birth_institution </th>
   <th style="text-align:right;"> litter_size </th>
   <th style="text-align:right;"> expected_gestation </th>
   <th style="text-align:left;"> estimated_concep </th>
   <th style="text-align:right;"> concep_month </th>
   <th style="text-align:left;"> dam_id </th>
   <th style="text-align:left;"> dam_name </th>
   <th style="text-align:left;"> dam_taxon </th>
   <th style="text-align:left;"> dam_dob </th>
   <th style="text-align:right;"> dam_age_at_concep_y </th>
   <th style="text-align:left;"> sire_id </th>
   <th style="text-align:left;"> sire_name </th>
   <th style="text-align:left;"> sire_taxon </th>
   <th style="text-align:left;"> sire_dob </th>
   <th style="text-align:right;"> sire_age_at_concep_y </th>
   <th style="text-align:left;"> dod </th>
   <th style="text-align:right;"> age_at_death_y </th>
   <th style="text-align:right;"> age_of_living_y </th>
   <th style="text-align:right;"> age_last_verified_y </th>
   <th style="text-align:right;"> age_max_live_or_dead_y </th>
   <th style="text-align:right;"> n_known_offspring </th>
   <th style="text-align:left;"> dob_estimated </th>
   <th style="text-align:right;"> weight_g </th>
   <th style="text-align:left;"> weight_date </th>
   <th style="text-align:right;"> month_of_weight </th>
   <th style="text-align:right;"> age_at_wt_d </th>
   <th style="text-align:right;"> age_at_wt_wk </th>
   <th style="text-align:right;"> age_at_wt_mo </th>
   <th style="text-align:right;"> age_at_wt_mo_no_dec </th>
   <th style="text-align:right;"> age_at_wt_y </th>
   <th style="text-align:right;"> change_since_prev_wt_g </th>
   <th style="text-align:right;"> days_since_prev_wt </th>
   <th style="text-align:right;"> avg_daily_wt_change_g </th>
   <th style="text-align:right;"> days_before_death </th>
   <th style="text-align:right;"> r_min_dam_age_at_concep_y </th>
   <th style="text-align:left;"> age_category </th>
   <th style="text-align:left;"> preg_status </th>
   <th style="text-align:right;"> expected_gestation_d </th>
   <th style="text-align:left;"> concep_date_if_preg </th>
   <th style="text-align:left;"> infant_dob_if_preg </th>
   <th style="text-align:right;"> days_before_inf_birth_if_preg </th>
   <th style="text-align:right;"> pct_preg_remain_if_preg </th>
   <th style="text-align:right;"> infant_lit_sz_if_preg </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> ERUF </td>
   <td style="text-align:left;"> 5740 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> ROSIE </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 982 </td>
   <td style="text-align:left;"> 1983-03-20 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:left;"> 1982-11-20 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> 4507 </td>
   <td style="text-align:left;"> ROUGE </td>
   <td style="text-align:left;"> ERUF </td>
   <td style="text-align:left;"> 1977-02-27 </td>
   <td style="text-align:right;"> 5.73 </td>
   <td style="text-align:left;"> 3556 </td>
   <td style="text-align:left;"> MENELAUS </td>
   <td style="text-align:left;"> ERUF </td>
   <td style="text-align:left;"> 1978-04-03 </td>
   <td style="text-align:right;"> 4.64 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 5.24 </td>
   <td style="text-align:right;"> 5.24 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 2651 </td>
   <td style="text-align:left;"> 1988-06-06 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 1905 </td>
   <td style="text-align:right;"> 272.14 </td>
   <td style="text-align:right;"> 62.63 </td>
   <td style="text-align:right;"> 62 </td>
   <td style="text-align:right;"> 5.22 </td>
   <td style="text-align:right;"> 236 </td>
   <td style="text-align:right;"> 251 </td>
   <td style="text-align:right;"> 0.94 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 1.55 </td>
   <td style="text-align:left;"> adult </td>
   <td style="text-align:left;"> NP </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ECOR </td>
   <td style="text-align:left;"> 6979 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> Seshat </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:left;"> 99 </td>
   <td style="text-align:left;"> 2010-07-13 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 124 </td>
   <td style="text-align:left;"> 2010-03-11 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> 6800 </td>
   <td style="text-align:left;"> Tasherit </td>
   <td style="text-align:left;"> ECOR </td>
   <td style="text-align:left;"> 2002-04-15 </td>
   <td style="text-align:right;"> 7.91 </td>
   <td style="text-align:left;"> 6369 </td>
   <td style="text-align:left;"> Ikenaten </td>
   <td style="text-align:left;"> ECOR </td>
   <td style="text-align:left;"> 1990-04-18 </td>
   <td style="text-align:right;"> 19.91 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 8.58 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 8.58 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1860 </td>
   <td style="text-align:left;"> 2014-05-08 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1395 </td>
   <td style="text-align:right;"> 199.29 </td>
   <td style="text-align:right;"> 45.86 </td>
   <td style="text-align:right;"> 45 </td>
   <td style="text-align:right;"> 3.82 </td>
   <td style="text-align:right;"> -40 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> -3.64 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 1.70 </td>
   <td style="text-align:left;"> adult </td>
   <td style="text-align:left;"> P </td>
   <td style="text-align:right;"> 124 </td>
   <td style="text-align:left;"> 2014-01-10 </td>
   <td style="text-align:left;"> 2014-05-14 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 0.0483871 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> CMED </td>
   <td style="text-align:left;"> 3619 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> ORIOLE </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 352 </td>
   <td style="text-align:left;"> 1988-08-10 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 62 </td>
   <td style="text-align:left;"> 1988-06-09 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> 1662 </td>
   <td style="text-align:left;"> FORTUNA </td>
   <td style="text-align:left;"> CMED </td>
   <td style="text-align:left;"> 1985-07-23 </td>
   <td style="text-align:right;"> 2.88 </td>
   <td style="text-align:left;"> 1693 </td>
   <td style="text-align:left;"> KINGLET </td>
   <td style="text-align:left;"> CMED </td>
   <td style="text-align:left;"> 1986-07-11 </td>
   <td style="text-align:right;"> 1.92 </td>
   <td style="text-align:left;"> 2010-11-16 </td>
   <td style="text-align:right;"> 22.28 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 22.28 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 220 </td>
   <td style="text-align:left;"> 2005-03-27 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 6073 </td>
   <td style="text-align:right;"> 867.57 </td>
   <td style="text-align:right;"> 199.66 </td>
   <td style="text-align:right;"> 199 </td>
   <td style="text-align:right;"> 16.64 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:right;"> 0.25 </td>
   <td style="text-align:right;"> 2060 </td>
   <td style="text-align:right;"> 0.79 </td>
   <td style="text-align:left;"> adult </td>
   <td style="text-align:left;"> NP </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> VVV </td>
   <td style="text-align:left;"> 6918 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> Grace </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 1857 </td>
   <td style="text-align:left;"> 1997-04-06 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Santa Ana Zoo </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 104 </td>
   <td style="text-align:left;"> 1996-12-23 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> oi_M92017 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> oi_0790 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> 2014-05-28 </td>
   <td style="text-align:right;"> 17.15 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 17.15 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 3980 </td>
   <td style="text-align:left;"> 2014-03-11 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 6183 </td>
   <td style="text-align:right;"> 883.29 </td>
   <td style="text-align:right;"> 203.28 </td>
   <td style="text-align:right;"> 203 </td>
   <td style="text-align:right;"> 16.94 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 78 </td>
   <td style="text-align:right;"> 1.59 </td>
   <td style="text-align:left;"> adult </td>
   <td style="text-align:left;"> NP </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MZAZ </td>
   <td style="text-align:left;"> 319 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> GAKA </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 101 </td>
   <td style="text-align:left;"> 1979-08-25 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:left;"> WB </td>
   <td style="text-align:left;"> Madagascar / </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 90 </td>
   <td style="text-align:left;"> 1979-05-27 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> WILD </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> WILD </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> 1995-09-22 </td>
   <td style="text-align:right;"> 16.09 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 16.09 </td>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:right;"> 280 </td>
   <td style="text-align:left;"> 1994-05-24 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5386 </td>
   <td style="text-align:right;"> 769.43 </td>
   <td style="text-align:right;"> 177.07 </td>
   <td style="text-align:right;"> 177 </td>
   <td style="text-align:right;"> 14.76 </td>
   <td style="text-align:right;"> -30 </td>
   <td style="text-align:right;"> 90 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> 486 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:left;"> adult </td>
   <td style="text-align:left;"> NP </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ERUF </td>
   <td style="text-align:left;"> 5799 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> COCHINEAL </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 1023 </td>
   <td style="text-align:left;"> 1983-05-08 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:left;"> 1983-01-08 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> 4502 </td>
   <td style="text-align:left;"> OCHRE </td>
   <td style="text-align:left;"> ERUF </td>
   <td style="text-align:left;"> 1968-10-01 </td>
   <td style="text-align:right;"> 14.28 </td>
   <td style="text-align:left;"> 515 </td>
   <td style="text-align:left;"> THURBER </td>
   <td style="text-align:left;"> ERUF </td>
   <td style="text-align:left;"> 1958-10-01 </td>
   <td style="text-align:right;"> 24.29 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 3.67 </td>
   <td style="text-align:right;"> 3.67 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 2293 </td>
   <td style="text-align:left;"> 1986-04-04 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1062 </td>
   <td style="text-align:right;"> 151.71 </td>
   <td style="text-align:right;"> 34.92 </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 2.91 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 1.55 </td>
   <td style="text-align:left;"> young_adult </td>
   <td style="text-align:left;"> NP </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NCOU </td>
   <td style="text-align:left;"> 1959 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> KIRITAN </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 1353 </td>
   <td style="text-align:left;"> 1985-04-06 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Skansen Akvariet </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 193 </td>
   <td style="text-align:left;"> 1984-09-25 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> 2006-06-03 </td>
   <td style="text-align:right;"> 21.17 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 21.17 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:right;"> 930 </td>
   <td style="text-align:left;"> 1994-02-02 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3224 </td>
   <td style="text-align:right;"> 460.57 </td>
   <td style="text-align:right;"> 105.99 </td>
   <td style="text-align:right;"> 105 </td>
   <td style="text-align:right;"> 8.83 </td>
   <td style="text-align:right;"> -30 </td>
   <td style="text-align:right;"> 64 </td>
   <td style="text-align:right;"> -0.47 </td>
   <td style="text-align:right;"> 4504 </td>
   <td style="text-align:right;"> 1.33 </td>
   <td style="text-align:left;"> adult </td>
   <td style="text-align:left;"> NP </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> VVV </td>
   <td style="text-align:left;"> 5712 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> RIGEL </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 133 </td>
   <td style="text-align:left;"> 1978-04-28 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Gladys Porter Zoo </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 104 </td>
   <td style="text-align:left;"> 1978-01-14 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> oi_524 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> oi_389 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> 2004-06-20 </td>
   <td style="text-align:right;"> 26.16 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 26.16 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 4159 </td>
   <td style="text-align:left;"> 1991-12-19 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 4983 </td>
   <td style="text-align:right;"> 711.86 </td>
   <td style="text-align:right;"> 163.82 </td>
   <td style="text-align:right;"> 163 </td>
   <td style="text-align:right;"> 13.65 </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:right;"> 0.27 </td>
   <td style="text-align:right;"> 4567 </td>
   <td style="text-align:right;"> 1.59 </td>
   <td style="text-align:left;"> adult </td>
   <td style="text-align:left;"> NP </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EFLA </td>
   <td style="text-align:left;"> 6521 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> LANGE </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 62 </td>
   <td style="text-align:left;"> 1993-03-11 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 124 </td>
   <td style="text-align:left;"> 1992-11-07 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> 6393 </td>
   <td style="text-align:left;"> GARBO </td>
   <td style="text-align:left;"> EFLA </td>
   <td style="text-align:left;"> 1987-11-01 </td>
   <td style="text-align:right;"> 5.02 </td>
   <td style="text-align:left;"> 6248 </td>
   <td style="text-align:left;"> BARRYMORE </td>
   <td style="text-align:left;"> EFLA </td>
   <td style="text-align:left;"> 1988-05-09 </td>
   <td style="text-align:right;"> 4.50 </td>
   <td style="text-align:left;"> 2007-05-07 </td>
   <td style="text-align:right;"> 14.16 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 14.16 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1710 </td>
   <td style="text-align:left;"> 1993-11-09 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 243 </td>
   <td style="text-align:right;"> 34.71 </td>
   <td style="text-align:right;"> 7.99 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 0.67 </td>
   <td style="text-align:right;"> 780 </td>
   <td style="text-align:right;"> 111 </td>
   <td style="text-align:right;"> 7.03 </td>
   <td style="text-align:right;"> 4927 </td>
   <td style="text-align:right;"> 1.58 </td>
   <td style="text-align:left;"> IJ </td>
   <td style="text-align:left;"> NP </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ECOR </td>
   <td style="text-align:left;"> 6478 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> Geb </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 54 </td>
   <td style="text-align:left;"> 1992-04-06 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 124 </td>
   <td style="text-align:left;"> 1991-12-04 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> 5937 </td>
   <td style="text-align:left;"> MERITATEN </td>
   <td style="text-align:left;"> ECOR </td>
   <td style="text-align:left;"> 1984-05-29 </td>
   <td style="text-align:right;"> 7.52 </td>
   <td style="text-align:left;"> 6251 </td>
   <td style="text-align:left;"> BES </td>
   <td style="text-align:left;"> ECOR </td>
   <td style="text-align:left;"> 1988-05-18 </td>
   <td style="text-align:right;"> 3.55 </td>
   <td style="text-align:left;"> 2018-10-25 </td>
   <td style="text-align:right;"> 26.57 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 26.57 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1470 </td>
   <td style="text-align:left;"> 2009-03-02 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 6174 </td>
   <td style="text-align:right;"> 882.00 </td>
   <td style="text-align:right;"> 202.98 </td>
   <td style="text-align:right;"> 202 </td>
   <td style="text-align:right;"> 16.92 </td>
   <td style="text-align:right;"> -50 </td>
   <td style="text-align:right;"> 111 </td>
   <td style="text-align:right;"> -0.45 </td>
   <td style="text-align:right;"> 3524 </td>
   <td style="text-align:right;"> 1.70 </td>
   <td style="text-align:left;"> adult </td>
   <td style="text-align:left;"> NP </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
</tbody>
</table>

`````

:::
:::


## About Our Data

This data comes from the Duke Lemur Center. The Duke Lemur Center houses over 200 lemurs across 14 species. Lemurs are the most threatened group of mammals and are at risk of extinction. Lemurs are native to Madagascar which is located in the southwestern Indian Ocean. This data set contains taxonomic code, specimen ID, hybrid status, sex, name, DOB, birth month, birth type, birth institution, litter size, and many more interesting variables about lemurs. This is a very large data set containing

## Interesting Questions to Ask

This is a very large data set so the purpose of asking these questions is to find interesting hypotheses to ask. Exploring the data will make it easier to create hypotheses

-   How many Lemurs are there? By sex?
-   What is the average weight of a Lemur? What about for each taxon?
-   What is the average birth type for a Lemur? By sex? By Taxon?
-   Does average litter size change by birth type?

## Hypotheses

1.  If hybrid Lemurs are born then they are more likely to be captive-born rather than wild-born.

2.  If Lemurs are mating it will more likely be in April and then the infants will be born around August and September.

## Answering Our Questions

Here we will look at the number of Lemurs in the data set and separate them by sex. We will also see the Lemurs grouped by their name. It is evident that there are 41,305 Lemurs within this data set. Out of these, 20,179 are female, 21,117 are male, and 8 are not determined. In is evident that in the table and the graph, MMUR has the biggest species count, with a count of 6,127.


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  count(sex)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 3 × 2
  sex       n
  <chr> <int>
1 F     20280
2 M     21015
3 ND        9
```
:::

```{.r .cell-code}
exploratory_data %>%
  count(taxon)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 27 × 2
   taxon     n
   <chr> <int>
 1 CMED   4039
 2 DMAD   2521
 3 EALB    159
 4 ECOL   1240
 5 ECOR   1055
 6 EFLA   1600
 7 EFUL    177
 8 EMAC    868
 9 EMON   1773
10 ERUB    666
# … with 17 more rows
```
:::

```{.r .cell-code}
exploratory_data %>%
  count(name)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 1,996 × 2
   name         n
   <chr>    <int>
 1 AARON        2
 2 ABAS         5
 3 ABDUL        1
 4 ABEDNIGO    12
 5 ABEL         2
 6 ABENA       54
 7 ABIGAIL      1
 8 ABNER        1
 9 ABSINTHE     2
10 Abu        134
# … with 1,986 more rows
```
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(name) %>%
  count(sex)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 1,998 × 3
# Groups:   name [1,996]
   name     sex       n
   <chr>    <chr> <int>
 1 AARON    M         2
 2 ABAS     M         5
 3 ABDUL    M         1
 4 ABEDNIGO M        12
 5 ABEL     M         2
 6 ABENA    F        54
 7 ABIGAIL  F         1
 8 ABNER    M         1
 9 ABSINTHE M         2
10 Abu      F       134
# … with 1,988 more rows
```
:::
:::

::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  ggplot() +
  geom_bar(mapping = aes(x = taxon), color = "purple", fill = "pink") +
  labs(title ="Count of Lemurs Species", x = "Species", y = "Count") +
coord_flip()
```

::: {.cell-output-display}
![](Lemurs_files/figure-html/unnamed-chunk-7-1.png){width=672}
:::
:::


Here we will see the average weight of a Lemur. To narrow this even more, we will look at the average weight of a Lemur per taxon. After running this, it is evident that the average weight of a Lemur is 1,484.9 grams. This is about 3.3 pounds which is very light. The taxon MMUR (Gray Mouse Lemur) has the lightest weight of 74 grams or 0.16 pounds. The taxon EFUL (Common Brown Lemur) has the heaviest weight of 2,372 grams or about 5 pounds.


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  summarize(avg_lemur_weight = mean(weight_g))
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 1 × 1
  avg_lemur_weight
             <dbl>
1            1484.
```
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(taxon) %>%
  summarize(avg_lemur_weight = mean(weight_g))
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 27 × 2
   taxon avg_lemur_weight
   <chr>            <dbl>
 1 CMED              209.
 2 DMAD             2188.
 3 EALB             2055.
 4 ECOL             2280.
 5 ECOR             1483.
 6 EFLA             2131.
 7 EFUL             2321.
 8 EMAC             2193.
 9 EMON             1434.
10 ERUB             2121.
# … with 17 more rows
```
:::
:::


Using the exploratory data it is evident that a larger majority are captive born. 37,489 are captive born while 3,724 are wild born. This may be because Lemurs are becoming extinct so it is harder for them to be born in the wild with larger numbers. When looking at birth type through taxon, it is evident that there are very few taxon that are wild born. When looking at sex, there is not a large gap between the different birth types when looking at sex. These variables are split pretty evenly. The graph shows the amount of birth types and it is very evident that the bar for captive born surpasses the bar for wild born by a large gap.


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  count(birth_type)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 3 × 2
  birth_type     n
  <chr>      <int>
1 CB         37544
2 Unk           81
3 WB          3679
```
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(taxon) %>%
  count(birth_type)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 56 × 3
# Groups:   taxon [27]
   taxon birth_type     n
   <chr> <chr>      <int>
 1 CMED  CB          4035
 2 CMED  WB             4
 3 DMAD  CB          1563
 4 DMAD  WB           958
 5 EALB  CB           159
 6 ECOL  CB          1130
 7 ECOL  WB           110
 8 ECOR  CB          1009
 9 ECOR  Unk            8
10 ECOR  WB            38
# … with 46 more rows
```
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(sex) %>%
  count(birth_type)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 7 × 3
# Groups:   sex [3]
  sex   birth_type     n
  <chr> <chr>      <int>
1 F     CB         18283
2 F     Unk           51
3 F     WB          1946
4 M     CB         19252
5 M     Unk           30
6 M     WB          1733
7 ND    CB             9
```
:::

```{.r .cell-code}
exploratory_data %>%
  ggplot() +
  geom_bar(mapping = aes(x = birth_type), color = "hotpink", fill = "forestgreen") +
  labs(title ="Birth Types", x = "Birth Type", y = "Count")
```

::: {.cell-output-display}
![](Lemurs_files/figure-html/unnamed-chunk-9-1.png){width=672}
:::
:::


It is evident that the average litter size is 1 with a count of 17,296. As the litter size goes up to 2, 3, and 4 it becomes more unlikely. A litter size of 4 has a count of 925. This could be another reason why Lemurs are becoming extinct. Since the litter size is lower, there is a lower number of species being born which correlates with the reduced population as a whole. Looking at the data regarding litter size and birth type it is evident that there is no data regarding the litter size of wild born Lemurs. This makes sense because it would be harder and maybe even impossible to track the litter size of wild born Lemurs.


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  count(litter_size)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 5 × 2
  litter_size     n
        <dbl> <int>
1           1 17369
2           2  9688
3           3  4282
4           4   906
5          NA  9059
```
:::

```{.r .cell-code}
exploratory_data %>%
  ggplot() +
  geom_bar(mapping = aes(x = litter_size), color = "purple", fill = "white") +
  labs(title ="Litter Size", x = "Litter Size", y = "Count")
```

::: {.cell-output .cell-output-stderr}
```
Warning: Removed 9059 rows containing non-finite values (`stat_count()`).
```
:::

::: {.cell-output-display}
![](Lemurs_files/figure-html/unnamed-chunk-10-1.png){width=672}
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(birth_type) %>%
  count(litter_size)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 7 × 3
# Groups:   birth_type [3]
  birth_type litter_size     n
  <chr>            <dbl> <int>
1 CB                   1 17369
2 CB                   2  9688
3 CB                   3  4282
4 CB                   4   906
5 CB                  NA  5299
6 Unk                 NA    81
7 WB                  NA  3679
```
:::
:::


## Answering Our Hypothesis

1.  If hybrid Lemurs are born then they are more likely to be captive-born rather than wild-born.

Here, we will look at the number of hybrids per taxon and per sex. In total there are 992 hybrids which is a small amount compared to the 40,312 that are not a hybrid. The ggplot is used to really accentuate the difference bewteen these two variables. The taxon EUL has the most hybrids which should be true as this is known as the hybrid species. There are 624 male hybrids and 368 female hybrids. My hypothesis was accepted, If hybrid Lemurs are born then they are more likely to be captive-born.


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  count(hybrid)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 2 × 2
  hybrid     n
  <chr>  <int>
1 N      40315
2 Sp       989
```
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(taxon) %>%
  count(hybrid)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 28 × 3
# Groups:   taxon [27]
   taxon hybrid     n
   <chr> <chr>  <int>
 1 CMED  N       4039
 2 DMAD  N       2521
 3 EALB  N        159
 4 ECOL  N       1240
 5 ECOR  N       1055
 6 EFLA  N       1600
 7 EFUL  N        177
 8 EMAC  N        868
 9 EMON  N       1773
10 ERUB  N        666
# … with 18 more rows
```
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(sex) %>%
  count(hybrid)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 6 × 3
# Groups:   sex [3]
  sex   hybrid     n
  <chr> <chr>  <int>
1 F     N      19923
2 F     Sp       357
3 M     N      20384
4 M     Sp       631
5 ND    N          8
6 ND    Sp         1
```
:::

```{.r .cell-code}
exploratory_data %>%
  ggplot() +
  geom_bar(mapping = aes(x = hybrid), color = "black", fill = "lightblue") +
  labs(title ="Hybrid Count", x = "Hybrid (SP) vs. Not Hybrid (N)", y = "Count")
```

::: {.cell-output-display}
![](Lemurs_files/figure-html/unnamed-chunk-11-1.png){width=672}
:::
:::


1.  If Lemurs are mating it will more likely be in April and then the infants will be born around August and September.

According to this data, the conception month tends to be more around April, May, and June. Compared to the hypothesis of April, this wasn't too off. The data also revealed that the infants are born in March, April, and May. The hypothesis predicted, August and September and this was way off. To find reasoning for this, I found the average expected gestation which was about 119 days. This tells us the period of developing inside the womb between conception and birth. 119 days is about 4 months so if conception occurred in April then the baby would be born around August which supports my hypothesis but does not hold true for the data.


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  count(concep_month)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 13 × 2
   concep_month     n
          <dbl> <int>
 1            1  4928
 2            2  2208
 3            3  2052
 4            4  4035
 5            5  5153
 6            6  4223
 7            7  2150
 8            8  1787
 9            9  2023
10           10  2036
11           11  5909
12           12  4796
13           NA     4
```
:::

```{.r .cell-code}
exploratory_data %>%
  count(infant_dob_if_preg)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 494 × 2
   infant_dob_if_preg     n
   <date>             <int>
 1 1972-03-08             1
 2 1972-04-27             1
 3 1972-05-08             1
 4 1972-06-01             1
 5 1972-07-05             1
 6 1972-07-12             1
 7 1972-10-04             1
 8 1980-05-02             1
 9 1980-07-14             1
10 1980-07-20             1
# … with 484 more rows
```
:::

```{.r .cell-code}
exploratory_data %>%
  mutate(infant_dob_month = lubridate::month(infant_dob_if_preg)) %>%
  select(infant_dob_if_preg, infant_dob_month, everything()) %>%
  filter(!is.na(infant_dob_month)) %>%
  filter(!is.na(litter_size)) %>%
  group_by(infant_dob_month) %>%
  summarize(total_births = sum(litter_size))
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 12 × 2
   infant_dob_month total_births
              <dbl>        <dbl>
 1                1           78
 2                2           63
 3                3          164
 4                4           95
 5                5          100
 6                6           72
 7                7           63
 8                8           45
 9                9           16
10               10           23
11               11            5
12               12           32
```
:::

```{.r .cell-code}
exploratory_data %>%
  summarize(avg_expected_gestation = mean(expected_gestation))
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 1 × 1
  avg_expected_gestation
                   <dbl>
1                   119.
```
:::
:::


## Inference

Here I'll be using the "new" data which was unseen during the exploratory analysis seen earlier.

here are the hypotheses, I till be testing with this new data.

1.  If hybrid Lemurs are born then they are more likely to be captive-born rather than wild-born.

2.  If Lemurs are mating it will more likely be in April and then the infants will be born around August and September.

Using the new data, we will look at the number of hybrids per taxon and per sex. In total there are 1,034 hybrids which is a small amount compared to the 40,270 that are not a hybrid. The taxon EUL has the most hybrids which should be true as this is known as the hybrid species. There are 657 male hybrids and 377 female hybrids. My hypothesis was accepted, If hybrid Lemurs are born then they are more likely to be captive-born. Comparing this results to the exploratory data it is evident that the results have similar findings. These findings allow us to understand the data very well.


::: {.cell}

```{.r .cell-code}
test_data %>%
  count(hybrid)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 2 × 2
  hybrid     n
  <chr>  <int>
1 N      40290
2 Sp      1015
```
:::

```{.r .cell-code}
test_data %>%
  group_by(taxon) %>%
  count(hybrid)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 29 × 3
# Groups:   taxon [27]
   taxon hybrid     n
   <chr> <chr>  <int>
 1 CMED  N       4031
 2 DMAD  N       2556
 3 EALB  N        168
 4 ECOL  N       1227
 5 ECOR  N       1039
 6 EFLA  N       1617
 7 EFUL  N        151
 8 EMAC  N        866
 9 EMON  N       1872
10 ERUB  N        690
# … with 19 more rows
```
:::

```{.r .cell-code}
test_data %>%
  group_by(sex) %>%
  count(hybrid)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 5 × 3
# Groups:   sex [3]
  sex   hybrid     n
  <chr> <chr>  <int>
1 F     N      19915
2 F     Sp       362
3 M     N      20369
4 M     Sp       653
5 ND    N          6
```
:::

```{.r .cell-code}
test_data %>%
  ggplot() +
  geom_bar(mapping = aes(x = hybrid), color = "brown", fill = "orange") +
  labs(title ="Hybrid Count", x = "Hybrid (SP) vs. Not Hybrid (N)", y = "Count")
```

::: {.cell-output-display}
![](Lemurs_files/figure-html/unnamed-chunk-13-1.png){width=672}
:::
:::


According to this new data, the conception month still tends to be more around April, May, and June. Compared to the hypothesis of April, this wasn't too off. This new data also revealed that the infants are born in March, April, and May. The hypothesis predicted, August and September and this was way off. For this new data, the average expected gestation was also about 119 days. 119 days is about 4 months so if conception occurred in April then the baby would be born around August which supports my hypothesis but does not hold true for the data. These findings were exactly the same as the exploratory data findings. These findings allow us to understand the data very well.


::: {.cell}

```{.r .cell-code}
test_data %>%
  count(concep_month)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 13 × 2
   concep_month     n
          <dbl> <int>
 1            1  4921
 2            2  2273
 3            3  1954
 4            4  4039
 5            5  5149
 6            6  4161
 7            7  2126
 8            8  1764
 9            9  2105
10           10  2049
11           11  5967
12           12  4793
13           NA     4
```
:::

```{.r .cell-code}
test_data %>%
  count(infant_dob_if_preg)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 475 × 2
   infant_dob_if_preg     n
   <date>             <int>
 1 1972-04-29             1
 2 1972-05-12             1
 3 1972-07-05             1
 4 1972-07-10             1
 5 1972-07-20             1
 6 1980-05-07             1
 7 1980-05-08             1
 8 1980-07-20             1
 9 1981-05-03             1
10 1982-04-05             2
# … with 465 more rows
```
:::

```{.r .cell-code}
test_data %>%
  mutate(infant_dob_month = lubridate::month(infant_dob_if_preg)) %>%
  select(infant_dob_if_preg, infant_dob_month, everything()) %>%
  filter(!is.na(infant_dob_month)) %>%
  filter(!is.na(litter_size)) %>%
  group_by(infant_dob_month) %>%
  summarize(total_births = sum(litter_size))
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 12 × 2
   infant_dob_month total_births
              <dbl>        <dbl>
 1                1           64
 2                2           72
 3                3          172
 4                4          121
 5                5           95
 6                6           74
 7                7           59
 8                8           33
 9                9           14
10               10           19
11               11            8
12               12           23
```
:::

```{.r .cell-code}
test_data %>%
  summarize(avg_expected_gestation = mean(expected_gestation))
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 1 × 1
  avg_expected_gestation
                   <dbl>
1                   120.
```
:::
:::
