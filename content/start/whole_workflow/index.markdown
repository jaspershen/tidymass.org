---
title: "Whole workflow using tidymass"
weight: 14
author:
- name: Xiaotao Shen (https://www.shenxt.info/)
date: "Created on 2022-02-18 and updated on 2022-03-11"
output:
  html_document:
    number_sections: true
    df_print: paged
    toc: yes
  pdf_document:
    toc: no
vignette: >
  %\VignetteIndexEntry{whole workflow}
  %\VignettePackage{tidymass}
  % \VignetteEngine{knitr::rmarkdown}
  % \usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}
---



# Data preparation

Download the demo data and [refer this article](https://tidymass.github.io/tidymass/articles/demo_data.html).

We have positive and negative mode. For each mode, we have `control`, `case` and `QC` groups. Control group have 110 samples, and case group have 110 samples as well.


```r
library(tidymass)
#> Registered S3 method overwritten by 'Hmisc':
#>   method       from      
#>   vcov.default fit.models
#> ── Attaching packages ─────────────────────────────────────── tidymass 0.99.6 ──
#> ✓ massdataset   0.99.20     ✓ metpath       0.99.4 
#> ✓ massprocesser 0.99.3      ✓ metid         1.2.4  
#> ✓ masscleaner   0.99.7      ✓ masstools     0.99.5 
#> ✓ massqc        0.99.7      ✓ dplyr         1.0.8  
#> ✓ massstat      0.99.13     ✓ ggplot2       3.3.5
#> ── Conflicts ─────────────────────────────────────────── tidymass_conflicts() ──
#> x massdataset::apply()     masks base::apply()
#> x dplyr::collect()         masks xcms::collect()
#> x BiocGenerics::colMeans() masks massdataset::colMeans(), base::colMeans()
#> x BiocGenerics::colSums()  masks massdataset::colSums(), base::colSums()
#> x dplyr::combine()         masks MSnbase::combine(), Biobase::combine(), BiocGenerics::combine()
#> x dplyr::filter()          masks metpath::filter(), massdataset::filter(), stats::filter()
#> x dplyr::first()           masks S4Vectors::first()
#> x dplyr::groups()          masks xcms::groups()
#> x S4Vectors::intersect()   masks BiocGenerics::intersect(), massdataset::intersect(), base::intersect()
#> x dplyr::lag()             masks stats::lag()
#> x masstools::mz_rt_match() masks massdataset::mz_rt_match()
#> x dplyr::rename()          masks S4Vectors::rename(), massdataset::rename()
#> x BiocGenerics::rowMeans() masks massdataset::rowMeans(), base::rowMeans()
#> x BiocGenerics::rowSums()  masks massdataset::rowSums(), base::rowSums()
library(tidyverse)
#> ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
#> ✓ tibble  3.1.6          ✓ purrr   0.3.4     
#> ✓ tidyr   1.2.0          ✓ stringr 1.4.0     
#> ✓ readr   2.1.2          ✓ forcats 0.5.1.9000
#> ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
#> x dplyr::collect()         masks xcms::collect()
#> x dplyr::combine()         masks MSnbase::combine(), Biobase::combine(), BiocGenerics::combine()
#> x tidyr::expand()          masks S4Vectors::expand()
#> x tidyr::extract()         masks magrittr::extract()
#> x dplyr::filter()          masks metpath::filter(), massdataset::filter(), stats::filter()
#> x dplyr::first()           masks S4Vectors::first()
#> x dplyr::groups()          masks xcms::groups()
#> x dplyr::lag()             masks stats::lag()
#> x BiocGenerics::Position() masks ggplot2::Position(), base::Position()
#> x purrr::reduce()          masks MSnbase::reduce()
#> x dplyr::rename()          masks S4Vectors::rename(), massdataset::rename()
#> x purrr::set_names()       masks magrittr::set_names()
```

![](figures/fig10.png)

---

# Raw data processing

`massprocesser` package is used to do the raw data processing. Please refer this [website](https://massprocesser.tidymass.org/).

## Positive mode

The code used to do raw data processing.


```r
process_data(
  path = "mzxml_ms1_data/POS",
  polarity = "positive",
  ppm = 10,
  peakwidth = c(10, 60),
  threads = 4,
  output_tic = FALSE,
  output_bpc = FALSE,
  output_rt_correction_plot = FALSE,
  min_fraction = 0.5,
  group_for_figure = "QC"
)
```

All the results will be placed in the folder named `mzxml_ms1_data/POS/Result`. More information about that can be found [here](https://tidymass.github.io/massprocesser/articles/raw_data_processing.html).

![](figures/fig1.png)

You can just load the `object`, which is a `mass_dataset` class object.


```r
load("mzxml_ms1_data/POS/Result/object")
object
#> -------------------- 
#> massdataset version: 0.99.8 
#> -------------------- 
#> 1.expression_data:[ 10149 x 259 data.frame]
#> 2.sample_info:[ 259 x 4 data.frame]
#> 3.variable_info:[ 10149 x 3 data.frame]
#> 4.sample_info_note:[ 4 x 2 data.frame]
#> 5.variable_info_note:[ 3 x 2 data.frame]
#> 6.ms2_data:[ 0 variables x 0 MS2 spectra]
#> -------------------- 
#> Processing information (extract_process_info())
#> create_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset create_mass_dataset() 2022-02-22 16:37:06
#> process_data ---------- 
#>         Package Function.used                Time
#> 1 massprocesser  process_data 2022-02-22 16:36:42
```

We can see that there are 10,149 metabolic features in positive mode.

![](figures/fig2.png)

You can use the `plot_adjusted_rt()` function to get the interactive plot.


```r
load("mzxml_ms1_data/POS/Result/intermediate_data/xdata2")
####set the group_for_figure if you want to show specific groups. 
####And set it as "all" if you want to show all samples.
plot <-
massprocesser::plot_adjusted_rt(object = xdata2, 
                 group_for_figure = "QC", 
                 interactive = TRUE)
plot
```

## Negative mode

The processing of negative mode is same with positive mode data.

Same with positive mode, change `polarity` to `negative`.


```r
massprocesser::process_data(
  path = "mzxml_ms1_data/NEG",
  polarity = "negative",
  ppm = 10,
  peakwidth = c(10, 60),
  threads = 4,
  output_tic = FALSE,
  output_bpc = FALSE,
  output_rt_correction_plot = FALSE,
  min_fraction = 0.5,
  group_for_figure = "QC"
)
```

Same with positive mode.


```r
load("mzxml_ms1_data/NEG/Result/object")
object
#> -------------------- 
#> massdataset version: 0.99.8 
#> -------------------- 
#> 1.expression_data:[ 8804 x 259 data.frame]
#> 2.sample_info:[ 259 x 4 data.frame]
#> 3.variable_info:[ 8804 x 3 data.frame]
#> 4.sample_info_note:[ 4 x 2 data.frame]
#> 5.variable_info_note:[ 3 x 2 data.frame]
#> 6.ms2_data:[ 0 variables x 0 MS2 spectra]
#> -------------------- 
#> Processing information (extract_process_info())
#> create_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset create_mass_dataset() 2022-02-22 16:38:19
#> process_data ---------- 
#>         Package Function.used                Time
#> 1 massprocesser  process_data 2022-02-22 16:38:02
```

We can see that there are 8,804 metabolic features in negative mode.

# Explore data

After the [`raw data processing`](https://tidymass.github.io/tidymass/articles/raw_data_processing.html), peak tables for positive and negative mode will be generated. 

## Positive mode


```r
load("mzxml_ms1_data/POS/Result/object")
object_pos <- object
object_pos
#> -------------------- 
#> massdataset version: 0.99.8 
#> -------------------- 
#> 1.expression_data:[ 10149 x 259 data.frame]
#> 2.sample_info:[ 259 x 4 data.frame]
#> 3.variable_info:[ 10149 x 3 data.frame]
#> 4.sample_info_note:[ 4 x 2 data.frame]
#> 5.variable_info_note:[ 3 x 2 data.frame]
#> 6.ms2_data:[ 0 variables x 0 MS2 spectra]
#> -------------------- 
#> Processing information (extract_process_info())
#> create_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset create_mass_dataset() 2022-02-22 16:37:06
#> process_data ---------- 
#>         Package Function.used                Time
#> 1 massprocesser  process_data 2022-02-22 16:36:42
```

Read sample information.


```r
sample_info_pos <- readr::read_csv("sample_info/sample_info_pos.csv")
#> Rows: 259 Columns: 6
#> ── Column specification ────────────────────────────────────────────────────────
#> Delimiter: ","
#> chr (4): sample_id, class, subject_id, group
#> dbl (2): injection.order, batch
#> 
#> ℹ Use `spec()` to retrieve the full column specification for this data.
#> ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
head(sample_info_pos)
#> # A tibble: 6 × 6
#>   sample_id    injection.order class   batch subject_id  group  
#>   <chr>                  <dbl> <chr>   <dbl> <chr>       <chr>  
#> 1 sample_QC_01               1 QC          1 <NA>        QC     
#> 2 sample_01                  2 Subject     1 subject_474 Control
#> 3 sample_02                  3 Subject     1 subject_431 Control
#> 4 sample_06                  4 Subject     1 subject_414 Case   
#> 5 sample_07                  5 Subject     1 subject_830 Control
#> 6 sample_11                  6 Subject     1 subject_125 Case
```

Add `sample_info_pos` to `object_pos`


```r
object_pos %>% 
  extract_sample_info() %>% 
  head()
#>    sample_id group   class injection.order
#> 1  sample_06  Case Subject               1
#> 2 sample_103  Case Subject               2
#> 3  sample_11  Case Subject               3
#> 4 sample_112  Case Subject               4
#> 5 sample_117  Case Subject               5
#> 6  sample_12  Case Subject               6

object_pos <- 
  object_pos %>% 
  activate_mass_dataset(what = "sample_info") %>%
  dplyr::select(-c("group", "class", "injection.order"))
```


```r
object_pos <-
  object_pos %>%
  activate_mass_dataset(what = "sample_info") %>%
  left_join(sample_info_pos,
            by = "sample_id")

object_pos %>% 
  extract_sample_info() %>% 
  head()
#>    sample_id injection.order   class batch  subject_id group
#> 1  sample_06               4 Subject     1 subject_414  Case
#> 2 sample_103              71 Subject     1 subject_330  Case
#> 3  sample_11               6 Subject     1 subject_125  Case
#> 4 sample_112              78 Subject     1 subject_295  Case
#> 5 sample_117              80 Subject     1 subject_793  Case
#> 6  sample_12               8 Subject     1 subject_387  Case
```


```r
dim(object_pos)
#> [1] 10149   259
object_pos %>% 
  activate_mass_dataset(what = "sample_info") %>% 
  dplyr::count(class)
#>     class   n
#> 1      QC  39
#> 2 Subject 220

object_pos %>% 
  activate_mass_dataset(what = "sample_info") %>% 
  dplyr::count(group)
#>     group   n
#> 1    Case 110
#> 2 Control 110
#> 3      QC  39

object_pos %>% 
  activate_mass_dataset(what = "sample_info") %>% 
  dplyr::count(batch)
#>   batch   n
#> 1     1 112
#> 2     2 147
```
So for positive mode, we have 259 samples and 10149 variables. 220 subject samples and 39 QC samples. 110 control samples and 110 case samples. Two batches in total, 112 samples in batch 1 and 147 in batch 2.

Next, we can get the peak distributation plot of positive mode.


```r
object_pos %>%
  `+`(1) %>% 
  log(10) %>%
  show_mz_rt_plot() +
  scale_size_continuous(range = c(0.01, 2))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-1.png" width="100%" />

We can explore the missing values in positive mode data.


```r
get_mv_number(object = object_pos)
#> [1] 785821
```

785,821 mvs in total.

Missing value number in each sample.


```r
get_mv_number(object = object_pos, by = "variable") %>% 
  head()
#>  M70T73_POS  M70T53_POS M70T183_POS M70T527_POS M71T695_POS M71T183_POS 
#>         129          16         155          54         133         169
```

Missing value number in each variable.

We can use the figure to show the missing value information.


```r
show_missing_values(object = object_pos, 
                    show_column_names = FALSE, percentage = TRUE)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-16-1.png" width="100%" />

Show the mvs in samples


```r
show_sample_missing_values(object = object_pos, percentage = TRUE)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-17-1.png" width="100%" />

Show the mvs in variables


```r
show_variable_missing_values(object = object_pos, 
                             percentage = TRUE, 
                             show_x_text = FALSE, 
                             show_x_ticks = FALSE) +
  scale_size_continuous(range = c(0.01, 1))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-1.png" width="100%" />

## Negative mode

Load `object`.


```r
load("mzxml_ms1_data/NEG/Result/object")
object_neg = object
object_neg
#> -------------------- 
#> massdataset version: 0.99.8 
#> -------------------- 
#> 1.expression_data:[ 8804 x 259 data.frame]
#> 2.sample_info:[ 259 x 4 data.frame]
#> 3.variable_info:[ 8804 x 3 data.frame]
#> 4.sample_info_note:[ 4 x 2 data.frame]
#> 5.variable_info_note:[ 3 x 2 data.frame]
#> 6.ms2_data:[ 0 variables x 0 MS2 spectra]
#> -------------------- 
#> Processing information (extract_process_info())
#> create_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset create_mass_dataset() 2022-02-22 16:38:19
#> process_data ---------- 
#>         Package Function.used                Time
#> 1 massprocesser  process_data 2022-02-22 16:38:02
```

Read sample information.


```r
sample_info_neg = readr::read_csv("sample_info/sample_info_neg.csv")
#> Rows: 259 Columns: 6
#> ── Column specification ────────────────────────────────────────────────────────
#> Delimiter: ","
#> chr (4): sample_id, class, subject_id, group
#> dbl (2): injection.order, batch
#> 
#> ℹ Use `spec()` to retrieve the full column specification for this data.
#> ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
head(sample_info_neg)
#> # A tibble: 6 × 6
#>   sample_id    injection.order class   batch subject_id  group  
#>   <chr>                  <dbl> <chr>   <dbl> <chr>       <chr>  
#> 1 sample_QC_01               1 QC          1 <NA>        QC     
#> 2 sample_01                  2 Subject     1 subject_474 Control
#> 3 sample_02                  3 Subject     1 subject_431 Control
#> 4 sample_06                  4 Subject     1 subject_414 Case   
#> 5 sample_07                  5 Subject     1 subject_830 Control
#> 6 sample_11                  6 Subject     1 subject_125 Case
```

Add `sample_info_neg` to `object_neg`


```r
object_neg %>% 
  extract_sample_info() %>% 
  head()
#>    sample_id group   class injection.order
#> 1  sample_06  Case Subject               1
#> 2 sample_103  Case Subject               2
#> 3  sample_11  Case Subject               3
#> 4 sample_112  Case Subject               4
#> 5 sample_117  Case Subject               5
#> 6  sample_12  Case Subject               6

object_neg <- 
  object_neg %>% 
  activate_mass_dataset(what = "sample_info") %>%
  dplyr::select(-c("group", "class", "injection.order"))
```


```r
object_neg <-
  object_neg %>%
  activate_mass_dataset(what = "sample_info") %>%
  left_join(sample_info_neg,
            by = "sample_id")

object_neg %>% 
  extract_sample_info() %>% 
  head()
#>    sample_id injection.order   class batch  subject_id group
#> 1  sample_06               4 Subject     1 subject_414  Case
#> 2 sample_103              71 Subject     1 subject_330  Case
#> 3  sample_11               6 Subject     1 subject_125  Case
#> 4 sample_112              78 Subject     1 subject_295  Case
#> 5 sample_117              80 Subject     1 subject_793  Case
#> 6  sample_12               8 Subject     1 subject_387  Case
```



```r
dim(object_neg)
#> [1] 8804  259
object_neg %>% 
  activate_mass_dataset(what = "sample_info") %>% 
  dplyr::count(class)
#>     class   n
#> 1      QC  39
#> 2 Subject 220

object_neg %>% 
  activate_mass_dataset(what = "sample_info") %>% 
  dplyr::count(group)
#>     group   n
#> 1    Case 110
#> 2 Control 110
#> 3      QC  39

object_neg %>% 
  activate_mass_dataset(what = "sample_info") %>% 
  dplyr::count(batch)
#>   batch   n
#> 1     1  86
#> 2     2 173
```

So for negative mode, we have 259 samples and 8804 variables. 220 subject samples and 39 QC samples. 110 control samples and 110 case samples. Two batches in total, 112 samples in batch 1 and 147 in batch 2.

Next, we can get the peak distributation plot of negative mode.


```r
object_neg %>%
  `+`(1) %>% 
  log(10) %>%
  show_mz_rt_plot() +
  scale_size_continuous(range = c(0.01, 2))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-24-1.png" width="100%" />


We can explore the missing values in negitive mode data.


```r
get_mv_number(object = object_neg)
#> [1] 748253
```


```r
get_mv_number(object = object_neg, by = "sample") %>% 
  head()
#>  sample_06 sample_103  sample_11 sample_112 sample_117  sample_12 
#>       3029       2766       3298       3100       2912       3053
```

Missing value number in each sample.


```r
get_mv_number(object = object_neg, by = "variable") %>% 
  head()
#> M70T712_NEG M70T527_NEG M70T587_NEG  M70T47_NEG M71T587_NEG M71T641_NEG 
#>          16         137           2         146          41          19
```

Missing value number in each variable.

We can use the figure to show the missing value information.


```r
show_missing_values(object = object_neg, show_column_names = FALSE, percentage = TRUE)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-28-1.png" width="100%" />

Show the mvs in samples


```r
show_sample_missing_values(object = object_neg, percentage = TRUE)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-29-1.png" width="100%" />

Show the mvs in variables


```r
show_variable_missing_values(object = object_neg, 
                             percentage = TRUE, 
                             show_x_text = FALSE, 
                             show_x_ticks = FALSE) +
  scale_size_continuous(range = c(0.01, 1))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-30-1.png" width="100%" />

---

# Data cleaning

## Data quality assessment before data cleaning

Change batch to character.


```r
object_pos <- 
  object_pos %>% 
  activate_mass_dataset(what = "sample_info") %>% 
  dplyr::mutate(batch = as.character(batch))

object_neg <- 
  object_neg %>% 
  activate_mass_dataset(what = "sample_info") %>% 
  dplyr::mutate(batch = as.character(batch))
```

Here, we can use the `massqc` package to [assess the data quality](https://tidymass.github.io/massqc/).

We can just use the `massqc_report()` function to get a html format quality assessment report.

### Positive mode


```r
massqc::massqc_report(object = object_pos, 
                      path = "data_cleaning/POS/data_quality_before_data_cleaning")
```

A html format report and pdf figures will be placed in the folder `data_cleaning/POS/data_quality_before_data_cleaning/Report`.

![](figures/fig3.png)

The html report is below:

![](figures/fig4.png)

### Negative mode.


```r
massqc::massqc_report(object = object_neg, 
                      path = "data_cleaning/NEG/data_quality_before_data_cleaning")
```

The PCA score plot is used to show the batch effect of positive and negative dataset.

Positive mode:

![](figures/fig5.png)

Negative mode:

![](figures/fig6.png)


## Remove noisy metabolic features

Remove variables which have MVs in more than 20% QC samples and in at lest 50% samples in control group or case group.

### Positive mode


```r
object_pos %>% 
  activate_mass_dataset(what = "sample_info") %>% 
  dplyr::count(group)
#>     group   n
#> 1    Case 110
#> 2 Control 110
#> 3      QC  39
```
MV percentage in QC samples.


```r
show_variable_missing_values(object = object_pos %>% 
                               activate_mass_dataset(what = "sample_info") %>% 
                               filter(class == "QC"), 
                             percentage = TRUE) +
  scale_size_continuous(range = c(0.01, 2))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-35-1.png" width="100%" />


```r
qc_id =
  object_pos %>%
  activate_mass_dataset(what = "sample_info") %>%
  filter(class == "QC") %>%
  pull(sample_id)

control_id =
  object_pos %>%
  activate_mass_dataset(what = "sample_info") %>%
  filter(group == "Control") %>%
  pull(sample_id)

case_id =
  object_pos %>%
  activate_mass_dataset(what = "sample_info") %>%
  filter(group == "Case") %>%
  pull(sample_id)

object_pos =
  object_pos %>%
  mutate_variable_na_freq(according_to_samples = qc_id) %>%
  mutate_variable_na_freq(according_to_samples = control_id) %>%
  mutate_variable_na_freq(according_to_samples = case_id)

head(extract_variable_info(object_pos))
#>   variable_id       mz        rt    na_freq na_freq.1 na_freq.2
#> 1  M70T73_POS 70.04074  73.31705 0.28205128 0.6000000 0.4727273
#> 2  M70T53_POS 70.06596  52.78542 0.00000000 0.1454545 0.0000000
#> 3 M70T183_POS 70.19977 182.87981 0.00000000 0.6636364 0.7454545
#> 4 M70T527_POS 70.36113 526.76657 0.02564103 0.1818182 0.3000000
#> 5 M71T695_POS 70.56911 694.84592 0.02564103 0.6454545 0.5545455
#> 6 M71T183_POS 70.75034 182.77790 0.05128205 0.7272727 0.7909091
```

Remove variables.


```r
object_pos <- 
  object_pos %>% 
  activate_mass_dataset(what = "variable_info") %>%
  filter(na_freq < 0.2 & (na_freq.1 < 0.5 | na_freq.2 < 0.5))
object_pos
#> -------------------- 
#> massdataset version: 0.99.8 
#> -------------------- 
#> 1.expression_data:[ 5101 x 259 data.frame]
#> 2.sample_info:[ 259 x 6 data.frame]
#> 3.variable_info:[ 5101 x 6 data.frame]
#> 4.sample_info_note:[ 6 x 2 data.frame]
#> 5.variable_info_note:[ 6 x 2 data.frame]
#> 6.ms2_data:[ 0 variables x 0 MS2 spectra]
#> -------------------- 
#> Processing information (extract_process_info())
#> create_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset create_mass_dataset() 2022-02-22 16:37:06
#> process_data ---------- 
#>         Package Function.used                Time
#> 1 massprocesser  process_data 2022-02-22 16:36:42
#> mutate ---------- 
#>       Package Function.used                Time
#> 1 massdataset      mutate() 2022-03-11 01:17:35
#> mutate_variable_na_freq ---------- 
#>       Package             Function.used                Time
#> 1 massdataset mutate_variable_na_freq() 2022-03-11 01:17:37
#> 2 massdataset mutate_variable_na_freq() 2022-03-11 01:17:37
#> 3 massdataset mutate_variable_na_freq() 2022-03-11 01:17:37
#> filter ---------- 
#>       Package Function.used                Time
#> 1 massdataset      filter() 2022-03-11 01:17:38
```

Only 5,101 variables left.


### Negative mode


```r
object_neg %>% 
  activate_mass_dataset(what = "sample_info") %>% 
  dplyr::count(group)
#>     group   n
#> 1    Case 110
#> 2 Control 110
#> 3      QC  39
```

MV percentage in QC samples.


```r
show_variable_missing_values(object = object_neg %>% 
                               activate_mass_dataset(what = "sample_info") %>% 
                               filter(class == "QC"), 
                             percentage = TRUE) +
  scale_size_continuous(range = c(0.01, 2))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-39-1.png" width="100%" />


```r
qc_id =
  object_neg %>%
  activate_mass_dataset(what = "sample_info") %>%
  filter(class == "QC") %>%
  pull(sample_id)

control_id =
  object_neg %>%
  activate_mass_dataset(what = "sample_info") %>%
  filter(group == "Control") %>%
  pull(sample_id)

case_id =
  object_neg %>%
  activate_mass_dataset(what = "sample_info") %>%
  filter(group == "Case") %>%
  pull(sample_id)

object_neg =
  object_neg %>%
  mutate_variable_na_freq(according_to_samples = qc_id) %>%
  mutate_variable_na_freq(according_to_samples = control_id) %>%
  mutate_variable_na_freq(according_to_samples = case_id)

head(extract_variable_info(object_neg))
#>   variable_id       mz        rt    na_freq   na_freq.1   na_freq.2
#> 1 M70T712_NEG 70.05911 711.97894 0.05128205 0.109090909 0.018181818
#> 2 M70T527_NEG 70.13902 526.85416 0.33333333 0.509090909 0.618181818
#> 3 M70T587_NEG 70.31217 587.25330 0.00000000 0.009090909 0.009090909
#> 4  M70T47_NEG 70.33835  46.57537 0.84615385 0.936363636 0.090909091
#> 5 M71T587_NEG 70.56315 587.02570 0.17948718 0.145454545 0.163636364
#> 6 M71T641_NEG 70.70657 641.16672 0.10256410 0.063636364 0.072727273
```

Remove variables.


```r
object_neg <- 
  object_neg %>% 
  activate_mass_dataset(what = "variable_info") %>%
  filter(na_freq < 0.2 & (na_freq.1 < 0.5 | na_freq.2 < 0.5))
object_neg
#> -------------------- 
#> massdataset version: 0.99.8 
#> -------------------- 
#> 1.expression_data:[ 4104 x 259 data.frame]
#> 2.sample_info:[ 259 x 6 data.frame]
#> 3.variable_info:[ 4104 x 6 data.frame]
#> 4.sample_info_note:[ 6 x 2 data.frame]
#> 5.variable_info_note:[ 6 x 2 data.frame]
#> 6.ms2_data:[ 0 variables x 0 MS2 spectra]
#> -------------------- 
#> Processing information (extract_process_info())
#> create_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset create_mass_dataset() 2022-02-22 16:38:19
#> process_data ---------- 
#>         Package Function.used                Time
#> 1 massprocesser  process_data 2022-02-22 16:38:02
#> mutate ---------- 
#>       Package Function.used                Time
#> 1 massdataset      mutate() 2022-03-11 01:17:35
#> mutate_variable_na_freq ---------- 
#>       Package             Function.used                Time
#> 1 massdataset mutate_variable_na_freq() 2022-03-11 01:17:40
#> 2 massdataset mutate_variable_na_freq() 2022-03-11 01:17:40
#> 3 massdataset mutate_variable_na_freq() 2022-03-11 01:17:40
#> filter ---------- 
#>       Package Function.used                Time
#> 1 massdataset      filter() 2022-03-11 01:17:41
```
4,104 features left.


## Filter outlier samples

We can use the `detect_outlier()` to find the outlier samples.

More information about how to detect outlier samples can be found [here](https://privefl.github.io/blog/detecting-outlier-samples-in-pca/).

### Positive mode


```r
massdataset::show_sample_missing_values(object = object_pos,
                                        color_by = "group",
                                        order_by = "injection.order",
                                        percentage = TRUE) +
  theme(axis.text.x = element_text(size = 2)) +
  scale_size_continuous(range = c(0.1, 2)) +
  ggsci::scale_color_aaas()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-42-1.png" width="100%" />

Detect outlier samples.


```r
outlier_samples <-
  object_pos %>%
  `+`(1) %>% 
  log(2) %>%
  scale() %>%
  detect_outlier()

outlier_samples
#> -------------------- 
#> masscleaner 
#> -------------------- 
#> 1 according_to_na : 0 outlier samples.
#> 2 according_to_pc_sd : 0 outlier samples.
#> 3 according_to_pc_mad : 0 outlier samples.
#> 4 accordint_to_distance : 0 outlier samples.
#> extract all outlier table using extract_outlier_table()
```


```r
outlier_table <-
extract_outlier_table(outlier_samples)

outlier_table %>% 
  head()
#>            according_to_na pc_sd pc_mad accordint_to_distance
#> sample_06            FALSE FALSE  FALSE                 FALSE
#> sample_103           FALSE FALSE  FALSE                 FALSE
#> sample_11            FALSE FALSE  FALSE                 FALSE
#> sample_112           FALSE FALSE  FALSE                 FALSE
#> sample_117           FALSE FALSE  FALSE                 FALSE
#> sample_12            FALSE FALSE  FALSE                 FALSE

outlier_table %>% 
  apply(1, function(x){
    sum(x)
  }) %>% 
  `>`(0) %>% 
  which()
#> named integer(0)
```

No outlier samples in positive mode.

### Negative mode


```r
massdataset::show_sample_missing_values(object = object_neg,
                                        color_by = "group",
                                        order_by = "injection.order",
                                        percentage = TRUE) +
  theme(axis.text.x = element_text(size = 2)) +
  scale_size_continuous(range = c(0.1, 2)) +
  ggsci::scale_color_aaas()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-45-1.png" width="100%" />

Detect outlier samples.


```r
outlier_samples <-
  object_neg %>%
  `+`(1) %>% 
  log(2) %>%
  scale() %>%
  detect_outlier()

outlier_samples
#> -------------------- 
#> masscleaner 
#> -------------------- 
#> 1 according_to_na : 0 outlier samples.
#> 2 according_to_pc_sd : 0 outlier samples.
#> 3 according_to_pc_mad : 0 outlier samples.
#> 4 accordint_to_distance : 0 outlier samples.
#> extract all outlier table using extract_outlier_table()
```


```r
outlier_table <-
extract_outlier_table(outlier_samples)

outlier_table %>% 
  head()
#>            according_to_na pc_sd pc_mad accordint_to_distance
#> sample_06            FALSE FALSE  FALSE                 FALSE
#> sample_103           FALSE FALSE  FALSE                 FALSE
#> sample_11            FALSE FALSE  FALSE                 FALSE
#> sample_112           FALSE FALSE  FALSE                 FALSE
#> sample_117           FALSE FALSE  FALSE                 FALSE
#> sample_12            FALSE FALSE  FALSE                 FALSE

outlier_table %>% 
  apply(1, function(x){
    sum(x)
  }) %>% 
  `>`(0) %>% 
  which()
#> named integer(0)
```

No outlier samples in negative mode.

## Missing value imputation


```r
get_mv_number(object_pos)
#> [1] 148965
object_pos <- 
  impute_mv(object = object_pos, method = "knn")
#> Cluster size 4983 broken into 88 4895 
#> Done cluster 88 
#> Cluster size 4895 broken into 4497 398 
#> Cluster size 4497 broken into 3737 760 
#> Cluster size 3737 broken into 2703 1034 
#> Cluster size 2703 broken into 1706 997 
#> Cluster size 1706 broken into 1240 466 
#> Done cluster 1240 
#> Done cluster 466 
#> Done cluster 1706 
#> Done cluster 997 
#> Done cluster 2703 
#> Done cluster 1034 
#> Done cluster 3737 
#> Done cluster 760 
#> Done cluster 4497 
#> Done cluster 398 
#> Done cluster 4895

get_mv_number(object_pos)
#> [1] 0
```



```r
get_mv_number(object_neg)
#> [1] 146427

object_neg <- 
  impute_mv(object = object_neg, method = "knn")
#> Cluster size 4006 broken into 3965 41 
#> Cluster size 3965 broken into 3743 222 
#> Cluster size 3743 broken into 505 3238 
#> Done cluster 505 
#> Cluster size 3238 broken into 2519 719 
#> Cluster size 2519 broken into 1721 798 
#> Cluster size 1721 broken into 676 1045 
#> Done cluster 676 
#> Done cluster 1045 
#> Done cluster 1721 
#> Done cluster 798 
#> Done cluster 2519 
#> Done cluster 719 
#> Done cluster 3238 
#> Done cluster 3743 
#> Done cluster 222 
#> Done cluster 3965 
#> Done cluster 41

get_mv_number(object_neg)
#> [1] 0
```

## Data normalization and integration

### Positive mode


```r
object_pos <- 
  normalize_data(object_pos, method = "median")

object_pos2 <- 
  integrate_data(object_pos, method = "subject_median")
```


```r
object_pos2 %>% 
  `+`(1) %>% 
  log(2) %>% 
  massqc::massqc_pca(color_by = "batch", line = FALSE)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-51-1.png" width="100%" />

### Negative mode


```r
object_neg <- 
  normalize_data(object_neg, method = "median")

object_neg2 <- 
  integrate_data(object_neg, method = "subject_median")
```


```r
object_neg2 %>% 
  `+`(1) %>% 
  log(2) %>% 
  massqc::massqc_pca(color_by = "batch", line = FALSE)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-53-1.png" width="100%" />

---

# Metabolite annotation

## Add MS2 spectra to object

Download the [MS2 data here](https://drive.google.com/file/d/1FY3x7q5i1g3oSLaVej__ZaZ4XSX-mWBQ/view?usp=sharing).

Uncompress it.

![](figures/fig7.png)

### Positive mode


```r
object_pos2 <-
  mutate_ms2(
    object = object_pos2,
    column = "rp",
    polarity = "positive",
    ms1.ms2.match.mz.tol = 15,
    ms1.ms2.match.rt.tol = 30,
    path = "mgf_ms2_data/POS"
  )
```

### Negative mode


```r
object_neg2 <-
  mutate_ms2(
    object = object_neg2,
    column = "rp",
    polarity = "negative",
    ms1.ms2.match.mz.tol = 15,
    ms1.ms2.match.rt.tol = 30,
    path = "mgf_ms2_data/NEG"
  )
```

## Metabolite annotation using `metid`

Metabolite annotation is based on the [`metid` package](https://tidymass.github.io/metid/).

### Download database

We need to download MS2 database from `metid` [website](https://tidymass.github.io/metid/articles/public_databases.html).

Here we download the `Michael Snyder RPLC databases`, `Orbitrap database` and `MoNA database`. And place them in a new folder named as `metabolite_annotation`.

![](figures/fig8.png)

### Positive mode

#### Annotate features using `snyder_database_rplc0.0.3`.


```r
load("metabolite_annotation/snyder_database_rplc0.0.3.rda")
```


```r
object_pos2 = 
  annotate_metabolites_mass_dataset(object = object_pos2, 
                                    ms1.match.ppm = 15, 
                                    rt.match.tol = 30, 
                                    polarity = "positive",
                                    database = snyder_database_rplc0.0.3)
```

#### Annotate features using `orbitrap_database0.0.3`.


```r
load("metabolite_annotation/orbitrap_database0.0.3.rda")
```


```r
object_pos2 = 
  annotate_metabolites_mass_dataset(object = object_pos2, 
                                    ms1.match.ppm = 15, 
                                    polarity = "positive",
                                    database = orbitrap_database0.0.3)
```

#### Annotate features using `mona_database0.0.3`.


```r
load("metabolite_annotation/mona_database0.0.3.rda")
```


```r
object_pos2 = 
  annotate_metabolites_mass_dataset(object = object_pos2, 
                                    ms1.match.ppm = 15, 
                                    polarity = "positive",
                                    database = mona_database0.0.3)
```

### Negative mode

#### Annotate features using `snyder_database_rplc0.0.3`.


```r
object_neg2 = 
  annotate_metabolites_mass_dataset(object = object_neg2, 
                                    ms1.match.ppm = 15, 
                                    rt.match.tol = 30,
                                    polarity = "negative",
                                    database = snyder_database_rplc0.0.3)
```


#### Annotate features using `orbitrap_database0.0.3`.


```r
object_neg2 = 
  annotate_metabolites_mass_dataset(object = object_neg2, 
                                    ms1.match.ppm = 15, 
                                    polarity = "negative",
                                    database = orbitrap_database0.0.3)
```

#### Annotate features using `mona_database0.0.3`.


```r
object_neg2 = 
  annotate_metabolites_mass_dataset(object = object_neg2, 
                                    ms1.match.ppm = 15, 
                                    polarity = "negative",
                                    database = mona_database0.0.3)
```




## Annotation result


```r
head(extract_annotation_table(object = object_pos2))
#>    variable_id
#> 1 M100T160_POS
#> 2 M103T100_POS
#> 3 M103T100_POS
#> 4  M104T51_POS
#> 5 M113T187_POS
#> 6  M113T81_POS
#>                                                                                                                      ms2_files_id
#> 1 QEP_SGA_QC_posi_ms2_ce25_01.mgf;QEP_SGA_QC_posi_ms2_ce25_02.mgf;QEP_SGA_QC_posi_ms2_ce50_01.mgf;QEP_SGA_QC_posi_ms2_ce50_02.mgf
#> 2 QEP_SGA_QC_posi_ms2_ce25_01.mgf;QEP_SGA_QC_posi_ms2_ce25_02.mgf;QEP_SGA_QC_posi_ms2_ce50_01.mgf;QEP_SGA_QC_posi_ms2_ce50_02.mgf
#> 3 QEP_SGA_QC_posi_ms2_ce25_01.mgf;QEP_SGA_QC_posi_ms2_ce25_02.mgf;QEP_SGA_QC_posi_ms2_ce50_01.mgf;QEP_SGA_QC_posi_ms2_ce50_02.mgf
#> 4 QEP_SGA_QC_posi_ms2_ce25_01.mgf;QEP_SGA_QC_posi_ms2_ce25_02.mgf;QEP_SGA_QC_posi_ms2_ce50_01.mgf;QEP_SGA_QC_posi_ms2_ce50_02.mgf
#> 5 QEP_SGA_QC_posi_ms2_ce25_01.mgf;QEP_SGA_QC_posi_ms2_ce25_02.mgf;QEP_SGA_QC_posi_ms2_ce50_01.mgf;QEP_SGA_QC_posi_ms2_ce50_02.mgf
#> 6 QEP_SGA_QC_posi_ms2_ce25_01.mgf;QEP_SGA_QC_posi_ms2_ce25_02.mgf;QEP_SGA_QC_posi_ms2_ce50_01.mgf;QEP_SGA_QC_posi_ms2_ce50_02.mgf
#>                  ms2_spectrum_id           Compound.name    CAS.ID   HMDB.ID
#> 1 mz100.076248168945rt158.377638  N-Methyl-2-pyrrolidone  872-50-4      <NA>
#> 2   mz103.054814801682rt96.92601      Phenylacetaldehyde  122-78-1 HMDB06236
#> 3   mz103.054814801682rt96.92601 3-Amino-2-oxazolidinone   80-65-9      <NA>
#> 4  mz104.107467651367rt49.510314      5-Amino-1-pentanol 2508-29-4      <NA>
#> 5 mz113.060150146484rt188.406384    1,4-Cyclohexanedione      <NA>      <NA>
#> 6   mz113.035087585449rt77.20827                  URACIL      <NA>      <NA>
#>   KEGG.ID     Lab.ID     Adduct  mz.error mz.match.score RT.error
#> 1  C11118 MONA_11509     (M+H)+  1.335652      0.9960435       NA
#> 2  C00601    NO07389 (M+H-H2O)+  1.537004      0.9947640       NA
#> 3    <NA>    NO07231     (M+H)+ 11.537004      0.7439487       NA
#> 4    <NA>    NO07238     (M+H)+  1.169128      0.9969671       NA
#> 5    <NA> MONA_14519     (M+H)+  1.051626      0.9975454       NA
#> 6    <NA> MONA_18148     (M+H)+  1.275544      0.9963909       NA
#>   RT.match.score                  CE        SS Total.score   Database Level
#> 1             NA       35  (nominal) 0.6871252   0.8029696 MoNA_0.0.1     2
#> 2             NA                  10 0.5748835   0.7323387 NIST_0.0.1     2
#> 3             NA                  20 0.5020256   0.5927468 NIST_0.0.1     2
#> 4             NA                   5 0.5971697   0.7470937 NIST_0.0.1     2
#> 5             NA HCD (NCE 20-30-40%) 0.5401414   0.7116679 MoNA_0.0.1     2
#> 6             NA                  10 0.6889885   0.8042644 MoNA_0.0.1     2
```



```r
variable_info_pos <- 
extract_variable_info(object = object_pos2)
head(variable_info_pos)
#>   variable_id       mz        rt    na_freq  na_freq.1  na_freq.2 Compound.name
#> 1  M70T53_POS 70.06596  52.78542 0.00000000 0.14545455 0.00000000          <NA>
#> 2 M70T527_POS 70.36113 526.76657 0.02564103 0.18181818 0.30000000          <NA>
#> 3 M71T775_POS 70.98125 775.44867 0.00000000 0.00000000 0.00000000          <NA>
#> 4 M71T669_POS 70.98125 668.52844 0.00000000 0.02727273 0.01818182          <NA>
#> 5 M71T715_POS 70.98125 714.74066 0.05128205 0.12727273 0.02727273          <NA>
#> 6  M71T54_POS 71.04999  54.45641 0.15384615 0.99090909 0.05454545          <NA>
#>   CAS.ID HMDB.ID KEGG.ID Lab.ID Adduct mz.error mz.match.score RT.error
#> 1   <NA>    <NA>    <NA>   <NA>   <NA>       NA             NA       NA
#> 2   <NA>    <NA>    <NA>   <NA>   <NA>       NA             NA       NA
#> 3   <NA>    <NA>    <NA>   <NA>   <NA>       NA             NA       NA
#> 4   <NA>    <NA>    <NA>   <NA>   <NA>       NA             NA       NA
#> 5   <NA>    <NA>    <NA>   <NA>   <NA>       NA             NA       NA
#> 6   <NA>    <NA>    <NA>   <NA>   <NA>       NA             NA       NA
#>   RT.match.score   CE SS Total.score Database Level
#> 1             NA <NA> NA          NA     <NA>    NA
#> 2             NA <NA> NA          NA     <NA>    NA
#> 3             NA <NA> NA          NA     <NA>    NA
#> 4             NA <NA> NA          NA     <NA>    NA
#> 5             NA <NA> NA          NA     <NA>    NA
#> 6             NA <NA> NA          NA     <NA>    NA
table(variable_info_pos$Level)
#> 
#>   1   2 
#>  23 183
table(variable_info_pos$Database)
#> 
#> MoNA_0.0.1   MS_0.0.2 NIST_0.0.1 
#>         78         23        105
```


```r
ms2_plot_mass_dataset(object = object_pos2, 
                      variable_id = "M86T95_POS", 
                      database = mona_database0.0.3)
#> $M86T95_POS_1
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-68-1.png" width="100%" />

```
#> 
#> $M86T95_POS_2
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-68-2.png" width="100%" />

```
#> 
#> $M86T95_POS_3
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-68-3.png" width="100%" />


```r
ms2_plot_mass_dataset(object = object_pos2, 
                      variable_id = "M147T54_POS", 
                      database = snyder_database_rplc0.0.3, 
                      interactive_plot = FALSE)
```
---

# Statistical analysis

## Remove the features without annotations

### Positive mode.


```r
object_pos2 <- 
  object_pos2 %>% 
  activate_mass_dataset(what = "annotation_table") %>% 
  filter(!is.na(Level)) %>% 
  filter(Level == 1 | Level == 2)
```


```r
object_pos2
#> -------------------- 
#> massdataset version: 0.99.1 
#> -------------------- 
#> 1.expression_data:[ 206 x 259 data.frame]
#> 2.sample_info:[ 259 x 6 data.frame]
#> 3.variable_info:[ 206 x 6 data.frame]
#> 4.sample_info_note:[ 6 x 2 data.frame]
#> 5.variable_info_note:[ 6 x 2 data.frame]
#> 6.ms2_data:[ 1042 variables x 951 MS2 spectra]
#> -------------------- 
#> Processing information (extract_process_info())
#> create_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset create_mass_dataset() 2022-01-16 16:19:04
#> process_data ---------- 
#>         Package Function.used                Time
#> 1 massprocesser  process_data 2022-01-16 16:18:43
#> mutate ---------- 
#>       Package Function.used                Time
#> 1 massdataset      mutate() 2022-01-16 23:48:08
#> mutate_variable_na_freq ---------- 
#>       Package             Function.used                Time
#> 1 massdataset mutate_variable_na_freq() 2022-01-18 09:11:43
#> 2 massdataset mutate_variable_na_freq() 2022-01-18 09:11:43
#> 3 massdataset mutate_variable_na_freq() 2022-01-18 09:11:43
#> filter ---------- 
#>       Package Function.used                Time
#> 1 massdataset      filter() 2022-01-18 09:11:44
#> 2 massdataset      filter() 2022-03-11 01:18:10
#> 3 massdataset      filter() 2022-03-11 01:18:10
#> impute_mv ---------- 
#>       Package Function.used                Time
#> 1 masscleaner   impute_mv() 2022-01-18 09:38:02
#> normalize_data ---------- 
#>       Package    Function.used                Time
#> 1 masscleaner normalize_data() 2022-01-18 09:38:07
#> integrate_data ---------- 
#>       Package    Function.used                Time
#> 1 masscleaner integrate_data() 2022-01-18 09:38:08
#> update_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset update_mass_dataset() 2022-01-19 21:53:01
#> mutate_ms2 ---------- 
#>       Package Function.used                Time
#> 1 massdataset  mutate_ms2() 2022-01-19 21:53:36
#> annotate_metabolites_mass_dataset ---------- 
#>   Package                       Function.used                Time
#> 1   metid annotate_metabolites_mass_dataset() 2022-02-22 21:16:23
#> 2   metid annotate_metabolites_mass_dataset() 2022-02-22 21:30:10
#> 3   metid annotate_metabolites_mass_dataset() 2022-02-22 21:47:59
```

### Negative mode.


```r
object_neg2 <- 
  object_neg2 %>% 
  activate_mass_dataset(what = "annotation_table") %>% 
  filter(!is.na(Level)) %>% 
  filter(Level == 1 | Level == 2)
```


```r
object_neg2
#> -------------------- 
#> massdataset version: 0.99.1 
#> -------------------- 
#> 1.expression_data:[ 165 x 259 data.frame]
#> 2.sample_info:[ 259 x 6 data.frame]
#> 3.variable_info:[ 165 x 6 data.frame]
#> 4.sample_info_note:[ 6 x 2 data.frame]
#> 5.variable_info_note:[ 6 x 2 data.frame]
#> 6.ms2_data:[ 1092 variables x 988 MS2 spectra]
#> -------------------- 
#> Processing information (extract_process_info())
#> create_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset create_mass_dataset() 2022-01-16 16:20:02
#> process_data ---------- 
#>         Package Function.used                Time
#> 1 massprocesser  process_data 2022-01-16 16:19:48
#> mutate ---------- 
#>       Package Function.used                Time
#> 1 massdataset      mutate() 2022-01-16 23:48:08
#> mutate_variable_na_freq ---------- 
#>       Package             Function.used                Time
#> 1 massdataset mutate_variable_na_freq() 2022-01-18 09:11:47
#> 2 massdataset mutate_variable_na_freq() 2022-01-18 09:11:47
#> 3 massdataset mutate_variable_na_freq() 2022-01-18 09:11:47
#> filter ---------- 
#>       Package Function.used                Time
#> 1 massdataset      filter() 2022-01-18 09:11:47
#> 2 massdataset      filter() 2022-03-11 01:18:10
#> 3 massdataset      filter() 2022-03-11 01:18:10
#> impute_mv ---------- 
#>       Package Function.used                Time
#> 1 masscleaner   impute_mv() 2022-01-18 09:38:06
#> normalize_data ---------- 
#>       Package    Function.used                Time
#> 1 masscleaner normalize_data() 2022-01-18 09:50:47
#> integrate_data ---------- 
#>       Package    Function.used                Time
#> 1 masscleaner integrate_data() 2022-01-18 09:50:47
#> update_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset update_mass_dataset() 2022-01-19 21:53:37
#> mutate_ms2 ---------- 
#>       Package Function.used                Time
#> 1 massdataset  mutate_ms2() 2022-01-19 21:54:06
#> annotate_metabolites_mass_dataset ---------- 
#>   Package                       Function.used                Time
#> 1   metid annotate_metabolites_mass_dataset() 2022-02-22 21:50:29
#> 2   metid annotate_metabolites_mass_dataset() 2022-02-22 22:08:10
#> 3   metid annotate_metabolites_mass_dataset() 2022-02-22 22:26:33
```

## Merge data

We need to merge positive and negative mode data as one `mass_dataset` class.


```r
head(colnames(object_pos2))
#> [1] "sample_06"  "sample_103" "sample_11"  "sample_112" "sample_117"
#> [6] "sample_12"
```



```r
head(colnames(object_neg2))
#> [1] "sample_06"  "sample_103" "sample_11"  "sample_112" "sample_117"
#> [6] "sample_12"
```


```r
object <- 
merge_mass_dataset(x = object_pos2, 
                   y = object_neg2, 
                   sample_direction = "inner",
                   variable_direction = "full", 
                   sample_by = "sample_id", 
                   variable_by = c("variable_id", "mz", "rt"))
```


```r
object
#> -------------------- 
#> massdataset version: 0.99.20 
#> -------------------- 
#> 1.expression_data:[ 371 x 259 data.frame]
#> 2.sample_info:[ 259 x 11 data.frame]
#> 3.variable_info:[ 371 x 9 data.frame]
#> 4.sample_info_note:[ 11 x 2 data.frame]
#> 5.variable_info_note:[ 9 x 2 data.frame]
#> 6.ms2_data:[ 2084 variables x 1902 MS2 spectra]
#> -------------------- 
#> Processing information (extract_process_info())
#> create_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset create_mass_dataset() 2022-01-16 16:19:04
#> process_data ---------- 
#>         Package Function.used                Time
#> 1 massprocesser  process_data 2022-01-16 16:18:43
#> mutate ---------- 
#>       Package Function.used                Time
#> 1 massdataset      mutate() 2022-01-16 23:48:08
#> mutate_variable_na_freq ---------- 
#>       Package             Function.used                Time
#> 1 massdataset mutate_variable_na_freq() 2022-01-18 09:11:43
#> 2 massdataset mutate_variable_na_freq() 2022-01-18 09:11:43
#> 3 massdataset mutate_variable_na_freq() 2022-01-18 09:11:43
#> filter ---------- 
#>       Package Function.used                Time
#> 1 massdataset      filter() 2022-01-18 09:11:44
#> 2 massdataset      filter() 2022-03-11 01:18:10
#> 3 massdataset      filter() 2022-03-11 01:18:10
#> impute_mv ---------- 
#>       Package Function.used                Time
#> 1 masscleaner   impute_mv() 2022-01-18 09:38:02
#> normalize_data ---------- 
#>       Package    Function.used                Time
#> 1 masscleaner normalize_data() 2022-01-18 09:38:07
#> integrate_data ---------- 
#>       Package    Function.used                Time
#> 1 masscleaner integrate_data() 2022-01-18 09:38:08
#> update_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset update_mass_dataset() 2022-01-19 21:53:01
#> mutate_ms2 ---------- 
#>       Package Function.used                Time
#> 1 massdataset  mutate_ms2() 2022-01-19 21:53:36
#> annotate_metabolites_mass_dataset ---------- 
#>   Package                       Function.used                Time
#> 1   metid annotate_metabolites_mass_dataset() 2022-02-22 21:16:23
#> 2   metid annotate_metabolites_mass_dataset() 2022-02-22 21:30:10
#> 3   metid annotate_metabolites_mass_dataset() 2022-02-22 21:47:59
#> create_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset create_mass_dataset() 2022-01-16 16:20:02
#> process_data ---------- 
#>         Package Function.used                Time
#> 1 massprocesser  process_data 2022-01-16 16:19:48
#> mutate ---------- 
#>       Package Function.used                Time
#> 1 massdataset      mutate() 2022-01-16 23:48:08
#> mutate_variable_na_freq ---------- 
#>       Package             Function.used                Time
#> 1 massdataset mutate_variable_na_freq() 2022-01-18 09:11:47
#> 2 massdataset mutate_variable_na_freq() 2022-01-18 09:11:47
#> 3 massdataset mutate_variable_na_freq() 2022-01-18 09:11:47
#> filter ---------- 
#>       Package Function.used                Time
#> 1 massdataset      filter() 2022-01-18 09:11:47
#> 2 massdataset      filter() 2022-03-11 01:18:10
#> 3 massdataset      filter() 2022-03-11 01:18:10
#> impute_mv ---------- 
#>       Package Function.used                Time
#> 1 masscleaner   impute_mv() 2022-01-18 09:38:06
#> normalize_data ---------- 
#>       Package    Function.used                Time
#> 1 masscleaner normalize_data() 2022-01-18 09:50:47
#> integrate_data ---------- 
#>       Package    Function.used                Time
#> 1 masscleaner integrate_data() 2022-01-18 09:50:47
#> update_mass_dataset ---------- 
#>       Package         Function.used                Time
#> 1 massdataset update_mass_dataset() 2022-01-19 21:53:37
#> mutate_ms2 ---------- 
#>       Package Function.used                Time
#> 1 massdataset  mutate_ms2() 2022-01-19 21:54:06
#> annotate_metabolites_mass_dataset ---------- 
#>   Package                       Function.used                Time
#> 1   metid annotate_metabolites_mass_dataset() 2022-02-22 21:50:29
#> 2   metid annotate_metabolites_mass_dataset() 2022-02-22 22:08:10
#> 3   metid annotate_metabolites_mass_dataset() 2022-02-22 22:26:33
#> merge_mass_dataset ---------- 
#>       Package      Function.used                Time
#> 1 massdataset merge_mass_dataset 2022-03-11 01:18:11
```

## Trace processing information of object

Then we can use the `report_parameters()` function to trace processing information of object.

All the analysis results will be placed in a folder named as `statistical_analysis`.


```r
dir.create(path = "statistical_analysis", showWarnings = FALSE)
```


```r
report_parameters(object = object, path = "statistical_analysis/")
```

A html format file named as `parameter_report.html` will be generated.

![](figures/fig9.png)

## Remove redundant metabolites

Remove the redundant annotated metabolites bases on `Level` and score.


```r
object <-
  object %>% 
  activate_mass_dataset(what = "annotation_table") %>% 
  group_by(Compound.name) %>% 
  filter(Level == min(Level)) %>% 
  filter(SS == max(SS)) %>% 
  slice_head(n = 1)
```

## Differential expression metabolites

### Calculate the fold changes.


```r
control_sample_id =
  object %>%
  activate_mass_dataset(what = "sample_info") %>%
  filter(group == "Control") %>%
  pull(sample_id)

case_sample_id =
  object %>%
  activate_mass_dataset(what = "sample_info") %>%
  filter(group == "Case") %>%
  pull(sample_id)
```


```r
object <-
  mutate_fc(object = object, 
            control_sample_id = control_sample_id, 
            case_sample_id = case_sample_id, 
            mean_median = "mean")
#> 110 control samples.
#> 110 case samples.
```


### Calculate p values.


```r
object <-
  mutate_p_value(
    object = object,
    control_sample_id = control_sample_id,
    case_sample_id = case_sample_id,
    method = "t.test",
    p_adjust_methods = "BH"
  )
#> 110 control samples.
#> 110 case samples.
```

### Volcano plot.


```r
volcano_plot(object = object,
             add_text = TRUE,
             text_from = "Compound.name", 
             point_size_scale = "p_value") +
  scale_size_continuous(range = c(0.5, 5))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-84-1.png" width="100%" />

## Output result

Output the differential expression metabolites.


```r
differential_metabolites <- 
  extract_variable_info(object = object) %>% 
  filter(fc > 2 | fc < 0.5) %>% 
  filter(p_value_adjust < 0.05)

readr::write_csv(differential_metabolites, 
                 file = "statistical_analysis/differential_metabolites.csv")
```


# Pathway enrichment analysis

All the results will be placed in a folder named as `pathway_enrichment`.


```r
dir.create(path = "pathway_enrichment", showWarnings = FALSE)
```


```r
diff_metabolites <-
  object %>% 
  activate_mass_dataset(what = "variable_info") %>% 
  filter(p_value_adjust < 0.05) %>% 
  extract_variable_info()
```


```r
head(diff_metabolites)
#>     variable_id        mz        rt    na_freq  na_freq.1  na_freq.2 na_freq_2
#> 1    M86T95_POS  86.09716  94.57264 0.02564103 0.55454545 0.29090909        NA
#> 2 M95T100_1_POS  95.04975  99.77637 0.00000000 0.00000000 0.00000000        NA
#> 3  M103T100_POS 103.05477  99.90601 0.00000000 0.00000000 0.00000000        NA
#> 4   M104T51_POS 104.10746  51.27993 0.00000000 0.07272727 0.00000000        NA
#> 5   M113T81_POS 113.03501  80.73506 0.00000000 0.00000000 0.00000000        NA
#> 6  M113T187_POS 113.06018 186.56470 0.02564103 0.01818182 0.02727273        NA
#>   na_freq.1_2 na_freq.2_2       fc      p_value p_value_adjust
#> 1          NA          NA 1.714629 3.034938e-20   4.549454e-19
#> 2          NA          NA 1.439791 6.761432e-31   3.673711e-29
#> 3          NA          NA 1.345809 5.215317e-19   6.800774e-18
#> 4          NA          NA 1.751085 2.695171e-08   1.220314e-07
#> 5          NA          NA 1.500756 5.966911e-22   1.296809e-20
#> 6          NA          NA 1.951417 1.848299e-17   1.882955e-16
#>             Compound.name    CAS.ID HMDB.ID KEGG.ID     Lab.ID Adduct  mz.error
#> 1              Piperidine  110-89-4    <NA>  C01746  MONA_2852 (M+H)+  1.746869
#> 2                  Phenol  108-95-2    <NA>  D01960 MONA_18506 (M+H)+  1.416428
#> 3 3-Amino-2-oxazolidinone   80-65-9    <NA>    <NA>    NO07231 (M+H)+ 11.537004
#> 4      5-Amino-1-pentanol 2508-29-4    <NA>    <NA>    NO07238 (M+H)+  1.169128
#> 5                  Uracil      <NA>    <NA>    <NA> MONA_15328 (M+H)+  1.275544
#> 6    1,4-Cyclohexanedione      <NA>    <NA>    <NA> MONA_14519 (M+H)+  1.051626
#>   mz.match.score RT.error RT.match.score                  CE        SS
#> 1      0.9932417       NA             NA                  30 0.6143541
#> 2      0.9955515       NA             NA                  10 0.6102452
#> 3      0.7439487       NA             NA                  20 0.5020256
#> 4      0.9969671       NA             NA                   5 0.5971697
#> 5      0.9963909       NA             NA HCD (NCE 20-30-40%) 0.6748260
#> 6      0.9975454       NA             NA HCD (NCE 20-30-40%) 0.5401414
#>   Total.score   Database Level
#> 1   0.7564369 MoNA_0.0.1     2
#> 2   0.7547351 MoNA_0.0.1     2
#> 3   0.5927468 NIST_0.0.1     2
#> 4   0.7470937 NIST_0.0.1     2
#> 5   0.7954128 MoNA_0.0.1     2
#> 6   0.7116679 MoNA_0.0.1     2
```

## Load `KEGG` human pathway database


```r
data("kegg_hsa_pathway", package = "metpath")
kegg_hsa_pathway
#> ---------Pathway source&version---------
#> KEGG & 2021-12-13 
#> -----------Pathway information------------
#> 345 pathways 
#> 334 pathways have genes 
#> 0 pathways have proteins 
#> 281 pathways have compounds 
#> Pathway class (top 10): Metabolism; Carbohydrate metabolism;Metabolism; Lipid metabolism
```

```r
get_pathway_class(kegg_hsa_pathway)
#> # A tibble: 43 × 2
#>    class                                                                       n
#>    <chr>                                                                   <int>
#>  1 Cellular Processes; Cell growth and death                                   8
#>  2 Cellular Processes; Cell motility                                           1
#>  3 Cellular Processes; Cellular community - eukaryotes                         5
#>  4 Cellular Processes; Transport and catabolism                                7
#>  5 Environmental Information Processing; Membrane transport                    1
#>  6 Environmental Information Processing; Signal transduction                  26
#>  7 Environmental Information Processing; Signaling molecules and interact…     5
#>  8 Genetic Information Processing; Folding, sorting and degradation            7
#>  9 Genetic Information Processing; Replication and repair                      7
#> 10 Genetic Information Processing; Transcription                               3
#> # … with 33 more rows
```

## Remove the disease pathways:


```r
#get the class of pathways
pathway_class <- 
  metpath::pathway_class(kegg_hsa_pathway)
head(pathway_class)
#> $hsa00010
#> [1] "Metabolism; Carbohydrate metabolism"
#> 
#> $hsa00020
#> [1] "Metabolism; Carbohydrate metabolism"
#> 
#> $hsa00030
#> [1] "Metabolism; Carbohydrate metabolism"
#> 
#> $hsa00040
#> [1] "Metabolism; Carbohydrate metabolism"
#> 
#> $hsa00051
#> [1] "Metabolism; Carbohydrate metabolism"
#> 
#> $hsa00052
#> [1] "Metabolism; Carbohydrate metabolism"
```



```r
remain_idx <-
  pathway_class %>%
  unlist() %>%
  stringr::str_detect("Disease") %>%
  `!`() %>%
  which()

remain_idx
#>   [1]   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18
#>  [19]  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35  36
#>  [37]  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53  54
#>  [55]  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71  72
#>  [73]  73  74  75  76  77  78  79  80  81  82  83  84  85  90  91  92  93  94
#>  [91]  95  96  97  98  99 100 101 102 103 104 105 106 107 108 109 110 111 112
#> [109] 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130
#> [127] 131 132 133 134 135 136 137 138 139 140 141 142 143 144 145 146 147 148
#> [145] 149 150 151 152 153 154 155 156 157 158 159 160 161 162 163 164 165 166
#> [163] 167 168 169 170 171 172 173 174 175 176 177 178 179 180 181 182 183 184
#> [181] 185 186 187 188 189 190 191 192 193 194 195 196 197 198 199 200 201 202
#> [199] 203 204 205 206 207 208 209 210 211 212 213 214 215 216 217 218 219 220
#> [217] 221 222 223 224 225 226 227 228 229 230 236 240 241 242 243 244 245 246
#> [235] 247 248 249 250 251 252 253 254
```



```r
pathway_database <-
  kegg_hsa_pathway[remain_idx]
pathway_database
#> ---------Pathway source&version---------
#> KEGG & 2021-12-13 
#> -----------Pathway information------------
#> 242 pathways 
#> 235 pathways have genes 
#> 0 pathways have proteins 
#> 191 pathways have compounds 
#> Pathway class (top 10): Metabolism; Carbohydrate metabolism;Metabolism; Lipid metabolism
```


## Pathway enrichment


```r
kegg_id <-
  diff_metabolites$KEGG.ID 
kegg_id <-
  kegg_id[!is.na(kegg_id)]
kegg_id
#>  [1] "C01746" "D01960" "C00148" "C00153" "C01108" "C00906" "C10438" "C00300"
#>  [9] "C00407" "C02505" "C14790" "C08493" "C02237" "C00073" "C05842" "C00637"
#> [17] "D00022" "C07481" "C17846" "D00029" "C00399" "C08362" "C14214" "C06427"
#> [25] "C01595" "C00410" "C10523" "C01780" "C00762" "C00735" "C17337" "C01921"
#> [33] "C04230" "C06539" "C00186" "C01546" "C00490" "C02226" "C00064" "C06104"
#> [41] "C02612" "C07599" "C01601" "C00079" "C07130" "C05635" "C00864" "C16038"
#> [49] "C08322" "C16308" "C10911" "C06429" "C05498" "C05472" "C04555"
```


```r
result <- 
enrich_kegg(query_id = kegg_id, 
            query_type = "compound", 
            id_type = "KEGG",
            pathway_database = pathway_database, 
            p_cutoff = 0.05, 
            p_adjust_method = "BH", 
            threads = 3)
#> 191 pathways.
```

## Check the result:


```r
result
#> ---------Pathway database&version---------
#> KEGG & 0.99.4 
#> -----------Enrichment result------------
#> 191 pathways are enriched 
#> 10 pathways p-values < 0.05 
#> Steroid hormone biosynthesis
#> Aldosterone-regulated sodium reabsorption
#> Aminoacyl-tRNA biosynthesis
#> D-Amino acid metabolism
#> Valine, leucine and isoleucine biosynthesis ... (only top 5 shows)
#> -----------Parameters------------
#>   Package Function.used                Time
#> 1 metpath enrich_kegg() 2022-03-11 01:18:51
```


## Plot to show pathway enrichment result


```r
enrich_bar_plot(object = result,
                x_axis = "p_value",
                cutoff = 0.05)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-97-1.png" width="100%" />


```r
enrich_scatter_plot(object = result, y_axis = "p_value", y_axis_cutoff = 0.05)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-98-1.png" width="100%" />


```r
enrich_network(
  object = result,
  point_size = "p_value",
  p_cutoff = 0.05,
  only_significant_pathway = TRUE
)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-99-1.png" width="100%" />

# Session information


```r
sessionInfo()
#> R version 4.1.2 (2021-11-01)
#> Platform: x86_64-apple-darwin17.0 (64-bit)
#> Running under: macOS Big Sur 10.16
#> 
#> Matrix products: default
#> BLAS:   /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRblas.0.dylib
#> LAPACK: /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRlapack.dylib
#> 
#> locale:
#> [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
#> 
#> attached base packages:
#> [1] stats4    stats     graphics  grDevices utils     datasets  methods  
#> [8] base     
#> 
#> other attached packages:
#>  [1] forcats_0.5.1.9000   stringr_1.4.0        purrr_0.3.4         
#>  [4] readr_2.1.2          tidyr_1.2.0          tibble_3.1.6        
#>  [7] tidyverse_1.3.1      dplyr_1.0.8          metid_1.2.4         
#> [10] metpath_0.99.4       massstat_0.99.13     ggfortify_0.4.14    
#> [13] massqc_0.99.7        masscleaner_0.99.7   xcms_3.16.1         
#> [16] MSnbase_2.20.4       ProtGenerics_1.26.0  S4Vectors_0.32.3    
#> [19] mzR_2.28.0           Rcpp_1.0.8           Biobase_2.54.0      
#> [22] BiocGenerics_0.40.0  BiocParallel_1.28.3  massprocesser_0.99.3
#> [25] ggplot2_3.3.5        masstools_0.99.5     massdataset_0.99.20 
#> [28] tidymass_0.99.6      magrittr_2.0.2      
#> 
#> loaded via a namespace (and not attached):
#>   [1] blogdown_1.7                bit64_4.0.5                
#>   [3] missForest_1.4              knitr_1.37                 
#>   [5] DelayedArray_0.20.0         data.table_1.14.2          
#>   [7] rpart_4.1.16                KEGGREST_1.34.0            
#>   [9] RCurl_1.98-1.5              doParallel_1.0.17          
#>  [11] generics_0.1.2              snow_0.4-4                 
#>  [13] leaflet_2.1.0               preprocessCore_1.56.0      
#>  [15] mixOmics_6.18.1             RANN_2.6.1                 
#>  [17] proxy_0.4-26                future_1.23.0              
#>  [19] bit_4.0.4                   tzdb_0.2.0                 
#>  [21] xml2_1.3.3                  lubridate_1.8.0            
#>  [23] ggsci_2.9                   SummarizedExperiment_1.24.0
#>  [25] assertthat_0.2.1            viridis_0.6.2              
#>  [27] xfun_0.29                   hms_1.1.1                  
#>  [29] jquerylib_0.1.4             evaluate_0.15              
#>  [31] DEoptimR_1.0-10             fansi_1.0.2                
#>  [33] dbplyr_2.1.1                readxl_1.3.1               
#>  [35] igraph_1.2.11               DBI_1.1.2                  
#>  [37] htmlwidgets_1.5.4           MsFeatures_1.3.0           
#>  [39] rARPACK_0.11-0              ellipsis_0.3.2             
#>  [41] RSpectra_0.16-0             crosstalk_1.2.0            
#>  [43] backports_1.4.1             bookdown_0.24              
#>  [45] ggcorrplot_0.1.3            MatrixGenerics_1.6.0       
#>  [47] vctrs_0.3.8                 remotes_2.4.2              
#>  [49] here_1.0.1                  withr_2.4.3                
#>  [51] ggforce_0.3.3               itertools_0.1-3            
#>  [53] robustbase_0.93-9           vroom_1.5.7                
#>  [55] checkmate_2.0.0             cluster_2.1.2              
#>  [57] lazyeval_0.2.2              crayon_1.5.0               
#>  [59] ellipse_0.4.2               labeling_0.4.2             
#>  [61] pkgconfig_2.0.3             tweenr_1.0.2               
#>  [63] GenomeInfoDb_1.30.0         nnet_7.3-17                
#>  [65] rlang_1.0.1                 globals_0.14.0             
#>  [67] lifecycle_1.0.1             affyio_1.64.0              
#>  [69] extrafontdb_1.0             fastDummies_1.6.3          
#>  [71] MassSpecWavelet_1.60.0      modelr_0.1.8               
#>  [73] cellranger_1.1.0            randomForest_4.7-1         
#>  [75] rprojroot_2.0.2             polyclip_1.10-0            
#>  [77] matrixStats_0.61.0          Matrix_1.4-0               
#>  [79] reprex_2.0.1                base64enc_0.1-3            
#>  [81] GlobalOptions_0.1.2         png_0.1-7                  
#>  [83] viridisLite_0.4.0           rjson_0.2.21               
#>  [85] clisymbols_1.2.0            bitops_1.0-7               
#>  [87] pander_0.6.4                Biostrings_2.62.0          
#>  [89] shape_1.4.6                 parallelly_1.30.0          
#>  [91] robust_0.7-0                jpeg_0.1-9                 
#>  [93] gridGraphics_0.5-1          scales_1.1.1               
#>  [95] plyr_1.8.6                  zlibbioc_1.40.0            
#>  [97] compiler_4.1.2              RColorBrewer_1.1-2         
#>  [99] pcaMethods_1.86.0           clue_0.3-60                
#> [101] rrcov_1.6-2                 cli_3.2.0                  
#> [103] affy_1.72.0                 XVector_0.34.0             
#> [105] listenv_0.8.0               patchwork_1.1.1            
#> [107] pbapply_1.5-0               htmlTable_2.4.0            
#> [109] Formula_1.2-4               MASS_7.3-55                
#> [111] tidyselect_1.1.1            vsn_3.62.0                 
#> [113] stringi_1.7.6               highr_0.9                  
#> [115] yaml_2.3.4                  latticeExtra_0.6-29        
#> [117] MALDIquant_1.21             ggrepel_0.9.1              
#> [119] grid_4.1.2                  sass_0.4.0                 
#> [121] tools_4.1.2                 parallel_4.1.2             
#> [123] circlize_0.4.14             rstudioapi_0.13            
#> [125] MsCoreUtils_1.6.0           foreach_1.5.2              
#> [127] foreign_0.8-82              gridExtra_2.3              
#> [129] farver_2.1.0                mzID_1.32.0                
#> [131] ggraph_2.0.5                rvcheck_0.2.1              
#> [133] digest_0.6.29               BiocManager_1.30.16        
#> [135] GenomicRanges_1.46.1        broom_0.7.12               
#> [137] ncdf4_1.19                  httr_1.4.2                 
#> [139] ComplexHeatmap_2.10.0       colorspace_2.0-2           
#> [141] rvest_1.0.2                 XML_3.99-0.8               
#> [143] fs_1.5.2                    IRanges_2.28.0             
#> [145] splines_4.1.2               yulab.utils_0.0.4          
#> [147] graphlayouts_0.8.0          ggplotify_0.1.0            
#> [149] plotly_4.10.0               fit.models_0.64            
#> [151] jsonlite_1.7.3              tidygraph_1.2.0            
#> [153] corpcor_1.6.10              R6_2.5.1                   
#> [155] Hmisc_4.6-0                 pillar_1.7.0               
#> [157] htmltools_0.5.2             glue_1.6.1                 
#> [159] fastmap_1.1.0               class_7.3-20               
#> [161] codetools_0.2-18            pcaPP_1.9-74               
#> [163] mvtnorm_1.1-3               furrr_0.2.3                
#> [165] utf8_1.2.2                  lattice_0.20-45            
#> [167] bslib_0.3.1                 magick_2.7.3               
#> [169] zip_2.2.0                   openxlsx_4.2.5             
#> [171] Rttf2pt1_1.3.9              survival_3.2-13            
#> [173] limma_3.50.0                rmarkdown_2.11             
#> [175] munsell_0.5.0               e1071_1.7-9                
#> [177] GetoptLong_1.0.5            GenomeInfoDbData_1.2.7     
#> [179] iterators_1.0.14            impute_1.68.0              
#> [181] haven_2.4.3                 reshape2_1.4.4             
#> [183] gtable_0.3.0                extrafont_0.17
```
