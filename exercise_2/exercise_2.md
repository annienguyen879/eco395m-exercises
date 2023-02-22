# Homework 2
## ECO 395M: Data Mining and Statistical Learning 

## Soo Jee Choi, Annie Nguyen, and Tarini Sudhakar

### 2023-02-22


## 1) Saratoga house prices

# Linear Model
    ## Number of coefficients
    ## [1] 22

    ## Predictions in sample
    ## [1] 57602.82

    ## Predictions out of sample
    ## [1] 55614.14

In a single linear model train/test split, we get an in sample RMSE of
57602.82 and an out of sample RMSE of 55614.14.

    ## Mean of 25 out of sample linear model RMSE
    ## 64566.64

The average out of sample RMSE of 25 random linear model train/test
splits is 64566.64.

# KNN Model
    ## Predictions in sample
    ## [1] 60014.98
    
    ## Predictions out of sample
    ## [1] 119245.9

In a single KNN regression model train/test split, we get an in sample
RMSE of about 60,000 and an out of sample RMSE of about 119,000.

    ## Mean of 25 out of sample KNN model RMSE 
    ## 121276.6

The average out of sample RMSE of 25 random KNN regression train/test
splits is 121276.6.

Linear Model vs KNN Regression Model Report: Linear Model vs KNN
Regression Model: 

We estimate housing prices using a linear regression
model and a KNN regression model. We find that in a single linear model
train/test split, we get an in sample RMSE of 57602.82 and an out of
sample RMSE of 55614.14. We standardize our variables before applying KNN and find using a single KNN regression model train/test
split, results in an in sample RMSE of 60014.98 and an out of sample RMSE
of 119245.9. We can see that in a single train/test split, the in
sample RMSE of both models are similar (approximately 58 thousand and 60
thousand for the Linear and KNN models, respectively.) However, the out
of sample RMSE of the models were notably different at approximately 56
thousand and 119 thousand. So while both models performed similarly
using in sample data, the linear model performed much better using the
out of sample/testing data.

When measuring out-of-sample performance, there is random variation due
to the particular choice of data points that end up in the train/test
split sample. To address this issue, we average 25 different/random
train/test split estimates of out-of-sample RMSE. By doing this, we see
that the average out of sample RMSE of 25 random linear model train/test
splits is 64566.64, and the average out of sample RMSE of 25 random KNN
regression train/test splits is 121276.6. These results confirm our
findings from the single train/test splits. So we conclude that the
linear regression model is better at achieving lower out-of-sample
mean-squared error than the KNN model.

However, we note this analysis relied on the best models I could produce
using the data. There may exist a KNN model that outperforms the linear
model I produced. However, looking at the out of sample RMSE using
single and average RMSE, that seems unlikely to be the case.

## 2) Classification and retrospective sampling

In this part of the assignment, we want to be able to predict whether a
person will default on their loan, based on factors such as credit
history. What is unique about this problem is its dataset. Since the
data available on loan defaults is small, the German bank decided to
oversample its defaults by matching each default to a similar set of
loans in the overall bank portfolio. Is this dataset appropriate for the
model that we wish to build? Can it predict which of the bank’s clients
will default on a loan?

What did we do to assess the data at hand? First, we wanted to see the
default probability by credit history. Our results show that individuals
with a good credit history have the highest probability of defaulting on
a loan. Whereas, people with terrible credit history have the lowest.
This seems counter intuitive since credit history indicates how well a
borrower has repayed their debts. A good credit history should have a
lower default probability.

![](exercise_2_files/figure-markdown_strict/pressure-1.png)

Nevertheless, if we build a logistic regression model around this
dataset, regressing Default on duration, amount, installment, age,
credit history, purpose, and foreign, we get the following results.

    ## 
    ## Call:
    ## glm(formula = Default ~ duration + amount + installment + age + 
    ##     history + purpose + foreign, family = "binomial", data = credit)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.3464  -0.8050  -0.5751   1.0250   2.4767  
    ## 
    ## Coefficients:
    ##                       Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)         -7.075e-01  4.726e-01  -1.497  0.13435    
    ## duration             2.526e-02  8.100e-03   3.118  0.00182 ** 
    ## amount               9.596e-05  3.650e-05   2.629  0.00856 ** 
    ## installment          2.216e-01  7.626e-02   2.906  0.00366 ** 
    ## age                 -2.018e-02  7.224e-03  -2.794  0.00521 ** 
    ## historypoor         -1.108e+00  2.473e-01  -4.479 7.51e-06 ***
    ## historyterrible     -1.885e+00  2.822e-01  -6.679 2.41e-11 ***
    ## purposeedu           7.248e-01  3.707e-01   1.955  0.05058 .  
    ## purposegoods/repair  1.049e-01  2.573e-01   0.408  0.68346    
    ## purposenewcar        8.545e-01  2.773e-01   3.081  0.00206 ** 
    ## purposeusedcar      -7.959e-01  3.598e-01  -2.212  0.02694 *  
    ## foreigngerman       -1.265e+00  5.773e-01  -2.191  0.02849 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 1221.7  on 999  degrees of freedom
    ## Residual deviance: 1070.0  on 988  degrees of freedom
    ## AIC: 1094
    ## 
    ## Number of Fisher Scoring iterations: 4

    ##         (Intercept)            duration              amount         installment 
    ##               -0.71                0.03                0.00                0.22 
    ##                 age         historypoor     historyterrible          purposeedu 
    ##               -0.02               -1.11               -1.88                0.72 
    ## purposegoods/repair       purposenewcar      purposeusedcar       foreigngerman 
    ##                0.10                0.85               -0.80               -1.26

If someone has a poor credit history, then the chance of them defaulting
on a loan goes down by 0.37. Similarly, if someone has a terrible credit
history, then the odds of them defaulting on a loan goes down by 0.18.
These results still are the opposite of what I expected. Odds of someone
defaulting on a loan should rise if they have a poor or terrible credit
history.

### What is the bigger problem at play?

Since the bank decided to conduct a case-control design, it used
existing data to predict whether certain people will default on a loan,
given other features. Since defaults are rare, the bank matched each
default with similar sets of loans that had not defaulted. This means
that they oversampled defaults relative to a random sample of loans in
the bank’s overall portfolio.

In a simpler way, imagine if we initially had a sample of 100 loans, of
which 10 loans were defaulted. By oversampling defaults, instead of
having a low default probability of 1/10, we now have a much higher
probability. This method results in unreliable estimates since we can no
longer draw reasonable conclusions about how a particular credit history
may affect the default probability of an individual. This is why we are
getting a negative coefficient for poor and terrible credit history
while predicting loan defaults.

### How do we go ahead from here?

This dataset is not appropriate for building a predictive model of
defaults. The case-control sample places a defaulted loan in a bag of
loans of similar value. But we do not know if the bank considered other
features such as credit history, age, and savings. This means that we
cannot draw useful conclusions about someone particular to their credit
history and classify them into “high” versus “low” probability of
default.

Instead of following this methodology, the bank can accord weights to
the default loans in the sample. This will allow for their adequate
representation in the sample that the bank has without diluting the
validity of the estimates.

## 3) Children and hotel reservations

In this section, we will focus on building a predictive model for
whether a booking at a hotel will have children on it. Oftentimes,
parents fail to include their children when making reservations.
Although this piece of information may seem trivial, being able to
anticipate guests’ needs are very important for hotel staff, so it is
essential for management to know how many children are staying to better
cater to families’ needs and to keep supplies/inventory well-stocked.

Therefore, in this portion, we will focus on predicting the `children`
variable.

### Model Building

Using only the data in `hotels_dev.csv`, we will compare the
out-of-sample performance of the following:

1.  baseline 1: a small model that uses only the `market_segment`,
    `adults`, `customer_type`, and `is_repeated_guest` variables as
    features.
2.  baseline 2: a big model that uses all the possible predictors except
    the `arrival_date` variable (main effects only)
3.  the best linear model you can build, including any engineered
    features that you can think of that improve the performance
    (interactions, features derived from time stamps, etc)

#### Baseline Model 1:

    ##            acc ppv  rmse_    auc
    ## Metrics 0.9188 NaN 3.1366 0.6809

After fitting the baseline model 1 to the training set and assessing
out-of-sample accuracy, we see this model predicts with about 91%
accuracy, but the area under the curve (AUC) is quite low. Although we
would not consider “bad” model performance, the low AUC indicates that
this is not the best model.

#### Baseline Model 2:

    ##            acc    ppv  rmse_    auc
    ## Metrics 0.9357 0.7054 4.0075 0.8666

From the out-of-sample performance measures, we see that this model has
both higher accuracy and a significantly higher AUC. Although the RMSE
is higher for baseline model 2, I would still choose baseline model 2
over model 1 because both accuracy and AUC are improved. Furthermore, I
have also calculated the value for positive predictive values (PPV),
which are the proportion of true positives. In this case, the value for
ppv is approximately 0.7, which means that, of the positive values
predicted, about 70% are true positives.

#### Testing Best Linear Model:

In finding the “best” linear model, we have two different approaches.
First, we manually build a linear model by adding variables and testing
out-of-sample performance. Then, we built a model using forward
selection. We, again, use out-of-sample performance to assess the
models.

    ##           acc    ppv  rmse_    auc
    ## Metrics 0.933 0.6684 0.2337 0.8617

In this first linear model, we include all variables (except
`arrival_date`) as well as a few interaction terms
(`adults:stays_in_weekend_nights`, `adults:stays_in_week_nights`,
`stays_in_weekend_nights:stays_in_week_nights`). When comparing this
linear model to Baseline Model 2, we see that the linear model has
similar accuracy and AUC values as Model 2, but the RMSE value is
significantly lower. This may have to do with differences between linear
and logistic models. When we look at the PPV value, we see that the PPV
for this linear model is slightly lower than the PPV in model 2.

Although the out-of-sample performance values may suggest this is also a
good model, the logistic model is still preferred when predicting binary
outcomes.

In the second linear model, we use forward selection to find a strong
linear model, which gives us a regression where we regress `children`
on: `market_segment`, `customer_type`, `is_repeated_guest`, `adults`,
`market_segment:adults`, `customer_type:adults`, and
`market_segment:is_repeated_guest`

    ##            acc ppv  rmse_    auc
    ## Metrics 0.9187   0 0.2687 0.6898

In the second linear model, accuracy and RMSE is comparable to the first
linear model, but AUC drops significantly. Between the two linear
models, the first linear model still outperforms the second.

Although the first linear model has similar accuracy, PPV, and AUC
values, the logistic model is still preferred. Therefore, Baseline Model
2 is still the best model.

### Model Validation: Step 1

The following is an ROC plot for Baseline Model 2, using the
`hotels_val.csv` data.

![](exercise_2_files/figure-markdown_strict/unnamed-chunk-8-1.png)

An ROC plots TPR vs. FPR. TPR is another name for sensitivity, while FPR
is defined as 1-specificity, which explains why the numbers on the
x-axis are flipped with 1 on the left and 0 on the right.

### Model Validation: Step 2

In this step, we create 20 folds of `hotels_val`, so each fold will have
about 250 bookings. We conduct 20-fold cross-validation on Baseline
Model 2.

For each fold, we will: 1. Predict whether each booking will have
children on it 2. Sum up predicted probabilities for all bookings in the
fold, giving an estimate of the expected number of bookings with
children 3. Compare the “expected” number of bookings with children
vs. the actual number of bookings with children in that fold

**The results for each fold is as follows:**

    ##              acc    ppv   rmse_    auc pred_children actual_children
    ## Metrics   0.9360 0.5714  7.5286 0.6112             7              17
    ## Metrics1  0.9280 0.6667  6.3321 0.6363             9              21
    ## Metrics2  0.9560 0.8750  5.3427 0.7037             8              17
    ## Metrics3  0.9280 0.6667  5.2280 0.5957             6              20
    ## Metrics4  0.9240 0.8333 91.0308 0.6065             6              23
    ## Metrics5  0.9400 0.3636  5.9665 0.6520            11              12
    ## Metrics6  0.9440 0.8571  8.3685 0.6557             7              19
    ## Metrics7  0.9320 0.7500  5.7253 0.6385             8              21
    ## Metrics8  0.9440 0.7000 11.3796 0.6880            10              18
    ## Metrics9  0.9080 0.6154  5.1458 0.6427            13              26
    ## Metrics10 0.9320 0.7692  5.6898 0.7017            13              24
    ## Metrics11 0.8720 0.5714  8.1846 0.6038            14              34
    ## Metrics12 0.9440 0.8000  5.2494 0.7543            15              23
    ## Metrics13 0.9400 0.6667  6.2247 0.7019            12              19
    ## Metrics14 0.9360 0.5556  7.3147 0.6385             9              17
    ## Metrics15 0.9240 0.5556  5.7867 0.6163             9              20
    ## Metrics16 0.9400 0.7500  4.5533 0.7077            12              21
    ## Metrics17 0.9360 0.5455  7.7718 0.6657            11              17
    ## Metrics18 0.9400 0.6429  4.8244 0.7260            14              19
    ## Metrics19 0.9438 0.5000  5.3276 0.6008             6              14

**Average of performance metrics across 20 folds:**

    ##       acc      ppv    rmse_     auc pred_children actual_children
    ## 1 0.93239 0.662805 10.64874 0.65735            10            20.1

From the performance metrics, we see that accuracy stays above 90% on
average, but PPV and AUC is relatively low. This means that this model
could use further improvement to make better predictions.
