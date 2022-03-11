---
title: "Data cleaning"
weight: 10
categories: [tidymass_usage]
author:
- name: Xiaotao Shen (https://www.shenxt.info/)
date: "Created on 2021-12-04 and updated on 2022-03-10"
output:
  html_document:
    number_sections: true
    df_print: paged
    toc: no
  pdf_document:
    toc: no
vignette: >
  %\VignetteIndexEntry{data_cleaning}
  %\VignettePackage{tidymass}
  % \VignetteEngine{knitr::rmarkdown}
  % \usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}
---



## Data preparation

We just use the dataset which are from [this step](https://tidymass.github.io/tidymass/articles/explore_data.html).


```r
library(tidymass)
#> Registered S3 method overwritten by 'Hmisc':
#>   method       from      
#>   vcov.default fit.models
#> ── Attaching packages ───────────────────────────── tidymass 0.99.6 ──
#> ✓ massdataset   0.99.20     ✓ metpath       0.99.4 
#> ✓ massprocesser 0.99.3      ✓ metid         1.2.4  
#> ✓ masscleaner   0.99.7      ✓ masstools     0.99.5 
#> ✓ massqc        0.99.7      ✓ dplyr         1.0.8  
#> ✓ massstat      0.99.13     ✓ ggplot2       3.3.5
#> ── Conflicts ───────────────────────────────── tidymass_conflicts() ──
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
load("data_cleaning/POS/object_pos")
load("data_cleaning/NEG/object_neg")
```
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


```r
object_pos
#> -------------------- 
#> massdataset version: 0.99.8 
#> -------------------- 
#> 1.expression_data:[ 10149 x 259 data.frame]
#> 2.sample_info:[ 259 x 6 data.frame]
#> 3.variable_info:[ 10149 x 3 data.frame]
#> 4.sample_info_note:[ 6 x 2 data.frame]
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
#> mutate ---------- 
#>       Package Function.used                Time
#> 1 massdataset      mutate() 2022-03-10 22:47:29
```


```r
object_neg
#> -------------------- 
#> massdataset version: 0.99.8 
#> -------------------- 
#> 1.expression_data:[ 8804 x 259 data.frame]
#> 2.sample_info:[ 259 x 6 data.frame]
#> 3.variable_info:[ 8804 x 3 data.frame]
#> 4.sample_info_note:[ 6 x 2 data.frame]
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
#> mutate ---------- 
#>       Package Function.used                Time
#> 1 massdataset      mutate() 2022-03-10 22:47:29
```

## Data quality assessment before data cleaning

Here, we can use the `massqc` package to [assess the data quality](https://tidymass.github.io/massqc/).

We can just use the `massqc_report()` function to get a html format quality assessment report.


```r
massqc::massqc_report(object = object_pos,
                      path = "data_cleaning/POS/data_quality_before_data_cleaning")
```

A html format report and pdf figures will be placed in the folder `data_cleaning/POS/data_quality_before_data_cleaning/Report`.

![](figures/Screen-Shot-3.png)

The html report is below:

![](figures/Screen-Shot-6.png)

Negative mode.


```r
massqc::massqc_report(object = object_neg, 
                      path = "data_cleaning/NEG/data_quality_before_data_cleaning")
```

The PCA score plot is used to show the batch effect of positive and negative dataset.

Positive mode:

![](figures/Screen-Shot-4.png)
Negative mode:

![](figures/Screen-Shot-5.png)

We can see that no matter in positive and negative mode, batch effect is serious.

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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.svg" width="672" />


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
#> 1 massdataset      mutate() 2022-03-10 22:47:29
#> mutate_variable_na_freq ---------- 
#>       Package             Function.used                Time
#> 1 massdataset mutate_variable_na_freq() 2022-03-10 22:47:33
#> 2 massdataset mutate_variable_na_freq() 2022-03-10 22:47:33
#> 3 massdataset mutate_variable_na_freq() 2022-03-10 22:47:33
#> filter ---------- 
#>       Package Function.used                Time
#> 1 massdataset      filter() 2022-03-10 22:47:34
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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-12-1.svg" width="672" />


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
#> 1 massdataset      mutate() 2022-03-10 22:47:29
#> mutate_variable_na_freq ---------- 
#>       Package             Function.used                Time
#> 1 massdataset mutate_variable_na_freq() 2022-03-10 22:47:35
#> 2 massdataset mutate_variable_na_freq() 2022-03-10 22:47:35
#> 3 massdataset mutate_variable_na_freq() 2022-03-10 22:47:35
#> filter ---------- 
#>       Package Function.used                Time
#> 1 massdataset      filter() 2022-03-10 22:47:36
```
4104 features left.


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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-15-1.svg" width="672" />

Detect outlier samples.


```r
outlier_samples =
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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-1.svg" width="672" />

Detect outlier samples.


```r
outlier_samples =
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

### Missing value imputation


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

### Data normalization and integration

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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-24-1.svg" width="672" />

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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-26-1.svg" width="672" />

Save data for subsequent analysis.


```r
save(object_pos2, file = "data_cleaning/POS/object_pos2")
save(object_neg2, file = "data_cleaning/NEG/object_neg2")
```

## Session information


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
#>  [1] dplyr_1.0.8          metid_1.2.4          metpath_0.99.4      
#>  [4] massstat_0.99.13     ggfortify_0.4.14     massqc_0.99.7       
#>  [7] masscleaner_0.99.7   xcms_3.16.1          MSnbase_2.20.4      
#> [10] ProtGenerics_1.26.0  S4Vectors_0.32.3     mzR_2.28.0          
#> [13] Rcpp_1.0.8           Biobase_2.54.0       BiocGenerics_0.40.0 
#> [16] BiocParallel_1.28.3  massprocesser_0.99.3 ggplot2_3.3.5       
#> [19] masstools_0.99.5     massdataset_0.99.20  tidymass_0.99.6     
#> [22] magrittr_2.0.2      
#> 
#> loaded via a namespace (and not attached):
#>   [1] blogdown_1.7                tidyr_1.2.0                
#>   [3] missForest_1.4              knitr_1.37                 
#>   [5] DelayedArray_0.20.0         data.table_1.14.2          
#>   [7] rpart_4.1.16                KEGGREST_1.34.0            
#>   [9] RCurl_1.98-1.5              doParallel_1.0.17          
#>  [11] generics_0.1.2              snow_0.4-4                 
#>  [13] leaflet_2.1.0               preprocessCore_1.56.0      
#>  [15] mixOmics_6.18.1             RANN_2.6.1                 
#>  [17] proxy_0.4-26                future_1.23.0              
#>  [19] tzdb_0.2.0                  xml2_1.3.3                 
#>  [21] lubridate_1.8.0             ggsci_2.9                  
#>  [23] SummarizedExperiment_1.24.0 assertthat_0.2.1           
#>  [25] tidyverse_1.3.1             viridis_0.6.2              
#>  [27] xfun_0.29                   hms_1.1.1                  
#>  [29] jquerylib_0.1.4             evaluate_0.15              
#>  [31] DEoptimR_1.0-10             fansi_1.0.2                
#>  [33] dbplyr_2.1.1                readxl_1.3.1               
#>  [35] igraph_1.2.11               DBI_1.1.2                  
#>  [37] htmlwidgets_1.5.4           MsFeatures_1.3.0           
#>  [39] rARPACK_0.11-0              purrr_0.3.4                
#>  [41] ellipsis_0.3.2              RSpectra_0.16-0            
#>  [43] crosstalk_1.2.0             backports_1.4.1            
#>  [45] bookdown_0.24               ggcorrplot_0.1.3           
#>  [47] MatrixGenerics_1.6.0        vctrs_0.3.8                
#>  [49] remotes_2.4.2               here_1.0.1                 
#>  [51] withr_2.4.3                 ggforce_0.3.3              
#>  [53] itertools_0.1-3             robustbase_0.93-9          
#>  [55] checkmate_2.0.0             svglite_2.0.0              
#>  [57] cluster_2.1.2               lazyeval_0.2.2             
#>  [59] crayon_1.5.0                ellipse_0.4.2              
#>  [61] labeling_0.4.2              pkgconfig_2.0.3            
#>  [63] tweenr_1.0.2                GenomeInfoDb_1.30.0        
#>  [65] nnet_7.3-17                 rlang_1.0.1                
#>  [67] globals_0.14.0              lifecycle_1.0.1            
#>  [69] affyio_1.64.0               extrafontdb_1.0            
#>  [71] fastDummies_1.6.3           MassSpecWavelet_1.60.0     
#>  [73] modelr_0.1.8                cellranger_1.1.0           
#>  [75] randomForest_4.7-1          rprojroot_2.0.2            
#>  [77] polyclip_1.10-0             matrixStats_0.61.0         
#>  [79] Matrix_1.4-0                reprex_2.0.1               
#>  [81] base64enc_0.1-3             GlobalOptions_0.1.2        
#>  [83] png_0.1-7                   viridisLite_0.4.0          
#>  [85] rjson_0.2.21                clisymbols_1.2.0           
#>  [87] bitops_1.0-7                pander_0.6.4               
#>  [89] Biostrings_2.62.0           shape_1.4.6                
#>  [91] stringr_1.4.0               parallelly_1.30.0          
#>  [93] robust_0.7-0                readr_2.1.2                
#>  [95] jpeg_0.1-9                  gridGraphics_0.5-1         
#>  [97] scales_1.1.1                plyr_1.8.6                 
#>  [99] zlibbioc_1.40.0             compiler_4.1.2             
#> [101] RColorBrewer_1.1-2          pcaMethods_1.86.0          
#> [103] clue_0.3-60                 rrcov_1.6-2                
#> [105] cli_3.2.0                   affy_1.72.0                
#> [107] XVector_0.34.0              listenv_0.8.0              
#> [109] patchwork_1.1.1             pbapply_1.5-0              
#> [111] htmlTable_2.4.0             Formula_1.2-4              
#> [113] MASS_7.3-55                 tidyselect_1.1.1           
#> [115] vsn_3.62.0                  stringi_1.7.6              
#> [117] forcats_0.5.1.9000          highr_0.9                  
#> [119] yaml_2.3.4                  latticeExtra_0.6-29        
#> [121] MALDIquant_1.21             ggrepel_0.9.1              
#> [123] grid_4.1.2                  sass_0.4.0                 
#> [125] tools_4.1.2                 parallel_4.1.2             
#> [127] circlize_0.4.14             rstudioapi_0.13            
#> [129] MsCoreUtils_1.6.0           foreach_1.5.2              
#> [131] foreign_0.8-82              gridExtra_2.3              
#> [133] farver_2.1.0                mzID_1.32.0                
#> [135] ggraph_2.0.5                rvcheck_0.2.1              
#> [137] digest_0.6.29               BiocManager_1.30.16        
#> [139] GenomicRanges_1.46.1        broom_0.7.12               
#> [141] ncdf4_1.19                  httr_1.4.2                 
#> [143] ComplexHeatmap_2.10.0       colorspace_2.0-2           
#> [145] rvest_1.0.2                 XML_3.99-0.8               
#> [147] fs_1.5.2                    IRanges_2.28.0             
#> [149] splines_4.1.2               yulab.utils_0.0.4          
#> [151] graphlayouts_0.8.0          ggplotify_0.1.0            
#> [153] systemfonts_1.0.3           plotly_4.10.0              
#> [155] fit.models_0.64             jsonlite_1.7.3             
#> [157] tidygraph_1.2.0             corpcor_1.6.10             
#> [159] R6_2.5.1                    Hmisc_4.6-0                
#> [161] pillar_1.7.0                htmltools_0.5.2            
#> [163] glue_1.6.1                  fastmap_1.1.0              
#> [165] class_7.3-20                codetools_0.2-18           
#> [167] pcaPP_1.9-74                mvtnorm_1.1-3              
#> [169] furrr_0.2.3                 utf8_1.2.2                 
#> [171] lattice_0.20-45             bslib_0.3.1                
#> [173] tibble_3.1.6                zip_2.2.0                  
#> [175] openxlsx_4.2.5              Rttf2pt1_1.3.9             
#> [177] survival_3.2-13             limma_3.50.0               
#> [179] rmarkdown_2.11              munsell_0.5.0              
#> [181] e1071_1.7-9                 GetoptLong_1.0.5           
#> [183] GenomeInfoDbData_1.2.7      iterators_1.0.14           
#> [185] impute_1.68.0               haven_2.4.3                
#> [187] reshape2_1.4.4              gtable_0.3.0               
#> [189] extrafont_0.17
```
