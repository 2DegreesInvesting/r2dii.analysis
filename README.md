
<!-- README.md is generated from README.Rmd. Please edit that file -->

# r2dii.analysis <a href='https://github.com/2DegreesInvesting/r2dii.match'><img src='https://imgur.com/A5ASZPE.png' align='right' height='43' /></a>

<!-- badges: start -->

[![lifecycle](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![CRAN
status](https://www.r-pkg.org/badges/version/r2dii.analysis)](https://CRAN.R-project.org/package=r2dii.analysis)
[![R build
status](https://github.com/2DegreesInvesting/r2dii.analysis/workflows/R-CMD-check/badge.svg)](https://github.com/2DegreesInvesting/r2dii.analysis/actions)
<!-- badges: end -->

These tools implement in R a fundamental part of the software PACTA
(Paris Agreement Capital Transition Assessment), which is a free tool
that calculates the alignment between financial portfolios and climate
scenarios (<https://2degrees-investing.org/>). Financial institutions
use PACTA to study how their capital allocation impacts the climate.
This package matches data from financial portfolios to asset level data
from market-intelligence databases (e.g. power plant capacities,
emission factors, etc.). This is the first step to assess if a financial
portfolio aligns with climate goals.

## Installation

Install the development version of r2dii.analysis with something like
this:

``` r
# install.packages("devtools")

# To install from a private repo, see ?usethis::browse_github_token()
devtools::install_github("2DegreesInvesting/r2dii.analysis", auth_token = "abc")
```

## Example

``` r
# r2dii.data is changing rapidly; ensure you have the latest version
# remotes::update_packages("r2dii.data", upgrade = "ask")
library(r2dii.data)
library(r2dii.match)
library(r2dii.analysis)
```

### `r2dii.analysis` picks up where `r2dii.match` leaves off.

First, identify matches between your loanbook and the asset level data.

``` r
valid_matches <- match_name(loanbook_demo, ald_demo) %>%
  prioritize()
```

### Join a validated matched loanbook object to asset and scenario data

Next, join your loanbook, to these validated matches, and to the
relevant scenario data. This step also subsets assets in the relevant
scenario regions.

``` r
loanbook_joined_to_ald_scenario <- valid_matches %>% 
  join_ald_scenario(ald_demo, 
                    scenario_demo_2020, 
                    region_isos_demo)
```

This dataset is used by all subsequent steps. \#\#\# Calculate company
and/ or portfolio level targets Scenario targets can be calculated per
company:

``` r
company_target <- summarize_company_production(loanbook_joined_to_ald_scenario) %>% 
  add_company_target()
```

…or for the whole portfolio:

``` r
portfolio_target <- summarize_portfolio_production(loanbook_joined_to_ald_scenario) %>% 
  add_portfolio_target()
```
