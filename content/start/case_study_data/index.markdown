---
title: "Case study data"
weight: 15
author:
- name: Xiaotao Shen (https://www.shenxt.info/)
date: "Created on 2022-03-07 and updated on 2022-03-11"
output:
  html_document:
    number_sections: true
    df_print: paged
    toc: yes
  pdf_document:
    toc: no
vignette: >
  %\VignetteIndexEntry{case_study_data}
  %\VignettePackage{tidymass}
  % \VignetteEngine{knitr::rmarkdown}
  % \usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}
---



# **MS1 Raw data**

The raw data of the case study (MS1, mzML format) is accessible on `MetaboLights` with [MTBLS1122 (HILIC positive)](https://www.ebi.ac.uk/metabolights/MTBLS1122), [MTBLS1124 (HILIC negative)](https://www.ebi.ac.uk/metabolights/MTBLS1124/descriptors), [MTBLS1122 (RPLC positive)](https://www.ebi.ac.uk/metabolights/MTBLS1129/descriptors) and [MTBLS1130 (RPLC negative)](https://www.ebi.ac.uk/metabolights/MTBLS1130/descriptors).

# **MS2 raw data**

The raw data of the case study (MS2, mgf format) is accessible [here](https://drive.google.com/file/d/1fR75tWp9jqp8vuWq85GvDGJ864Pb6AH2/view?usp=sharing).

# **Processed data**

The processed data (“mass_dataset” class) from the `massProcesser` package are [available here](https://drive.google.com/file/d/1BxILAJYAHBiIKLKaAcsTY-tgMCWV2YcO/view?usp=sharing).

# **Other data**

You can also download all the other dataset which are necessary for reproducible analysis for case_study.

1. [data_cleaning](https://drive.google.com/file/d/1Rg5wzSrIQm5i4Eb6TyK5MrOs342UbOPK/view?usp=sharing)

2. [metabolite_annotation](https://drive.google.com/file/d/1lht4LAG8VKGHqRceslPUc-dZVm72o7eD/view?usp=sharing)

3. [pathway_enrichment](https://drive.google.com/file/d/1ysz9opVjiCVNpLC0R75O2SD56h_2EyPg/view?usp=sharing)

4. [sample_info](https://drive.google.com/file/d/1v47gH9jvcARIuG8DUwSY7RgC58msjNo2/view?usp=sharing)

5. [Statistical_analysis](https://drive.google.com/file/d/1V7U7R12Qpsh3QR3c82F9dGq9x4QxRcpj/view?usp=sharing)

# **Session information**


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
#>  [1] bookdown_0.24   digest_0.6.29   R6_2.5.1        jsonlite_1.7.3 
#>  [5] evaluate_0.15   blogdown_1.7    rlang_1.0.1     stringi_1.7.6  
#>  [9] cli_3.2.0       rstudioapi_0.13 jquerylib_0.1.4 bslib_0.3.1    
#> [13] rmarkdown_2.11  tools_4.1.2     stringr_1.4.0   xfun_0.29      
#> [17] yaml_2.3.4      fastmap_1.1.0   compiler_4.1.2  htmltools_0.5.2
#> [21] knitr_1.37      sass_0.4.0
```
