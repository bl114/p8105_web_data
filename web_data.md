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

## Get some water data

This is coming from an API

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") %>% 
  content("parsed")
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   year = col_double(),
    ##   new_york_city_population = col_double(),
    ##   nyc_consumption_million_gallons_per_day = col_double(),
    ##   per_capita_gallons_per_person_per_day = col_double()
    ## )

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") %>% 
  content("text") %>% 
  jsonlite::fromJSON() %>% 
  as_tibble()

nyc_water
```

    ## # A tibble: 42 x 4
    ##    year  new_york_city_popul… nyc_consumption_million_… per_capita_gallons_per_…
    ##    <chr> <chr>                <chr>                     <chr>                   
    ##  1 1979  7102100              1512                      213                     
    ##  2 1980  7071639              1506                      213                     
    ##  3 1981  7089241              1309                      185                     
    ##  4 1982  7109105              1382                      194                     
    ##  5 1983  7181224              1424                      198                     
    ##  6 1984  7234514              1465                      203                     
    ##  7 1985  7274054              1326                      182                     
    ##  8 1986  7319246              1351                      185                     
    ##  9 1987  7342476              1447                      197                     
    ## 10 1988  7353719              1484                      202                     
    ## # … with 32 more rows
