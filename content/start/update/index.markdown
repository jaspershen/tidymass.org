---
title: "Update tidymass"
weight: 2
categories: [installation]
author:
- name: Xiaotao Shen (https://www.shenxt.info/)
date: "Created on 2022-02-25 and updated on 2022-03-11"
output:
  html_document:
    number_sections: true
    df_print: paged
    toc: yes
  pdf_document:
    toc: no
vignette: >
  %\VignetteIndexEntry{tidymass_update}
  %\VignettePackage{tidymass}
  % \VignetteEngine{knitr::rmarkdown}
  % \usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}
---





You can use the `tidymass` to check the version of all packages and update them. 

# Check version of tidymass

If you want to check if there are updates for `tidymass` and packages in it. Just check it like this.


```r
tidymass::check_tidymass_version()
```

# Update tidymass

The `update_tidymass()` function can be used to update `tidymass` and packages in it.


```r
tidymass::update_tidymass(from = "github")
```

> If the `from = "github"` doesn't work, try set it as `from = "gitlab"` or `from = "gitee"`.

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
#> [1] stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] magrittr_2.0.2
#> 
#> loaded via a namespace (and not attached):
#>   [1] metpath_0.99.4              blogdown_1.7               
#>   [3] tidyr_1.2.0                 missForest_1.4             
#>   [5] ggplot2_3.3.5               tidymass_0.99.6            
#>   [7] knitr_1.37                  DelayedArray_0.20.0        
#>   [9] data.table_1.14.2           rpart_4.1.16               
#>  [11] KEGGREST_1.34.0             RCurl_1.98-1.5             
#>  [13] doParallel_1.0.17           generics_0.1.2             
#>  [15] snow_0.4-4                  leaflet_2.1.0              
#>  [17] BiocGenerics_0.40.0         preprocessCore_1.56.0      
#>  [19] mixOmics_6.18.1             RANN_2.6.1                 
#>  [21] proxy_0.4-26                future_1.23.0              
#>  [23] tzdb_0.2.0                  xml2_1.3.3                 
#>  [25] lubridate_1.8.0             ggsci_2.9                  
#>  [27] SummarizedExperiment_1.24.0 assertthat_0.2.1           
#>  [29] tidyverse_1.3.1             viridis_0.6.2              
#>  [31] xfun_0.29                   hms_1.1.1                  
#>  [33] jquerylib_0.1.4             evaluate_0.15              
#>  [35] DEoptimR_1.0-10             fansi_1.0.2                
#>  [37] dbplyr_2.1.1                readxl_1.3.1               
#>  [39] igraph_1.2.11               DBI_1.1.2                  
#>  [41] htmlwidgets_1.5.4           MsFeatures_1.3.0           
#>  [43] massprocesser_0.99.3        rARPACK_0.11-0             
#>  [45] stats4_4.1.2                purrr_0.3.4                
#>  [47] ellipsis_0.3.2              RSpectra_0.16-0            
#>  [49] crosstalk_1.2.0             dplyr_1.0.8                
#>  [51] backports_1.4.1             bookdown_0.24              
#>  [53] ggcorrplot_0.1.3            MatrixGenerics_1.6.0       
#>  [55] vctrs_0.3.8                 Biobase_2.54.0             
#>  [57] remotes_2.4.2               here_1.0.1                 
#>  [59] withr_2.4.3                 ggforce_0.3.3              
#>  [61] itertools_0.1-3             robustbase_0.93-9          
#>  [63] checkmate_2.0.0             cluster_2.1.2              
#>  [65] lazyeval_0.2.2              crayon_1.5.0               
#>  [67] ellipse_0.4.2               pkgconfig_2.0.3            
#>  [69] tweenr_1.0.2                GenomeInfoDb_1.30.0        
#>  [71] ProtGenerics_1.26.0         nnet_7.3-17                
#>  [73] rlang_1.0.1                 globals_0.14.0             
#>  [75] lifecycle_1.0.1             affyio_1.64.0              
#>  [77] extrafontdb_1.0             fastDummies_1.6.3          
#>  [79] MassSpecWavelet_1.60.0      modelr_0.1.8               
#>  [81] cellranger_1.1.0            randomForest_4.7-1         
#>  [83] rprojroot_2.0.2             polyclip_1.10-0            
#>  [85] matrixStats_0.61.0          Matrix_1.4-0               
#>  [87] reprex_2.0.1                base64enc_0.1-3            
#>  [89] GlobalOptions_0.1.2         png_0.1-7                  
#>  [91] viridisLite_0.4.0           rjson_0.2.21               
#>  [93] mzR_2.28.0                  clisymbols_1.2.0           
#>  [95] bitops_1.0-7                pander_0.6.4               
#>  [97] Biostrings_2.62.0           shape_1.4.6                
#>  [99] stringr_1.4.0               parallelly_1.30.0          
#> [101] robust_0.7-0                readr_2.1.2                
#> [103] jpeg_0.1-9                  gridGraphics_0.5-1         
#> [105] S4Vectors_0.32.3            scales_1.1.1               
#> [107] plyr_1.8.6                  zlibbioc_1.40.0            
#> [109] compiler_4.1.2              RColorBrewer_1.1-2         
#> [111] pcaMethods_1.86.0           clue_0.3-60                
#> [113] rrcov_1.6-2                 cli_3.2.0                  
#> [115] affy_1.72.0                 XVector_0.34.0             
#> [117] listenv_0.8.0               patchwork_1.1.1            
#> [119] pbapply_1.5-0               htmlTable_2.4.0            
#> [121] Formula_1.2-4               MASS_7.3-55                
#> [123] tidyselect_1.1.1            vsn_3.62.0                 
#> [125] stringi_1.7.6               forcats_0.5.1.9000         
#> [127] yaml_2.3.4                  latticeExtra_0.6-29        
#> [129] MALDIquant_1.21             ggrepel_0.9.1              
#> [131] grid_4.1.2                  sass_0.4.0                 
#> [133] xcms_3.16.1                 metid_1.2.4                
#> [135] tools_4.1.2                 parallel_4.1.2             
#> [137] circlize_0.4.14             rstudioapi_0.13            
#> [139] MsCoreUtils_1.6.0           foreach_1.5.2              
#> [141] foreign_0.8-82              masscleaner_0.99.7         
#> [143] gridExtra_2.3               masstools_0.99.5           
#> [145] farver_2.1.0                mzID_1.32.0                
#> [147] ggraph_2.0.5                rvcheck_0.2.1              
#> [149] digest_0.6.29               BiocManager_1.30.16        
#> [151] Rcpp_1.0.8                  GenomicRanges_1.46.1       
#> [153] broom_0.7.12                ncdf4_1.19                 
#> [155] httr_1.4.2                  MSnbase_2.20.4             
#> [157] ComplexHeatmap_2.10.0       colorspace_2.0-2           
#> [159] rvest_1.0.2                 XML_3.99-0.8               
#> [161] fs_1.5.2                    IRanges_2.28.0             
#> [163] splines_4.1.2               yulab.utils_0.0.4          
#> [165] massdataset_0.99.20         graphlayouts_0.8.0         
#> [167] ggplotify_0.1.0             plotly_4.10.0              
#> [169] fit.models_0.64             jsonlite_1.7.3             
#> [171] tidygraph_1.2.0             corpcor_1.6.10             
#> [173] R6_2.5.1                    Hmisc_4.6-0                
#> [175] pillar_1.7.0                htmltools_0.5.2            
#> [177] glue_1.6.1                  fastmap_1.1.0              
#> [179] massqc_0.99.7               BiocParallel_1.28.3        
#> [181] class_7.3-20                codetools_0.2-18           
#> [183] pcaPP_1.9-74                mvtnorm_1.1-3              
#> [185] furrr_0.2.3                 utf8_1.2.2                 
#> [187] lattice_0.20-45             bslib_0.3.1                
#> [189] tibble_3.1.6                massstat_0.99.13           
#> [191] ggfortify_0.4.14            zip_2.2.0                  
#> [193] openxlsx_4.2.5              Rttf2pt1_1.3.9             
#> [195] survival_3.2-13             limma_3.50.0               
#> [197] rmarkdown_2.11              munsell_0.5.0              
#> [199] e1071_1.7-9                 GetoptLong_1.0.5           
#> [201] GenomeInfoDbData_1.2.7      iterators_1.0.14           
#> [203] impute_1.68.0               haven_2.4.3                
#> [205] reshape2_1.4.4              gtable_0.3.0               
#> [207] extrafont_0.17
```


