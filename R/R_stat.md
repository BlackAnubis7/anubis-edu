# Statistics in R
## Example data-set
Training operations on data in R is best done on a medium-sized data-set. R has a lot of built-in data sets for
experimenting (for a complete list use `library(help = "datasets")`). We will use `mtcars`:
```
                     mpg cyl  disp  hp drat    wt  qsec vs am gear carb
Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

## Operations on data
### MMMM - Minimum, Maximum, Mean and Median
```r
min(mtcars$hp) == 52
max(mtcars$hp) == 335

which.min(mtcars$hp) == 19  # 31st record has the minimum value
which.max(mtcars$hp) == 31  # 19th record has the maximum value
# which.min and which.max can be used on any vector

rownames(mtcars)[which.min(mtcars$hp)] == "Honda Civic"  # row with minimum HP is called Honda Civic
mtcars$cyl[which.max(mtcars$hp)] == 8  # car with maximum HP has 8 cyllinders

mean(mtcars$hp) == 146.6875  # mean = average
median(mtcars$hp) == 123  # median
```

### Quantile
Quantiles divide data into two parts. For example, quantile 3/4 (called also *"3rd quartile"*) divides data into parts
containing first 75% and last 25% elements. Quantile 1/2 is synonymous to *"median"*
```r
quantile(mtcars$hp) == c(52, 96.5, 123, 180, 335)  # min, quartiles 1-3 and max
names(quantile(mtcars$hp)) == c('0%', '25%', '50%', '75%', '100%')

quantile(mtcars$hp, .75) == 180  # quantile 75%
quantile(mtcars$hp, 5/9) == 150  # quantile 7/9
names(quantile(mtcars$hp, 5/9)) == "55.55556%"
```

## Distributions
### R names
|   Distribution | R name     |
|---------------:|------------|
|           beta | `beta`     |
|       binomial | `binom`    |
|         Cauchy | `cauchy`   |
|    chi-squared | `chisq`    |
|    exponential | `exp`      |
|              F | `f`        |
|          gamma | `gamma`    |
|      geometric | `geom`     |
| hypergeometric | `hyper`    |
|     log-normal | `lnorm`    |
|       logistic | `logis`    |
|       negative | `binomial` |
|         normal | `norm`     |
|        Poisson | `pois`     |
|    signed rank | `signrank` |
|    Student’s t | `t`        |
|        uniform | `unif`     |
|        Weibull | `weibull`  |
|       Wilcoxon | `wilcox`   |

### Functions
Each distribution has a couple of functions. Each has its own list of arguments (you can use `?function` to obtain help)
Examples will be given using `norm` distribution
* `d...` - returns the **density function** values for given numbers 
  * `dnorm(0.3) == 0.3813878`
  * `dnorm(c(.3, .4, Inf)) == c(0.3813878, 0.3682701, 0)`
* `p...` - returns the **distribution function** (or **CDF**) values for given numbers
  * `pnorm(0.3) == 0.6179114`
  * `pnorm(c(.3, .4)) == c(0.6179114, 0.6554217)`
  * `p...` will always return 0 for `-Inf`, and 1 for `Inf`
* `q...` - returns the **quantile function** values for given numbers
  * `qnorm(c(.2, .7)) == c(-0.8416212, 0.5244005)`
  * `q...` will always return `NaN` for numbers outside the `[0, 1]` range
* `r...` - returns a given amount of quasi-random numbers
  * *("quasi-random" means that the generated numbers seem to be random, but they are kept within chosen distribution)*
  * `rnorm(100)` will generate 100 quasi-random numbers within standard normal distribution
  * You can use `set.seed(<number>)` to have randomisation reproducibility

