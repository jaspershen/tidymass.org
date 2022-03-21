---
title: "Repeat case study by docker"
weight: 17
author:
- name: Xiaotao Shen (https://www.shenxt.info/)
date: "Created on 2022-03-07 and updated on 2022-03-21"
output:
  html_document:
    number_sections: true
    df_print: paged
    toc: yes
  pdf_document:
    toc: no
vignette: >
  %\VignetteIndexEntry{repeat_case_study}
  %\VignettePackage{tidymass}
  % \VignetteEngine{knitr::rmarkdown}
  % \usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}
---



# Introduction

You can use the docker image of case study to repeat all the analysis and results in the `tidymass` manuscript.

The link of the docker image: [https://hub.docker.com/r/jaspershen/tidymass-case-study](https://hub.docker.com/r/jaspershen/tidymass-case-study) 

# Download and install `docker`

Please refer to the [official website](https://www.docker.com/get-started) to download and install docker. And then run docker.

1. If you are using Mac, see [here](https://docs.docker.com/desktop/mac/install/).

2. If you are using Windows, see [here](https://docs.docker.com/desktop/windows/install/).

3. If you are using Linux, see [here](https://docs.docker.com/engine/install/ubuntu/).

![](figures/fig2.png)

# Pull the `tidymass-case-study` image

Open your `terminal` and then type the code below:

```
docker pull jaspershen/tidymass-case-study:latest
```

# Run `tidymass-case-study` docker image

In your terminal, run the code below:

```
docker run -e PASSWORD=tidymass -p 8787:8787 jaspershen/tidymass-case-study:latest
```

Then open the browser and visit <http://localhost:8787> to power on RStudio server. The user name is `rstudio` and the password is `tidymass`.

![](figures/fig1.png)

# Run case study code

Open the `tidymass_case_study.Rmd`, and then click `Knit`.

![](figures/repeat_case_study.gif)

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
#>  [1] bookdown_0.24   here_1.0.1      rprojroot_2.0.2 digest_0.6.29  
#>  [5] R6_2.5.1        jsonlite_1.7.3  evaluate_0.15   blogdown_1.7   
#>  [9] rlang_1.0.1     stringi_1.7.6   cli_3.2.0       rstudioapi_0.13
#> [13] jquerylib_0.1.4 bslib_0.3.1     rmarkdown_2.11  tools_4.1.2    
#> [17] stringr_1.4.0   xfun_0.29       yaml_2.3.4      fastmap_1.1.0  
#> [21] compiler_4.1.2  htmltools_0.5.2 knitr_1.37      sass_0.4.0
```
