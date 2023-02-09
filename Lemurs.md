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
• Use suppressPackageStartupMessages() to eliminate package startup messages
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
   <td style="text-align:right;"> 1986 </td>
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
   <td style="text-align:right;"> 1966 </td>
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
   <td style="text-align:right;"> 1354 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1152 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> estimated_dob </td>
   <td style="text-align:right;"> 36061 </td>
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
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 85 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_id </td>
   <td style="text-align:right;"> 531 </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 547 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_name </td>
   <td style="text-align:right;"> 7573 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 469 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_taxon </td>
   <td style="text-align:right;"> 7573 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_id </td>
   <td style="text-align:right;"> 863 </td>
   <td style="text-align:right;"> 0.98 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 473 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_name </td>
   <td style="text-align:right;"> 12341 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 399 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_taxon </td>
   <td style="text-align:right;"> 12341 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dob_estimated </td>
   <td style="text-align:right;"> 36061 </td>
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
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 1946-10-01 </td>
   <td style="text-align:left;"> 2018-07-24 </td>
   <td style="text-align:left;"> 1994-03-20 </td>
   <td style="text-align:right;"> 1386 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> estimated_concep </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 1946-06-03 </td>
   <td style="text-align:left;"> 2018-05-22 </td>
   <td style="text-align:left;"> 1993-11-01 </td>
   <td style="text-align:right;"> 1422 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_dob </td>
   <td style="text-align:right;"> 7573 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:left;"> 1958-10-01 </td>
   <td style="text-align:left;"> 2015-01-08 </td>
   <td style="text-align:left;"> 1986-06-26 </td>
   <td style="text-align:right;"> 414 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_dob </td>
   <td style="text-align:right;"> 12341 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:left;"> 1946-10-01 </td>
   <td style="text-align:left;"> 2014-08-07 </td>
   <td style="text-align:left;"> 1985-11-29 </td>
   <td style="text-align:right;"> 360 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dod </td>
   <td style="text-align:right;"> 18753 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:left;"> 1971-11-17 </td>
   <td style="text-align:left;"> 2019-01-15 </td>
   <td style="text-align:left;"> 2008-11-13 </td>
   <td style="text-align:right;"> 1181 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> weight_date </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 1970-06-11 </td>
   <td style="text-align:left;"> 2019-02-05 </td>
   <td style="text-align:left;"> 2005-12-06 </td>
   <td style="text-align:right;"> 8770 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> concep_date_if_preg </td>
   <td style="text-align:right;"> 40413 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:left;"> 1971-12-29 </td>
   <td style="text-align:left;"> 2018-05-22 </td>
   <td style="text-align:left;"> 2003-09-09 </td>
   <td style="text-align:right;"> 481 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> infant_dob_if_preg </td>
   <td style="text-align:right;"> 40413 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:left;"> 1972-04-27 </td>
   <td style="text-align:left;"> 2018-07-24 </td>
   <td style="text-align:left;"> 2004-02-16 </td>
   <td style="text-align:right;"> 466 </td>
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
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 5.57 </td>
   <td style="text-align:right;"> 2.70 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:right;"> 5.00 </td>
   <td style="text-align:right;"> 7.00 </td>
   <td style="text-align:right;"> 12.00 </td>
   <td style="text-align:left;"> ▇▇▇▂▃ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> litter_size </td>
   <td style="text-align:right;"> 9008 </td>
   <td style="text-align:right;"> 0.78 </td>
   <td style="text-align:right;"> 1.65 </td>
   <td style="text-align:right;"> 0.82 </td>
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
   <td style="text-align:right;"> 119.66 </td>
   <td style="text-align:right;"> 39.64 </td>
   <td style="text-align:right;"> 62.00 </td>
   <td style="text-align:right;"> 90.00 </td>
   <td style="text-align:right;"> 124.00 </td>
   <td style="text-align:right;"> 160.00 </td>
   <td style="text-align:right;"> 193.00 </td>
   <td style="text-align:left;"> ▆▃▇▅▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> concep_month </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6.64 </td>
   <td style="text-align:right;"> 3.70 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:right;"> 6.00 </td>
   <td style="text-align:right;"> 11.00 </td>
   <td style="text-align:right;"> 12.00 </td>
   <td style="text-align:left;"> ▆▆▃▂▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_age_at_concep_y </td>
   <td style="text-align:right;"> 7578 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:right;"> 7.18 </td>
   <td style="text-align:right;"> 4.62 </td>
   <td style="text-align:right;"> 0.59 </td>
   <td style="text-align:right;"> 3.63 </td>
   <td style="text-align:right;"> 6.42 </td>
   <td style="text-align:right;"> 10.16 </td>
   <td style="text-align:right;"> 26.03 </td>
   <td style="text-align:left;"> ▇▆▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_age_at_concep_y </td>
   <td style="text-align:right;"> 12346 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 9.13 </td>
   <td style="text-align:right;"> 6.25 </td>
   <td style="text-align:right;"> 0.61 </td>
   <td style="text-align:right;"> 4.65 </td>
   <td style="text-align:right;"> 7.48 </td>
   <td style="text-align:right;"> 12.44 </td>
   <td style="text-align:right;"> 33.36 </td>
   <td style="text-align:left;"> ▇▅▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_death_y </td>
   <td style="text-align:right;"> 18758 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 17.51 </td>
   <td style="text-align:right;"> 8.44 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 11.13 </td>
   <td style="text-align:right;"> 17.36 </td>
   <td style="text-align:right;"> 23.42 </td>
   <td style="text-align:right;"> 39.39 </td>
   <td style="text-align:left;"> ▃▇▇▅▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_of_living_y </td>
   <td style="text-align:right;"> 26748 </td>
   <td style="text-align:right;"> 0.35 </td>
   <td style="text-align:right;"> 13.50 </td>
   <td style="text-align:right;"> 8.89 </td>
   <td style="text-align:right;"> 0.54 </td>
   <td style="text-align:right;"> 6.60 </td>
   <td style="text-align:right;"> 10.65 </td>
   <td style="text-align:right;"> 19.81 </td>
   <td style="text-align:right;"> 35.20 </td>
   <td style="text-align:left;"> ▇▇▃▃▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_last_verified_y </td>
   <td style="text-align:right;"> 37107 </td>
   <td style="text-align:right;"> 0.10 </td>
   <td style="text-align:right;"> 12.80 </td>
   <td style="text-align:right;"> 8.14 </td>
   <td style="text-align:right;"> 0.36 </td>
   <td style="text-align:right;"> 6.74 </td>
   <td style="text-align:right;"> 10.52 </td>
   <td style="text-align:right;"> 18.73 </td>
   <td style="text-align:right;"> 34.26 </td>
   <td style="text-align:left;"> ▆▇▃▃▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_max_live_or_dead_y </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 15.61 </td>
   <td style="text-align:right;"> 8.82 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 7.81 </td>
   <td style="text-align:right;"> 14.53 </td>
   <td style="text-align:right;"> 22.54 </td>
   <td style="text-align:right;"> 39.39 </td>
   <td style="text-align:left;"> ▇▇▆▅▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> n_known_offspring </td>
   <td style="text-align:right;"> 18393 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 5.59 </td>
   <td style="text-align:right;"> 4.76 </td>
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
   <td style="text-align:right;"> 1486.43 </td>
   <td style="text-align:right;"> 1314.23 </td>
   <td style="text-align:right;"> 4.74 </td>
   <td style="text-align:right;"> 200.00 </td>
   <td style="text-align:right;"> 1310.00 </td>
   <td style="text-align:right;"> 2480.00 </td>
   <td style="text-align:right;"> 10337.00 </td>
   <td style="text-align:left;"> ▇▅▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> month_of_weight </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6.51 </td>
   <td style="text-align:right;"> 3.41 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:right;"> 7.00 </td>
   <td style="text-align:right;"> 9.00 </td>
   <td style="text-align:right;"> 12.00 </td>
   <td style="text-align:left;"> ▇▆▆▆▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_d </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3100.58 </td>
   <td style="text-align:right;"> 2817.17 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 770.00 </td>
   <td style="text-align:right;"> 2312.00 </td>
   <td style="text-align:right;"> 4805.00 </td>
   <td style="text-align:right;"> 14319.00 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_wk </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 442.94 </td>
   <td style="text-align:right;"> 402.45 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 110.00 </td>
   <td style="text-align:right;"> 330.29 </td>
   <td style="text-align:right;"> 686.43 </td>
   <td style="text-align:right;"> 2045.57 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_mo </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 101.94 </td>
   <td style="text-align:right;"> 92.62 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 25.32 </td>
   <td style="text-align:right;"> 76.01 </td>
   <td style="text-align:right;"> 157.97 </td>
   <td style="text-align:right;"> 470.76 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_mo_no_dec </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 101.45 </td>
   <td style="text-align:right;"> 92.61 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 25.00 </td>
   <td style="text-align:right;"> 76.00 </td>
   <td style="text-align:right;"> 157.00 </td>
   <td style="text-align:right;"> 470.00 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_y </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 8.49 </td>
   <td style="text-align:right;"> 7.72 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 2.11 </td>
   <td style="text-align:right;"> 6.33 </td>
   <td style="text-align:right;"> 13.16 </td>
   <td style="text-align:right;"> 39.23 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> change_since_prev_wt_g </td>
   <td style="text-align:right;"> 1117 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 19.49 </td>
   <td style="text-align:right;"> 173.62 </td>
   <td style="text-align:right;"> -1760.00 </td>
   <td style="text-align:right;"> -20.00 </td>
   <td style="text-align:right;"> 2.00 </td>
   <td style="text-align:right;"> 50.00 </td>
   <td style="text-align:right;"> 3701.00 </td>
   <td style="text-align:left;"> ▁▇▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> days_since_prev_wt </td>
   <td style="text-align:right;"> 1117 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 55.35 </td>
   <td style="text-align:right;"> 149.85 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 13.00 </td>
   <td style="text-align:right;"> 28.00 </td>
   <td style="text-align:right;"> 55.00 </td>
   <td style="text-align:right;"> 6394.00 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> avg_daily_wt_change_g </td>
   <td style="text-align:right;"> 1220 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 0.58 </td>
   <td style="text-align:right;"> 11.86 </td>
   <td style="text-align:right;"> -920.00 </td>
   <td style="text-align:right;"> -0.64 </td>
   <td style="text-align:right;"> 0.13 </td>
   <td style="text-align:right;"> 1.82 </td>
   <td style="text-align:right;"> 271.67 </td>
   <td style="text-align:left;"> ▁▁▁▇▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> days_before_death </td>
   <td style="text-align:right;"> 18753 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 2698.99 </td>
   <td style="text-align:right;"> 2274.51 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 868.00 </td>
   <td style="text-align:right;"> 2114.00 </td>
   <td style="text-align:right;"> 4048.00 </td>
   <td style="text-align:right;"> 13051.00 </td>
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
   <td style="text-align:right;"> 40413 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 140.67 </td>
   <td style="text-align:right;"> 33.57 </td>
   <td style="text-align:right;"> 62.00 </td>
   <td style="text-align:right;"> 124.00 </td>
   <td style="text-align:right;"> 145.00 </td>
   <td style="text-align:right;"> 160.00 </td>
   <td style="text-align:right;"> 193.00 </td>
   <td style="text-align:left;"> ▂▂▇▇▅ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> days_before_inf_birth_if_preg </td>
   <td style="text-align:right;"> 40413 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 66.27 </td>
   <td style="text-align:right;"> 46.15 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 26.00 </td>
   <td style="text-align:right;"> 59.00 </td>
   <td style="text-align:right;"> 102.00 </td>
   <td style="text-align:right;"> 192.00 </td>
   <td style="text-align:left;"> ▇▆▅▃▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pct_preg_remain_if_preg </td>
   <td style="text-align:right;"> 40413 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.47 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.21 </td>
   <td style="text-align:right;"> 0.46 </td>
   <td style="text-align:right;"> 0.74 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> ▇▇▆▆▆ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> infant_lit_sz_if_preg </td>
   <td style="text-align:right;"> 40417 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 1.29 </td>
   <td style="text-align:right;"> 0.57 </td>
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
   <td style="text-align:left;"> HGG </td>
   <td style="text-align:left;"> 1366 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> Beavis </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:left;"> T10061 </td>
   <td style="text-align:left;"> 1997-04-08 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 145 </td>
   <td style="text-align:left;"> 1996-11-14 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> 1357 </td>
   <td style="text-align:left;"> Beware </td>
   <td style="text-align:left;"> HGG </td>
   <td style="text-align:left;"> 1995-05-06 </td>
   <td style="text-align:right;"> 1.53 </td>
   <td style="text-align:left;"> 1324 </td>
   <td style="text-align:left;"> BETSIK </td>
   <td style="text-align:left;"> HGG </td>
   <td style="text-align:left;"> 1984-12-01 </td>
   <td style="text-align:right;"> 11.96 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 21.85 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 21.85 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1020 </td>
   <td style="text-align:left;"> 2004-09-14 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 2716 </td>
   <td style="text-align:right;"> 388.00 </td>
   <td style="text-align:right;"> 89.29 </td>
   <td style="text-align:right;"> 89 </td>
   <td style="text-align:right;"> 7.44 </td>
   <td style="text-align:right;"> -50 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> -10.00 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 1.53 </td>
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
   <td style="text-align:left;"> LCAT </td>
   <td style="text-align:left;"> 6440 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> Aracus </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:left;"> 2219 </td>
   <td style="text-align:left;"> 1991-05-23 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 138 </td>
   <td style="text-align:left;"> 1991-01-05 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> 5847 </td>
   <td style="text-align:left;"> CORINNA </td>
   <td style="text-align:left;"> LCAT </td>
   <td style="text-align:left;"> 1984-03-16 </td>
   <td style="text-align:right;"> 6.81 </td>
   <td style="text-align:left;"> MULT </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 27.73 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 27.73 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 2380 </td>
   <td style="text-align:left;"> 2005-01-10 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 4981 </td>
   <td style="text-align:right;"> 711.57 </td>
   <td style="text-align:right;"> 163.76 </td>
   <td style="text-align:right;"> 163 </td>
   <td style="text-align:right;"> 13.65 </td>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:right;"> 108 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 1.32 </td>
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
   <td style="text-align:left;"> 2521 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> ADONIS </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 72 </td>
   <td style="text-align:left;"> 1975-05-13 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 104 </td>
   <td style="text-align:left;"> 1975-01-29 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> 569 </td>
   <td style="text-align:left;"> THALIA </td>
   <td style="text-align:left;"> VVV </td>
   <td style="text-align:left;"> 1965-10-01 </td>
   <td style="text-align:right;"> 9.33 </td>
   <td style="text-align:left;"> 1501 </td>
   <td style="text-align:left;"> MERCURY </td>
   <td style="text-align:left;"> VVV </td>
   <td style="text-align:left;"> 1966-10-01 </td>
   <td style="text-align:right;"> 8.33 </td>
   <td style="text-align:left;"> 1999-12-11 </td>
   <td style="text-align:right;"> 24.60 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 24.60 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1975-05-16 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> 0.10 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 8975 </td>
   <td style="text-align:right;"> 1.59 </td>
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
   <td style="text-align:left;"> DMAD </td>
   <td style="text-align:left;"> 6202 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> Poe </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:left;"> 104 </td>
   <td style="text-align:left;"> 1986-12-19 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:left;"> WB </td>
   <td style="text-align:left;"> Madagascar / </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:left;"> 1986-07-07 </td>
   <td style="text-align:right;"> 7 </td>
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
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 32.16 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 32.16 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:right;"> 2700 </td>
   <td style="text-align:left;"> 2007-03-02 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 7378 </td>
   <td style="text-align:right;"> 1054.00 </td>
   <td style="text-align:right;"> 242.56 </td>
   <td style="text-align:right;"> 242 </td>
   <td style="text-align:right;"> 20.21 </td>
   <td style="text-align:right;"> -75 </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> -2.68 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 4.22 </td>
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
   <td style="text-align:left;"> MMUR </td>
   <td style="text-align:left;"> 7038 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> Hurtleberry </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:left;"> 724 </td>
   <td style="text-align:left;"> 2010-06-10 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 63 </td>
   <td style="text-align:left;"> 2010-04-08 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> 7028 </td>
   <td style="text-align:left;"> Calendula </td>
   <td style="text-align:left;"> MMUR </td>
   <td style="text-align:left;"> 2008-05-25 </td>
   <td style="text-align:right;"> 1.87 </td>
   <td style="text-align:left;"> 7032 </td>
   <td style="text-align:left;"> Pesto </td>
   <td style="text-align:left;"> MMUR </td>
   <td style="text-align:left;"> 2008-06-04 </td>
   <td style="text-align:right;"> 1.84 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 8.67 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 8.67 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 64 </td>
   <td style="text-align:left;"> 2016-03-22 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 2112 </td>
   <td style="text-align:right;"> 301.71 </td>
   <td style="text-align:right;"> 69.44 </td>
   <td style="text-align:right;"> 69 </td>
   <td style="text-align:right;"> 5.79 </td>
   <td style="text-align:right;"> -9 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> -0.60 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 0.59 </td>
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
   <td style="text-align:left;"> PCOQ </td>
   <td style="text-align:left;"> 6583 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> Jovian </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 160 </td>
   <td style="text-align:left;"> 1994-04-10 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:left;"> 1993-11-01 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> 6108 </td>
   <td style="text-align:left;"> FLAVIA </td>
   <td style="text-align:left;"> PCOQ </td>
   <td style="text-align:left;"> 1982-07-01 </td>
   <td style="text-align:right;"> 11.35 </td>
   <td style="text-align:left;"> 597 </td>
   <td style="text-align:left;"> NIGEL </td>
   <td style="text-align:left;"> PCOQ </td>
   <td style="text-align:left;"> 1972-02-10 </td>
   <td style="text-align:right;"> 21.74 </td>
   <td style="text-align:left;"> 2014-11-10 </td>
   <td style="text-align:right;"> 20.60 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 20.60 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 3880 </td>
   <td style="text-align:left;"> 2011-05-15 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 6244 </td>
   <td style="text-align:right;"> 892.00 </td>
   <td style="text-align:right;"> 205.28 </td>
   <td style="text-align:right;"> 205 </td>
   <td style="text-align:right;"> 17.11 </td>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 20.00 </td>
   <td style="text-align:right;"> 1275 </td>
   <td style="text-align:right;"> 2.64 </td>
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
   <td style="text-align:left;"> DMAD </td>
   <td style="text-align:left;"> 6820 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> Sabrina </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 168 </td>
   <td style="text-align:left;"> 2003-09-26 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:left;"> 2003-04-14 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> 6454 </td>
   <td style="text-align:left;"> Morticia </td>
   <td style="text-align:left;"> DMAD </td>
   <td style="text-align:left;"> 1988-11-30 </td>
   <td style="text-align:right;"> 14.38 </td>
   <td style="text-align:left;"> 6202 </td>
   <td style="text-align:left;"> Poe </td>
   <td style="text-align:left;"> DMAD </td>
   <td style="text-align:left;"> 1986-12-19 </td>
   <td style="text-align:right;"> 16.33 </td>
   <td style="text-align:left;"> 2016-03-29 </td>
   <td style="text-align:right;"> 12.52 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 12.52 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1620 </td>
   <td style="text-align:left;"> 2004-10-12 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 382 </td>
   <td style="text-align:right;"> 54.57 </td>
   <td style="text-align:right;"> 12.56 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 1.05 </td>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 1.38 </td>
   <td style="text-align:right;"> 4186 </td>
   <td style="text-align:right;"> 4.22 </td>
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
   <td style="text-align:left;"> PCOQ </td>
   <td style="text-align:left;"> 6757 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> ZENO </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 187 </td>
   <td style="text-align:left;"> 2000-02-02 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:left;"> 1999-08-26 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> 6398 </td>
   <td style="text-align:left;"> PAULINA </td>
   <td style="text-align:left;"> PCOQ </td>
   <td style="text-align:left;"> 1991-01-24 </td>
   <td style="text-align:right;"> 8.59 </td>
   <td style="text-align:left;"> 6610 </td>
   <td style="text-align:left;"> NERO </td>
   <td style="text-align:left;"> PCOQ </td>
   <td style="text-align:left;"> 1994-12-04 </td>
   <td style="text-align:right;"> 4.73 </td>
   <td style="text-align:left;"> 2011-10-15 </td>
   <td style="text-align:right;"> 11.71 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 11.71 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 3700 </td>
   <td style="text-align:left;"> 2002-05-01 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 819 </td>
   <td style="text-align:right;"> 117.00 </td>
   <td style="text-align:right;"> 26.93 </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:right;"> 2.24 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 72 </td>
   <td style="text-align:right;"> 1.94 </td>
   <td style="text-align:right;"> 3454 </td>
   <td style="text-align:right;"> 2.64 </td>
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
   <td style="text-align:left;"> LCAT </td>
   <td style="text-align:left;"> 6143 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> NEMO </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 1673 </td>
   <td style="text-align:left;"> 1987-03-23 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 138 </td>
   <td style="text-align:left;"> 1986-11-05 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> 5510 </td>
   <td style="text-align:left;"> ANEMONE </td>
   <td style="text-align:left;"> LCAT </td>
   <td style="text-align:left;"> 1980-03-14 </td>
   <td style="text-align:right;"> 6.65 </td>
   <td style="text-align:left;"> MULT </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> 2010-10-17 </td>
   <td style="text-align:right;"> 23.59 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 23.59 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 3280 </td>
   <td style="text-align:left;"> 2008-10-16 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 7878 </td>
   <td style="text-align:right;"> 1125.43 </td>
   <td style="text-align:right;"> 259.00 </td>
   <td style="text-align:right;"> 259 </td>
   <td style="text-align:right;"> 21.58 </td>
   <td style="text-align:right;"> -60 </td>
   <td style="text-align:right;"> 55 </td>
   <td style="text-align:right;"> -1.09 </td>
   <td style="text-align:right;"> 731 </td>
   <td style="text-align:right;"> 1.32 </td>
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
   <td style="text-align:left;"> LCAT </td>
   <td style="text-align:left;"> 6837 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> Ivy </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 3288 </td>
   <td style="text-align:left;"> 2004-05-04 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 138 </td>
   <td style="text-align:left;"> 2003-12-18 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> 5984 </td>
   <td style="text-align:left;"> CLEIS </td>
   <td style="text-align:left;"> LCAT </td>
   <td style="text-align:left;"> 1985-04-09 </td>
   <td style="text-align:right;"> 18.70 </td>
   <td style="text-align:left;"> 6440 </td>
   <td style="text-align:left;"> Aracus </td>
   <td style="text-align:left;"> LCAT </td>
   <td style="text-align:left;"> 1991-05-23 </td>
   <td style="text-align:right;"> 12.58 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 14.45 </td>
   <td style="text-align:right;"> 14.45 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 2780 </td>
   <td style="text-align:left;"> 2011-02-05 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 2468 </td>
   <td style="text-align:right;"> 352.57 </td>
   <td style="text-align:right;"> 81.14 </td>
   <td style="text-align:right;"> 81 </td>
   <td style="text-align:right;"> 6.76 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 8.00 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 1.32 </td>
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
1 F     20386
2 M     20910
3 ND        8
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
 1 CMED   4041
 2 DMAD   2481
 3 EALB    147
 4 ECOL   1224
 5 ECOR   1056
 6 EFLA   1613
 7 EFUL    180
 8 EMAC    831
 9 EMON   1805
10 ERUB    716
# … with 17 more rows
```
:::

```{.r .cell-code}
exploratory_data %>%
  count(name)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 1,966 × 2
   name         n
   <chr>    <int>
 1 AARON        1
 2 ABAS         8
 3 ABDUL        1
 4 ABEDNIGO    11
 5 ABEL         2
 6 ABENA       49
 7 ABNER        1
 8 ABSINTHE     1
 9 Abu        129
10 ACHERNAR     1
# … with 1,956 more rows
```
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(name) %>%
  count(sex)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 1,968 × 3
# Groups:   name [1,966]
   name     sex       n
   <chr>    <chr> <int>
 1 AARON    M         1
 2 ABAS     M         8
 3 ABDUL    M         1
 4 ABEDNIGO M        11
 5 ABEL     M         2
 6 ABENA    F        49
 7 ABNER    M         1
 8 ABSINTHE M         1
 9 Abu      F       129
10 ACHERNAR M         1
# … with 1,958 more rows
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
1            1486.
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
 1 CMED              208.
 2 DMAD             2184.
 3 EALB             2074.
 4 ECOL             2298.
 5 ECOR             1485.
 6 EFLA             2154.
 7 EFUL             2262.
 8 EMAC             2218.
 9 EMON             1432.
10 ERUB             2106.
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
1 CB         37551
2 Unk           85
3 WB          3668
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
 1 CMED  CB          4039
 2 CMED  WB             2
 3 DMAD  CB          1555
 4 DMAD  WB           926
 5 EALB  CB           147
 6 ECOL  CB          1118
 7 ECOL  WB           106
 8 ECOR  CB          1004
 9 ECOR  Unk           10
10 ECOR  WB            42
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
1 F     CB         18376
2 F     Unk           58
3 F     WB          1952
4 M     CB         19167
5 M     Unk           27
6 M     WB          1716
7 ND    CB             8
```
:::

```{.r .cell-code}
exploratory_data %>%
  ggplot() +
  geom_bar(mapping = aes(x = birth_type), color = "blue", fill = "black") +
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
1           1 17391
2           2  9690
3           3  4266
4           4   949
5          NA  9008
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
Warning: Removed 9008 rows containing non-finite values (`stat_count()`).
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
1 CB                   1 17391
2 CB                   2  9690
3 CB                   3  4266
4 CB                   4   949
5 CB                  NA  5255
6 Unk                 NA    85
7 WB                  NA  3668
```
:::
:::


## Answering Our Hypothesis

1.  If hybrid Lemurs are born then they are more likely to be captive-born rather than wild-born.

Here, we will look at the number of hybrids per taxon and per sex. In total there are 1,034 hybrids which is a small amount compared to the 40,270 that are not a hybrid. The taxon EUL has the most hybrids which should be true as this is known as the hybrid species. There are 657 male hybrids and 377 female hybrids. My hypothesis was accepted, If hybrid Lemurs are born then they are more likley to be captive-born.


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
1 N      40300
2 Sp      1004
```
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(taxon) %>%
  count(hybrid)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 29 × 3
# Groups:   taxon [27]
   taxon hybrid     n
   <chr> <chr>  <int>
 1 CMED  N       4041
 2 DMAD  N       2481
 3 EALB  N        147
 4 ECOL  N       1224
 5 ECOR  N       1056
 6 EFLA  N       1613
 7 EFUL  N        180
 8 EMAC  N        831
 9 EMON  N       1805
10 ERUB  N        716
# … with 19 more rows
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
1 F     N      20038
2 F     Sp       348
3 M     N      20255
4 M     Sp       655
5 ND    N          7
6 ND    Sp         1
```
:::
:::


1.  If Lemurs are mating it will more likely be in April and then the infants will be born around August and September.

According to this data, the conception month tends to be more around April, May, and June.


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
 1            1  4991
 2            2  2182
 3            3  1993
 4            4  3985
 5            5  5108
 6            6  4171
 7            7  2123
 8            8  1780
 9            9  2068
10           10  2119
11           11  5929
12           12  4850
13           NA     5
```
:::

```{.r .cell-code}
exploratory_data %>%
  count(infant_dob_if_preg)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 467 × 2
   infant_dob_if_preg     n
   <date>             <int>
 1 1972-04-27             1
 2 1972-04-29             1
 3 1972-07-05             1
 4 1972-07-10             1
 5 1972-07-20             1
 6 1972-10-04             1
 7 1980-05-08             1
 8 1980-07-28             1
 9 1981-05-03             1
10 1983-08-21             1
# … with 457 more rows
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
 1                1           56
 2                2           65
 3                3          166
 4                4           98
 5                5           95
 6                6           72
 7                7           62
 8                8           33
 9                9           14
10               10           21
11               11            9
12               12           25
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
1                   120.
```
:::
:::


## Inference

Here I'll be using the "new" data which was unseen during the exploratory analysis seen earlier.

here are the hypotheses, I till be testing with this new data.

1.  If hybrid Lemurs are born then they are more likely to be captive-born rather than wild-born.

2.  If Lemurs are mating it will more likely be in April and then the infants will be born around August and September.


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
1 N      40305
2 Sp      1000
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
 1 CMED  N       4029
 2 DMAD  N       2596
 3 EALB  N        180
 4 ECOL  N       1243
 5 ECOR  N       1038
 6 EFLA  N       1604
 7 EFUL  N        148
 8 EMAC  N        903
 9 EMON  N       1840
10 ERUB  N        640
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
1 F     N      19800
2 F     Sp       371
3 M     N      20498
4 M     Sp       629
5 ND    N          7
```
:::
:::

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
 1            1  4858
 2            2  2299
 3            3  2013
 4            4  4089
 5            5  5194
 6            6  4213
 7            7  2153
 8            8  1771
 9            9  2060
10           10  1966
11           11  5947
12           12  4739
13           NA     3
```
:::

```{.r .cell-code}
test_data %>%
  count(infant_dob_if_preg)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 506 × 2
   infant_dob_if_preg     n
   <date>             <int>
 1 1972-03-08             1
 2 1972-05-08             1
 3 1972-05-12             1
 4 1972-06-01             1
 5 1972-07-05             1
 6 1972-07-12             1
 7 1980-05-02             1
 8 1980-05-07             1
 9 1980-07-14             1
10 1980-07-20             2
# … with 496 more rows
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
 1                1           86
 2                2           70
 3                3          170
 4                4          118
 5                5          100
 6                6           74
 7                7           60
 8                8           45
 9                9           16
10               10           21
11               11            4
12               12           30
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
1                   119.
```
:::
:::
