---
title: ""
---

# Install `tidymass`

> Please make sure your internet is connected and stable. 

And if you have any problems during the installation, please feel free to send email to us (shenxt1990@outlook.edu) or contact me via [other social medias](https://www.tidymass.org/contact/).

-------

## Update R version

`tidymass` require R version > 4.1. 

You can check your R version in your console:


```r
version
```

![](figures/fig1.png)


If your R version is < 4.1, please [download and install the latest version of R](https://cran.r-project.org/mirrors.html), and then restart your R.

-------

## From `local packages`

Copy and paste the below code in your console.


```r
source("https://www.tidymass.org/tidymass-packages/install_tidymass.txt")
```


```r
install_tidymass(from = "tidymass.org")
```

-------

## From `GitLab`

Please update `remotes` first and then restart your r session.


```r
install.packages("remotes")
```

Install `tidymass` by:


```r
remotes::install_gitlab("tidymass/tidymass", dependencies = TRUE)
```

> During installing, it may ask you several times: "Would you like to update some pacakges?" Just Enter the `Enter` or `Retrun` key to skip updates.

![](figures/fig2.png)

> If you have a completely fresh R enivorment, it needs to install all the dependent packages, so it will take around 30 mins to finish the installation of `tidymass`. In my Mac pro (macOS Monterey, 2.3 GHz 8-core intel core i9, 16GB 2667 MHz DDR4), it takes about 30 mins to finish the installation in a completely fresh R enivorment.


-------

## Install `tidymass` from `GitHub`


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

![](figures/fig3.png)

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


-------

## Install `tidymass` from `Gitee`

If you can't install packages from `GitHub` and `GitLab`, please try install packages from `Gitee`.


```r
remotes::install_git(url = "https://gitee.com/tidymass/tidymass", dependencies = TRUE)
```


-------

# Update `tidymass`

You can use the `tidymass` to check the version of all packages and update them. 

If you want to check if there are updates for `tidymass` and packages in it. Just check it like this.


```r
tidymass::check_tidymass_version()
```

The `update_tidymass()` function can be used to update `tidymass` and packages in it.


```r
tidymass::update_tidymass(from = "gitlab")
```

> If the `from = "gitlab"` doesn't work, try set it as `from = "github"` or `from = "gitee"`.


-----

# Install docker version of `tidymass`

Docker is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers. So it is useful for people who want to share the code, data, and even analysis environment with other people to repeat their analysis and results.

We provide a docker version of `tidymass`, all the packages in `tidymass` and the dependent packages have been installed.

-----

## Install docker

Please refer to the [offical website](https://www.docker.com/get-started) to download and install docker. And then run docker.

![](figures/fig4.png)

-----

## Pull the `tidymass` image

Open you terminal and then type code below:

```
docker pull jaspershen/tidymass:latest
```

-----

## Run `tidymass` docker image

In your terminal, run the code below:

```
docker run -e PASSWORD=tidymass -p 8787:8787 jaspershen/tidymass:latest
```

The below command will link the RStudio home folder with the desktop of the local machine running the container. Anything saved or edited in the home folder when using the container will be stored on the local desktop.

```
docker run -e PASSWORD=tidymass -v ~/Desktop:/home/rstudio/ -p 8787:8787 jaspershen/tidymass:latest
```

-----

## Open the Rstudio server

Then open the browser and visit http://localhost:8787 to power on RStudio server. The user name is `rstudio` and the password is `tidymass`.

![](figures/fig5.png)
![](figures/fig6.png)

![](figures/fig7.png)

![](figures/Untitled.gif)

-----

# Build docker image based on `tidymass`

You can build your own docker image, which contains all your `code`, `data` and `analysis environment`, which is more efficient for reproducible analysis.

-----

## Create `dockerfile`

Create a `dockerfile` without extension. And then open and modify it.

```
FROM jaspershen/tidymass:latest
MAINTAINER "Xiaotao Shen" shenxt1990@outlook.com

RUN apt-get update && apt-get install -y curl

COPY demo_data/ /home/rstudio/demo_data/

RUN chmod 777 /home/rstudio/demo_data/

RUN R -e 'install.packages("remotes")'

RUN R -e "remotes::install_gitlab('tidymass/tidymass')"
```

If you want to install packages (for example `ggraph`) which are necessary for you analysis, please add a new line:

```
RUN R -e 'install.packages("ggraph")'
```

And you also need to copy your data to the image use the `COPY`.

-----

## Build image

In the `terminal`, use below code to build the image.

```
docker build -t image-name -f Dockerfile .
```

Change the `image-name`.

-----

## Use the `docker tag` command to give the `tidymass` image a new name

We need to create a account on the docker hub (https://hub.docker.com/) and then use the next code to link the local image to our account.

```
docker tag image-name your-account/image-name:latest
```

-----

## Push image to docker hub

```
docker push your-account/image-name:latest
```

Then other people can download your image which contains your code, data and analysis environment, which make it is pretty easy to repeat your analysis and results.

How to pull docker image and run it can [refer this document](https://tidymass.github.io/tidymass/articles/docker.html).


-----

# Session information


```r
sessionInfo()
```

```
## R version 4.2.1 (2022-06-23)
## Platform: x86_64-apple-darwin17.0 (64-bit)
## Running under: macOS Big Sur ... 10.16
## 
## Matrix products: default
## BLAS:   /Library/Frameworks/R.framework/Versions/4.2/Resources/lib/libRblas.0.dylib
## LAPACK: /Library/Frameworks/R.framework/Versions/4.2/Resources/lib/libRlapack.dylib
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] magrittr_2.0.3
## 
## loaded via a namespace (and not attached):
##   [1] utf8_1.2.2                  tidyselect_1.1.2           
##   [3] robust_0.7-0                htmlwidgets_1.5.4          
##   [5] grid_4.2.1                  BiocParallel_1.30.3        
##   [7] munsell_0.5.0               codetools_0.2-18           
##   [9] preprocessCore_1.58.0       interp_1.1-2               
##  [11] future_1.26.1               colorspace_2.0-3           
##  [13] Biobase_2.56.0              ggfortify_0.4.14           
##  [15] knitr_1.39                  rstudioapi_0.14            
##  [17] stats4_4.2.1                robustbase_0.95-0          
##  [19] Rttf2pt1_1.3.10             listenv_0.8.0              
##  [21] mzID_1.34.0                 MatrixGenerics_1.8.1       
##  [23] GenomeInfoDbData_1.2.8      polyclip_1.10-0            
##  [25] farver_2.1.1                parallelly_1.32.0          
##  [27] vctrs_0.4.1                 generics_0.1.3             
##  [29] xfun_0.31                   itertools_0.1-3            
##  [31] randomForest_4.7-1.1        R6_2.5.1                   
##  [33] doParallel_1.0.17           GenomeInfoDb_1.32.4        
##  [35] graphlayouts_0.8.0          clue_0.3-61                
##  [37] MsCoreUtils_1.8.0           bitops_1.0-7               
##  [39] gridGraphics_0.5-1          DelayedArray_0.22.0        
##  [41] assertthat_0.2.1            scales_1.2.0               
##  [43] ggraph_2.0.5                nnet_7.3-17                
##  [45] gtable_0.3.0                globals_0.15.1             
##  [47] affy_1.74.0                 tidygraph_1.2.1            
##  [49] rlang_1.0.5                 tidymass_1.0.7             
##  [51] mzR_2.30.0                  GlobalOptions_0.1.2        
##  [53] massqc_1.0.5                splines_4.2.1              
##  [55] extrafontdb_1.0             Rdisop_1.56.0              
##  [57] lazyeval_0.2.2              impute_1.70.0              
##  [59] checkmate_2.1.0             reshape2_1.4.4             
##  [61] BiocManager_1.30.18         yaml_2.3.5                 
##  [63] backports_1.4.1             Hmisc_4.7-0                
##  [65] extrafont_0.18              MassSpecWavelet_1.62.0     
##  [67] tools_4.2.1                 bookdown_0.27              
##  [69] ggplotify_0.1.0             metid_1.2.24               
##  [71] ggplot2_3.3.6               affyio_1.66.0              
##  [73] ellipsis_0.3.2              jquerylib_0.1.4            
##  [75] RColorBrewer_1.1-3          proxy_0.4-27               
##  [77] BiocGenerics_0.42.0         MSnbase_2.22.0             
##  [79] Rcpp_1.0.8.3                plyr_1.8.7                 
##  [81] progress_1.2.2              base64enc_0.1-3            
##  [83] zlibbioc_1.42.0             purrr_0.3.4                
##  [85] RCurl_1.98-1.7              prettyunits_1.1.1          
##  [87] rpart_4.1.16                deldir_1.0-6               
##  [89] viridis_0.6.2               pbapply_1.5-0              
##  [91] GetoptLong_1.0.5            S4Vectors_0.34.0           
##  [93] SummarizedExperiment_1.26.1 masscleaner_1.0.6          
##  [95] ggrepel_0.9.1               cluster_2.1.3              
##  [97] furrr_0.3.0                 RSpectra_0.16-1            
##  [99] masstools_1.0.8             data.table_1.14.2          
## [101] blogdown_1.10               openxlsx_4.2.5             
## [103] circlize_0.4.15             RANN_2.6.1                 
## [105] pcaMethods_1.88.0           mvtnorm_1.1-3              
## [107] ProtGenerics_1.28.0         matrixStats_0.62.0         
## [109] hms_1.1.1                   patchwork_1.1.1            
## [111] evaluate_0.15               XML_3.99-0.10              
## [113] massstat_1.0.3              jpeg_0.1-9                 
## [115] massprocesser_1.0.5         readxl_1.4.0               
## [117] fastDummies_1.6.3           IRanges_2.30.0             
## [119] gridExtra_2.3               shape_1.4.6                
## [121] compiler_4.2.1              ellipse_0.4.3              
## [123] tibble_3.1.7                ncdf4_1.19                 
## [125] crayon_1.5.1                htmltools_0.5.2            
## [127] corpcor_1.6.10              pcaPP_2.0-1                
## [129] tzdb_0.3.0                  Formula_1.2-4              
## [131] tidyr_1.2.0                 rrcov_1.7-0                
## [133] DBI_1.1.3                   tweenr_1.0.2               
## [135] ComplexHeatmap_2.12.1       MASS_7.3-57                
## [137] MsFeatures_1.4.0            Matrix_1.4-1               
## [139] readr_2.1.2                 cli_3.3.0                  
## [141] vsn_3.64.0                  igraph_1.3.2               
## [143] parallel_4.2.1              GenomicRanges_1.48.0       
## [145] pkgconfig_2.0.3             fit.models_0.64            
## [147] foreign_0.8-82              plotly_4.10.0              
## [149] MALDIquant_1.21             foreach_1.5.2              
## [151] rARPACK_0.11-0              bslib_0.3.1                
## [153] ggcorrplot_0.1.3            missForest_1.5             
## [155] rngtools_1.5.2              XVector_0.36.0             
## [157] massdataset_1.0.18          metpath_1.0.5              
## [159] doRNG_1.8.2                 yulab.utils_0.0.5          
## [161] stringr_1.4.1               digest_0.6.29              
## [163] Biostrings_2.64.0           xcms_3.18.0                
## [165] rmarkdown_2.14              cellranger_1.1.0           
## [167] htmlTable_2.4.0             rjson_0.2.21               
## [169] lifecycle_1.0.1             jsonlite_1.8.0             
## [171] mixOmics_6.20.0             viridisLite_0.4.0          
## [173] limma_3.52.2                fansi_1.0.3                
## [175] pillar_1.7.0                ggsci_2.9                  
## [177] lattice_0.20-45             KEGGREST_1.36.2            
## [179] fastmap_1.1.0               httr_1.4.3                 
## [181] DEoptimR_1.0-11             survival_3.3-1             
## [183] glue_1.6.2                  remotes_2.4.2              
## [185] zip_2.2.0                   png_0.1-7                  
## [187] iterators_1.0.14            ggforce_0.3.3              
## [189] class_7.3-20                stringi_1.7.8              
## [191] sass_0.4.1                  latticeExtra_0.6-30        
## [193] dplyr_1.0.9                 e1071_1.7-11
```


