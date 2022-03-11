---
title: "Statistical analysis"
weight: 12
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
  %\VignetteIndexEntry{statistical_analysis}
  %\VignettePackage{tidymass}
  % \VignetteEngine{knitr::rmarkdown}
  % \usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}
---



## Data preparation

Now the positive mode and negative mode have been annotated respectively. We need to merge positive and negative mode data.


```r
library(tidymass)
library(tidyverse)
```

### Load data


```r
load("metabolite_annotation/object_pos2")
load("metabolite_annotation/object_neg2")
```

### Remove the features without annotations

#### Positive mode


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
#> 2 massdataset      filter() 2022-03-10 23:57:33
#> 3 massdataset      filter() 2022-03-10 23:57:33
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

#### Negative mode


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
#> 2 massdataset      filter() 2022-03-10 23:57:33
#> 3 massdataset      filter() 2022-03-10 23:57:33
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

### Merge data

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
#> 2 massdataset      filter() 2022-03-10 23:57:33
#> 3 massdataset      filter() 2022-03-10 23:57:33
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
#> 2 massdataset      filter() 2022-03-10 23:57:33
#> 3 massdataset      filter() 2022-03-10 23:57:33
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
#> 1 massdataset merge_mass_dataset 2022-03-10 23:57:34
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

![](figures/fig1.png)

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


```r
object <-
  object %>% 
  activate_mass_dataset(what = "annotation_table") %>% 
  group_by(variable_id) %>% 
  filter(Level == min(Level)) %>% 
  filter(SS == max(SS)) %>% 
  slice_head(n = 1)
```

## Differential expression metabolites

Calculate the fold changes.


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


Calculate p values.


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

Volcano plot.


```r
volcano_plot(object = object,
             add_text = TRUE,
             text_from = "Compound.name", 
             point_size_scale = "p_value") +
  scale_size_continuous(range = c(0.5, 5))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-19-1.png" width="100%" />

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

Save result for subsequent analysis.


```r
object_final <- object
save(object_final, file = "statistical_analysis/object_final")
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
#> [167] bslib_0.3.1                 zip_2.2.0                  
#> [169] openxlsx_4.2.5              Rttf2pt1_1.3.9             
#> [171] survival_3.2-13             limma_3.50.0               
#> [173] rmarkdown_2.11              munsell_0.5.0              
#> [175] e1071_1.7-9                 GetoptLong_1.0.5           
#> [177] GenomeInfoDbData_1.2.7      iterators_1.0.14           
#> [179] impute_1.68.0               haven_2.4.3                
#> [181] reshape2_1.4.4              gtable_0.3.0               
#> [183] extrafont_0.17
```
