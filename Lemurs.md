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

## Running Code

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


You can add options to executable code like this


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
lemurs %>%
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
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 0005 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> KANGA </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1961-08-25 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1961-04-18 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> 0001 </td>
   <td style="text-align:left;"> WHITE-TAIL </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 2.22 </td>
   <td style="text-align:left;"> 0002 </td>
   <td style="text-align:left;"> BRUISER </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 2.22 </td>
   <td style="text-align:left;"> 1977-02-07 </td>
   <td style="text-align:right;"> 15.47 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 15.47 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1086 </td>
   <td style="text-align:left;"> 1972-02-16 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3827 </td>
   <td style="text-align:right;"> 546.71 </td>
   <td style="text-align:right;"> 125.82 </td>
   <td style="text-align:right;"> 125 </td>
   <td style="text-align:right;"> 10.48 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 1818 </td>
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
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 0005 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> KANGA </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1961-08-25 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1961-04-18 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> 0001 </td>
   <td style="text-align:left;"> WHITE-TAIL </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 2.22 </td>
   <td style="text-align:left;"> 0002 </td>
   <td style="text-align:left;"> BRUISER </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 2.22 </td>
   <td style="text-align:left;"> 1977-02-07 </td>
   <td style="text-align:right;"> 15.47 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 15.47 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1190 </td>
   <td style="text-align:left;"> 1972-06-20 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 3952 </td>
   <td style="text-align:right;"> 564.57 </td>
   <td style="text-align:right;"> 129.93 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:right;"> 10.83 </td>
   <td style="text-align:right;"> 104 </td>
   <td style="text-align:right;"> 125 </td>
   <td style="text-align:right;"> 0.83 </td>
   <td style="text-align:right;"> 1693 </td>
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
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 0006 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> ROO </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1961-03-17 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1960-11-08 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> 0001 </td>
   <td style="text-align:left;"> WHITE-TAIL </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 1.78 </td>
   <td style="text-align:left;"> 0002 </td>
   <td style="text-align:left;"> BRUISER </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 1.78 </td>
   <td style="text-align:left;"> 1974-10-15 </td>
   <td style="text-align:right;"> 13.59 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 13.59 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 947 </td>
   <td style="text-align:left;"> 1972-02-16 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3988 </td>
   <td style="text-align:right;"> 569.71 </td>
   <td style="text-align:right;"> 131.11 </td>
   <td style="text-align:right;"> 131 </td>
   <td style="text-align:right;"> 10.93 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 972 </td>
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
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 0006 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> ROO </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1961-03-17 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1960-11-08 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> 0001 </td>
   <td style="text-align:left;"> WHITE-TAIL </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 1.78 </td>
   <td style="text-align:left;"> 0002 </td>
   <td style="text-align:left;"> BRUISER </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 1.78 </td>
   <td style="text-align:left;"> 1974-10-15 </td>
   <td style="text-align:right;"> 13.59 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 13.59 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1174 </td>
   <td style="text-align:left;"> 1972-06-26 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 4119 </td>
   <td style="text-align:right;"> 588.43 </td>
   <td style="text-align:right;"> 135.42 </td>
   <td style="text-align:right;"> 135 </td>
   <td style="text-align:right;"> 11.28 </td>
   <td style="text-align:right;"> 227 </td>
   <td style="text-align:right;"> 131 </td>
   <td style="text-align:right;"> 1.73 </td>
   <td style="text-align:right;"> 841 </td>
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
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 0009 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> POOH BEAR </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1963-09-30 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1963-05-24 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> 0001 </td>
   <td style="text-align:left;"> WHITE-TAIL </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 4.32 </td>
   <td style="text-align:left;"> 0007 </td>
   <td style="text-align:left;"> HERMIT </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 4.32 </td>
   <td style="text-align:left;"> 1974-02-13 </td>
   <td style="text-align:right;"> 10.38 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 10.38 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 899 </td>
   <td style="text-align:left;"> 1972-02-16 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3061 </td>
   <td style="text-align:right;"> 437.29 </td>
   <td style="text-align:right;"> 100.64 </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 8.39 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 728 </td>
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
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 0009 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> POOH BEAR </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1963-09-30 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1963-05-24 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> 0001 </td>
   <td style="text-align:left;"> WHITE-TAIL </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 4.32 </td>
   <td style="text-align:left;"> 0007 </td>
   <td style="text-align:left;"> HERMIT </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 4.32 </td>
   <td style="text-align:left;"> 1974-02-13 </td>
   <td style="text-align:right;"> 10.38 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 10.38 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 917 </td>
   <td style="text-align:left;"> 1972-02-29 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3074 </td>
   <td style="text-align:right;"> 439.14 </td>
   <td style="text-align:right;"> 101.06 </td>
   <td style="text-align:right;"> 101 </td>
   <td style="text-align:right;"> 8.42 </td>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 1.38 </td>
   <td style="text-align:right;"> 715 </td>
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
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 0009 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> POOH BEAR </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1963-09-30 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1963-05-24 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> 0001 </td>
   <td style="text-align:left;"> WHITE-TAIL </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 4.32 </td>
   <td style="text-align:left;"> 0007 </td>
   <td style="text-align:left;"> HERMIT </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 4.32 </td>
   <td style="text-align:left;"> 1974-02-13 </td>
   <td style="text-align:right;"> 10.38 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 10.38 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 910 </td>
   <td style="text-align:left;"> 1972-06-22 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 3188 </td>
   <td style="text-align:right;"> 455.43 </td>
   <td style="text-align:right;"> 104.81 </td>
   <td style="text-align:right;"> 104 </td>
   <td style="text-align:right;"> 8.73 </td>
   <td style="text-align:right;"> -7 </td>
   <td style="text-align:right;"> 114 </td>
   <td style="text-align:right;"> -0.06 </td>
   <td style="text-align:right;"> 601 </td>
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
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 0010 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> EEYORE </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1964-05-20 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1964-01-12 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> 0001 </td>
   <td style="text-align:left;"> WHITE-TAIL </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 4.96 </td>
   <td style="text-align:left;"> 0007 </td>
   <td style="text-align:left;"> HERMIT </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 4.96 </td>
   <td style="text-align:left;"> 1977-11-02 </td>
   <td style="text-align:right;"> 13.46 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 13.46 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1185 </td>
   <td style="text-align:left;"> 1972-02-16 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 2828 </td>
   <td style="text-align:right;"> 404.00 </td>
   <td style="text-align:right;"> 92.98 </td>
   <td style="text-align:right;"> 92 </td>
   <td style="text-align:right;"> 7.75 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 2086 </td>
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
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 0010 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:left;"> EEYORE </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1964-05-20 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1964-01-12 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> 0001 </td>
   <td style="text-align:left;"> WHITE-TAIL </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 4.96 </td>
   <td style="text-align:left;"> 0007 </td>
   <td style="text-align:left;"> HERMIT </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1959-01-28 </td>
   <td style="text-align:right;"> 4.96 </td>
   <td style="text-align:left;"> 1977-11-02 </td>
   <td style="text-align:right;"> 13.46 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 13.46 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1236 </td>
   <td style="text-align:left;"> 1972-06-20 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 2953 </td>
   <td style="text-align:right;"> 421.86 </td>
   <td style="text-align:right;"> 97.08 </td>
   <td style="text-align:right;"> 97 </td>
   <td style="text-align:right;"> 8.09 </td>
   <td style="text-align:right;"> 51 </td>
   <td style="text-align:right;"> 125 </td>
   <td style="text-align:right;"> 0.41 </td>
   <td style="text-align:right;"> 1961 </td>
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
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 0014 </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:left;"> ROOLETTE </td>
   <td style="text-align:left;"> N </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> 1964-10-27 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> CB </td>
   <td style="text-align:left;"> Duke Lemur Center </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:left;"> 1964-06-20 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> 0006 </td>
   <td style="text-align:left;"> ROO </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1961-03-17 </td>
   <td style="text-align:right;"> 3.26 </td>
   <td style="text-align:left;"> 0005 </td>
   <td style="text-align:left;"> KANGA </td>
   <td style="text-align:left;"> OGG </td>
   <td style="text-align:left;"> 1961-08-25 </td>
   <td style="text-align:right;"> 2.82 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 14.16 </td>
   <td style="text-align:right;"> 14.16 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 897 </td>
   <td style="text-align:left;"> 1972-02-29 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 2681 </td>
   <td style="text-align:right;"> 383.00 </td>
   <td style="text-align:right;"> 88.14 </td>
   <td style="text-align:right;"> 88 </td>
   <td style="text-align:right;"> 7.35 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
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
</tbody>
</table>

`````

:::
:::


## About Our Data

This data comes from the Duke Lemur Center. The Duke Lemur Center houses over 200 lemurs across 14 species. Lemurs are the most threatened group of mammals and are at risk of extinction. This data set contains taxonomic code, specimen ID, hybrid status, sex, name, DOB, birth month, birth type, birth institution, litter size, and many more interesting variables about lemurs. This is a very large data set containing


::: {.cell}

```{.r .cell-code}
lemurs %>%
  count(name)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 2,244 × 2
   name         n
   <chr>    <int>
 1 AARON        2
 2 ABAS        16
 3 ABDUL        4
 4 ABEDNIGO    21
 5 ABEL         2
 6 ABENA      101
 7 ABIGAIL      2
 8 ABNER        1
 9 ABSINTHE     2
10 Abu        245
# … with 2,234 more rows
```
:::

```{.r .cell-code}
lemurs %>%
  count(sex)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 3 × 2
  sex       n
  <chr> <int>
1 F     40557
2 M     42037
3 ND       15
```
:::

```{.r .cell-code}
lemurs %>%
  group_by(name) %>%
  count(sex)
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 2,246 × 3
# Groups:   name [2,244]
   name     sex       n
   <chr>    <chr> <int>
 1 AARON    M         2
 2 ABAS     M        16
 3 ABDUL    M         4
 4 ABEDNIGO M        21
 5 ABEL     M         2
 6 ABENA    F       101
 7 ABIGAIL  F         2
 8 ABNER    M         1
 9 ABSINTHE M         2
10 Abu      F       245
# … with 2,236 more rows
```
:::
:::
