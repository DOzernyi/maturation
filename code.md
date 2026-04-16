# Table of Contents {#table-of-contents .TOC-Heading}

This document contains the replication of the main and some
supplementary analyses included in the paper "Contra Periodicity in
Language Acquisition...". Please refer for TOC for further details. This
document should be used in conjunction with the two data files provided
together with the submission. Those data files contain the data we
collected. Mostly, we suggest that readers look at p-values and --
importantly -- at adjusted R-squared. Adjusted R-squared us a statistic
which accounts for how much of variation in Growth (marked with the
symbol Δ in our datasets) can be accounted for by Age. Throughout our
analyses, however, the Adjusted R-squared does not exceed 1% and
p-values follow this trend, ie are insignificant.

## Needed Packages

We make use of the following packages:

    library(tidyverse)
    library(skimr)
    library(moderndive)
    library(readr)
    library(knitr)
    library(broom)
    library(bayestestR)
    library(data.table)
    library(lsr)

## Reading the data files

    # import dataset
    Expt1 <- read.csv("Expt1_3m.csv")
    Expt2 <- read.csv("Expt2_6m.csv")
    coeff <- read.csv("coefficients.csv")

# Preliminary information

It is generally a healthy thing to give an overview of the datasets we
are operating on, which we do now:

    skim(Expt1)

Data summary

  -------------------------------------------------- -----------------------------------
  Name                                               Expt1

  Number of rows                                     289

  Number of columns                                  18

  \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_     

  Column type frequency:                             

  numeric                                            18

  \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   

  Group variables                                    None
  -------------------------------------------------- -----------------------------------

  : Data summary

**Variable type: numeric**

  --------------------------------------------------------------------------------------------------------------
  skim_variable   n_missing   complete_rate   mean     sd      p0      p25    p50      p75      p100     hist
  --------------- ----------- --------------- -------- ------- ------- ------ -------- -------- -------- -------
  Student         0           1               145.00   83.57   1.00    73.0   145.00   217.00   289.00   ▇▇▇▇▇

  Vb1             0           1               2.87     0.64    1.00    2.5    3.00     3.00     6.00     ▂▇▂▁▁

  Gr1             0           1               2.71     0.68    1.00    2.0    2.50     3.00     5.00     ▁▇▆▂▁

  FCD1            0           1               4.07     0.87    1.00    3.5    4.00     5.00     6.00     ▁▂▇▇▁

  IC1             0           1               3.82     0.80    1.00    3.5    4.00     4.00     6.00     ▁▂▇▃▁

  Pn1             0           1               3.70     0.69    2.00    3.0    3.50     4.00     6.00     ▁▇▆▃▁

  Vb2             0           1               3.05     0.55    1.50    3.0    3.00     3.33     5.00     ▁▂▇▁▁

  Gr2             0           1               2.87     0.68    1.50    2.5    3.00     3.33     5.00     ▅▆▇▃▁

  FCD2            0           1               4.28     0.78    1.00    4.0    4.50     5.00     6.00     ▁▁▅▇▁

  IC2             0           1               4.12     0.81    1.33    4.0    4.00     4.50     6.00     ▁▂▇▆▁

  Pn2             0           1               3.92     0.70    2.00    3.5    4.00     4.50     6.00     ▁▇▇▇▁

  Age             0           1               16.99    2.55    13.00   15.0   17.00    19.00    22.00    ▆▇▇▅▃

  ΔVb             0           1               0.18     0.43    -2.00   0.0    0.00     0.50     1.33     ▁▁▇▃▂

  ΔGr             0           1               0.16     0.52    -1.50   0.0    0.00     0.50     2.00     ▁▁▇▁▁

  ΔPn             0           1               0.22     0.43    -1.00   0.0    0.00     0.50     2.00     ▁▇▅▂▁

  ΔFCD            0           1               0.21     0.53    -2.00   0.0    0.00     0.50     2.00     ▁▁▇▅▁

  ΔIC             0           1               0.29     0.60    -2.00   0.0    0.33     0.50     2.00     ▁▂▇▇▁

  Δoverall        0           1               0.21     0.30    -0.80   0.0    0.20     0.40     1.10     ▁▂▇▃▁
  --------------------------------------------------------------------------------------------------------------

    skim(Expt2)

Data summary

  -------------------------------------------------- -----------------------------------
  Name                                               Expt2

  Number of rows                                     147

  Number of columns                                  18

  \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_     

  Column type frequency:                             

  numeric                                            18

  \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   

  Group variables                                    None
  -------------------------------------------------- -----------------------------------

  : Data summary

**Variable type: numeric**

  ----------------------------------------------------------------------------------------------------------
  skim_variable   n_missing   complete_rate   mean    sd      p0     p25    p50     p75     p100     hist
  --------------- ----------- --------------- ------- ------- ------ ------ ------- ------- -------- -------
  Student         0           1               74.00   42.58   1.0    37.5   74.00   110.5   147.00   ▇▇▇▇▇

  Vb1             0           1               2.86    0.63    1.0    2.5    3.00    3.0     5.00     ▁▃▇▂▁

  Gr1             0           1               2.64    0.63    1.0    2.0    2.50    3.0     4.00     ▂▅▇▇▃

  FCD1            0           1               4.08    0.91    1.0    3.5    4.50    5.0     5.00     ▁▁▂▅▇

  IC1             0           1               3.71    0.80    1.0    3.5    4.00    4.0     5.50     ▁▂▅▇▂

  Pn1             0           1               3.63    0.67    2.0    3.0    3.67    4.0     5.50     ▂▃▇▂▁

  Vb2             0           1               3.08    0.57    1.5    3.0    3.00    3.5     5.50     ▁▇▂▁▁

  Gr2             0           1               2.92    0.62    1.5    2.5    3.00    3.5     4.50     ▅▇▇▆▃

  FCD2            0           1               4.54    0.74    1.0    4.0    5.00    5.0     6.00     ▁▁▃▇▁

  IC2             0           1               4.19    0.64    2.0    4.0    4.00    4.5     6.00     ▁▃▇▇▁

  Pn2             0           1               4.15    0.68    3.0    3.5    4.00    4.5     5.50     ▇▇▇▅▁

  Age             0           1               16.69   2.58    13.0   14.0   16.00   18.0    22.00    ▇▇▇▅▃

  ΔVb             0           1               0.22    0.42    -1.0   0.0    0.00    0.5     1.67     ▂▇▇▂▁

  ΔGr             0           1               0.28    0.53    -1.5   0.0    0.50    0.5     2.17     ▁▁▇▁▁

  ΔPn             0           1               0.52    0.47    -0.5   0.0    0.50    1.0     2.00     ▆▇▆▁▁

  ΔFCD            0           1               0.46    0.60    -1.0   0.0    0.50    1.0     2.50     ▁▇▇▁▁

  ΔIC             0           1               0.48    0.63    -1.5   0.0    0.50    1.0     2.50     ▁▇▇▆▁

  Δoverall        0           1               0.39    0.33    -0.5   0.2    0.40    0.6     1.50     ▁▇▇▂▁
  ----------------------------------------------------------------------------------------------------------

# Plots of age and growth

## Boxplots of growth for Experiments 1 and 2

    ggplot(data = Expt1, mapping = aes(x = factor(Age), y = Δoverall)) +
      geom_boxplot()

![](./14/media/rId23.png){width="5.0526312335958in"
height="4.0421052055993in"}

    ggplot(data = Expt2, mapping = aes(x = factor(Age), y = Δoverall)) +
      geom_boxplot()

![](./14/media/rId26.png){width="5.0526312335958in"
height="4.0421052055993in"}

## Distribution of "age" variable for for Experiments 1 and 2

    Expt1_age <- ggplot(Expt1, aes(Age))
    Expt1_age + geom_bar(position = "stack") +  
        theme(axis.text.x = element_text(angle=90, vjust=1)) + 
        geom_text(stat='count', aes(label=..count..), position = position_stack(vjust = 0.5),size=4)
    ## Warning: The dot-dot notation (`..count..`) was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `after_stat(count)` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

![](./14/media/rId30.png){width="5.0526312335958in"
height="4.0421052055993in"}

    Expt2_age <- ggplot(Expt2, aes(Age))
    Expt2_age + geom_bar(position = "stack") +  
        theme(axis.text.x = element_text(angle=90, vjust=1)) + 
        geom_text(stat='count', aes(label=..count..), position = position_stack(vjust = 0.5),size=4)

![](./14/media/rId33.png){width="5.0526312335958in"
height="4.0421052055993in"}

# Experiment 1 analyses

## Linear regression

    model_Δo_Expt1 <- lm(Age ~ Δoverall, data = Expt1)
    model_Δo_Expt1
    ## 
    ## Call:
    ## lm(formula = Age ~ Δoverall, data = Expt1)
    ## 
    ## Coefficients:
    ## (Intercept)     Δoverall  
    ##     17.0852      -0.4516
    summary(model_Δo_Expt1)
    ## 
    ## Call:
    ## lm(formula = Age ~ Δoverall, data = Expt1)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.2659 -2.0401  0.0051  1.9599  5.0954 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  17.0852     0.1843  92.711   <2e-16 ***
    ## Δoverall     -0.4516     0.5047  -0.895    0.372    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.552 on 287 degrees of freedom
    ## Multiple R-squared:  0.002781,   Adjusted R-squared:  -0.0006933 
    ## F-statistic: 0.8005 on 1 and 287 DF,  p-value: 0.3717
    confint(model_Δo_Expt1)
    ##                 2.5 %     97.5 %
    ## (Intercept) 16.722502 17.4479424
    ## Δoverall    -1.445027  0.5418669

These are the results presented in the paper.

### Coefficient-Level Estimates for a Model Fitted to Estimate Variation in Age

    lm(Age ~ Δoverall, data = Expt1) %>%
      tidy() %>%
      mutate(
        p.value = scales::pvalue(p.value),
        term = c("Intercept", "Growth")
      ) %>%
      kable(
        caption = "Coefficient-Level Estimates for a Model Fitted to Estimate Variation in Age",
        col.names = c("Predictor", "B", "SE", "t", "p"),
        digits = c(0, 2, 3, 2, 3),
        align = c("l", "r", "r", "r", "r")
      )

Coefficient-Level Estimates for a Model Fitted to Estimate Variation in
Age

  --------------------------------------------------------------------------
  Predictor      B              SE             t              p
  -------------- -------------- -------------- -------------- --------------
  Intercept      17.09          0.184          92.71          \<0.001

  Growth         -0.45          0.505          -0.89          0.372
  --------------------------------------------------------------------------

  : Coefficient-Level Estimates for a Model Fitted to Estimate Variation
  in Age

The first thing we consider here is the coefficient: -0.45. What this
means is that with an increase in a unit of age, there's an associated
decrease in improvement. Bear in mind, however, that this change is not
to the score, it's to the growth which we have as a variable. The
problem us that p-value is .372 and so we cannot generalize this
conclusion. For our sample, and for a value as small as predicted, to
have any certainty in the conclusion, we would want to see a p-value of
\<.01 or at least \<.05. The interested readers can extend this test to
other regressions we will run further down in the document.

We also go over the regressions for every subscale:

### Linear regressions for subscales

#### Vocabulary

    model <- lm(Age ~ ΔVb, data = Expt1)
    model
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔVb, data = Expt1)
    ## 
    ## Coefficients:
    ## (Intercept)          ΔVb  
    ##    16.98272      0.03929
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔVb, data = Expt1)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.0351 -1.9958  0.0173  2.0173  5.0271 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 16.98272    0.16234 104.610   <2e-16 ***
    ## ΔVb          0.03929    0.34877   0.113     0.91    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.556 on 287 degrees of freedom
    ## Multiple R-squared:  4.422e-05,  Adjusted R-squared:  -0.00344 
    ## F-statistic: 0.01269 on 1 and 287 DF,  p-value: 0.9104
    confint(model)
    ##                 2.5 %     97.5 %
    ## (Intercept) 16.663184 17.3022552
    ## ΔVb         -0.647174  0.7257552

#### Interactive Communication

    model <- lm(Age ~ ΔIC, data = Expt1)
    model
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔIC, data = Expt1)
    ## 
    ## Coefficients:
    ## (Intercept)          ΔIC  
    ##      17.022       -0.109
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔIC, data = Expt1)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.0762 -2.0217 -0.0217  1.9783  5.0510 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  17.0217     0.1674 101.694   <2e-16 ***
    ## ΔIC          -0.1090     0.2502  -0.436    0.663    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.555 on 287 degrees of freedom
    ## Multiple R-squared:  0.0006607,  Adjusted R-squared:  -0.002821 
    ## F-statistic: 0.1898 on 1 and 287 DF,  p-value: 0.6634
    confint(model)
    ##                 2.5 %     97.5 %
    ## (Intercept) 16.692257 17.3511573
    ## ΔIC         -0.601452  0.3834704

#### FCD

    model <- lm(Age ~ ΔFCD, data = Expt1)
    model
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔFCD, data = Expt1)
    ## 
    ## Coefficients:
    ## (Intercept)         ΔFCD  
    ##     17.1420      -0.7159
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔFCD, data = Expt1)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -4.500 -2.142 -0.142  1.858  5.932 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  17.1420     0.1605 106.833   <2e-16 ***
    ## ΔFCD         -0.7159     0.2832  -2.527    0.012 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.528 on 287 degrees of freedom
    ## Multiple R-squared:  0.02177,    Adjusted R-squared:  0.01837 
    ## F-statistic: 6.388 on 1 and 287 DF,  p-value: 0.01203
    confint(model)
    ##                 2.5 %     97.5 %
    ## (Intercept) 16.826139 17.4577791
    ## ΔFCD        -1.273355 -0.1583889

#### Pronunciation

    model <- lm(Age ~ ΔPn, data = Expt1)
    model
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔPn, data = Expt1)
    ## 
    ## Coefficients:
    ## (Intercept)          ΔPn  
    ##     16.9611       0.1302
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔPn, data = Expt1)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.0696 -1.9828 -0.0262  2.0389  5.1257 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  16.9611     0.1684 100.732   <2e-16 ***
    ## ΔPn           0.1302     0.3467   0.376    0.708    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.555 on 287 degrees of freedom
    ## Multiple R-squared:  0.0004911,  Adjusted R-squared:  -0.002991 
    ## F-statistic: 0.141 on 1 and 287 DF,  p-value: 0.7075
    confint(model)
    ##                  2.5 %     97.5 %
    ## (Intercept) 16.6297141 17.2925390
    ## ΔPn         -0.5521653  0.8125422

#### Grammar

    model <- lm(Age ~ ΔGr, data = Expt1)
    model
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔGr, data = Expt1)
    ## 
    ## Coefficients:
    ## (Intercept)          ΔGr  
    ##     16.9867       0.0186
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔGr, data = Expt1)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -4.005 -1.996  0.004  2.007  5.023 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  16.9867     0.1570 108.219   <2e-16 ***
    ## ΔGr           0.0186     0.2875   0.065    0.948    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.556 on 287 degrees of freedom
    ## Multiple R-squared:  1.459e-05,  Adjusted R-squared:  -0.00347 
    ## F-statistic: 0.004187 on 1 and 287 DF,  p-value: 0.9484
    confint(model)
    ##                  2.5 %     97.5 %
    ## (Intercept) 16.6777496 17.2956533
    ## ΔGr         -0.5472073  0.5844113

The conclusion is, in every case, that p-values is non-significant; and
adjusted R-squared shows that an extremely small percentage of variation
in growth can be explained by age.

## Multilinear regression (degree = 2)

    model = lm(Δoverall ~ Age + I(Age^2), data = Expt1)
    model
    ## 
    ## Call:
    ## lm(formula = Δoverall ~ Age + I(Age^2), data = Expt1)
    ## 
    ## Coefficients:
    ## (Intercept)          Age     I(Age^2)  
    ##   -0.045200     0.036702    -0.001242
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Δoverall ~ Age + I(Age^2), data = Expt1)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.01971 -0.19192 -0.02198  0.17802  0.89633 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)
    ## (Intercept) -0.045200   0.828583  -0.055    0.957
    ## Age          0.036702   0.097463   0.377    0.707
    ## I(Age^2)    -0.001242   0.002818  -0.441    0.660
    ## 
    ## Residual standard error: 0.2985 on 286 degrees of freedom
    ## Multiple R-squared:  0.003459,   Adjusted R-squared:  -0.00351 
    ## F-statistic: 0.4963 on 2 and 286 DF,  p-value: 0.6093
    confint(model)
    ##                    2.5 %      97.5 %
    ## (Intercept) -1.676093832 1.585694253
    ## Age         -0.155133322 0.228536955
    ## I(Age^2)    -0.006788546 0.004303967
    ggplot(Expt1, aes(Age, Δoverall) ) + geom_point() + 
    stat_smooth(method = lm, formula = y ~ poly(x, 2, raw = TRUE))

![](./14/media/rId46.png){width="5.0526312335958in"
height="4.0421052055993in"}

We do not give these for eveyr subscale, but those interested can run
these models as well.

## Multilinear regressions (degree = 3)

    model = lm(Δoverall ~ Age + I(Age^3), data = Expt1)
    model
    ## 
    ## Call:
    ## lm(formula = Δoverall ~ Age + I(Age^3), data = Expt1)
    ## 
    ## Coefficients:
    ## (Intercept)          Age     I(Age^3)  
    ##   7.661e-02    1.524e-02   -2.364e-05
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Δoverall ~ Age + I(Age^3), data = Expt1)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.01955 -0.19230 -0.02279  0.17721  0.89597 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)  7.661e-02  5.616e-01   0.136    0.892
    ## Age          1.524e-02  4.948e-02   0.308    0.758
    ## I(Age^3)    -2.364e-05  5.412e-05  -0.437    0.663
    ## 
    ## Residual standard error: 0.2985 on 286 degrees of freedom
    ## Multiple R-squared:  0.003446,   Adjusted R-squared:  -0.003523 
    ## F-statistic: 0.4945 on 2 and 286 DF,  p-value: 0.6104
    confint(model)
    ##                     2.5 %       97.5 %
    ## (Intercept) -1.0287876513 1.182005e+00
    ## Age         -0.0821554804 1.126338e-01
    ## I(Age^3)    -0.0001301686 8.289578e-05
    ggplot(Expt1, aes(Age, Δoverall) ) + geom_point() + 
    stat_smooth(method = lm, formula = y ~ poly(x, 3, raw = TRUE))

![](./14/media/rId50.png){width="5.0526312335958in"
height="4.0421052055993in"}

    ggplot(Expt1, aes(Age, Δoverall) ) + geom_point() + 
    stat_smooth(method = lm, formula = y ~ poly(x, 4, raw = TRUE))

![](./14/media/rId53.png){width="5.0526312335958in"
height="4.0421052055993in"}

# Experiment 2 analyses

## Linear regression

    model_Δo_Expt2 <- lm(Age ~ Δoverall, data = Expt2)
    model_Δo_Expt2
    ## 
    ## Call:
    ## lm(formula = Age ~ Δoverall, data = Expt2)
    ## 
    ## Coefficients:
    ## (Intercept)     Δoverall  
    ##      17.181       -1.236
    summary(model_Δo_Expt2)
    ## 
    ## Call:
    ## lm(formula = Age ~ Δoverall, data = Expt2)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.1814 -2.1923 -0.0687  1.8131  5.9726 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  17.1814     0.3264  52.640   <2e-16 ***
    ## Δoverall     -1.2364     0.6326  -1.954   0.0526 .  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.552 on 145 degrees of freedom
    ## Multiple R-squared:  0.02567,    Adjusted R-squared:  0.01895 
    ## F-statistic:  3.82 on 1 and 145 DF,  p-value: 0.05257
    confint(model_Δo_Expt2)
    ##                 2.5 %      97.5 %
    ## (Intercept) 16.536336 17.82654029
    ## Δoverall    -2.486741  0.01389256

These are the results presented in the paper.

### Coefficient-Level Estimates for a Model Fitted to Estimate Variation in Age

    lm(Age ~ Δoverall, data = Expt2) %>%
      tidy() %>%
      mutate(
        p.value = scales::pvalue(p.value),
        term = c("Intercept", "Growth")
      ) %>%
      kable(
        caption = "Coefficient-Level Estimates for a Model Fitted to Estimate Variation in Age",
        col.names = c("Predictor", "B", "SE", "t", "p"),
        digits = c(0, 2, 3, 2, 3),
        align = c("l", "r", "r", "r", "r")
      )

Coefficient-Level Estimates for a Model Fitted to Estimate Variation in
Age

  --------------------------------------------------------------------------
  Predictor      B              SE             t              p
  -------------- -------------- -------------- -------------- --------------
  Intercept      17.18          0.326          52.64          \<0.001

  Growth         -1.24          0.633          -1.95          0.053
  --------------------------------------------------------------------------

  : Coefficient-Level Estimates for a Model Fitted to Estimate Variation
  in Age

The results echo what we reported above.

We also go over the regressions for every subscale:

### Linear regressions for subscales

#### Vocabulary

    model <- lm(Age ~ ΔVb, data = Expt2)
    model
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔVb, data = Expt2)
    ## 
    ## Coefficients:
    ## (Intercept)          ΔVb  
    ##     16.7770      -0.3781
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔVb, data = Expt2)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -3.966 -2.494 -0.588  1.601  5.601 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  16.7770     0.2399  69.932   <2e-16 ***
    ## ΔVb          -0.3781     0.5034  -0.751    0.454    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.58 on 145 degrees of freedom
    ## Multiple R-squared:  0.003876,   Adjusted R-squared:  -0.002994 
    ## F-statistic: 0.5642 on 1 and 145 DF,  p-value: 0.4538
    confint(model)
    ##                 2.5 %     97.5 %
    ## (Intercept) 16.302880 17.2512051
    ## ΔVb         -1.373014  0.6168109

#### Interactive Communication

    model <- lm(Age ~ ΔIC, data = Expt2)
    model
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔIC, data = Expt2)
    ## 
    ## Coefficients:
    ## (Intercept)          ΔIC  
    ##     17.1184      -0.8789
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔIC, data = Expt2)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.1184 -2.2395 -0.1184  1.8211  5.4676 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  17.1184     0.2620  65.329   <2e-16 ***
    ## ΔIC          -0.8789     0.3294  -2.668   0.0085 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.524 on 145 degrees of freedom
    ## Multiple R-squared:  0.04679,    Adjusted R-squared:  0.04022 
    ## F-statistic: 7.118 on 1 and 145 DF,  p-value: 0.008501
    confint(model)
    ##                 2.5 %    97.5 %
    ## (Intercept) 16.600485 17.636286
    ## ΔIC         -1.530035 -0.227787

#### FCD

    model <- lm(Age ~ ΔFCD, data = Expt2)
    model
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔFCD, data = Expt2)
    ## 
    ## Coefficients:
    ## (Intercept)         ΔFCD  
    ##     16.9151      -0.4759
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔFCD, data = Expt2)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.9151 -2.2409 -0.4392  1.9178  6.0367 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  16.9151     0.2676  63.208   <2e-16 ***
    ## ΔFCD         -0.4759     0.3516  -1.354    0.178    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.569 on 145 degrees of freedom
    ## Multiple R-squared:  0.01248,    Adjusted R-squared:  0.005669 
    ## F-statistic: 1.832 on 1 and 145 DF,  p-value: 0.178
    confint(model)
    ##                 2.5 %     97.5 %
    ## (Intercept) 16.386193 17.4440274
    ## ΔFCD        -1.170803  0.2189625

#### Pronunciation

    model <- lm(Age ~ ΔPn, data = Expt2)
    model
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔPn, data = Expt2)
    ## 
    ## Coefficients:
    ## (Intercept)          ΔPn  
    ##    16.72763     -0.06501
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔPn, data = Expt2)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.7276 -2.6626 -0.5976  1.3374  5.3482 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 16.72763    0.31686  52.793   <2e-16 ***
    ## ΔPn         -0.06501    0.45135  -0.144    0.886    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.585 on 145 degrees of freedom
    ## Multiple R-squared:  0.000143,   Adjusted R-squared:  -0.006753 
    ## F-statistic: 0.02074 on 1 and 145 DF,  p-value: 0.8857
    confint(model)
    ##                  2.5 %     97.5 %
    ## (Intercept) 16.1013822 17.3538858
    ## ΔPn         -0.9570896  0.8270758

#### Grammar

    model <- lm(Age ~ ΔGr, data = Expt2)
    model
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔGr, data = Expt2)
    ## 
    ## Coefficients:
    ## (Intercept)          ΔGr  
    ##     16.7751      -0.2853
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Age ~ ΔGr, data = Expt2)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.0604 -2.4898 -0.4898  1.5816  5.5102 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  16.7751     0.2422  69.255   <2e-16 ***
    ## ΔGr          -0.2853     0.4061  -0.703    0.483    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.581 on 145 degrees of freedom
    ## Multiple R-squared:  0.003393,   Adjusted R-squared:  -0.00348 
    ## F-statistic: 0.4937 on 1 and 145 DF,  p-value: 0.4834
    confint(model)
    ##                 2.5 %     97.5 %
    ## (Intercept) 16.296335 17.2538144
    ## ΔGr         -1.087895  0.5172492

Again, the results mirror our conclusions above.

## Multilinear regression (degree = 2)

    model = lm(Δoverall ~ Age + I(Age^2), data = Expt2)
    model
    ## 
    ## Call:
    ## lm(formula = Δoverall ~ Age + I(Age^2), data = Expt2)
    ## 
    ## Coefficients:
    ## (Intercept)          Age     I(Age^2)  
    ##   0.7052004   -0.0165058   -0.0001238
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Δoverall ~ Age + I(Age^2), data = Expt2)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.82556 -0.17592 -0.02975  0.15015  1.11118 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)  0.7052004  1.2665725   0.557    0.579
    ## Age         -0.0165058  0.1497739  -0.110    0.912
    ## I(Age^2)    -0.0001238  0.0043475  -0.028    0.977
    ## 
    ## Residual standard error: 0.3318 on 144 degrees of freedom
    ## Multiple R-squared:  0.02567,    Adjusted R-squared:  0.01214 
    ## F-statistic: 1.897 on 2 and 144 DF,  p-value: 0.1537
    confint(model)
    ##                    2.5 %      97.5 %
    ## (Intercept) -1.798275169 3.208676024
    ## Age         -0.312545167 0.279533591
    ## I(Age^2)    -0.008717017 0.008469373
    ggplot(Expt2, aes(Age, Δoverall) ) + geom_point() + 
    stat_smooth(method = lm, formula = y ~ poly(x, 2, raw = TRUE))

![](./14/media/rId66.png){width="5.0526312335958in"
height="4.0421052055993in"}

We do not give these for every subscale, but those interested can run
these models as well.

## Multilinear regressions (degree = 3)

    model = lm(Δoverall ~ Age + I(Age^3), data = Expt2)
    model
    ## 
    ## Call:
    ## lm(formula = Δoverall ~ Age + I(Age^3), data = Expt2)
    ## 
    ## Coefficients:
    ## (Intercept)          Age     I(Age^3)  
    ##   7.744e-01   -2.377e-02    3.344e-06
    summary(model)
    ## 
    ## Call:
    ## lm(formula = Δoverall ~ Age + I(Age^3), data = Expt2)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.82583 -0.17979 -0.02919  0.14916  1.11320 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)  7.744e-01  8.518e-01   0.909    0.365
    ## Age         -2.377e-02  7.549e-02  -0.315    0.753
    ## I(Age^3)     3.344e-06  8.318e-05   0.040    0.968
    ## 
    ## Residual standard error: 0.3318 on 144 degrees of freedom
    ## Multiple R-squared:  0.02568,    Adjusted R-squared:  0.01215 
    ## F-statistic: 1.898 on 2 and 144 DF,  p-value: 0.1536
    confint(model)
    ##                     2.5 %       97.5 %
    ## (Intercept) -0.9092926153 2.4580554103
    ## Age         -0.1729697778 0.1254392873
    ## I(Age^3)    -0.0001610593 0.0001677475
    ggplot(Expt2, aes(Age, Δoverall) ) + geom_point() + 
    stat_smooth(method = lm, formula = y ~ poly(x, 3, raw = TRUE))

![](./14/media/rId70.png){width="5.0526312335958in"
height="4.0421052055993in"}

    ggplot(Expt2, aes(Age, Δoverall) ) + geom_point() + 
    stat_smooth(method = lm, formula = y ~ poly(x, 4, raw = TRUE))

![](./14/media/rId73.png){width="5.0526312335958in"
height="4.0421052055993in"}

# Coefficients (similarity measures) for Experiment 1

These resuls are discussed in the paper, so we do not delve into further
discussion here; however, we give the method which we used to achieve
our results.

## Overlap coefficients for Experiment 1

    coeff3m <- coeff %>% 
    tidyr::pivot_wider(
        names_from  = c("Age3m"),
        values_from = c("Δ3moverall")
      ) %>%
      select(-Student, -Age6m,-Δ6moverall)

    coeff3m <- data.table(coeff3m)[, lapply(.SD, function(x) x[order(is.na(x))])]
    coeff3m[!coeff3m[, Reduce(`&`, lapply(.SD, is.na))]]
    ##         Age_16     Age_19     Age_21     Age_18     Age_17     Age_20
    ##          <num>      <num>      <num>      <num>      <num>      <num>
    ##  1:  0.5000000  1.1000000  0.7000000  0.4000000  1.0000000  0.2000000
    ##  2:  0.0000000  0.4000000  0.8000000  0.9333334  0.2566666  0.3333334
    ##  3:  0.4666670 -0.0666666  0.3000000  0.5333334 -0.6000000  0.5000000
    ##  4:  0.1333332  0.2566668  0.4333332 -0.0666668 -0.8000000  0.2000000
    ##  5: -0.4000000  0.1000000 -0.3000000  0.3000000  0.8000000  0.2000000
    ##  6:  0.0000000 -0.7000000  0.1000000 -0.0666668 -0.0666666  0.6000000
    ##  7:  0.6000000  0.1000000  0.1000000  0.2000000  0.2000000  0.4000000
    ##  8:  0.1000000  0.6000000  0.2000000  0.4000000  0.8000000  0.2000000
    ##  9:  0.1000000  0.2000000  0.0000000 -0.4000000 -0.4000000  0.1000000
    ## 10:  0.2000000  0.4000002  0.0000000  0.2000000  0.3000000  0.0000000
    ## 11:  0.7000000 -0.2000000  0.2000000  0.2000000 -0.4000000 -0.1000000
    ## 12:  0.2000000  0.0000000  0.1000000  0.4000000  0.4000000  0.2566666
    ## 13:  0.5000000  0.5000000  0.2000000 -0.4000000 -0.3000000  0.3999998
    ## 14: -0.1000000 -0.5000000  0.3000000  0.0000000  0.7000000  0.3000000
    ## 15:  0.0000000  0.0000000  0.5000000 -0.3000000  0.5000000  0.1000000
    ## 16:  0.1000000  0.0000000  0.2000000  0.3000000  0.0000000  0.3000000
    ## 17:  0.7333334  0.4000000  0.3000000  0.8000000  0.0000000  0.5000000
    ## 18:  0.7000000  0.3000000  0.1000000  0.3000000  0.3000000  0.1000000
    ## 19:  0.3333334  0.1333332  0.3000000  0.0000000  0.2000000  0.0000000
    ## 20:  0.3000000  0.0000000  0.0000000  0.3999996  0.7000000  0.0000000
    ## 21:  0.6000000  0.5000000  0.2566666  0.4333334  0.3000000         NA
    ## 22:  0.6000000  0.0000000  0.1000000  0.0666666  0.8000000         NA
    ## 23:  0.2000000  0.4000000  0.0000000  0.1666668  0.5000000         NA
    ## 24: -0.1000000 -0.1000000  0.0000000  0.2566668  0.2000000         NA
    ## 25:  0.2000000  0.5000000  0.4000000  0.0666664  0.2000000         NA
    ## 26:  0.1333332  0.2000000 -0.1000000  0.0666664  0.4000000         NA
    ## 27:  0.3000000  0.2000000  0.0000000  0.1000000  0.4000000         NA
    ## 28:         NA  0.1000000         NA -0.0666666  0.3000000         NA
    ## 29:         NA -0.2000000         NA  0.3000000  0.6666666         NA
    ## 30:         NA         NA         NA  0.5000000 -0.2000000         NA
    ## 31:         NA         NA         NA  0.1000000  0.9000000         NA
    ## 32:         NA         NA         NA  0.1000000         NA         NA
    ## 33:         NA         NA         NA  0.1000000         NA         NA
    ## 34:         NA         NA         NA  0.1000000         NA         NA
    ## 35:         NA         NA         NA  0.2566668         NA         NA
    ## 36:         NA         NA         NA  0.3000000         NA         NA
    ## 37:         NA         NA         NA  0.0000000         NA         NA
    ## 38:         NA         NA         NA  0.0000000         NA         NA
    ## 39:         NA         NA         NA  0.2000000         NA         NA
    ## 40:         NA         NA         NA  0.5000000         NA         NA
    ## 41:         NA         NA         NA  0.1000000         NA         NA
    ## 42:         NA         NA         NA         NA         NA         NA
    ## 43:         NA         NA         NA         NA         NA         NA
    ## 44:         NA         NA         NA         NA         NA         NA
    ## 45:         NA         NA         NA         NA         NA         NA
    ## 46:         NA         NA         NA         NA         NA         NA
    ## 47:         NA         NA         NA         NA         NA         NA
    ## 48:         NA         NA         NA         NA         NA         NA
    ## 49:         NA         NA         NA         NA         NA         NA
    ##         Age_16     Age_19     Age_21     Age_18     Age_17     Age_20
    ##         Age_15     Age_22     Age_14 Age_13
    ##          <num>      <num>      <num>  <num>
    ##  1:  0.5333332 -0.0500000  0.3999998    0.5
    ##  2:  0.4000002 -0.3999998 -0.4000000   -0.2
    ##  3:  0.4000000  0.3999998  0.0000000   -0.1
    ##  4: -0.3333336  0.3000000  0.0000000    0.2
    ##  5:  0.2000000  0.1333334 -0.1000000    0.2
    ##  6:  0.0000000  0.1000000  0.4000000    0.2
    ##  7: -0.2000000  0.4000000  0.2000000   -0.1
    ##  8:  0.3000000  0.2000000  0.5000000    0.1
    ##  9:  0.2000000  0.1333332 -0.1000000    0.4
    ## 10: -0.1000000         NA  0.0666668    0.2
    ## 11:  0.1000000         NA  0.2000000    0.4
    ## 12:  0.1000000         NA  0.4000000    0.6
    ## 13:  0.4000000         NA  0.6000000    0.6
    ## 14:  0.0000000         NA  0.2000000    1.0
    ## 15:  0.1000000         NA -0.0666668    0.3
    ## 16:  0.0000000         NA  1.0000000    0.2
    ## 17:  0.4000000         NA  0.8000000    0.2
    ## 18: -0.2000000         NA  0.1000000    0.1
    ## 19:  0.1000000         NA  0.7000000    0.3
    ## 20:  0.4000000         NA -0.1000000    0.2
    ## 21:  0.7000000         NA  0.0000000    0.1
    ## 22:  0.0000000         NA  0.3999996   -0.4
    ## 23:  0.0000000         NA  0.4000000     NA
    ## 24:  0.4000000         NA  0.6000000     NA
    ## 25:  0.5000000         NA -0.2000000     NA
    ## 26:  0.1000000         NA  0.5000000     NA
    ## 27:  0.2000000         NA -0.2000000     NA
    ## 28:  0.3666666         NA -0.1000000     NA
    ## 29:  0.6000000         NA  0.0000000     NA
    ## 30:  0.4000000         NA  0.6000000     NA
    ## 31:  0.5666666         NA  0.1000000     NA
    ## 32: -0.0000002         NA  0.2000000     NA
    ## 33: -0.1000000         NA  0.1000000     NA
    ## 34:  0.2000000         NA  0.5000000     NA
    ## 35:  0.3000000         NA         NA     NA
    ## 36:  0.0000000         NA         NA     NA
    ## 37:  0.3000000         NA         NA     NA
    ## 38:  0.3000000         NA         NA     NA
    ## 39:  0.2000000         NA         NA     NA
    ## 40:  0.1000000         NA         NA     NA
    ## 41:  0.1000000         NA         NA     NA
    ## 42:  0.3333332         NA         NA     NA
    ## 43:  0.8000000         NA         NA     NA
    ## 44: -0.1000000         NA         NA     NA
    ## 45:  0.2000000         NA         NA     NA
    ## 46:  0.2000000         NA         NA     NA
    ## 47:  0.1000000         NA         NA     NA
    ## 48:  0.2000000         NA         NA     NA
    ## 49:  0.1000000         NA         NA     NA
    ##         Age_15     Age_22     Age_14 Age_13
    coeff3m[complete.cases(coeff3m), ]
    ##        Age_16     Age_19     Age_21     Age_18     Age_17    Age_20     Age_15
    ##         <num>      <num>      <num>      <num>      <num>     <num>      <num>
    ## 1:  0.5000000  1.1000000  0.7000000  0.4000000  1.0000000 0.2000000  0.5333332
    ## 2:  0.0000000  0.4000000  0.8000000  0.9333334  0.2566666 0.3333334  0.4000002
    ## 3:  0.4666670 -0.0666666  0.3000000  0.5333334 -0.6000000 0.5000000  0.4000000
    ## 4:  0.1333332  0.2566668  0.4333332 -0.0666668 -0.8000000 0.2000000 -0.3333336
    ## 5: -0.4000000  0.1000000 -0.3000000  0.3000000  0.8000000 0.2000000  0.2000000
    ## 6:  0.0000000 -0.7000000  0.1000000 -0.0666668 -0.0666666 0.6000000  0.0000000
    ## 7:  0.6000000  0.1000000  0.1000000  0.2000000  0.2000000 0.4000000 -0.2000000
    ## 8:  0.1000000  0.6000000  0.2000000  0.4000000  0.8000000 0.2000000  0.3000000
    ## 9:  0.1000000  0.2000000  0.0000000 -0.4000000 -0.4000000 0.1000000  0.2000000
    ##        Age_22     Age_14 Age_13
    ##         <num>      <num>  <num>
    ## 1: -0.0500000  0.3999998    0.5
    ## 2: -0.3999998 -0.4000000   -0.2
    ## 3:  0.3999998  0.0000000   -0.1
    ## 4:  0.3000000  0.0000000    0.2
    ## 5:  0.1333334 -0.1000000    0.2
    ## 6:  0.1000000  0.4000000    0.2
    ## 7:  0.4000000  0.2000000   -0.1
    ## 8:  0.2000000  0.5000000    0.1
    ## 9:  0.1333332 -0.1000000    0.4
    coeff3m %>% summarise_each(funs(sum(., na.rm = TRUE))) 
    ## Warning: `summarise_each()` was deprecated in dplyr 0.7.0.
    ## ℹ Please use `across()` instead.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.
    ## Warning: `funs()` was deprecated in dplyr 0.8.0.
    ## ℹ Please use a list of either functions or lambdas:
    ## 
    ## # Simple named list: list(mean = mean, median = median)
    ## 
    ## # Auto named with `tibble::lst()`: tibble::lst(mean, median)
    ## 
    ## # Using lambdas list(~ mean(., trim = .2), ~ median(., na.rm = TRUE))
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.
    ##   Age_16   Age_19 Age_21   Age_18   Age_17 Age_20   Age_15   Age_22   Age_14
    ## 1    7.1 4.623334   5.19 7.779999 8.056667   4.59 9.866666 1.216667 7.699999
    ##   Age_13
    ## 1      5
    age <- c("a13", "a14", "a15", "a16", "a17", "a18", "a19", "a20", "a21")
    a13 = c(overlap(coeff3m$Age_13, coeff3m$Age_13), 
                 overlap(coeff3m$Age_13, coeff3m$Age_14), 
                 overlap(coeff3m$Age_13, coeff3m$Age_15),
                 overlap(coeff3m$Age_13, coeff3m$Age_16),
                 overlap(coeff3m$Age_13, coeff3m$Age_17),
                 overlap(coeff3m$Age_13, coeff3m$Age_18),
                 overlap(coeff3m$Age_13, coeff3m$Age_19),
                 overlap(coeff3m$Age_13, coeff3m$Age_20),
                 overlap(coeff3m$Age_13, coeff3m$Age_21))

    a14 <- c(overlap(coeff3m$Age_14, coeff3m$Age_13), 
                 overlap(coeff3m$Age_14, coeff3m$Age_14), 
                 overlap(coeff3m$Age_14, coeff3m$Age_15),
                 overlap(coeff3m$Age_14, coeff3m$Age_16),
                 overlap(coeff3m$Age_14, coeff3m$Age_17),
                 overlap(coeff3m$Age_14, coeff3m$Age_18),
                 overlap(coeff3m$Age_14, coeff3m$Age_19),
                 overlap(coeff3m$Age_14, coeff3m$Age_20),
                 overlap(coeff3m$Age_14, coeff3m$Age_21))

    a15 <- c(overlap(coeff3m$Age_15, coeff3m$Age_13), 
                 overlap(coeff3m$Age_15, coeff3m$Age_14), 
                 overlap(coeff3m$Age_15, coeff3m$Age_15),
                 overlap(coeff3m$Age_15, coeff3m$Age_16),
                 overlap(coeff3m$Age_15, coeff3m$Age_17),
                 overlap(coeff3m$Age_15, coeff3m$Age_18),
                 overlap(coeff3m$Age_15, coeff3m$Age_19),
                 overlap(coeff3m$Age_15, coeff3m$Age_20),
                 overlap(coeff3m$Age_15, coeff3m$Age_21))

    a16 <- c(overlap(coeff3m$Age_16, coeff3m$Age_13), 
                 overlap(coeff3m$Age_16, coeff3m$Age_14), 
                 overlap(coeff3m$Age_16, coeff3m$Age_15),
                 overlap(coeff3m$Age_16, coeff3m$Age_16),
                 overlap(coeff3m$Age_16, coeff3m$Age_17),
                 overlap(coeff3m$Age_16, coeff3m$Age_18),
                 overlap(coeff3m$Age_16, coeff3m$Age_19),
                 overlap(coeff3m$Age_16, coeff3m$Age_20),
                 overlap(coeff3m$Age_16, coeff3m$Age_21))

    a17 <- c(overlap(coeff3m$Age_17, coeff3m$Age_13), 
                 overlap(coeff3m$Age_17, coeff3m$Age_14), 
                 overlap(coeff3m$Age_17, coeff3m$Age_15),
                 overlap(coeff3m$Age_17, coeff3m$Age_16),
                 overlap(coeff3m$Age_17, coeff3m$Age_17),
                 overlap(coeff3m$Age_17, coeff3m$Age_18),
                 overlap(coeff3m$Age_17, coeff3m$Age_19),
                 overlap(coeff3m$Age_17, coeff3m$Age_20),
                 overlap(coeff3m$Age_17, coeff3m$Age_21))

    a18 <- c(overlap(coeff3m$Age_18, coeff3m$Age_13), 
                 overlap(coeff3m$Age_18, coeff3m$Age_14), 
                 overlap(coeff3m$Age_18, coeff3m$Age_15),
                 overlap(coeff3m$Age_18, coeff3m$Age_16),
                 overlap(coeff3m$Age_18, coeff3m$Age_17),
                 overlap(coeff3m$Age_18, coeff3m$Age_18),
                 overlap(coeff3m$Age_18, coeff3m$Age_19),
                 overlap(coeff3m$Age_18, coeff3m$Age_20),
                 overlap(coeff3m$Age_18, coeff3m$Age_21))

    a19 <- c(overlap(coeff3m$Age_19, coeff3m$Age_13), 
                 overlap(coeff3m$Age_19, coeff3m$Age_14), 
                 overlap(coeff3m$Age_19, coeff3m$Age_15),
                 overlap(coeff3m$Age_19, coeff3m$Age_16),
                 overlap(coeff3m$Age_19, coeff3m$Age_17),
                 overlap(coeff3m$Age_19, coeff3m$Age_18),
                 overlap(coeff3m$Age_19, coeff3m$Age_19),
                 overlap(coeff3m$Age_19, coeff3m$Age_20),
                 overlap(coeff3m$Age_19, coeff3m$Age_21))

    a20 <- c(overlap(coeff3m$Age_20, coeff3m$Age_13), 
                 overlap(coeff3m$Age_20, coeff3m$Age_14), 
                 overlap(coeff3m$Age_20, coeff3m$Age_15),
                 overlap(coeff3m$Age_20, coeff3m$Age_16),
                 overlap(coeff3m$Age_20, coeff3m$Age_17),
                 overlap(coeff3m$Age_20, coeff3m$Age_18),
                 overlap(coeff3m$Age_20, coeff3m$Age_19),
                 overlap(coeff3m$Age_20, coeff3m$Age_20),
                 overlap(coeff3m$Age_20, coeff3m$Age_21))

    a21 <- c(overlap(coeff3m$Age_21, coeff3m$Age_13), 
                 overlap(coeff3m$Age_21, coeff3m$Age_14), 
                 overlap(coeff3m$Age_21, coeff3m$Age_15),
                 overlap(coeff3m$Age_21, coeff3m$Age_16),
                 overlap(coeff3m$Age_21, coeff3m$Age_17),
                 overlap(coeff3m$Age_21, coeff3m$Age_18),
                 overlap(coeff3m$Age_21, coeff3m$Age_19),
                 overlap(coeff3m$Age_21, coeff3m$Age_20),
                 overlap(coeff3m$Age_21, coeff3m$Age_21))

    # Join the variables to create a data frame
    df <- data.frame(age,a13,a14,a15,a16,a17,a18, a19, a20, a21)

    df_tidy <- df %>% 
      pivot_longer(
        cols = c(a13,a14,a15,a16,a17,a18,a19,a20,a21),
        names_to = "age_2", 
        values_to = "value"
        )

## Heatmap

    ggplot(df_tidy, aes(x = age, y = age_2, fill = value)) +
      geom_tile(color = "black") +
      geom_text(aes(label = round(value, digits = 2)), color = "white", size = 4) +
      coord_fixed()

![](./14/media/rId79.png){width="5.0526312335958in"
height="4.0421052055993in"}

## Cohen's Ds for Experiment 1

    coeff3m <- coeff %>% 
    tidyr::pivot_wider(
        names_from  = c("Age3m"),
        values_from = c("Δ3moverall")
      ) %>%
      select(-Age6m,-Δ6moverall)

    coeff3m <- data.table(coeff3m)[, lapply(.SD, function(x) x[order(is.na(x))])]
    coeff3m[!coeff3m[, Reduce(`&`, lapply(.SD, is.na))]]
    ##      Student     Age_16     Age_19     Age_21     Age_18     Age_17    Age_20
    ##        <int>      <num>      <num>      <num>      <num>      <num>     <num>
    ##   1:       1  0.5000000  1.1000000  0.7000000  0.4000000  1.0000000 0.2000000
    ##   2:       2  0.0000000  0.4000000  0.8000000  0.9333334  0.2566666 0.3333334
    ##   3:       3  0.4666670 -0.0666666  0.3000000  0.5333334 -0.6000000 0.5000000
    ##   4:       4  0.1333332  0.2566668  0.4333332 -0.0666668 -0.8000000 0.2000000
    ##   5:       5 -0.4000000  0.1000000 -0.3000000  0.3000000  0.8000000 0.2000000
    ##  ---                                                                         
    ## 285:     285         NA         NA         NA         NA         NA        NA
    ## 286:     286         NA         NA         NA         NA         NA        NA
    ## 287:     287         NA         NA         NA         NA         NA        NA
    ## 288:     288         NA         NA         NA         NA         NA        NA
    ## 289:     289         NA         NA         NA         NA         NA        NA
    ##          Age_15     Age_22     Age_14 Age_13
    ##           <num>      <num>      <num>  <num>
    ##   1:  0.5333332 -0.0500000  0.3999998    0.5
    ##   2:  0.4000002 -0.3999998 -0.4000000   -0.2
    ##   3:  0.4000000  0.3999998  0.0000000   -0.1
    ##   4: -0.3333336  0.3000000  0.0000000    0.2
    ##   5:  0.2000000  0.1333334 -0.1000000    0.2
    ##  ---                                        
    ## 285:         NA         NA         NA     NA
    ## 286:         NA         NA         NA     NA
    ## 287:         NA         NA         NA     NA
    ## 288:         NA         NA         NA     NA
    ## 289:         NA         NA         NA     NA
    coeff3m[complete.cases(coeff3m), ]
    ##    Student     Age_16     Age_19     Age_21     Age_18     Age_17    Age_20
    ##      <int>      <num>      <num>      <num>      <num>      <num>     <num>
    ## 1:       1  0.5000000  1.1000000  0.7000000  0.4000000  1.0000000 0.2000000
    ## 2:       2  0.0000000  0.4000000  0.8000000  0.9333334  0.2566666 0.3333334
    ## 3:       3  0.4666670 -0.0666666  0.3000000  0.5333334 -0.6000000 0.5000000
    ## 4:       4  0.1333332  0.2566668  0.4333332 -0.0666668 -0.8000000 0.2000000
    ## 5:       5 -0.4000000  0.1000000 -0.3000000  0.3000000  0.8000000 0.2000000
    ## 6:       6  0.0000000 -0.7000000  0.1000000 -0.0666668 -0.0666666 0.6000000
    ## 7:       7  0.6000000  0.1000000  0.1000000  0.2000000  0.2000000 0.4000000
    ## 8:       8  0.1000000  0.6000000  0.2000000  0.4000000  0.8000000 0.2000000
    ## 9:       9  0.1000000  0.2000000  0.0000000 -0.4000000 -0.4000000 0.1000000
    ##        Age_15     Age_22     Age_14 Age_13
    ##         <num>      <num>      <num>  <num>
    ## 1:  0.5333332 -0.0500000  0.3999998    0.5
    ## 2:  0.4000002 -0.3999998 -0.4000000   -0.2
    ## 3:  0.4000000  0.3999998  0.0000000   -0.1
    ## 4: -0.3333336  0.3000000  0.0000000    0.2
    ## 5:  0.2000000  0.1333334 -0.1000000    0.2
    ## 6:  0.0000000  0.1000000  0.4000000    0.2
    ## 7: -0.2000000  0.4000000  0.2000000   -0.1
    ## 8:  0.3000000  0.2000000  0.5000000    0.1
    ## 9:  0.2000000  0.1333332 -0.1000000    0.4
    age <- c("a13", "a14", "a15", "a16", "a17", "a18", "a19", "a20", "a21")
    a13 = c(cohensD(coeff3m$Age_13, coeff3m$Age_13), 
                 cohensD(coeff3m$Age_13, coeff3m$Age_14), 
                 cohensD(coeff3m$Age_13, coeff3m$Age_15),
                 cohensD(coeff3m$Age_13, coeff3m$Age_16),
                 cohensD(coeff3m$Age_13, coeff3m$Age_17),
                 cohensD(coeff3m$Age_13, coeff3m$Age_18),
                 cohensD(coeff3m$Age_13, coeff3m$Age_19),
                 cohensD(coeff3m$Age_13, coeff3m$Age_20),
                 cohensD(coeff3m$Age_13, coeff3m$Age_21))

    a14 <- c(cohensD(coeff3m$Age_14, coeff3m$Age_13), 
                 cohensD(coeff3m$Age_14, coeff3m$Age_14), 
                 cohensD(coeff3m$Age_14, coeff3m$Age_15),
                 cohensD(coeff3m$Age_14, coeff3m$Age_16),
                 cohensD(coeff3m$Age_14, coeff3m$Age_17),
                 cohensD(coeff3m$Age_14, coeff3m$Age_18),
                 cohensD(coeff3m$Age_14, coeff3m$Age_19),
                 cohensD(coeff3m$Age_14, coeff3m$Age_20),
                 cohensD(coeff3m$Age_14, coeff3m$Age_21))

    a15 <- c(cohensD(coeff3m$Age_15, coeff3m$Age_13), 
                 cohensD(coeff3m$Age_15, coeff3m$Age_14), 
                 cohensD(coeff3m$Age_15, coeff3m$Age_15),
                 cohensD(coeff3m$Age_15, coeff3m$Age_16),
                 cohensD(coeff3m$Age_15, coeff3m$Age_17),
                 cohensD(coeff3m$Age_15, coeff3m$Age_18),
                 cohensD(coeff3m$Age_15, coeff3m$Age_19),
                 cohensD(coeff3m$Age_15, coeff3m$Age_20),
                 cohensD(coeff3m$Age_15, coeff3m$Age_21))

    a16 <- c(cohensD(coeff3m$Age_16, coeff3m$Age_13), 
                 cohensD(coeff3m$Age_16, coeff3m$Age_14), 
                 cohensD(coeff3m$Age_16, coeff3m$Age_15),
                 cohensD(coeff3m$Age_16, coeff3m$Age_16),
                 cohensD(coeff3m$Age_16, coeff3m$Age_17),
                 cohensD(coeff3m$Age_16, coeff3m$Age_18),
                 cohensD(coeff3m$Age_16, coeff3m$Age_19),
                 cohensD(coeff3m$Age_16, coeff3m$Age_20),
                 cohensD(coeff3m$Age_16, coeff3m$Age_21))

    a17 <- c(cohensD(coeff3m$Age_17, coeff3m$Age_13), 
                 cohensD(coeff3m$Age_17, coeff3m$Age_14), 
                 cohensD(coeff3m$Age_17, coeff3m$Age_15),
                 cohensD(coeff3m$Age_17, coeff3m$Age_16),
                 cohensD(coeff3m$Age_17, coeff3m$Age_17),
                 cohensD(coeff3m$Age_17, coeff3m$Age_18),
                 cohensD(coeff3m$Age_17, coeff3m$Age_19),
                 cohensD(coeff3m$Age_17, coeff3m$Age_20),
                 cohensD(coeff3m$Age_17, coeff3m$Age_21))

    a18 <- c(cohensD(coeff3m$Age_18, coeff3m$Age_13), 
                 cohensD(coeff3m$Age_18, coeff3m$Age_14), 
                 cohensD(coeff3m$Age_18, coeff3m$Age_15),
                 cohensD(coeff3m$Age_18, coeff3m$Age_16),
                 cohensD(coeff3m$Age_18, coeff3m$Age_17),
                 cohensD(coeff3m$Age_18, coeff3m$Age_18),
                 cohensD(coeff3m$Age_18, coeff3m$Age_19),
                 cohensD(coeff3m$Age_18, coeff3m$Age_20),
                 cohensD(coeff3m$Age_18, coeff3m$Age_21))

    a19 <- c(cohensD(coeff3m$Age_19, coeff3m$Age_13), 
                 cohensD(coeff3m$Age_19, coeff3m$Age_14), 
                 cohensD(coeff3m$Age_19, coeff3m$Age_15),
                 cohensD(coeff3m$Age_19, coeff3m$Age_16),
                 cohensD(coeff3m$Age_19, coeff3m$Age_17),
                 cohensD(coeff3m$Age_19, coeff3m$Age_18),
                 cohensD(coeff3m$Age_19, coeff3m$Age_19),
                 cohensD(coeff3m$Age_19, coeff3m$Age_20),
                 cohensD(coeff3m$Age_19, coeff3m$Age_21))

    a20 <- c(cohensD(coeff3m$Age_20, coeff3m$Age_13), 
                 cohensD(coeff3m$Age_20, coeff3m$Age_14), 
                 cohensD(coeff3m$Age_20, coeff3m$Age_15),
                 cohensD(coeff3m$Age_20, coeff3m$Age_16),
                 cohensD(coeff3m$Age_20, coeff3m$Age_17),
                 cohensD(coeff3m$Age_20, coeff3m$Age_18),
                 cohensD(coeff3m$Age_20, coeff3m$Age_19),
                 cohensD(coeff3m$Age_20, coeff3m$Age_20),
                 cohensD(coeff3m$Age_20, coeff3m$Age_21))

    a21 <- c(cohensD(coeff3m$Age_21, coeff3m$Age_13), 
                 cohensD(coeff3m$Age_21, coeff3m$Age_14), 
                 cohensD(coeff3m$Age_21, coeff3m$Age_15),
                 cohensD(coeff3m$Age_21, coeff3m$Age_16),
                 cohensD(coeff3m$Age_21, coeff3m$Age_17),
                 cohensD(coeff3m$Age_21, coeff3m$Age_18),
                 cohensD(coeff3m$Age_21, coeff3m$Age_19),
                 cohensD(coeff3m$Age_21, coeff3m$Age_20),
                 cohensD(coeff3m$Age_21, coeff3m$Age_21))
    df <- data.frame(age,a13,a14,a15,a16,a17,a18, a19, a20, a21)

    df_tidy <- df %>% 
      pivot_longer(
        cols = c(a13,a14,a15,a16,a17,a18,a19,a20,a21),
        names_to = "age_2", 
        values_to = "value"
        )

## Heatmap

    ggplot(df_tidy, aes(x = age, y = age_2, fill = value)) +
      geom_tile(color = "black") +
      geom_text(aes(label = round(value, digits = 2)), color = "white", size = 4) +
      coord_fixed()

![](./14/media/rId84.png){width="5.0526312335958in"
height="4.0421052055993in"}

# Coefficients (similarity measures) for Experiment 2

## Overlap coefficients for Epxeriment 2

    coeff6m <- coeff %>% 
      na.omit() %>%
      tidyr::pivot_wider(
        names_from  = c("Age6m"),
        values_from = c("Δ6moverall")
      ) %>%
      select(-Student, -Age3m,-Δ3moverall)

    coeff6m <- data.table(coeff6m)[, lapply(.SD, function(x) x[order(is.na(x))])]
    coeff6m[complete.cases(coeff6m), ]
    ##    Age_13    Age_14 Age_15 Age_16    Age_17     Age_18    Age_19    Age_20
    ##     <num>     <num>  <num>  <num>     <num>      <num>     <num>     <num>
    ## 1:    0.6 0.8666668    0.9    0.1 1.5000000  0.1999998 0.2000000 0.6666666
    ## 2:    0.2 0.2000000    0.6    0.4 0.7666666  0.3000000 0.4333332 1.0000000
    ## 3:    0.5 0.6000000    0.3    0.9 0.4666666  0.5000000 0.2566666 0.4000000
    ## 4:    0.3 0.4000000   -0.2    0.5 0.2000000  0.9000000 0.3000000 0.0000000
    ## 5:    1.1 0.0000000    1.0    0.9 0.4000000 -0.2000000 0.0000000 0.4000000
    ## 6:    1.0 0.6000000    0.8    0.4 0.5000000  0.0999998 0.2000000 0.3333330
    ##    Age_21    Age_22
    ##     <num>     <num>
    ## 1:    0.3 0.9333334
    ## 2:    0.2 0.6000000
    ## 3:    0.0 0.1000000
    ## 4:    0.7 0.2000000
    ## 5:    0.6 0.3000000
    ## 6:    0.1 0.3000000
    coeff3m %>% group_by(Age_13) %>% summarise_each(funs(sum(., na.rm = TRUE))) 
    ## Warning: `summarise_each()` was deprecated in dplyr 0.7.0.
    ## ℹ Please use `across()` instead.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.
    ## Warning: `funs()` was deprecated in dplyr 0.8.0.
    ## ℹ Please use a list of either functions or lambdas:
    ## 
    ## # Simple named list: list(mean = mean, median = median)
    ## 
    ## # Auto named with `tibble::lst()`: tibble::lst(mean, median)
    ## 
    ## # Using lambdas list(~ mean(., trim = .2), ~ median(., na.rm = TRUE))
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.
    ## # A tibble: 11 × 11
    ##    Age_13 Student Age_16  Age_19 Age_21  Age_18 Age_17 Age_20 Age_15 Age_22
    ##     <dbl>   <int>  <dbl>   <dbl>  <dbl>   <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
    ##  1   -0.4      22  0.6    0       0.1    0.0667  0.8    0      0      0    
    ##  2   -0.2       2  0      0.4     0.8    0.933   0.257  0.333  0.400 -0.400
    ##  3   -0.1      10  1.07   0.0333  0.4    0.733  -0.4    0.9    0.2    0.800
    ##  4    0.1      47  1.4    1.4     0.557  1.13    1.4    0.3    0.8    0.2  
    ##  5    0.2      78  1.07   0.457   0.733  1.87    0.933  1.8    0.567  0.533
    ##  6    0.3      34  0.333  0.133   0.8   -0.3     0.7    0.1    0.2    0    
    ##  7    0.4      20  0.8    0       0.2   -0.2    -0.8    0      0.3    0.133
    ##  8    0.5       1  0.5    1.1     0.7    0.4     1      0.2    0.533 -0.05 
    ##  9    0.6      25  0.7    0.5     0.3    0       0.1    0.657  0.5    0    
    ## 10    1        14 -0.1   -0.5     0.3    0       0.7    0.3    0      0    
    ## 11   NA     41652  0.733  1.1     0.3    3.15    3.37   0      6.37   0    
    ## # ℹ 1 more variable: Age_14 <dbl>
    age <- c("a13", "a14", "a15", "a16", "a17", "a18", "a19", "a20", "a21")
    a13 = c(overlap(coeff6m$Age_13, coeff6m$Age_13), 
                 overlap(coeff6m$Age_13, coeff6m$Age_14), 
                 overlap(coeff6m$Age_13, coeff6m$Age_15),
                 overlap(coeff6m$Age_13, coeff6m$Age_16),
                 overlap(coeff6m$Age_13, coeff6m$Age_17),
                 overlap(coeff6m$Age_13, coeff6m$Age_18),
                 overlap(coeff6m$Age_13, coeff6m$Age_19),
                 overlap(coeff6m$Age_13, coeff6m$Age_20),
                 overlap(coeff6m$Age_13, coeff6m$Age_21))

    a14 <- c(overlap(coeff6m$Age_14, coeff6m$Age_13), 
                 overlap(coeff6m$Age_14, coeff6m$Age_14), 
                 overlap(coeff6m$Age_14, coeff6m$Age_15),
                 overlap(coeff6m$Age_14, coeff6m$Age_16),
                 overlap(coeff6m$Age_14, coeff6m$Age_17),
                 overlap(coeff6m$Age_14, coeff6m$Age_18),
                 overlap(coeff6m$Age_14, coeff6m$Age_19),
                 overlap(coeff6m$Age_14, coeff6m$Age_20),
                 overlap(coeff6m$Age_14, coeff6m$Age_21))

    a15 <- c(overlap(coeff6m$Age_15, coeff6m$Age_13), 
                 overlap(coeff6m$Age_15, coeff6m$Age_14), 
                 overlap(coeff6m$Age_15, coeff6m$Age_15),
                 overlap(coeff6m$Age_15, coeff6m$Age_16),
                 overlap(coeff6m$Age_15, coeff6m$Age_17),
                 overlap(coeff6m$Age_15, coeff6m$Age_18),
                 overlap(coeff6m$Age_15, coeff6m$Age_19),
                 overlap(coeff6m$Age_15, coeff6m$Age_20),
                 overlap(coeff6m$Age_15, coeff6m$Age_21))

    a16 <- c(overlap(coeff6m$Age_16, coeff6m$Age_13), 
                 overlap(coeff6m$Age_16, coeff6m$Age_14), 
                 overlap(coeff6m$Age_16, coeff6m$Age_15),
                 overlap(coeff6m$Age_16, coeff6m$Age_16),
                 overlap(coeff6m$Age_16, coeff6m$Age_17),
                 overlap(coeff6m$Age_16, coeff6m$Age_18),
                 overlap(coeff6m$Age_16, coeff6m$Age_19),
                 overlap(coeff6m$Age_16, coeff6m$Age_20),
                 overlap(coeff6m$Age_16, coeff6m$Age_21))

    a17 <- c(overlap(coeff6m$Age_17, coeff6m$Age_13), 
                 overlap(coeff6m$Age_17, coeff6m$Age_14), 
                 overlap(coeff6m$Age_17, coeff6m$Age_15),
                 overlap(coeff6m$Age_17, coeff6m$Age_16),
                 overlap(coeff6m$Age_17, coeff6m$Age_17),
                 overlap(coeff6m$Age_17, coeff6m$Age_18),
                 overlap(coeff6m$Age_17, coeff6m$Age_19),
                 overlap(coeff6m$Age_17, coeff6m$Age_20),
                 overlap(coeff6m$Age_17, coeff6m$Age_21))

    a18 <- c(overlap(coeff6m$Age_18, coeff6m$Age_13), 
                 overlap(coeff6m$Age_18, coeff6m$Age_14), 
                 overlap(coeff6m$Age_18, coeff6m$Age_15),
                 overlap(coeff6m$Age_18, coeff6m$Age_16),
                 overlap(coeff6m$Age_18, coeff6m$Age_17),
                 overlap(coeff6m$Age_18, coeff6m$Age_18),
                 overlap(coeff6m$Age_18, coeff6m$Age_19),
                 overlap(coeff6m$Age_18, coeff6m$Age_20),
                 overlap(coeff6m$Age_18, coeff6m$Age_21))

    a19 <- c(overlap(coeff6m$Age_19, coeff6m$Age_13), 
                 overlap(coeff6m$Age_19, coeff6m$Age_14), 
                 overlap(coeff6m$Age_19, coeff6m$Age_15),
                 overlap(coeff6m$Age_19, coeff6m$Age_16),
                 overlap(coeff6m$Age_19, coeff6m$Age_17),
                 overlap(coeff6m$Age_19, coeff6m$Age_18),
                 overlap(coeff6m$Age_19, coeff6m$Age_19),
                 overlap(coeff6m$Age_19, coeff6m$Age_20),
                 overlap(coeff6m$Age_19, coeff6m$Age_21))

    a20 <- c(overlap(coeff6m$Age_20, coeff6m$Age_13), 
                 overlap(coeff6m$Age_20, coeff6m$Age_14), 
                 overlap(coeff6m$Age_20, coeff6m$Age_15),
                 overlap(coeff6m$Age_20, coeff6m$Age_16),
                 overlap(coeff6m$Age_20, coeff6m$Age_17),
                 overlap(coeff6m$Age_20, coeff6m$Age_18),
                 overlap(coeff6m$Age_20, coeff6m$Age_19),
                 overlap(coeff6m$Age_20, coeff6m$Age_20),
                 overlap(coeff6m$Age_20, coeff6m$Age_21))

    a21 <- c(overlap(coeff6m$Age_21, coeff6m$Age_13), 
                 overlap(coeff6m$Age_21, coeff6m$Age_14), 
                 overlap(coeff6m$Age_21, coeff6m$Age_15),
                 overlap(coeff6m$Age_21, coeff6m$Age_16),
                 overlap(coeff6m$Age_21, coeff6m$Age_17),
                 overlap(coeff6m$Age_21, coeff6m$Age_18),
                 overlap(coeff6m$Age_21, coeff6m$Age_19),
                 overlap(coeff6m$Age_21, coeff6m$Age_20),
                 overlap(coeff6m$Age_21, coeff6m$Age_21))

    # Join the variables to create a data frame
    df <- data.frame(age,a13,a14,a15,a16,a17,a18, a19, a20, a21)

    df_tidy <- df %>% 
      pivot_longer(
        cols = c(a13,a14,a15,a16,a17,a18,a19,a20,a21),
        names_to = "age_2", 
        values_to = "value"
        )

## Heatmap

    ggplot(df_tidy, aes(x = age, y = age_2, fill = value)) +
      geom_tile(color = "black") +
      geom_text(aes(label = round(value, digits = 2)), color = "white", size = 4) +
      coord_fixed()

![](./14/media/rId90.png){width="5.0526312335958in"
height="4.0421052055993in"}

## Cohens Ds for Experiment 2

    age <- c("a13", "a14", "a15", "a16", "a17", "a18", "a19", "a20", "a21")
    a13 = c(cohensD(coeff6m$Age_13, coeff6m$Age_13), 
                 cohensD(coeff6m$Age_13, coeff6m$Age_14), 
                 cohensD(coeff6m$Age_13, coeff6m$Age_15),
                 cohensD(coeff6m$Age_13, coeff6m$Age_16),
                 cohensD(coeff6m$Age_13, coeff6m$Age_17),
                 cohensD(coeff6m$Age_13, coeff6m$Age_18),
                 cohensD(coeff6m$Age_13, coeff6m$Age_19),
                 cohensD(coeff6m$Age_13, coeff6m$Age_20),
                 cohensD(coeff6m$Age_13, coeff6m$Age_21))

    a14 <- c(cohensD(coeff6m$Age_14, coeff6m$Age_13), 
                 cohensD(coeff6m$Age_14, coeff6m$Age_14), 
                 cohensD(coeff6m$Age_14, coeff6m$Age_15),
                 cohensD(coeff6m$Age_14, coeff6m$Age_16),
                 cohensD(coeff6m$Age_14, coeff6m$Age_17),
                 cohensD(coeff6m$Age_14, coeff6m$Age_18),
                 cohensD(coeff6m$Age_14, coeff6m$Age_19),
                 cohensD(coeff6m$Age_14, coeff6m$Age_20),
                 cohensD(coeff6m$Age_14, coeff6m$Age_21))

    a15 <- c(cohensD(coeff6m$Age_15, coeff6m$Age_13), 
                 cohensD(coeff6m$Age_15, coeff6m$Age_14), 
                 cohensD(coeff6m$Age_15, coeff6m$Age_15),
                 cohensD(coeff6m$Age_15, coeff6m$Age_16),
                 cohensD(coeff6m$Age_15, coeff6m$Age_17),
                 cohensD(coeff6m$Age_15, coeff6m$Age_18),
                 cohensD(coeff6m$Age_15, coeff6m$Age_19),
                 cohensD(coeff6m$Age_15, coeff6m$Age_20),
                 cohensD(coeff6m$Age_15, coeff6m$Age_21))

    a16 <- c(cohensD(coeff6m$Age_16, coeff6m$Age_13), 
                 cohensD(coeff6m$Age_16, coeff6m$Age_14), 
                 cohensD(coeff6m$Age_16, coeff6m$Age_15),
                 cohensD(coeff6m$Age_16, coeff6m$Age_16),
                 cohensD(coeff6m$Age_16, coeff6m$Age_17),
                 cohensD(coeff6m$Age_16, coeff6m$Age_18),
                 cohensD(coeff6m$Age_16, coeff6m$Age_19),
                 cohensD(coeff6m$Age_16, coeff6m$Age_20),
                 cohensD(coeff6m$Age_16, coeff6m$Age_21))

    a17 <- c(cohensD(coeff6m$Age_17, coeff6m$Age_13), 
                 cohensD(coeff6m$Age_17, coeff6m$Age_14), 
                 cohensD(coeff6m$Age_17, coeff6m$Age_15),
                 cohensD(coeff6m$Age_17, coeff6m$Age_16),
                 cohensD(coeff6m$Age_17, coeff6m$Age_17),
                 cohensD(coeff6m$Age_17, coeff6m$Age_18),
                 cohensD(coeff6m$Age_17, coeff6m$Age_19),
                 cohensD(coeff6m$Age_17, coeff6m$Age_20),
                 cohensD(coeff6m$Age_17, coeff6m$Age_21))

    a18 <- c(cohensD(coeff6m$Age_18, coeff6m$Age_13), 
                 cohensD(coeff6m$Age_18, coeff6m$Age_14), 
                 cohensD(coeff6m$Age_18, coeff6m$Age_15),
                 cohensD(coeff6m$Age_18, coeff6m$Age_16),
                 cohensD(coeff6m$Age_18, coeff6m$Age_17),
                 cohensD(coeff6m$Age_18, coeff6m$Age_18),
                 cohensD(coeff6m$Age_18, coeff6m$Age_19),
                 cohensD(coeff6m$Age_18, coeff6m$Age_20),
                 cohensD(coeff6m$Age_18, coeff6m$Age_21))

    a19 <- c(cohensD(coeff6m$Age_19, coeff6m$Age_13), 
                 cohensD(coeff6m$Age_19, coeff6m$Age_14), 
                 cohensD(coeff6m$Age_19, coeff6m$Age_15),
                 cohensD(coeff6m$Age_19, coeff6m$Age_16),
                 cohensD(coeff6m$Age_19, coeff6m$Age_17),
                 cohensD(coeff6m$Age_19, coeff6m$Age_18),
                 cohensD(coeff6m$Age_19, coeff6m$Age_19),
                 cohensD(coeff6m$Age_19, coeff6m$Age_20),
                 cohensD(coeff6m$Age_19, coeff6m$Age_21))

    a20 <- c(cohensD(coeff6m$Age_20, coeff6m$Age_13), 
                 cohensD(coeff6m$Age_20, coeff6m$Age_14), 
                 cohensD(coeff6m$Age_20, coeff6m$Age_15),
                 cohensD(coeff6m$Age_20, coeff6m$Age_16),
                 cohensD(coeff6m$Age_20, coeff6m$Age_17),
                 cohensD(coeff6m$Age_20, coeff6m$Age_18),
                 cohensD(coeff6m$Age_20, coeff6m$Age_19),
                 cohensD(coeff6m$Age_20, coeff6m$Age_20),
                 cohensD(coeff6m$Age_20, coeff6m$Age_21))

    a21 <- c(cohensD(coeff6m$Age_21, coeff6m$Age_13), 
                 cohensD(coeff6m$Age_21, coeff6m$Age_14), 
                 cohensD(coeff6m$Age_21, coeff6m$Age_15),
                 cohensD(coeff6m$Age_21, coeff6m$Age_16),
                 cohensD(coeff6m$Age_21, coeff6m$Age_17),
                 cohensD(coeff6m$Age_21, coeff6m$Age_18),
                 cohensD(coeff6m$Age_21, coeff6m$Age_19),
                 cohensD(coeff6m$Age_21, coeff6m$Age_20),
                 cohensD(coeff6m$Age_21, coeff6m$Age_21))
    df <- data.frame(age,a13,a14,a15,a16,a17,a18, a19, a20, a21)

    df_tidy <- df %>% 
      pivot_longer(
        cols = c(a13,a14,a15,a16,a17,a18,a19,a20,a21),
        names_to = "age_2", 
        values_to = "value"
        )

## Heatmap

    ggplot(df_tidy, aes(x = age, y = age_2, fill = value)) +
      geom_tile(color = "black") +
      geom_text(aes(label = round(value, digits = 2)), color = "white", size = 4) +
      coord_fixed()

![](./14/media/rId95.png){width="5.0526312335958in"
height="4.0421052055993in"}
