BRIDGES
================

make a file with bridge ID, year, fips codes, condition ratings, and a
few other variables that interest you. Make your code reproducible. Make
a plot.

``` r
library(readr)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.3     ✓ dplyr   1.0.3
    ## ✓ tibble  3.0.5     ✓ stringr 1.4.0
    ## ✓ tidyr   1.1.2     ✓ forcats 0.5.0
    ## ✓ purrr   0.3.4

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(dplyr)
delaware<-read_csv("DE1919.txt",col_names = T)
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   .default = col_double(),
    ##   STRUCTURE_NUMBER_008 = col_character(),
    ##   ROUTE_NUMBER_005D = col_character(),
    ##   HIGHWAY_DISTRICT_002 = col_character(),
    ##   COUNTY_CODE_003 = col_character(),
    ##   PLACE_CODE_004 = col_character(),
    ##   FEATURES_DESC_006A = col_character(),
    ##   CRITICAL_FACILITY_006B = col_logical(),
    ##   FACILITY_CARRIED_007 = col_character(),
    ##   LOCATION_009 = col_character(),
    ##   LRS_INV_ROUTE_013A = col_character(),
    ##   LAT_016 = col_character(),
    ##   LONG_017 = col_character(),
    ##   MAINTENANCE_021 = col_character(),
    ##   OWNER_022 = col_character(),
    ##   FUNCTIONAL_CLASS_026 = col_character(),
    ##   DESIGN_LOAD_031 = col_character(),
    ##   RAILINGS_036A = col_character(),
    ##   TRANSITIONS_036B = col_character(),
    ##   APPR_RAIL_036C = col_character(),
    ##   APPR_RAIL_END_036D = col_character()
    ##   # ... with 31 more columns
    ## )
    ## ℹ Use `spec()` for the full column specifications.

``` r
head(delaware)
```

    ## # A tibble: 6 x 123
    ##   STATE_CODE_001 STRUCTURE_NUMBE… RECORD_TYPE_005A ROUTE_PREFIX_00…
    ##            <dbl> <chr>                       <dbl>            <dbl>
    ## 1             10 1001 279                        1                4
    ## 2             10 1001A279                        1                4
    ## 3             10 1001B009                        1                3
    ## 4             10 1002 232                        1                4
    ## 5             10 1003 225                        1                3
    ## 6             10 1008 221                        1                4
    ## # … with 119 more variables: SERVICE_LEVEL_005C <dbl>, ROUTE_NUMBER_005D <chr>,
    ## #   DIRECTION_005E <dbl>, HIGHWAY_DISTRICT_002 <chr>, COUNTY_CODE_003 <chr>,
    ## #   PLACE_CODE_004 <chr>, FEATURES_DESC_006A <chr>,
    ## #   CRITICAL_FACILITY_006B <lgl>, FACILITY_CARRIED_007 <chr>,
    ## #   LOCATION_009 <chr>, MIN_VERT_CLR_010 <dbl>, KILOPOINT_011 <dbl>,
    ## #   BASE_HWY_NETWORK_012 <dbl>, LRS_INV_ROUTE_013A <chr>,
    ## #   SUBROUTE_NO_013B <dbl>, LAT_016 <chr>, LONG_017 <chr>,
    ## #   DETOUR_KILOS_019 <dbl>, TOLL_020 <dbl>, MAINTENANCE_021 <chr>,
    ## #   OWNER_022 <chr>, FUNCTIONAL_CLASS_026 <chr>, YEAR_BUILT_027 <dbl>,
    ## #   TRAFFIC_LANES_ON_028A <dbl>, TRAFFIC_LANES_UND_028B <dbl>, ADT_029 <dbl>,
    ## #   YEAR_ADT_030 <dbl>, DESIGN_LOAD_031 <chr>, APPR_WIDTH_MT_032 <dbl>,
    ## #   MEDIAN_CODE_033 <dbl>, DEGREES_SKEW_034 <dbl>, STRUCTURE_FLARED_035 <dbl>,
    ## #   RAILINGS_036A <chr>, TRANSITIONS_036B <chr>, APPR_RAIL_036C <chr>,
    ## #   APPR_RAIL_END_036D <chr>, HISTORY_037 <dbl>, NAVIGATION_038 <chr>,
    ## #   NAV_VERT_CLR_MT_039 <dbl>, NAV_HORR_CLR_MT_040 <dbl>,
    ## #   OPEN_CLOSED_POSTED_041 <chr>, SERVICE_ON_042A <dbl>,
    ## #   SERVICE_UND_042B <dbl>, STRUCTURE_KIND_043A <dbl>,
    ## #   STRUCTURE_TYPE_043B <chr>, APPR_KIND_044A <dbl>, APPR_TYPE_044B <chr>,
    ## #   MAIN_UNIT_SPANS_045 <dbl>, APPR_SPANS_046 <dbl>, HORR_CLR_MT_047 <dbl>,
    ## #   MAX_SPAN_LEN_MT_048 <dbl>, STRUCTURE_LEN_MT_049 <dbl>,
    ## #   LEFT_CURB_MT_050A <dbl>, RIGHT_CURB_MT_050B <dbl>,
    ## #   ROADWAY_WIDTH_MT_051 <dbl>, DECK_WIDTH_MT_052 <dbl>,
    ## #   VERT_CLR_OVER_MT_053 <dbl>, VERT_CLR_UND_REF_054A <chr>,
    ## #   VERT_CLR_UND_054B <dbl>, LAT_UND_REF_055A <chr>, LAT_UND_MT_055B <dbl>,
    ## #   LEFT_LAT_UND_MT_056 <dbl>, DECK_COND_058 <chr>,
    ## #   SUPERSTRUCTURE_COND_059 <chr>, SUBSTRUCTURE_COND_060 <chr>,
    ## #   CHANNEL_COND_061 <chr>, CULVERT_COND_062 <chr>, OPR_RATING_METH_063 <dbl>,
    ## #   OPERATING_RATING_064 <dbl>, INV_RATING_METH_065 <dbl>,
    ## #   INVENTORY_RATING_066 <dbl>, STRUCTURAL_EVAL_067 <dbl>,
    ## #   DECK_GEOMETRY_EVAL_068 <chr>, UNDCLRENCE_EVAL_069 <chr>,
    ## #   POSTING_EVAL_070 <dbl>, WATERWAY_EVAL_071 <chr>, APPR_ROAD_EVAL_072 <dbl>,
    ## #   WORK_PROPOSED_075A <dbl>, WORK_DONE_BY_075B <dbl>, IMP_LEN_MT_076 <dbl>,
    ## #   DATE_OF_INSPECT_090 <dbl>, INSPECT_FREQ_MONTHS_091 <dbl>,
    ## #   FRACTURE_092A <chr>, UNDWATER_LOOK_SEE_092B <chr>, SPEC_INSPECT_092C <chr>,
    ## #   FRACTURE_LAST_DATE_093A <chr>, UNDWATER_LAST_DATE_093B <chr>,
    ## #   SPEC_LAST_DATE_093C <chr>, BRIDGE_IMP_COST_094 <dbl>,
    ## #   ROADWAY_IMP_COST_095 <dbl>, TOTAL_IMP_COST_096 <dbl>,
    ## #   YEAR_OF_IMP_097 <dbl>, OTHER_STATE_CODE_098A <dbl>,
    ## #   OTHER_STATE_PCNT_098B <dbl>, OTHR_STATE_STRUC_NO_099 <chr>,
    ## #   STRAHNET_HIGHWAY_100 <dbl>, PARALLEL_STRUCTURE_101 <chr>,
    ## #   TRAFFIC_DIRECTION_102 <dbl>, TEMP_STRUCTURE_103 <lgl>,
    ## #   HIGHWAY_SYSTEM_104 <dbl>, …

``` r
library(ggplot2)
ggplot(data = delaware, mapping = aes(x = YEAR_BUILT_027, y = STRUCTURAL_EVAL_067)) + 
  geom_point(alpha = 1/3) + ggtitle("Structural vs Year Built")
```

![](README_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
ggtitle("Structural vs Year Built")
```

    ## $title
    ## [1] "Structural vs Year Built"
    ## 
    ## attr(,"class")
    ## [1] "labels"

``` r
smaller<-select(.data = delaware, STRUCTURE_NUMBER_008,YEAR_BUILT_027,COUNTY_CODE_003,STRUCTURAL_EVAL_067,SERVICE_LEVEL_005C)
smaller
```

    ## # A tibble: 879 x 5
    ##    STRUCTURE_NUMBE… YEAR_BUILT_027 COUNTY_CODE_003 STRUCTURAL_EVAL…
    ##    <chr>                     <dbl> <chr>                      <dbl>
    ##  1 1001 279                   1928 003                            5
    ##  2 1001A279                   1900 003                            6
    ##  3 1001B009                   1919 003                            7
    ##  4 1002 232                   1933 003                            6
    ##  5 1003 225                   1990 003                            6
    ##  6 1008 221                   1960 003                            7
    ##  7 1009 221                   1839 003                            6
    ##  8 1011N267                   1997 003                            6
    ##  9 1011S267                   1997 003                            5
    ## 10 1014 221                   1962 003                            7
    ## # … with 869 more rows, and 1 more variable: SERVICE_LEVEL_005C <dbl>
