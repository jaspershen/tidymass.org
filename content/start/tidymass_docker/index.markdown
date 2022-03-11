---
title: "Install docker version of tidymass"
weight: 3
categories: [installation]
author:
- name: Xiaotao Shen (https://www.shenxt.info/)
date: "Created on 2022-02-18 and updated on 2022-03-10"
output:
  html_document:
    number_sections: true
    df_print: paged
    toc: yes
  pdf_document:
    toc: no
vignette: >
  %\VignetteIndexEntry{docker}
  %\VignettePackage{tidymass}
  % \VignetteEngine{knitr::rmarkdown}
  % \usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}
---



Docker is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers. So it is useful for people who want to share the code, data, and even analysis environment with other people to repeat their analysis and results.

We provide a docker version of `tidymass`, all the packages in `tidymass` and the dependent packages have been installed.


## **Install docker**

Please refer to the [offical website](https://www.docker.com/get-started) to download and install docker. And then run docker.

![](figures/Screen-Shot4-2.png)

## **Pull the tidymass image**

Open you terminal and then type code below:

```
docker pull jaspershen/tidymass:latest
```

## **Run tidymass docker image**

In you terminal, run the code below:

```
docker run -e PASSWORD=tidymass -p 8787:8787 jaspershen/tidymass:latest
```

The below command will link the RStudio home folder with the desktop of the local machine running the container. Anything saved or edited in the home folder when using the container will be stored on the local desktop.

```
docker run -e PASSWORD=tidymass -v ~/Desktop:/home/rstudio/ -p 8787:8787 jaspershen/tidymass:latest
```

## **Open the Rstudio server**

Then open the browser and visit http://localhost:8787 to power on RStudio server. The user name is `rstudio` and the password is `tidymass`.

![](figures/Untitled.gif)

## **Demo data and example analysis code**

In this tidymass docker image, a folder named "demo_data" is included to help users to learn how to use `tidymass`.

Open the `tidymass_demo.Rmd` file in `demo_data` folder, and then run it code chunk by chunk or just render it by clicking `Knit` on Rstudio, you will get a reporting result (HTML format) of all the whole workflow.

![](figures/Untitled-2.gif)
## **Session information**



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


