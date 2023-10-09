# GGPlot
This document is meant to be an expansion to [ggplot2 cheatsheet](cheatsheets/ggplot2.pdf)

## Intro
* First of all, `ggplot2` package must be installed (`install.packages("ggplot2")`) and imported (`library(ggplot2)`)
* GGPlot operates on data frames. Vector or a matrix can be converted to one using `as.data.frame(...)`
* GGPlot is created on a builder basis - layers and styles are applied onto a plot one by one:
  * first, a blank `ggplot(...)` containing just the axes is created (can be saved to a variable)
  * data can be drawn on the blank plot by adding various functions (using a simple `+` operator)
  * other data can be then added to make the plot more complex
  * styles are applied to the plot in a similar way
  * simplified example: `ggplot(data) + some_points() + some_lines() + some_style() + other_points()`
  * if the plot is saved to a variable (e.g. `graph <- ...`), call `graph` directly to draw it

## Theme
* Altering a theme instance:
  * `<theme1> + <theme2>` - merges two themes; clashing options are taken from theme1
  * `<theme1> %+replace% <theme2>` - merges two themes; clashing options are taken from theme2
* Obtaining current global theme: `theme_get()`
* Setting a global theme (if needed, all three functions return the old theme, so it can be saved):
  * `theme_set(<theme>)` - overrides the current theme
  * `theme_update(<theme>)` - merges two themes using `+`
  * `theme_replace(<theme>)` - merges two themes using `%+replace%`


