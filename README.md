# lordif

The `lordif` package performs an ordinal (common odds-ratio) logistic regression differential item functioning (DIF) analysis using IRT theta estimates as the conditioning variable. The graded response model (GRM) or the generalized partial credit model (GPCM) is used for IRT parameter estimation. Items flagged for DIF are treated as unique to each group and group-specific item parameters are obtained. Non-DIF items serve as anchor items to the initial calibration. The procedure then runs iteratively until the same set of items is flagged over two consecutive iterations, unless anchor items are specified _a priori_.

## Quick start

### Preparing data

Load the PROMIS Anxiety data (containing 766 observations on 29 five-point rating scale items measuring anxiety; try `?Anxiety` for more information):

```r
data("Anxiety")
```

The dataset also includes three demographic variables (`age`, `gender`, and `education`). Create a data frame containing only the item responses:

```r
resp.data <- Anxiety[paste0("R", 1:29)]
```

Create a DIF grouping variable:

```r
gender <- Anxiety$gender
```

This numeric variable and coded as `0 = Male` and `1 = Female`. The lowest level serves as the reference group (`0 = Male`).

### Running lordif

Running `lordif` accepting all default settings:

```r
gender_DIF <- lordif(resp.data, gender)
```

By default, the graded response model is fitted to the response data and the likelihood-ratio $\chi^2$ statistic is used to flag DIF items with $\alpha = 0.01$.

### Plotting DIF

Graphically displaying the DIF effects:

```r
plot(gender_DIF, labels = c("Male", "Female"))
```

### Flagging using empirical thresholds

The flagging criterion can be determined empirically through Monte Carlo simulations:

```r
lordif(resp.data, gender, MonteCarlo = TRUE)
```

### Running Monte Carlo simulations to generate empirical thresholds

The `MonteCarlo = TRUE` option above calls the `montecarlo()` function internally to generate empirical thresholds. We can generate

```r
gender_DIF_MC <- montecarlo(gender_DIF, nr = 100)
```



