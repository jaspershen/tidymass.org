---
title: "Convert raw data to mzXML/mgf"
weight: 7
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
  %\VignetteIndexEntry{data_convert}
  %\VignettePackage{tidymass}
  % \VignetteEngine{knitr::rmarkdown}
  % \usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}
---





## Data preparation

Please place the raw data in one folder according to MS1 and MS2. Then you can convert them using `Proteowizard` or `massconverter` package.

## Proteowizard

### Convert MS1 data to `mzXML` format data

Just convert data into `mzXML` format data according to the parameters below:

![](../man/figures/Screen Shot 2022-01-13 at 12.06.14 AM.png)
### Convert MS2 data to `mzXML` or `mgf` format data

Just convert data into `mzXML` or `mgf` format data according to the parameters below:

![](../man/figures/Screen Shot 2022-01-13 at 12.09.12 AM.png)

## Convert data using `massconverter`

`massconverter` is a package in `tidymass` project which can be used to convert mass spectrometry raw data based on docker image of [`pwid`](https://hub.docker.com/r/chambm/pwiz-skyline-i-agree-to-the-vendor-licenses). See more information [here](https://tidymass.github.io/massconverter/).

More information can be found [here](https://tidymass.github.io/massconverter/articles/convert_data.html).

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
#> [1] stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] magrittr_2.0.2
#> 
#> loaded via a namespace (and not attached):
#>  [1] bookdown_0.24   here_1.0.1      rprojroot_2.0.2 digest_0.6.29  
#>  [5] R6_2.5.1        jsonlite_1.7.3  evaluate_0.15   blogdown_1.7   
#>  [9] rlang_1.0.1     stringi_1.7.6   cli_3.2.0       rstudioapi_0.13
#> [13] jquerylib_0.1.4 bslib_0.3.1     rmarkdown_2.11  tools_4.1.2    
#> [17] stringr_1.4.0   xfun_0.29       yaml_2.3.4      fastmap_1.1.0  
#> [21] compiler_4.1.2  htmltools_0.5.2 knitr_1.37      sass_0.4.0
```
