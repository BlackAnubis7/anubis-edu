# Built-in plots
## Common traits
Built-in plots have much in common (mostly because under the hood these functions call each other). Some additional
options are available in all types (with obvious exceptions - for example you can't use axis options in pie charts):
* `main='...'` - title for the plot
* `sub='...'` - subtitle for the plot
* `xlab='...'` - X axis title
* `ylab='...'` - Y axis title
* `ann=TRUE` - if set to FALSE, axes will not have any titles
* `xlim=c(x1, x2)` - X axis will span from x1 to x2
* `ylim=c(y1, y2)` - Y axis will span from y1 to y2
* `log='...'` - if `'x'`, `'y'` or `'xy'`, given axes will have logarithmic scale; if `''` or unspecified, neither axis will
* `plot=TRUE` - if set to FALSE, nothing will be drawn - plot parameters will be printed instead
* Various subsets of options from "Graphical parameters" section down below. Most importantly:
  * `col` - sets colour of points, lines, bars, pie slices etc.

## Plot
`plot` is the most basic plot type - majority of other plots call this function under the hood.
Calling just `plot(...)` is synonymous to `plot.default(...)`. To, add points to an existing plot instead of making a
new one, use `points(...)` with similar arguments.
* If given one vector of values, they are placed on Y axis, with indices as X axis
* If given two vectors, first is put on X axis and second on Y axis. The vector lengths have to be equal.

```r
plot(5)  # only point (1, 5)
plot(7:9)  # points (1,7), (2,8), (3,9)
plot(3, 7)  # point (3, 7)
plot(2:4, 6:8)  # points (2,6), (3,7), (4,8)
```

### Extra arguments
* `type='...'` - affect what is drawn
  * `'p'` (default) - "points" (only points are drawn)
  * `'l'` - "lines" (only lines connecting points are drawn)
  * `'b'` - "both" (points with lines; lines do not touch the points)
  * `'c'` - ('b', but without points; lines are separated)
  * `'o'` - "overplotted" ('p' + 'l'; lines are drawn over the points)
  * `'h'` - "histogram" (only vertical lines going from 0 to the points)
  * `'s'` - "steps"
  * `'n'` - "no" (only draws the background, does not draw points and lines)
* "Graphical parameters" options (can often accept a vector - values are used cyclically; such vector can have any length)

## Matplot (matrix plot)
Plot for matrices - puts multiple standard plots on top of each other. Matrices must have equal number of rows.
* If given one matrix, its values will be placed on Y axis; X axis will have row numbers
* If given matrices have different sizes, the smaller one's columns will be cycled through (no need to be a sub-multiple)
* Vectors can serve as 1-columned matrices

### Extra arguments
* All `plot(...)` arguments; vector values, instead of being applied to each point, get applied to whole matrix columns

## Pie
Takes a vector of non-negative values and draws a pie-chart out of them. They don't have to sum up to any specific
number, the percentages will be calculated automatically
* `labels=...` - vector of labels the values should have
  * if vector is too short, it is not cycled - remaining pie fragments will not have labels at all
  * if a value is `NA`, or produces `NA` upon conversion to a string, particular pie fragment will have no label
  * if a vector is too long, remaining values will be ignored
  * if not passed, uses `names(<data>)` function value (or indices, if `names` return NULL)
* `edges=...` - pie chart outline will have *approximately* this many edges (default: 200)
* `radius=...` - accepts a number from [-1, 1] range; sets chart size (default: 0.8)
  * negative radius will rotate the chart by 180 degrees (labels will not be rotated)
* `clockwise=FALSE` - self-explanatory (default: FALSE)
* `init.angle=...` - angle in degrees where the pie chart starts (default: 0 normally, 90 if `clockwise=TRUE`)
  * angle is counted from 3 o'clock counter-clockwise - e.g. `init=120` will be at 11 o'clock
* `col=...` - vector of colours to fill pie chart fragments (will be cycled; default: 6 pastel colours)
* `border='...'` - colour of border (both pie outlines and lines between fragments). Cycled through fragments if vector
* `lty='...'` - described in "Graphical parameters" section. Cycled through fragments if vector
* "Graphical parameters" will be applied only to main title and fragment labels

## Bar plot
* Usage: `barplot(<data>)`
* If given a vector, will simply draw a bar plot
* If given a matrix, will draw a bar group for each column. Each bar group will consist of bars (one for each row) on
    top of each other (if the value for one column's rows are 2, 3 and 4, the bar will have total height of 9)
* If given a data frame, instead of `barplot(<data>)` use `barplot(<formula>, data=<data>)`. Formula can **only** be:
  * `<Y> ~ <X>` - data frame column X will be on x-axis, and column Y on y-axis
  * `<Y> ~ <X1 + X2>` - each X2 value will have a bar group, consisting of a bars for each X1
  * `cbind(Y1, Y2, ...) ~ <X>` - each X value will have a bar group, with a bar for each Y column
  * *(in all examples above, Y columns have to be numeric, while X can be of any type)*
* Additional arguments:
  * `width=...` - vector describing width of bars (cycled if too short). Setting it to `c(1,4)` means that columns
      will have widths of 1x and 4x, but the value of x will be optimised to fit in the plot perfectly (thus setting
      it to a single value has no effect)
  * `beside=FALSE` - if set to TRUE, bars in bar groups are not on top of each other, but next to each other
  * `space=...` - space between bars (relative, where 1 means 100% of average bar width)
    * if `beside` is FALSE or not specified, `space` can take a vector with length being a sub-multiple of number of
        bars - vector is cycled and placed before each bar (first bar consumes a value, but space is not shown)
    * if `beside` is TRUE, `space` can take:
      * vector with length of **exactly** bar number (usually number of elements in a matrix)
      * vector of **exactly** two numbers - space between bars in one bar group and between bar groups respectively
  * `horiz=FALSE` - if set to TRUE, bars will be horizontal
  * `col=...` - a vector of colours to fill pie chart fragments (will be cycled; default: shades of gray)
  * `border=...` - border colour for columns (if a vector, will be cycled; `NA` values mean no border)
  * `add=FALSE` - if set to TRUE, bars will be added to existing plot

## Histogram
* Usage: `hist(<data>)` (data can be a vector or a matrix; values have to be numeric) - histogram bar heights will be
    based on number of values in automatically generated ranges
* Additional options:
  * `breaks=...` - can be:
    * a vector of "breakpoints". If set to `c(0, 20, 100)`, there will be two bars - one from 0 to 20, and second
        from 20 to 100. All values in histogram data have to be between first and last element of given vector
    * ... or a function that takes histogram data as its argument and returns that vector
    * number of equal-spaced breakpoints to be placed (R documentation warns that this will be treated as a suggestion)
    * ... or a function that takes histogram data as its argument and returns that number
  * `right=TRUE` - decides whether a value equal to a breakpoint will be assigned to a bar to the right or to the left
    (default: TRUE)
  * `col=...` - a vector of colours to fill the bars (will be cycled; default: `"lightgray"`)
  * `border=...` - border colour for bars (can be a vector; will be cycled)
  * `labels=FALSE` - if TRUE, additional labels will be drawn on top of bars

## Box plot
* Usage: `boxplot(<data>)` - one box will be drawn for each matrix column (we treat vector as a one-column matrix)
* For usage with data frames, refer to "Bar plot" section above
* Additional options:
  * `boxwex=...` - relative width of the boxes (default: 1)
  * `horizontal=FALSE` - if set to TRUE, boxes will be horizontal
  * `add=FALSE` - if set to TRUE, bars will be added to existing plot

## Adding a legend
After a plot is drawn, a legend can be added with a `legend(...)` function.
* `legend('bottomright', c('abc', 'def'))` will add a two-element legend in the plot's bottom-right corner
  * `top`, `right`, `bottom`, `left`, `topright`, `bottomright`, `bottomleft` and `topleft` can be used this way
* `legend(5, 107, c('abc', 'def'))` - the legend will be placed so that its top-left corner is on coords (5, 107)
  * if unable to find these coords, for example one of the axes is non-numerical, legend will not be drawn
  * drawing by coords is impossible for plots with no coords at all (like pie chart or bar plot)
* List of labels can contain `NA` values (treated like empty strings)
* Additional options
  * `pch`, `col` - graphical options affecting points shown next to labels (can be vectors, will be cycled)
    * if `pch` is not specified, no points will be drawn
    * wherever a `NA` is used as a `pch` vector value, no point will be drawn
  * `lty`, `lwd` - line types and widths (can be vectors, will be cycled)
    * if none of those is specified, lines will not be drawn
  * `fill` - takes colour (or a vector of these; will be cycled). Labels will have squares filled with colours
    * good for showing colours, but might not work well if mixed with points or lines
    * if not specified, squares will not be drawn
  * `bty='...'` - if is `'o'` *[default]*, the box is drawn; if `'n'` - it's not
  * `bg` - box background colour
  * `box.lty`, `box.lwd`, `box.col` - box line type, line width and colour respectively
  * `pt.cex`, `pt.lwd` - point size and its line width
  * `title='...'` - legend title
  * `ncol=...` - number of columns for legend elements. Value 1 *[default]* means a normal vertical list
  * `xjust`, `yjust` - if coordinates are specified by numbers, by default they set top-left corner location
    * `0` - top / left respectively
    * `.5` - center
    * `1` - bottom / right

---
# Graphical parameters
For help (and more options), use `?par`. These arguments might be interpreted differently on different machines.
**Not all arguments are supported on all graphic functions**
![](img/r_plot_pch.png)
*https://www.w3schools.com/r/r_plot_pch2.png*
* `ann=TRUE` - if set to FALSE, axis names and plot titles will not be drawn
* `bg='...'` - point fill (only `pch` between 21 and 25 support filling with a colour)
  * Might do something else in functions that call neither `plot` nor `points`
* `bty=''` - shape of the box (lines around the plot)
  * `'o'` *[default]* - box around the plot (like O, thus the name)
  * `'l'`, `'7'`, `'c'`, `'u'`, `']'` - shaped like `L`, `7`, `C`, `U` and `]` respectively
  * `'n'` - none
* `cex=...` - point size, relative to the default (2 = 200%, .3 = 30%)
  * `cex.axis=...` - axis elements (i.e. numbers on the axis) size, relative to main `cex` (or to default, if `cex` not set)
  * `cex.lab=...` - same, but for axis titles
  * `cex.main=...` - same, but for plot title
  * `cex.sub=...` - same, but for plot subtitle
* `col='...'` - colour of points and lines
  * colour names can be passed (all colour names can be generated by `colors()`)
  * `'#RRGGBB'` hex string can be used
  * `rgb`, `hsv`, `hcl`, `gray` and `rainbow` functions can be used
  * `NA` usually means transparent
  * `col.axis`, `col.lab`, `col.main`, `cex.sub` - similar to `cex`
* `family='...'` - font family
  * `'serif'`, `'sans'`, `'mono'`
  * fonts described in `?Hershey`
  * device-specific fonts may be available
* `fg='...'` - colour of axis lines and the box
* `font=...` - font style
  * 1 *[default]* - plain
  * 2 - bold
  * 3 - italic
  * 4 - bold italic
  * `font.axis`, `font.lab`, `font.main`, `font.sub` - similar to `cex`
* `lab=c(x, y, _)` - *(approximate)* number of fragments X and Y axes are divided. Third number is mandatory, but unused
* `las=...` - style of axis numbers
  * 0 *[default]* - parallel to the axis
  * 1 - horizontal
  * 2 - perpendicular to the axis
  * 3 - vertical (for reading from the side)
* `lend=...` - line ends
  * 0 or `'round'` *[default]* - round
  * 1 or `'butt'` - square
  * 2 or `'square'` - square *(lines appear longer than for `"butt"`)*
* `lty=...` - line type
  * 0 or `'blank'` - no lines
  * 1 or `'solid'` *[default]*
  * 2 or `'dashed'` 
  * 3 or `'dotted'` 
  * 4 or `'dotdash'` 
  * 5 or `'longdash'` 
  * 6 or `'twodash'` - short, long, short, long...
  * a string of length 2, 4, 6 or 8 characters can be used for full customisation
    * `'42'` means line of length 4, and then a gap of length 2
    * `'1234'` short line of 1 (basically a dot), gap 2, line 3, gap 4
    * `'FA12CD88'` line 15, gap 10, dot, gap 2, line 12, gap 13, line 8, gap 8
* `lwd=...` - line width, relative to the default (2 = 200%, .3 = 30%)
* `pch=...` - point shape - either a number 0-25 (see image above) or any character
* `xaxp=c(x1, x2, n)`, `yaxp=c(y1, y2, n)` - sets axis range and number of tick-marks (overrides `lab` for that axis)
  * `x1`, `y1` - lowest value with a tick-mark (beginning of the axis)
  * `x2`, `y2` - highest value with a tick-mark (ending of the axis)
  * `n` - number of fragments, to which axis is divided (same as with `lab` argument)
  * does not work for non-numeric axes (like names or dates)
