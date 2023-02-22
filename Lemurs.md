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
• Search for functions across packages at https://www.tidymodels.org/find/
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
   <td style="text-align:right;"> 2012 </td>
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
   <td style="text-align:right;"> 1991 </td>
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
   <td style="text-align:right;"> 1389 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1145 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> estimated_dob </td>
   <td style="text-align:right;"> 36008 </td>
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
   <td style="text-align:right;"> 83 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_id </td>
   <td style="text-align:right;"> 521 </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 545 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_name </td>
   <td style="text-align:right;"> 7622 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 466 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_taxon </td>
   <td style="text-align:right;"> 7622 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_id </td>
   <td style="text-align:right;"> 865 </td>
   <td style="text-align:right;"> 0.98 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 471 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_name </td>
   <td style="text-align:right;"> 12431 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 395 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_taxon </td>
   <td style="text-align:right;"> 12431 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dob_estimated </td>
   <td style="text-align:right;"> 36008 </td>
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
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 1958-10-01 </td>
   <td style="text-align:left;"> 2018-07-24 </td>
   <td style="text-align:left;"> 1994-03-20 </td>
   <td style="text-align:right;"> 1403 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> estimated_concep </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 1958-06-03 </td>
   <td style="text-align:left;"> 2018-05-22 </td>
   <td style="text-align:left;"> 1993-10-26 </td>
   <td style="text-align:right;"> 1434 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_dob </td>
   <td style="text-align:right;"> 7622 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:left;"> 1958-10-01 </td>
   <td style="text-align:left;"> 2014-09-13 </td>
   <td style="text-align:left;"> 1986-06-26 </td>
   <td style="text-align:right;"> 409 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_dob </td>
   <td style="text-align:right;"> 12431 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:left;"> 1946-10-01 </td>
   <td style="text-align:left;"> 2014-08-07 </td>
   <td style="text-align:left;"> 1985-11-29 </td>
   <td style="text-align:right;"> 358 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dod </td>
   <td style="text-align:right;"> 18698 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:left;"> 1969-06-02 </td>
   <td style="text-align:left;"> 2019-01-15 </td>
   <td style="text-align:left;"> 2008-12-02 </td>
   <td style="text-align:right;"> 1192 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> weight_date </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 1968-09-16 </td>
   <td style="text-align:left;"> 2019-02-05 </td>
   <td style="text-align:left;"> 2005-12-19 </td>
   <td style="text-align:right;"> 8848 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> concep_date_if_preg </td>
   <td style="text-align:right;"> 40358 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:left;"> 1971-11-09 </td>
   <td style="text-align:left;"> 2018-05-22 </td>
   <td style="text-align:left;"> 2003-09-09 </td>
   <td style="text-align:right;"> 497 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> infant_dob_if_preg </td>
   <td style="text-align:right;"> 40358 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:left;"> 1972-03-08 </td>
   <td style="text-align:left;"> 2018-07-24 </td>
   <td style="text-align:left;"> 2004-02-16 </td>
   <td style="text-align:right;"> 489 </td>
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
   <td style="text-align:right;"> 3 </td>
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
   <td style="text-align:right;"> 9048 </td>
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
   <td style="text-align:right;"> 119.58 </td>
   <td style="text-align:right;"> 39.73 </td>
   <td style="text-align:right;"> 62.00 </td>
   <td style="text-align:right;"> 90.00 </td>
   <td style="text-align:right;"> 124.00 </td>
   <td style="text-align:right;"> 160.00 </td>
   <td style="text-align:right;"> 193.00 </td>
   <td style="text-align:left;"> ▆▃▇▅▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> concep_month </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6.63 </td>
   <td style="text-align:right;"> 3.69 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:right;"> 6.00 </td>
   <td style="text-align:right;"> 11.00 </td>
   <td style="text-align:right;"> 12.00 </td>
   <td style="text-align:left;"> ▆▆▃▂▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dam_age_at_concep_y </td>
   <td style="text-align:right;"> 7625 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:right;"> 7.17 </td>
   <td style="text-align:right;"> 4.62 </td>
   <td style="text-align:right;"> 0.59 </td>
   <td style="text-align:right;"> 3.63 </td>
   <td style="text-align:right;"> 6.38 </td>
   <td style="text-align:right;"> 10.16 </td>
   <td style="text-align:right;"> 26.03 </td>
   <td style="text-align:left;"> ▇▆▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sire_age_at_concep_y </td>
   <td style="text-align:right;"> 12434 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 9.11 </td>
   <td style="text-align:right;"> 6.28 </td>
   <td style="text-align:right;"> 0.61 </td>
   <td style="text-align:right;"> 4.62 </td>
   <td style="text-align:right;"> 7.44 </td>
   <td style="text-align:right;"> 12.37 </td>
   <td style="text-align:right;"> 33.36 </td>
   <td style="text-align:left;"> ▇▅▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_death_y </td>
   <td style="text-align:right;"> 18701 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 17.50 </td>
   <td style="text-align:right;"> 8.50 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 11.04 </td>
   <td style="text-align:right;"> 17.34 </td>
   <td style="text-align:right;"> 23.42 </td>
   <td style="text-align:right;"> 39.39 </td>
   <td style="text-align:left;"> ▃▇▇▅▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_of_living_y </td>
   <td style="text-align:right;"> 26771 </td>
   <td style="text-align:right;"> 0.35 </td>
   <td style="text-align:right;"> 13.50 </td>
   <td style="text-align:right;"> 8.93 </td>
   <td style="text-align:right;"> 0.54 </td>
   <td style="text-align:right;"> 6.60 </td>
   <td style="text-align:right;"> 10.65 </td>
   <td style="text-align:right;"> 19.81 </td>
   <td style="text-align:right;"> 35.20 </td>
   <td style="text-align:left;"> ▇▇▃▃▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_last_verified_y </td>
   <td style="text-align:right;"> 37139 </td>
   <td style="text-align:right;"> 0.10 </td>
   <td style="text-align:right;"> 12.61 </td>
   <td style="text-align:right;"> 8.09 </td>
   <td style="text-align:right;"> 0.36 </td>
   <td style="text-align:right;"> 6.71 </td>
   <td style="text-align:right;"> 10.08 </td>
   <td style="text-align:right;"> 18.55 </td>
   <td style="text-align:right;"> 34.26 </td>
   <td style="text-align:left;"> ▆▇▃▃▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_max_live_or_dead_y </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 15.60 </td>
   <td style="text-align:right;"> 8.87 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 7.79 </td>
   <td style="text-align:right;"> 14.46 </td>
   <td style="text-align:right;"> 22.65 </td>
   <td style="text-align:right;"> 39.39 </td>
   <td style="text-align:left;"> ▇▇▆▅▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> n_known_offspring </td>
   <td style="text-align:right;"> 18315 </td>
   <td style="text-align:right;"> 0.56 </td>
   <td style="text-align:right;"> 5.62 </td>
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
   <td style="text-align:right;"> 1479.33 </td>
   <td style="text-align:right;"> 1308.11 </td>
   <td style="text-align:right;"> 4.74 </td>
   <td style="text-align:right;"> 199.00 </td>
   <td style="text-align:right;"> 1300.00 </td>
   <td style="text-align:right;"> 2480.00 </td>
   <td style="text-align:right;"> 10245.00 </td>
   <td style="text-align:left;"> ▇▅▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> month_of_weight </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6.50 </td>
   <td style="text-align:right;"> 3.39 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.00 </td>
   <td style="text-align:right;"> 7.00 </td>
   <td style="text-align:right;"> 9.00 </td>
   <td style="text-align:right;"> 12.00 </td>
   <td style="text-align:left;"> ▇▆▆▆▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_d </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3087.62 </td>
   <td style="text-align:right;"> 2829.28 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 744.00 </td>
   <td style="text-align:right;"> 2285.00 </td>
   <td style="text-align:right;"> 4791.00 </td>
   <td style="text-align:right;"> 14373.00 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_wk </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 441.09 </td>
   <td style="text-align:right;"> 404.18 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 106.29 </td>
   <td style="text-align:right;"> 326.43 </td>
   <td style="text-align:right;"> 684.43 </td>
   <td style="text-align:right;"> 2053.29 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_mo </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 101.51 </td>
   <td style="text-align:right;"> 93.02 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 24.46 </td>
   <td style="text-align:right;"> 75.12 </td>
   <td style="text-align:right;"> 157.51 </td>
   <td style="text-align:right;"> 472.54 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_mo_no_dec </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 101.02 </td>
   <td style="text-align:right;"> 93.01 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 24.00 </td>
   <td style="text-align:right;"> 75.00 </td>
   <td style="text-align:right;"> 157.00 </td>
   <td style="text-align:right;"> 472.00 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age_at_wt_y </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 8.46 </td>
   <td style="text-align:right;"> 7.75 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 2.04 </td>
   <td style="text-align:right;"> 6.26 </td>
   <td style="text-align:right;"> 13.13 </td>
   <td style="text-align:right;"> 39.38 </td>
   <td style="text-align:left;"> ▇▃▂▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> change_since_prev_wt_g </td>
   <td style="text-align:right;"> 1137 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 20.00 </td>
   <td style="text-align:right;"> 173.60 </td>
   <td style="text-align:right;"> -1760.00 </td>
   <td style="text-align:right;"> -20.00 </td>
   <td style="text-align:right;"> 2.00 </td>
   <td style="text-align:right;"> 47.00 </td>
   <td style="text-align:right;"> 3662.00 </td>
   <td style="text-align:left;"> ▁▇▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> days_since_prev_wt </td>
   <td style="text-align:right;"> 1137 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 54.90 </td>
   <td style="text-align:right;"> 151.37 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 13.00 </td>
   <td style="text-align:right;"> 28.00 </td>
   <td style="text-align:right;"> 55.00 </td>
   <td style="text-align:right;"> 7113.00 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> avg_daily_wt_change_g </td>
   <td style="text-align:right;"> 1247 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 0.56 </td>
   <td style="text-align:right;"> 11.51 </td>
   <td style="text-align:right;"> -920.00 </td>
   <td style="text-align:right;"> -0.63 </td>
   <td style="text-align:right;"> 0.13 </td>
   <td style="text-align:right;"> 1.79 </td>
   <td style="text-align:right;"> 300.00 </td>
   <td style="text-align:left;"> ▁▁▁▇▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> days_before_death </td>
   <td style="text-align:right;"> 18698 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 2710.07 </td>
   <td style="text-align:right;"> 2281.73 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 860.00 </td>
   <td style="text-align:right;"> 2120.00 </td>
   <td style="text-align:right;"> 4084.00 </td>
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
   <td style="text-align:right;"> 40358 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 141.59 </td>
   <td style="text-align:right;"> 33.58 </td>
   <td style="text-align:right;"> 62.00 </td>
   <td style="text-align:right;"> 124.00 </td>
   <td style="text-align:right;"> 145.00 </td>
   <td style="text-align:right;"> 165.00 </td>
   <td style="text-align:right;"> 193.00 </td>
   <td style="text-align:left;"> ▂▂▇▇▅ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> days_before_inf_birth_if_preg </td>
   <td style="text-align:right;"> 40358 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 67.57 </td>
   <td style="text-align:right;"> 47.25 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 26.00 </td>
   <td style="text-align:right;"> 62.00 </td>
   <td style="text-align:right;"> 104.00 </td>
   <td style="text-align:right;"> 193.00 </td>
   <td style="text-align:left;"> ▇▆▅▃▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pct_preg_remain_if_preg </td>
   <td style="text-align:right;"> 40358 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.48 </td>
   <td style="text-align:right;"> 0.31 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.20 </td>
   <td style="text-align:right;"> 0.48 </td>
   <td style="text-align:right;"> 0.76 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> ▇▆▆▆▆ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> infant_lit_sz_if_preg </td>
   <td style="text-align:right;"> 40361 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 1.31 </td>
   <td style="text-align:right;"> 0.61 </td>
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
   <td style="text-align:left;"> PCOQ </td>
   <td style="text-align:left;"> 6727 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> Antonia </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 178 </td>
   <td style="text-align:left;"> 1998-02-22 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:left;"> 1997-09-15 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> 6398 </td>
   <td style="text-align:left;"> PAULINA </td>
   <td style="text-align:left;"> PCOQ </td>
   <td style="text-align:left;"> 1991-01-24 </td>
   <td style="text-align:right;"> 6.65 </td>
   <td style="text-align:left;"> 6450 </td>
   <td style="text-align:left;"> VALENTINIAN </td>
   <td style="text-align:left;"> PCOQ </td>
   <td style="text-align:left;"> 1991-11-02 </td>
   <td style="text-align:right;"> 5.87 </td>
   <td style="text-align:left;"> 2017-05-23 </td>
   <td style="text-align:right;"> 19.26 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 19.26 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 5500 </td>
   <td style="text-align:left;"> 2002-09-18 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 1669 </td>
   <td style="text-align:right;"> 238.43 </td>
   <td style="text-align:right;"> 54.87 </td>
   <td style="text-align:right;"> 54 </td>
   <td style="text-align:right;"> 4.57 </td>
   <td style="text-align:right;"> -70 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> -4.67 </td>
   <td style="text-align:right;"> 5361 </td>
   <td style="text-align:right;"> 2.64 </td>
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
   <td style="text-align:left;"> 1958 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> RANI </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 1403 </td>
   <td style="text-align:left;"> 1989-03-17 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 193 </td>
   <td style="text-align:left;"> 1988-09-05 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> 1903 </td>
   <td style="text-align:left;"> LALITA </td>
   <td style="text-align:left;"> NCOU </td>
   <td style="text-align:left;"> 1985-04-08 </td>
   <td style="text-align:right;"> 3.41 </td>
   <td style="text-align:left;"> 993 </td>
   <td style="text-align:left;"> KALKI </td>
   <td style="text-align:left;"> NCOU </td>
   <td style="text-align:left;"> 1984-05-03 </td>
   <td style="text-align:right;"> 4.35 </td>
   <td style="text-align:left;"> 2009-01-09 </td>
   <td style="text-align:right;"> 19.83 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 19.83 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1336 </td>
   <td style="text-align:left;"> 1995-07-05 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 2301 </td>
   <td style="text-align:right;"> 328.71 </td>
   <td style="text-align:right;"> 75.65 </td>
   <td style="text-align:right;"> 75 </td>
   <td style="text-align:right;"> 6.30 </td>
   <td style="text-align:right;"> 41 </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 1.21 </td>
   <td style="text-align:right;"> 4937 </td>
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
   <td style="text-align:left;"> LTAR </td>
   <td style="text-align:left;"> 1921 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> ASHWANI </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 1067 </td>
   <td style="text-align:left;"> 1986-12-23 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 167 </td>
   <td style="text-align:left;"> 1986-07-09 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> 991 </td>
   <td style="text-align:left;"> KIRAN </td>
   <td style="text-align:left;"> LTAR </td>
   <td style="text-align:left;"> 1982-04-12 </td>
   <td style="text-align:right;"> 4.24 </td>
   <td style="text-align:left;"> 990 </td>
   <td style="text-align:left;"> RAJIV </td>
   <td style="text-align:left;"> LTAR </td>
   <td style="text-align:left;"> 1982-04-12 </td>
   <td style="text-align:right;"> 4.24 </td>
   <td style="text-align:left;"> 2005-08-09 </td>
   <td style="text-align:right;"> 18.64 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 18.64 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:left;"> 2000-03-02 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 4818 </td>
   <td style="text-align:right;"> 688.29 </td>
   <td style="text-align:right;"> 158.40 </td>
   <td style="text-align:right;"> 158 </td>
   <td style="text-align:right;"> 13.20 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.99 </td>
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
   <td style="text-align:left;"> 6451 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> Mephistopheles </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 114 </td>
   <td style="text-align:left;"> 1981-10-08 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:left;"> Y01 </td>
   <td style="text-align:left;"> WB </td>
   <td style="text-align:left;"> Madagascar / </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:left;"> 1981-04-26 </td>
   <td style="text-align:right;"> 4 </td>
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
   <td style="text-align:left;"> 2014-02-12 </td>
   <td style="text-align:right;"> 32.37 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 32.37 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> Y01 </td>
   <td style="text-align:right;"> 2790 </td>
   <td style="text-align:left;"> 2003-12-09 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 8097 </td>
   <td style="text-align:right;"> 1156.71 </td>
   <td style="text-align:right;"> 266.20 </td>
   <td style="text-align:right;"> 266 </td>
   <td style="text-align:right;"> 22.18 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 1.25 </td>
   <td style="text-align:right;"> 3718 </td>
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
   <td style="text-align:left;"> GMOH </td>
   <td style="text-align:left;"> 3141 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> SEAGRAPE </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1989-02-01 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 124 </td>
   <td style="text-align:left;"> 1988-09-30 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> 3099 </td>
   <td style="text-align:left;"> LYSILOMA </td>
   <td style="text-align:left;"> GMOH </td>
   <td style="text-align:left;"> 1987-03-22 </td>
   <td style="text-align:right;"> 1.53 </td>
   <td style="text-align:left;"> 3104 </td>
   <td style="text-align:left;"> VIRBURNAM </td>
   <td style="text-align:left;"> GMOH </td>
   <td style="text-align:left;"> 1987-05-12 </td>
   <td style="text-align:right;"> 1.39 </td>
   <td style="text-align:left;"> 1998-09-24 </td>
   <td style="text-align:right;"> 9.65 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 9.65 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 147 </td>
   <td style="text-align:left;"> 1996-06-05 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 2681 </td>
   <td style="text-align:right;"> 383.00 </td>
   <td style="text-align:right;"> 88.14 </td>
   <td style="text-align:right;"> 88 </td>
   <td style="text-align:right;"> 7.35 </td>
   <td style="text-align:right;"> -3 </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> -0.11 </td>
   <td style="text-align:right;"> 841 </td>
   <td style="text-align:right;"> 0.40 </td>
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
   <td style="text-align:left;"> 7047 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> Spalt </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:left;"> 727 </td>
   <td style="text-align:left;"> 2011-06-07 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 63 </td>
   <td style="text-align:left;"> 2011-04-05 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> 7028 </td>
   <td style="text-align:left;"> Calendula </td>
   <td style="text-align:left;"> MMUR </td>
   <td style="text-align:left;"> 2008-05-25 </td>
   <td style="text-align:right;"> 2.86 </td>
   <td style="text-align:left;"> MULT2 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 7.67 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 7.67 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 71 </td>
   <td style="text-align:left;"> 2014-09-26 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 1207 </td>
   <td style="text-align:right;"> 172.43 </td>
   <td style="text-align:right;"> 39.68 </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:right;"> 3.31 </td>
   <td style="text-align:right;"> -3 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> -0.43 </td>
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
   <td style="text-align:left;"> EFUL </td>
   <td style="text-align:left;"> 6123 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> Frigga </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 1075 </td>
   <td style="text-align:left;"> 1984-03-31 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> BREC's  Baton Rouge Zoo </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:left;"> 1983-12-02 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> 3532 </td>
   <td style="text-align:left;"> LIBYA </td>
   <td style="text-align:left;"> EFUL </td>
   <td style="text-align:left;"> 1977-06-27 </td>
   <td style="text-align:right;"> 6.44 </td>
   <td style="text-align:left;"> 556 </td>
   <td style="text-align:left;"> CADMUS </td>
   <td style="text-align:left;"> EFUL </td>
   <td style="text-align:left;"> 1970-03-15 </td>
   <td style="text-align:right;"> 13.73 </td>
   <td style="text-align:left;"> 2014-01-22 </td>
   <td style="text-align:right;"> 29.83 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 29.83 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 3040 </td>
   <td style="text-align:left;"> 2005-07-21 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 7782 </td>
   <td style="text-align:right;"> 1111.71 </td>
   <td style="text-align:right;"> 255.85 </td>
   <td style="text-align:right;"> 255 </td>
   <td style="text-align:right;"> 21.32 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 3107 </td>
   <td style="text-align:right;"> 1.39 </td>
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
   <td style="text-align:left;"> 360 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> MONJO </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 146 </td>
   <td style="text-align:left;"> 1987-04-21 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 90 </td>
   <td style="text-align:left;"> 1987-01-21 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> 318 </td>
   <td style="text-align:left;"> FANDRASA </td>
   <td style="text-align:left;"> MZAZ </td>
   <td style="text-align:left;"> 1979-08-25 </td>
   <td style="text-align:right;"> 7.41 </td>
   <td style="text-align:left;"> 319 </td>
   <td style="text-align:left;"> GAKA </td>
   <td style="text-align:left;"> MZAZ </td>
   <td style="text-align:left;"> 1979-08-25 </td>
   <td style="text-align:right;"> 7.41 </td>
   <td style="text-align:left;"> 2002-11-20 </td>
   <td style="text-align:right;"> 15.59 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 15.59 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 345 </td>
   <td style="text-align:left;"> 2000-01-04 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 4641 </td>
   <td style="text-align:right;"> 663.00 </td>
   <td style="text-align:right;"> 152.58 </td>
   <td style="text-align:right;"> 152 </td>
   <td style="text-align:right;"> 12.72 </td>
   <td style="text-align:right;"> -10 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> -0.34 </td>
   <td style="text-align:right;"> 1051 </td>
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
   <td style="text-align:left;"> MMUR </td>
   <td style="text-align:left;"> 7032 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> Pesto </td>
   <td style="text-align:left;"> Y </td>
   <td style="text-align:left;"> 718 </td>
   <td style="text-align:left;"> 2008-06-04 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Museum National d'Histoire Naturelle </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 63 </td>
   <td style="text-align:left;"> 2008-04-02 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> oi_162A </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> oi_149I </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 10.68 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 10.68 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 72 </td>
   <td style="text-align:left;"> 2018-10-24 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 3794 </td>
   <td style="text-align:right;"> 542.00 </td>
   <td style="text-align:right;"> 124.73 </td>
   <td style="text-align:right;"> 124 </td>
   <td style="text-align:right;"> 10.39 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 0.07 </td>
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
   <td style="text-align:left;"> MZAZ </td>
   <td style="text-align:left;"> 2310 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> KIKIMOVA </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> 205 </td>
   <td style="text-align:left;"> 1993-08-02 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 90 </td>
   <td style="text-align:left;"> 1993-05-04 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> 321 </td>
   <td style="text-align:left;"> PITSY </td>
   <td style="text-align:left;"> MZAZ </td>
   <td style="text-align:left;"> 1982-09-02 </td>
   <td style="text-align:right;"> 10.68 </td>
   <td style="text-align:left;"> 315 </td>
   <td style="text-align:left;"> AROSY </td>
   <td style="text-align:left;"> MZAZ </td>
   <td style="text-align:left;"> 1979-08-25 </td>
   <td style="text-align:right;"> 13.70 </td>
   <td style="text-align:left;"> 2010-02-03 </td>
   <td style="text-align:right;"> 16.52 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 16.52 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 219 </td>
   <td style="text-align:left;"> 1994-03-09 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 219 </td>
   <td style="text-align:right;"> 31.29 </td>
   <td style="text-align:right;"> 7.20 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 0.60 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 0.08 </td>
   <td style="text-align:right;"> 5810 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:left;"> IJ </td>
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


## Introduction

This data comes from the Duke Lemur Center. The Duke Lemur Center houses over 200 lemurs across 14 species. Lemurs are the most threatened group of mammals and are at risk of extinction. Lemurs are native to Madagascar which is located in the southwestern Indian Ocean. This data set contains taxonomic code, specimen ID, hybrid status, sex, name, DOB, birth month, birth type, birth institution, litter size, and many more interesting variables about lemurs. This is a very large data set containing several different variables.

## Abstract

This is a very large data set so the purpose of asking these questions is to find interesting hypotheses to ask. Exploring the data will make it easier to create hypotheses. Here are some interesting questions I asked.

-   How many Lemurs are there? By sex?
-   What is the average weight of a Lemur? What about for each taxon?
-   What is the average birth type for a Lemur? By sex? By Taxon?
-   Does average litter size change by birth type?

In this report, these questions will be answered using different functions and visuals. An hypothesis will also be created and functions and visuals will support or refute the hypothesis. The purpose of this research is to shed light on Lemurs because I feel like Lemurs as a whole aren't really talked about much. The outcomes of this research will provide us with a lot more knowledge about Lemurs and their lifestyle.

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
1 F     20349
2 M     20948
3 ND        7
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
 1 CMED   4015
 2 DMAD   2480
 3 EALB    161
 4 ECOL   1231
 5 ECOR   1050
 6 EFLA   1602
 7 EFUL    159
 8 EMAC    880
 9 EMON   1804
10 ERUB    688
# … with 17 more rows
```
:::

```{.r .cell-code}
exploratory_data %>%
  count(name)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 1,991 × 2
   name         n
   <chr>    <int>
 1 AARON        1
 2 ABAS         9
 3 ABDUL        2
 4 ABEDNIGO    13
 5 ABEL         2
 6 ABENA       51
 7 ABIGAIL      2
 8 ABSINTHE     2
 9 Abu        116
10 ACHILLES     1
# … with 1,981 more rows
```
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(name) %>%
  count(sex)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 1,992 × 3
# Groups:   name [1,991]
   name     sex       n
   <chr>    <chr> <int>
 1 AARON    M         1
 2 ABAS     M         9
 3 ABDUL    M         2
 4 ABEDNIGO M        13
 5 ABEL     M         2
 6 ABENA    F        51
 7 ABIGAIL  F         2
 8 ABSINTHE M         2
 9 Abu      F       116
10 ACHILLES M         1
# … with 1,982 more rows
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
1            1479.
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
 2 DMAD             2179.
 3 EALB             2092.
 4 ECOL             2285.
 5 ECOR             1484.
 6 EFLA             2134.
 7 EFUL             2321.
 8 EMAC             2221.
 9 EMON             1427.
10 ERUB             2105.
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
1 CB         37491
2 Unk           85
3 WB          3728
```
:::

```{.r .cell-code}
exploratory_data %>%
  group_by(taxon) %>%
  count(birth_type)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 55 × 3
# Groups:   taxon [27]
   taxon birth_type     n
   <chr> <chr>      <int>
 1 CMED  CB          4013
 2 CMED  WB             2
 3 DMAD  CB          1541
 4 DMAD  WB           939
 5 EALB  CB           161
 6 ECOL  CB          1122
 7 ECOL  WB           109
 8 ECOR  CB           998
 9 ECOR  Unk            6
10 ECOR  WB            46
# … with 45 more rows
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
1 F     CB         18350
2 F     Unk           59
3 F     WB          1940
4 M     CB         19134
5 M     Unk           26
6 M     WB          1788
7 ND    CB             7
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
1           1 17267
2           2  9804
3           3  4241
4           4   944
5          NA  9048
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
Warning: Removed 9048 rows containing non-finite values (`stat_count()`).
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
1 CB                   1 17267
2 CB                   2  9804
3 CB                   3  4241
4 CB                   4   944
5 CB                  NA  5235
6 Unk                 NA    85
7 WB                  NA  3728
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
1 N      40292
2 Sp      1012
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
 1 CMED  N       4015
 2 DMAD  N       2480
 3 EALB  N        161
 4 ECOL  N       1231
 5 ECOR  N       1050
 6 EFLA  N       1602
 7 EFUL  N        159
 8 EMAC  N        880
 9 EMON  N       1804
10 ERUB  N        688
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
1 F     N      19991
2 F     Sp       358
3 M     N      20295
4 M     Sp       653
5 ND    N          6
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
 1            1  4925
 2            2  2238
 3            3  1973
 4            4  4014
 5            5  5182
 6            6  4189
 7            7  2154
 8            8  1762
 9            9  2078
10           10  2070
11           11  5932
12           12  4784
13           NA     3
```
:::

```{.r .cell-code}
exploratory_data %>%
  count(infant_dob_if_preg)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 490 × 2
   infant_dob_if_preg     n
   <date>             <int>
 1 1972-03-08             1
 2 1972-04-29             1
 3 1972-07-05             1
 4 1972-07-10             1
 5 1972-07-20             1
 6 1972-10-04             1
 7 1980-05-07             1
 8 1980-05-08             1
 9 1980-07-14             1
10 1981-03-06             1
# … with 480 more rows
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
 1                1           61
 2                2           62
 3                3          172
 4                4          121
 5                5          105
 6                6           71
 7                7           63
 8                8           24
 9                9           15
10               10           22
11               11            6
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
1 N      40313
2 Sp       992
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
 1 CMED  N       4055
 2 DMAD  N       2597
 3 EALB  N        166
 4 ECOL  N       1236
 5 ECOR  N       1044
 6 EFLA  N       1615
 7 EFUL  N        169
 8 EMAC  N        854
 9 EMON  N       1841
10 ERUB  N        668
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
1 F     N      19847
2 F     Sp       361
3 M     N      20458
4 M     Sp       631
5 ND    N          8
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
 1            1  4924
 2            2  2243
 3            3  2033
 4            4  4060
 5            5  5120
 6            6  4195
 7            7  2122
 8            8  1789
 9            9  2050
10           10  2015
11           11  5944
12           12  4805
13           NA     5
```
:::

```{.r .cell-code}
test_data %>%
  count(infant_dob_if_preg)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 488 × 2
   infant_dob_if_preg     n
   <date>             <int>
 1 1972-04-27             1
 2 1972-05-08             1
 3 1972-05-12             1
 4 1972-06-01             1
 5 1972-07-05             1
 6 1972-07-12             1
 7 1980-05-02             1
 8 1980-07-20             2
 9 1980-07-28             1
10 1980-08-16             1
# … with 478 more rows
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
 1                1           81
 2                2           73
 3                3          164
 4                4           95
 5                5           90
 6                6           75
 7                7           59
 8                8           54
 9                9           15
10               10           20
11               11            7
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


## Conclusion

In conclusion, Lemurs are very interesting species. Lemurs are the most threatened group of mammals and are at risk of extinction. Lemurs are native to Madagascar which is located in the southwestern Indian Ocean. I've learned a lot about Lemurs through this report. My first hypothesis was If hybrid Lemurs are born then they are more likely to be captive-born rather than wild-born. In conclusion, my hypothesis was accepted, If hybrid Lemurs are born then they are more likely to be captive-born. My second hypothesis was If Lemurs are mating it will more likely be in April and then the infants will be born around August and September. In conclusion, the hypothesis was refuted but according to the expected gestation my hypothesis should have been on point. There are many other things to explore in this data set and many more interesting things to learn about Lemurs. This data comes from the Duke Lemur Center.
