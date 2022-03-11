---
title: "Build your own docker image based on tidymass"
weight: 4
categories: [installation]
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
  %\VignetteIndexEntry{build_docker}
  %\VignettePackage{tidymass}
  % \VignetteEngine{knitr::rmarkdown}
  % \usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}
---





You can build your own docker image, which contains all your `code`, `data` and `analysis environment`, which is more efficient for reproducible analysis.

## Create `dockerfile`

Create a `dockerfile` without extension. And then open and modify it.

```
FROM jaspershen/tidymass:latest
MAINTAINER "Xiaotao Shen" shenxt1990@outlook.com

RUN apt-get update && apt-get install -y curl

COPY demo_data/ /home/rstudio/demo_data/

RUN chmod 777 /home/rstudio/demo_data/

RUN R -e 'install.packages("remotes")'

RUN R -e "remotes::install_gitlab('jaspershen/tidymass')"
```

If you want to install packages (for example ggraph) which are necessary for you analysis, please add a new line:

```
RUN R -e 'install.packages("ggraph")'
```

And you also need to copy your data to the image use the `COPY`.

### Build image

In the `terminal`, use below code to build the image.

```
docker build -t image-name -f Dockerfile .
```

Change the `image-name`.

## Use the `docker tag` command to give the `tidymass` image a new name

We need to create a account on the docker hub (https://hub.docker.com/) and then use the next code to link the local image to our account.

```
docker tag image-name your-account/image-name:latest
```

## Push image to docker hub

```
docker push your-account/image-name:latest
```

Then other people can download your image which contains your code, data and analysis environment, which make it is pretty easy to repeat your analysis and results.

How to pull docker image and run it can [refer this document](https://tidymass.github.io/tidymass/articles/docker.html).

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


