Reading data from the web
================

## Scrape a table

I want the first table from \[this page\]
<http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm>

Read in the html

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url) 
```

Extract the table(s): focus on the first one

``` r
tabl_marj =
  drug_use_html %>% 
  html_nodes(css = "table") %>% 
  first() %>% 
  html_table() %>% 
  slice(-1) %>% 
  as_tibble()
```

## Star Wars movie info

I want the data from here: <https://www.imdb.com/list/ls070150896/>

``` r
url = "https://www.imdb.com/list/ls070150896/"

swm_html = read_html(url)
```

Grab elements that I want

``` r
title_vec =
  swm_html %>% 
  html_nodes(css = ".lister-item-header a") %>% 
  html_text()
  
gross_rev_vec =
  swm_html %>% 
  html_nodes(css = ".text-muted .ghost~ .text-muted+ span") %>% 
  html_text()

runtime_vec =
  swm_html %>% 
  html_nodes(css = ".runtime") %>% 
  html_text()

swm_df =
  tibble(
    title = title_vec,
    gross_rec = gross_rev_vec,
    runtime = runtime_vec
  )

swm_df
```

    ## # A tibble: 9 x 3
    ##   title                                          gross_rec runtime
    ##   <chr>                                          <chr>     <chr>  
    ## 1 Star Wars: Episode I - The Phantom Menace      $474.54M  136 min
    ## 2 Star Wars: Episode II - Attack of the Clones   $310.68M  142 min
    ## 3 Star Wars: Episode III - Revenge of the Sith   $380.26M  140 min
    ## 4 Star Wars: Episode IV - A New Hope             $322.74M  121 min
    ## 5 Star Wars: Episode V - The Empire Strikes Back $290.48M  124 min
    ## 6 Star Wars: Episode VI - Return of the Jedi     $309.13M  131 min
    ## 7 Star Wars: Episode VII - The Force Awakens     $936.66M  138 min
    ## 8 Star Wars: Episode VIII - The Last Jedi        $620.18M  152 min
    ## 9 Star Wars: The Rise Of Skywalker               $515.20M  141 min
