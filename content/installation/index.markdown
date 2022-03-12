---
title: "Install tidymass"
---

# Install `tidymass` packages

## Update R version

`tidymass` require R version > 4.1. You can check your R version in your console:


```r
version
```

If your R version is < 4.1, please [download and install the latest version of R](https://cran.r-project.org/mirrors.html), and then restart your R.

## Uninstall `tidyTools` and `metID`

If you have installed `tinyTools` or `metID`, please uninstall them.

`tinyTools` has been renamed as `tinytools` and `metID` has been renamed as `metid`. So please uninstall them before you install `tidymass`. **And don't use them any more.**

1. detach them by:


```r
detach("package:tinyTools")
detach("package:metID")
```

2. Remove them by:


```r
remove.packages(pkgs = "tinyTools")
remove.packages(pkgs = "metID")
```

3. Restart your R session. 

> If there are errors, ignore them.

## Install `tidymass` from GitLab

### Update `remotes`

You can use the `remotes` package to install `tidymass`. 

Please update `remotes` first and then restart your r session.


```r
install.packages("remotes")
```

### Install `tidymass`

Install `tidymass` by:


```r
remotes::install_gitlab("jaspershen/tidymass", dependencies = TRUE)
```

> During installing, it may ask you several times: "Would you like to uodate some pacakges?" Just Enter the `Enter` or `Retrun` key to skip updates.

![](figures/Screen-Shot 9.png)

### Install `tidymass` packages one by one

If you use the Windows OS and the dependencies can not be installed, try to install all the `tidymass` packages one by one.


```r
remotes::install_gitlab("jaspershen/masstools", dependencies = TRUE)
remotes::install_gitlab("jaspershen/massdataset", dependencies = TRUE)
remotes::install_gitlab("jaspershen/massqc", dependencies = TRUE)
remotes::install_gitlab("jaspershen/massprocesser", dependencies = TRUE)
remotes::install_gitlab("jaspershen/masscleaner", dependencies = TRUE)
remotes::install_gitlab("jaspershen/massstat", dependencies = TRUE)
remotes::install_gitlab("jaspershen/metid", dependencies = TRUE)
remotes::install_gitlab("jaspershen/metpath", dependencies = TRUE)
```


## Install `tidymass` from GitHub

### Install `tidymass`

Then install `tidymass`:


```r
remotes::install_github("tidymass/tidymass", dependencies = TRUE)
```

> During the installation, it will ask if you want to update some packages for few times, just enter `Enter` or `Reurn` key to skip it.

If there is a error like below:

```
Error: Failed to install 'tidymass' from GitHub: HTTP error 403. API rate limit exceeded for 171.66.10.237. (But here's the good news: Authenticated requests get a higher rate limit. Check out the documentation for more details.)
```

Try to resolve it by:

1. In you R console, type this code:


```r
usethis::create_github_token()
```

It will open a page in browser, and create a "New personal access token" and copy it.

![](figures/Screen-Shot-2021-12-24-at-9.28.18-PM.png)

2. Then type this code:


```r
usethis::edit_r_environ()
```

and then add one line like below:

```
GITHUB_PAT=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
```
> The `GITHUB_PAT` should be yours that is created in step 1.

And then restart R session and try again.


### Install `tidymass` packages one by one

If you use the Windows OS and the dependencies can not be installed, try to install all the `tidymass` packages one by one.


```r
remotes::install_github("tidymass/masstools", dependencies = TRUE)
remotes::install_github("tidymass/massdataset", dependencies = TRUE)
remotes::install_github("tidymass/massqc", dependencies = TRUE)
remotes::install_github("tidymass/massprocesser", dependencies = TRUE)
remotes::install_github("tidymass/masscleaner", dependencies = TRUE)
remotes::install_github("tidymass/massstat", dependencies = TRUE)
remotes::install_github("tidymass/metid", dependencies = TRUE)
remotes::install_github("tidymass/metpath", dependencies = TRUE)
```


## Install `tidymass` from Gitee

If you can't install pacakgs from GitHub and GitLab, please try install packags from Gitee.

### Install `tidymass`

Then install `tidymass`:


```r
remotes::install_git(url = "https://gitee.com/jaspershen/tidymass", dependencies = TRUE)
```

### Install `tidymass` packages one by one.

If you use the Windows OS and the dependencies can not be installed, try to install all the `tidymass` packages one by one.


```r
remotes::install_git(url = "https://gitee.com/jaspershen/masstools", dependencies = TRUE)
remotes::install_git(url = "https://gitee.com/jaspershen/massdataset", dependencies = TRUE)
remotes::install_git(url = "https://gitee.com/jaspershen/massqc", dependencies = TRUE)
remotes::install_git(url = "https://gitee.com/jaspershen/massprocesser", dependencies = TRUE)
remotes::install_git(url = "https://gitee.com/jaspershen/masscleaner", dependencies = TRUE)
remotes::install_git(url = "https://gitee.com/jaspershen/massstat", dependencies = TRUE)
remotes::install_git(url = "https://gitee.com/jaspershen/metid", dependencies = TRUE)
remotes::install_git(url = "https://gitee.com/jaspershen/metpath", dependencies = TRUE)
```

[Let me know if you have any questions](https://github.com/tidymass/tidymass/issues).

# Update `tidymass`

You can use the `tidymass` to check the version of all packages and update them. 

## Check version

If you want to check if there are updates for `tidymass` and packages in it. Just check it like this.


```r
tidymass::check_tidymass_version()
```

## Update

The `update_tidymass()` function can be used to update `tidymass` and packages in it.


```r
tidymass::update_tidymass(from = "github")
```

> If the `from = "github"` doesn't work, try set it as `from = "gitlab"` or `from = "gitee"`.

# Install docker version of `tidymass`

Docker is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers. So it is useful for people who want to share the code, data, and even analysis environment with other people to repeat their analysis and results.

We provide a docker version of `tidymass`, all the packages in `tidymass` and the dependent packages have been installed.


## Install docker

Please refer to the [offical website](https://www.docker.com/get-started) to download and install docker. And then run docker.

![](figures/Screen-Shot 4.png)

## Pull the `tidymass` image

Open you terminal and then type code below:

```
docker pull jaspershen/tidymass:latest
```

## Run `tidymass` docker image

In you terminal, run the code below:

```
docker run -e PASSWORD=tidymass -p 8787:8787 jaspershen/tidymass:latest
```

The below command will link the RStudio home folder with the desktop of the local machine running the container. Anything saved or edited in the home folder when using the container will be stored on the local desktop.

```
docker run -e PASSWORD=tidymass -v ~/Desktop:/home/rstudio/ -p 8787:8787 jaspershen/tidymass:latest
```

## Open the Rstudio server

Then open the browser and visit http://localhost:8787 to power on RStudio server. The user name is `rstudio` and the password is `tidymass`.

![](figures/Screen-Shot 5.png)
![](figures/Screen-Shot 6.png)

![](figures/Screen-Shot 7.png)

![](figures/Untitled.gif)

# Build your own docker image based on `tidymass`

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

## Build image

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


# Session information


```r
sessionInfo()
```

```
## R version 4.1.2 (2021-11-01)
## Platform: x86_64-apple-darwin17.0 (64-bit)
## Running under: macOS Big Sur 10.16
## 
## Matrix products: default
## BLAS:   /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRblas.0.dylib
## LAPACK: /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRlapack.dylib
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] magrittr_2.0.2
## 
## loaded via a namespace (and not attached):
##   [1] metpath_0.99.4              blogdown_1.7               
##   [3] tidyr_1.2.0                 missForest_1.4             
##   [5] ggplot2_3.3.5               tidymass_0.99.6            
##   [7] knitr_1.37                  DelayedArray_0.20.0        
##   [9] data.table_1.14.2           rpart_4.1.16               
##  [11] KEGGREST_1.34.0             RCurl_1.98-1.5             
##  [13] doParallel_1.0.17           generics_0.1.2             
##  [15] snow_0.4-4                  leaflet_2.1.0              
##  [17] BiocGenerics_0.40.0         preprocessCore_1.56.0      
##  [19] mixOmics_6.18.1             RANN_2.6.1                 
##  [21] proxy_0.4-26                future_1.23.0              
##  [23] tzdb_0.2.0                  xml2_1.3.3                 
##  [25] lubridate_1.8.0             ggsci_2.9                  
##  [27] SummarizedExperiment_1.24.0 assertthat_0.2.1           
##  [29] tidyverse_1.3.1             viridis_0.6.2              
##  [31] xfun_0.29                   hms_1.1.1                  
##  [33] jquerylib_0.1.4             evaluate_0.15              
##  [35] DEoptimR_1.0-10             fansi_1.0.2                
##  [37] dbplyr_2.1.1                readxl_1.3.1               
##  [39] igraph_1.2.11               DBI_1.1.2                  
##  [41] htmlwidgets_1.5.4           MsFeatures_1.3.0           
##  [43] massprocesser_0.99.3        rARPACK_0.11-0             
##  [45] stats4_4.1.2                purrr_0.3.4                
##  [47] ellipsis_0.3.2              RSpectra_0.16-0            
##  [49] crosstalk_1.2.0             dplyr_1.0.8                
##  [51] backports_1.4.1             bookdown_0.24              
##  [53] ggcorrplot_0.1.3            MatrixGenerics_1.6.0       
##  [55] vctrs_0.3.8                 Biobase_2.54.0             
##  [57] remotes_2.4.2               withr_2.4.3                
##  [59] ggforce_0.3.3               itertools_0.1-3            
##  [61] robustbase_0.93-9           checkmate_2.0.0            
##  [63] cluster_2.1.2               lazyeval_0.2.2             
##  [65] crayon_1.5.0                ellipse_0.4.2              
##  [67] pkgconfig_2.0.3             tweenr_1.0.2               
##  [69] GenomeInfoDb_1.30.0         ProtGenerics_1.26.0        
##  [71] nnet_7.3-17                 rlang_1.0.1                
##  [73] globals_0.14.0              lifecycle_1.0.1            
##  [75] affyio_1.64.0               extrafontdb_1.0            
##  [77] fastDummies_1.6.3           MassSpecWavelet_1.60.0     
##  [79] modelr_0.1.8                cellranger_1.1.0           
##  [81] randomForest_4.7-1          polyclip_1.10-0            
##  [83] matrixStats_0.61.0          Matrix_1.4-0               
##  [85] reprex_2.0.1                base64enc_0.1-3            
##  [87] GlobalOptions_0.1.2         png_0.1-7                  
##  [89] viridisLite_0.4.0           rjson_0.2.21               
##  [91] mzR_2.28.0                  clisymbols_1.2.0           
##  [93] bitops_1.0-7                pander_0.6.4               
##  [95] Biostrings_2.62.0           shape_1.4.6                
##  [97] stringr_1.4.0               parallelly_1.30.0          
##  [99] robust_0.7-0                readr_2.1.2                
## [101] jpeg_0.1-9                  gridGraphics_0.5-1         
## [103] S4Vectors_0.32.3            scales_1.1.1               
## [105] plyr_1.8.6                  zlibbioc_1.40.0            
## [107] compiler_4.1.2              RColorBrewer_1.1-2         
## [109] pcaMethods_1.86.0           clue_0.3-60                
## [111] rrcov_1.6-2                 cli_3.2.0                  
## [113] affy_1.72.0                 XVector_0.34.0             
## [115] listenv_0.8.0               patchwork_1.1.1            
## [117] pbapply_1.5-0               htmlTable_2.4.0            
## [119] Formula_1.2-4               MASS_7.3-55                
## [121] tidyselect_1.1.1            vsn_3.62.0                 
## [123] stringi_1.7.6               forcats_0.5.1.9000         
## [125] yaml_2.3.4                  latticeExtra_0.6-29        
## [127] MALDIquant_1.21             ggrepel_0.9.1              
## [129] grid_4.1.2                  sass_0.4.0                 
## [131] xcms_3.16.1                 metid_1.2.4                
## [133] tools_4.1.2                 parallel_4.1.2             
## [135] circlize_0.4.14             rstudioapi_0.13            
## [137] MsCoreUtils_1.6.0           foreach_1.5.2              
## [139] foreign_0.8-82              masscleaner_0.99.7         
## [141] gridExtra_2.3               masstools_0.99.5           
## [143] farver_2.1.0                mzID_1.32.0                
## [145] ggraph_2.0.5                rvcheck_0.2.1              
## [147] digest_0.6.29               BiocManager_1.30.16        
## [149] Rcpp_1.0.8                  GenomicRanges_1.46.1       
## [151] broom_0.7.12                ncdf4_1.19                 
## [153] httr_1.4.2                  MSnbase_2.20.4             
## [155] ComplexHeatmap_2.10.0       colorspace_2.0-2           
## [157] rvest_1.0.2                 XML_3.99-0.8               
## [159] fs_1.5.2                    IRanges_2.28.0             
## [161] splines_4.1.2               yulab.utils_0.0.4          
## [163] massdataset_0.99.20         graphlayouts_0.8.0         
## [165] ggplotify_0.1.0             plotly_4.10.0              
## [167] fit.models_0.64             jsonlite_1.7.3             
## [169] tidygraph_1.2.0             corpcor_1.6.10             
## [171] R6_2.5.1                    Hmisc_4.6-0                
## [173] pillar_1.7.0                htmltools_0.5.2            
## [175] glue_1.6.1                  fastmap_1.1.0              
## [177] massqc_0.99.7               BiocParallel_1.28.3        
## [179] class_7.3-20                codetools_0.2-18           
## [181] pcaPP_1.9-74                mvtnorm_1.1-3              
## [183] furrr_0.2.3                 utf8_1.2.2                 
## [185] lattice_0.20-45             bslib_0.3.1                
## [187] tibble_3.1.6                massstat_0.99.13           
## [189] ggfortify_0.4.14            zip_2.2.0                  
## [191] openxlsx_4.2.5              Rttf2pt1_1.3.9             
## [193] survival_3.2-13             limma_3.50.0               
## [195] rmarkdown_2.11              munsell_0.5.0              
## [197] e1071_1.7-9                 GetoptLong_1.0.5           
## [199] GenomeInfoDbData_1.2.7      iterators_1.0.14           
## [201] impute_1.68.0               haven_2.4.3                
## [203] reshape2_1.4.4              gtable_0.3.0               
## [205] extrafont_0.17
```


