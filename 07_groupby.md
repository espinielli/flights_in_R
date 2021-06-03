# Looping with groups {#groups}

It feels like we have hardly started to investigate the lovely set of data from the SID. It would be natural to compare countries, to produce totals or min and max per country, to compare months, to give just three examples. This calls for a sort of looping: for each country or for each month, do x. In the `tidyverse` this is done with `group_by` and a host of supporting functions, as we'll explore in this chapter.

The patterns that we explore in this chapter are some of those that you'll use most. Often, your main decision is whether you need to summarise (section \@ref(summarise)) or to change and add variables, `mutate` in `tidyverse`-speak (section \@ref(mutate)), or a bit of both.


|In this chapter, you'll be introduced to:                                                                |
|:--------------------------------------------------------------------------------------------------------|
|`lapply`, `rep`, `function`, `pmin`, `pmax`, `is.list`, `unlist`, `read_xlsx`, `map`, `pmap`, `crossing` |



## Summarising {#summarise}

Summarising radically reduces the number of rows in your dataset. You will also lose variables in the process. We saw an example in section \@ref(loadco2), where we summarised to get annual totals. For that monthly data, you can think of it row-wise, as "aggregate the rows so that I just have years", or column-wise, as "get rid of the months". These are essentially the same, and both are achieved with `summarise` and related functions.

After a `summarise` all that is left are the variables created by `summarise` and the grouping variables.^[There's an exception to this rule, with geographic data using package `sf` that we'll see in chapter \@ref(maps).] 



## Dates


## Adding variables {#mutate}
