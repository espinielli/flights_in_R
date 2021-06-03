# Filtering a dataset and refining the CO~2~ graph {#filter}

In this chapter we improve the CO~2~ emissions graph, _en route_ learning how to filter observations from a dataset, to add new variables, and to use the `pipe` operator `%>%`.


|In this chapter, you'll be introduced to:                                                            |
|:----------------------------------------------------------------------------------------------------|
|`filter()`, `==`, `%in%`, `mutate()`, `slice_max()`, `%>%`, `geom_text_repel()`, `scales_colour_...` |

On re-opening your `justlearning` project, you should still have the `annual_co2` dataset in your environment, since RStudio saves these data and reloads on restart. If not, you should find that `annual_co2.rda` is saved in the `data` folder, and you can load it using `load("data/annual_co2.rda")`. One R speciality here is that you don't need to assign the result with a `annual_co2 <- ...`; in fact the single file that we saved could have contained a number of datasets, and they're all reloaded _with their original dataset names_.^[If even that doesn't work, and for some reason the `rda` file is not in your data folder, go back to section \@ref(loadco2), load the Excel file again, and find the code to summarise over years.] 

If you have closed RStudio and re-opened it, then you have a new session and need to reload packages, in this case `library(tidyverse)`. Make yourself a new R script file for 'chapter4' starting with this.

## Sequences of functions

It's time for a bit more syntax. We saw already that you can combine functions by making one the parameter to another. Or to put it another way, wrapping one around the other. As you combine more functions, this soon becomes hard to read, and you have to rely on the editor to help you spot whether you've enough brackets closed at the right point.

For that reason we use a different syntax, the 'pipe' operator `%>%` introduced by the `magrittr` package and adopted by the tidyverse. If you learn only one control-key combination in RStudio, do learn shift-ctrl-M (also shift-command-M on Mac). This must be the fastest way to type '%>%'! [Try it in your console.]  

With a new line after each `%>%` and appropriate indenting (RStudio helps with that), you get code that looks like this. This says, fill `a` with what you get from dataset `b` after applying function `fun1`, then function `fun2`.



The pipe syntax works because `fun1` actually has a parameter list that _starts_ with the dataset to which it should be applied `fun1(data, p1,...)`. So `b %>% fun1(p1)` is just another way of writing `fun1(b, p1)`. Or, the other way around, you can use `%>%` whenever you have a function whose first parameter is the dataset to be operated on. It turns out that this is true for very many of them, and the tidyverse is designed that way. [Does the code to summarise by year that you saw in section \@ref(loadco2) make more sense now that you know about `%>%`?]

## Filtering datasets and logical tests

As is so often the case, in R there are several of ways to select rows from a dataframe or tibble. We'll focus on the `filter(data, test)` function. For this we need to know how to construct a logical test.

There are three parts to this: the logic, the test functions and what's being tested. 

1) Logic is mostly given by `&`, `!` and `|`^[that's a vertical bar, not an 'l' or 'I'] for 'and', 'not' and 'or', grouped with round brackets in the normal way. 
2) The test functions are _almost_ as you might expect: `>`, `<`. However, in R you need to use `==` not `=` to test for equality. I suspect this creates the most common typo in R code! Check the documentation of `dplyr::filter` for a more complete list of test functions: in particular look out for `%in%`, which we will use shortly.
3) One really nice touch in `filter()` is in the third part: what's being tested. One of the trickiest bits of learning R is knowing how, within a function, to refer to one or more variables of the dataset. In `filter()` you can just use the name of the variable; so no quotes needed around the name, and the code assumes it is a variable from the `data` parameter, so no need to use `annual_co2$...`.

In this example, we don't push the result into a dataset (no `a <- `), so it gets printed out directly.


```
## # A tibble: 2 x 4
##    YEAR STATE_NAME CO2_QTY_TONNES    TF
##   <dbl> <chr>               <dbl> <dbl>
## 1  2018 ALBANIA           143871. 12744
## 2  2018 CZECHIA          1381672. 90679
```

Note that:

* the order of the rows is as in the original dataset, not at all influenced by the order of the naming of the states in the test
* `YEAR` and `STATE_NAME` in the `filter` function are so-called 'bare' strings (ie without quotes) that _name_ variables, and are shorthand for `annual_co2$YEAR` etc
* but "CZECHIA" is a _value_ of a variable so needs to be a string in quotes
* and the test is case-sensitive. [Try changing it to "Czechia".]

## Selecting the busiest states {#topStates}

In the previous chapter we selected the first-named states with `head()`. Now we do something more useful: selecting the top states by flights, using `slice_max()`. We need to define this a bit more clearly. 'Top' could mean in a particular year, or over the whole period (where flights have decreased as well as increased). We choose to mean 'top by flights in 2019'. This is partly out of habit (top in the latest year is often the meaning) and partly because we need to introduce fewer new bits of R to implement it.

The code is like this. A final novelty is that we use `pull()` which, as its help-file says, does the same as `$` (learned in the last chapter) but looks nicer in pipes. [Where can you look at `top_states` to check that there are 8 values?]



`slice_max()` is a good example of how R changes with time. New versions of packages introduce minor or major changes. Sometimes a function is `superseded` (left to rot), other times it may be `deprecated` (you have some time to switch to a new version before it's removed). The function `top_n`, which you might see lying around in legacy code, has been superseded by `slice_max()`. [Check out the documentation of `top_n` for some of the reasons.] 

These changes mean that just updating to the latest version of the package is not always the best idea, because you might have to spend some time checking for changes. It also means that, when searching on the web for hints, snippets and answers, you need to look at the date of the answer. `ggplot` in particular has changed quite a bit, so answers more than 5 years old or so might not be that helpful.

## CO~2~ graph for the top states

With `top_states` in place we can easily plot the data for the busiest states. 

We update the title, and add a footnote (`caption`) to explain what's going on. We could have created a new dataset, eg `top_co2 <- annual_co2 %>% filter(STATE_NAME %in% top_states)`, and then used this in the `ggplot`. But we only plan to use this filter once, so to avoid cluttering the environment with datasets, we filter 'on the fly', within the `ggplot` statement. In this case, this is a matter of personal preference. If the datasets were a lot larger, and we intended analysing and transforming just the top states in further graphs, the decision might be different.

<img src="04_FilteredCO2_files/figure-html/unnamed-chunk-4-1.png" width="672" style="display: block; margin: auto;" />

One advantage of filtering on the fly is that we can change on the fly. [Change the filter to the states _not_ in the top 8. It takes one key press. Re-run the graph. Update the titles too.]

As an analyst, I really want to know which country is which. With just 8 states shown, maybe I can use symbols? At one level it's quick to do. Just replace `group = ` with `shape = `. [Try this.] `ggplot` complains that it doesn't really like using more than 6 symbols, because it becomes hard for the reader to jump between legend and graph. We could roll with it and learn how to extend the palette, but is there another solution?

We can separate states by colours. Again, quite quick: replace `colour = YEAR, group = STATE_NAME` with `colour = STATE_NAME`. [Try this.] The line means we can work out the ordering of the years easily, so losing the year isn't a big issue here. But eight colours is also a lot to tell apart. Even without colour blindness, you might think they're clear but when projected onto a screen or printed or on a tiny phone screen, perhaps not.

## Labelling the CO~2~ graph {#labelco2}

It's relatively easy to follow a slightly different route: adding state names directly to the graph. This is done with `geom_text` added to the `ggplot`. We only want to label the point in 2019 rather than all years, so we create a new variable which is empty except in rows for the year 2019. `mutate(a = ...)` is the function for adding a variable 'a' to the dataset^[and we use lower case for the variable name, because that's our preference, even if the imported names don't do this]. `if_else()` is how we give a value for only some years. As with the `filter()` function, we can refer to other variables in `annual_co2` without inverted commas. 

This time, we amend the `annual_co2` dataset itself, because we want to be able to use this in several places, not just in a single graph. The syntax `a <- a %>% ...` follows the same pattern as you saw earlier, but you're overwriting the original dataset.

The `geom_text()` _inherits_ all of the aesthetics from the `ggplot` function, so it already knows where to find x and y coordinates, and what colour to use. We still need to tell `geom_text()` where to find the labels. This is also an aesthetic.

<img src="04_FilteredCO2_files/figure-html/unnamed-chunk-5-1.png" width="672" style="display: block; margin: auto;" />

This is close, but not quite good enough. The text is centred on the 2019 point, and this creates some ugly overlaps. There are lots of options in `geom_text()` to adjust the position, and you'll find lots of examples on the web. So we could spend time adjusting the positions. But this is a first example of the rule: 'surely someone has already come across this problem?'.

Someone has indeed spent time to come up with good ways to deconflict and position labels on graphs. The package is called `ggrepel`, which you might need to install with `install.packages("ggrepel")`, and it provides a 'drop in' replacement for `geom_text` naturally called `geom_text_repel`. To avoid using `library("ggrepel")` when we're just using one function from the package on one occasion, we use the double-colon syntax in the code (see \@ref(twocolons)).

The defaults for this function work pretty well in this particular case. But there are a couple of things I'd like to fix: the block capitals and the year legend. Title case would be nicer, so we convert the `STATE_NAME` using the `stringr` package function `str_to_title()`. `stringr` is already loaded as part of the tidyverse, and since most of its functions begin 'str_' it's quite easy to start searching in the help pane for the right one [Try this.]. In this case `state_label` already exists and we overwrite it.

The other thing to improve is the year scale, which shows with meaningless decimals. We use a quick-ish fix, rather than the tidiest-possible solution. Scales, whether the axes or colours, are controlled by `ggplot` functions starting `scales_` in this case `scales_colour_steps()` gets us a scale that shows the individual years. This is a 'dirty' solution in the sense that, if the data for 2020 get included, you might need to tweak the code; but then, we've hard-coded 2019 in a number of places, so this is dirty elsewhere. We'll see cleaner options later (for example in section \@ref(factors)).

<img src="04_FilteredCO2_files/figure-html/unnamed-chunk-6-1.png" width="672" style="display: block; margin: auto;" />

## What does the graph say about CO~2~?

Longer-haul flights use heavier aircraft and therefore the [proportion of long-haul flights in a national mix is a major influence on CO~2~ per flight](https://www.eurocontrol.int/publication/eurocontrol-data-snapshot-co2-emissions-flight-distance). For example, the Netherlands has more CO~2~ per flight than Norway: Norway has a significant domestic (so short-range) market, which the Netherlands does not, being instead a major long-haul hub. These two states have been relatively stable. 

The UK also has a proportionally larger long-haul market. And a decline in its domestic market has led to quite a rapid increase in CO~2~ per flight in recent years. 

So, the graph helps to build a story: though we needed some supporting information to provide some of the explanation. It has also become clear that we're interested in CO~2~ per flight. See the exercises for a graph on that more directly.

## What's gone wrong?

It's inevitable that you will type `=` in tests where you mean `==`. Some functions have friendly messages, since this is so common. Others less so. 

Watch that case! We are using the function `filter()`, not the function `Filter()` which is something else entirely.

`if_else` is a fussy version of the base function `ifelse`, that we use here to maximise use of tidyverse functions. If it warns you that the 'false' must be something, then it has your long-term interests at heart. It just means that it's a different type to the 'true'. Compare `ifelse(1<2,"true",NA)` and `if_else(1<2,"true",NA)` where `NA` is the code for missing. 

## Exercises

### Questions

1) Where can you look to see that `top_states` has 8 values?
2) How does `geom_text()` know where to place the text on the graph?
3) Adapt `geom_text()` to shift all labels up 2 (million tonnes!). (Hint: Check the help file.)
4) Adapt the final graph to show the smallest 8 states instead. (Hints: What's the most likely counterpart to `slice_max`? Closely-related functions are often to be found in the _same_ help file, so checking the help for a function you know might help you find similar ones that you don't.)
5) Adapt the graph to show year on the x-axis and CO~2~/flight on the y-axis. (Hints: Mostly changing the first `aes()` and deleting some elements. For a pretty graph you might google how to hide the legend "ggplot hide legend", and how to set the breaks on the x-axis.)

### Answers

1) The environment pane, but you need to scroll down to "values" because it's a simple vector.
2) `geom_text()` inherits all aesthetics from the opening `ggplot(..., aes(...))`, which can be supplemented or over-written by its own `aes()`. In fact you could put the `label=` into the first `aes()`.
3) `geom_text(aes(label = state_label), nudge_y = 2)`. If you put the `nudge_y` inside the `aes()` you get an error ('Ignoring unknown aesthetics: nudge_y') because it's not something that can vary with the values of a variable ("not an aesthetic").
4) `geom_text_repel` uses call-out lines when it can't get the text close. You could experiment with making these a less confusing colour. You might also drop the 'millions'.
<img src="04_FilteredCO2_files/figure-html/unnamed-chunk-7-1.png" width="672" style="display: block; margin: auto;" />

4) A pedant might say this shouldn't be a line chart, but here's one possibility. We'll see a different version of this in section (TBD)

<img src="04_FilteredCO2_files/figure-html/unnamed-chunk-8-1.png" width="672" style="display: block; margin: auto;" />
