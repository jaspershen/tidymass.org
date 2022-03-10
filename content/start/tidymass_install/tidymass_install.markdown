---
title: "Install tidymass"
weight: 1
categories: [installation]
---



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

<img src="figures/Screen-Shot-9.png" width="100%" style="display: block; margin: auto;" />

![](figures/Screen-Shot-9.png)

### Install tidymass packages one by one

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


### Install tidymass packages one by one

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

### Install tidymass packages one by one.

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

## Frequently Asked Questions

1. Can not install dependent packages `raster` and `Cario`

`raster` is a package in [CRAN](https://cran.r-project.org/web/packages/raster/index.html), try to install it first and then install `tidymass`.


```r
install.packages("raster")
```


`Cario` is a package in [CRAN](https://cran.r-project.org/web/packages/Cairo/index.html), try to install it first and then install `tidymass`.


```r
install.packages("Cairo")
```

2. Error when install `massprocesser`

The error is:

```
Error: .onLoad failed in loadNamespace() for 'affy', details:
  call: assign(".affyInternalEnv", .affyInternalEnv, envir = topenv(parent.frame()))
  error: cannot add binding of '.affyInternalEnv' to the base environment
Execution halted
ERROR: lazy loading failed for package 'massprocesser'
* removing 'D:/R_packages/R4.0/library/massprocesser'
```

Just try to restart R and install `affy`.


```r
BiocManager::install("affy")
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


