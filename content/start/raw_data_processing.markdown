---
title: "Raw data processing"
weight: 8
categories: [tidymass_usage]
author:
- name: Xiaotao Shen (https://www.shenxt.info/)
date: "Created on 2021-12-04 and updated on 2022-03-10"
output:
  html_document:
    number_sections: true
    df_print: paged
    toc: yes
  pdf_document:
    toc: no
vignette: >
  %\VignetteIndexEntry{raw_data_processing}
  %\VignettePackage{tidymass}
  % \VignetteEngine{knitr::rmarkdown}
  % \usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}
---





## Data preparation

Download the demo data and [refer this article](https://tidymass.github.io/tidymass/articles/demo_data.html).

We have positive and negative mode. For each mode, we have `control`, `case` and `QC` groups. Control group have 110 samples, and case group have 110 samples as well.

![](figures/Screen-Shot-2022-01-13-at-10.33.07-PM.png)

### Positive mode

`massprocesser` package is used to do the raw data processing. Please refer this [website](file:///Users/xiaotaoshen/tidymass/massprocesser/docs/index.html).

#### Code

The code used to do raw data processing.


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
```

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

#### Results

All the results will be placed in the folder `mzxml_ms1_data/POS/Result`. More information can be found [here](https://tidymass.github.io/massprocesser/articles/raw_data_processing.html).

![](figures/Screen-Shot-2.png)

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

![](figures/Screen-Shot.png)

You can use the `plot_adjusted_rt()` function to get the interactive plot.


```r
load("mzxml_ms1_data/POS/Result/intermediate_data/xdata2")
####set the group_for_figure if you want to show specific groups. And set it as "all" if you want to show all samples.
plot = 
massprocesser::plot_adjusted_rt(object = xdata2, 
                 group_for_figure = "QC", 
                 interactive = TRUE)
plot
```

### Negative mode

The processing of negative mode is same with positive mode data.

### Code

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

### Results

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
#>  [55] checkmate_2.0.0             cluster_2.1.2              
#>  [57] lazyeval_0.2.2              crayon_1.5.0               
#>  [59] ellipse_0.4.2               pkgconfig_2.0.3            
#>  [61] tweenr_1.0.2                GenomeInfoDb_1.30.0        
#>  [63] nnet_7.3-17                 rlang_1.0.1                
#>  [65] globals_0.14.0              lifecycle_1.0.1            
#>  [67] affyio_1.64.0               extrafontdb_1.0            
#>  [69] fastDummies_1.6.3           MassSpecWavelet_1.60.0     
#>  [71] modelr_0.1.8                cellranger_1.1.0           
#>  [73] randomForest_4.7-1          rprojroot_2.0.2            
#>  [75] polyclip_1.10-0             matrixStats_0.61.0         
#>  [77] Matrix_1.4-0                reprex_2.0.1               
#>  [79] base64enc_0.1-3             GlobalOptions_0.1.2        
#>  [81] png_0.1-7                   viridisLite_0.4.0          
#>  [83] rjson_0.2.21                clisymbols_1.2.0           
#>  [85] bitops_1.0-7                pander_0.6.4               
#>  [87] Biostrings_2.62.0           shape_1.4.6                
#>  [89] stringr_1.4.0               parallelly_1.30.0          
#>  [91] robust_0.7-0                readr_2.1.2                
#>  [93] jpeg_0.1-9                  gridGraphics_0.5-1         
#>  [95] scales_1.1.1                plyr_1.8.6                 
#>  [97] zlibbioc_1.40.0             compiler_4.1.2             
#>  [99] RColorBrewer_1.1-2          pcaMethods_1.86.0          
#> [101] clue_0.3-60                 rrcov_1.6-2                
#> [103] cli_3.2.0                   affy_1.72.0                
#> [105] XVector_0.34.0              listenv_0.8.0              
#> [107] patchwork_1.1.1             pbapply_1.5-0              
#> [109] htmlTable_2.4.0             Formula_1.2-4              
#> [111] MASS_7.3-55                 tidyselect_1.1.1           
#> [113] vsn_3.62.0                  stringi_1.7.6              
#> [115] forcats_0.5.1.9000          yaml_2.3.4                 
#> [117] latticeExtra_0.6-29         MALDIquant_1.21            
#> [119] ggrepel_0.9.1               grid_4.1.2                 
#> [121] sass_0.4.0                  tools_4.1.2                
#> [123] parallel_4.1.2              circlize_0.4.14            
#> [125] rstudioapi_0.13             MsCoreUtils_1.6.0          
#> [127] foreach_1.5.2               foreign_0.8-82             
#> [129] gridExtra_2.3               farver_2.1.0               
#> [131] mzID_1.32.0                 ggraph_2.0.5               
#> [133] rvcheck_0.2.1               digest_0.6.29              
#> [135] BiocManager_1.30.16         GenomicRanges_1.46.1       
#> [137] broom_0.7.12                ncdf4_1.19                 
#> [139] httr_1.4.2                  ComplexHeatmap_2.10.0      
#> [141] colorspace_2.0-2            rvest_1.0.2                
#> [143] XML_3.99-0.8                fs_1.5.2                   
#> [145] IRanges_2.28.0              splines_4.1.2              
#> [147] yulab.utils_0.0.4           graphlayouts_0.8.0         
#> [149] ggplotify_0.1.0             plotly_4.10.0              
#> [151] fit.models_0.64             jsonlite_1.7.3             
#> [153] tidygraph_1.2.0             corpcor_1.6.10             
#> [155] R6_2.5.1                    Hmisc_4.6-0                
#> [157] pillar_1.7.0                htmltools_0.5.2            
#> [159] glue_1.6.1                  fastmap_1.1.0              
#> [161] class_7.3-20                codetools_0.2-18           
#> [163] pcaPP_1.9-74                mvtnorm_1.1-3              
#> [165] furrr_0.2.3                 utf8_1.2.2                 
#> [167] lattice_0.20-45             bslib_0.3.1                
#> [169] tibble_3.1.6                zip_2.2.0                  
#> [171] openxlsx_4.2.5              Rttf2pt1_1.3.9             
#> [173] survival_3.2-13             limma_3.50.0               
#> [175] rmarkdown_2.11              munsell_0.5.0              
#> [177] e1071_1.7-9                 GetoptLong_1.0.5           
#> [179] GenomeInfoDbData_1.2.7      iterators_1.0.14           
#> [181] impute_1.68.0               haven_2.4.3                
#> [183] reshape2_1.4.4              gtable_0.3.0               
#> [185] extrafont_0.17
```
