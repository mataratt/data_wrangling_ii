reading_data_from_the_web
================
2025-10-20

Import NSDUH data from the web

``` r
url = "https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
```

Defining the url and then reading in the url as html.

This is an “easy” case

``` r
ndsuh_df =
drug_use_html |> 
  html_table() |> 
  first() |> 
  slice(-1)
```

`first()` gives you the first table (the first thing in the list) you
can also use `nth()` and insert the table number in () `slice()` can get
rid of the row

Slightly harder case

``` r
url = "https://www.imdb.com/list/ls070150896/"

sw_html =
  read_html(url)
```

Now pull out elements of the html that I care about using SelectorGadget

``` r
title_vec =
sw_html |> 
  html_elements(".ipc-title-link-wrapper .ipc-title__text--reduced") |> 
  html_text()

metascore_vec =
  sw_html |> 
  html_elements(".metacritic-score-box") |> 
  html_text()

runtime_vec =
  sw_html |> 
  html_elements(".dli-title-metadata-item:nth-child(2)") |> 
  html_text()

sw_df =
  tibble(
    title = title_vec,
    metascore = metascore_vec,
    runtime = runtime_vec
  )
```
