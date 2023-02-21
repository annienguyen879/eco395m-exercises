## Saratoga house prices

# Split into training and testing sets

# Linear Model

    ## [1] 22

    ## [1] 55327.96

    ## [1] 64724.24

In a single linear model train/test split, we get an in sample RMSE of
55327.96 and an out of sample RMSE of 64724.24.

    ##   result 
    ## 61303.02

The average out of sample RMSE of 25 random linear model train/test
splits is 61303.02.

# KNN Model

    ## [1] 59190.94

    ## [1] 120314.5

In a single KNN regression model train/test split, we get an in sample
RMSE of about 59,000 and an out of sample RMSE of about 120,000.

    ##   result 
    ## 120726.9

The average out of sample RMSE of 25 random KNN regression train/test
splits is 120,726.9.

Linear Model vs KNN Regression Model Report: Linear Model vs KNN
Regression Model: We estimate housing prices using a linear regression
model and a KNN regression model. We find that in a single linear model
train/test split, we get an in sample RMSE of 55,327.96 and an out of
sample RMSE of 64,724.24. Using a single KNN regression model train/test
split, we get an in sample RMSE of 59,190.94 and an out of sample RMSE
of 120,314.5. We can see that in a single train/test split, the in
sample RMSE of both models are similar (approximately 55 thousand and 59
thousand for the Linear and KNN models, respectively.) However, the out
of sample RMSE of the models were notably different at approximately 64
thousand and 120 thousand. So while both models performed similarly
using in sample data, the linear model performed much better using the
out of sample/testing data.

When measuring out-of-sample performance, there is random variation due
to the particular choice of data points that end up in the train/test
split sample. To address this issue, we average 25 different/random
train/test split estimates of out-of-sample RMSE. By doing this, we see
that the average out of sample RMSE of 25 random linear model train/test
splits is 61,303.02, and the average out of sample RMSE of 25 random KNN
regression train/test splits is 120,726.9. These results confirm our
findings from the single train/test splits. So we conclude that the
linear regression model is better at achieving lower out-of-sample
mean-squared error than the KNN model.

However, we note this analysis relied on the best models I could produce
using the data. There may exist a KNN model that outperforms the linear
model I produced. However, looking at the out of sample RMSE using
single and averages RMSE, this seems unlikely to be the case.

## Classification and retrospective sampling

What do you notice about the history variable vis-a-vis predicting
defaults? What do you think is going on here? In light of what you see
here, do you think this data set is appropriate for building a
predictive model of defaults, if the purpose of the model is to screen
prospective borrowers to classify them into “high” versus “low”
probability of default? Why or why not—and if not, would you recommend
any changes to the bank’s sampling scheme?

1.  What do you notice about the history variable vis-a-vis predicting
    defaults? If someone has a poor credit history, then the chance of
    them defaulting on a loan goes down by 0.37. Similarly, if someone
    has a terrible credit history, then the odds of them defaulting on a
    loan goes down by 0.18. These results seem to be the opposite of
    what I expected. Odds of someone defaulting on a loan should rise if
    they have a poor or terrible credit history.

2.  What do you think is going on here? Since the bank decided to
    conduct a case-control design, it used existing data to predict
    whether certain people will default on a loan, given other features.
    Since defaults are rare, the bank matched each default with similar
    sets of loans that had not defaulted. This means that they
    oversampled defaults relative to a random sample of loans in the
    bank’s overall portfolio.

In a simpler way, imagine if we initially had a sample of 100 loans, of
which 10 loans were defaulted. By oversampling defaults, instead of
having a low default probability of 1/10, we now have a much higher
probability. This method results in unreliable estimates since we can no
longer draw reasonable conclusions about how a particular credit history
may affect the default probability of an individual. This is why we are
getting a negative coefficient for poor and terrible credit history
while predicting loan defaults.

1.  In light of what you see here, do you think this data set is
    appropriate for building a predictive model of defaults, if the
    purpose of the model is to screen prospective borrowers to classify
    them into “high” versus “low” probability of default? This dataset
    is therefore, not appropriate for building a predictive model of
    defaults. The case-control sample places a defaulted loan in a bag
    of loans of similar value without considering other features such as
    credit history, age, and savings. This means that we cannot draw
    useful conclusions about someone particular to their credit history
    and classify them into “high” versus “low” probability of default.

2.  Why or why not—and if not, would you recommend any changes to the
    bank’s sampling scheme? The bank should accord weights to the
    default loans in the sample.

<!-- -->

    ## 'data.frame':    1000 obs. of  23 variables:
    ##  $ X              : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Default        : int  0 1 0 0 1 0 0 0 0 1 ...
    ##  $ checkingstatus1: chr  "A11" "A12" "A14" "A11" ...
    ##  $ duration       : int  6 48 12 42 24 36 24 36 12 30 ...
    ##  $ history        : chr  "terrible" "poor" "terrible" "poor" ...
    ##  $ purpose        : chr  "goods/repair" "goods/repair" "edu" "goods/repair" ...
    ##  $ amount         : int  1169 5951 2096 7882 4870 9055 2835 6948 3059 5234 ...
    ##  $ savings        : chr  "A65" "A61" "A61" "A61" ...
    ##  $ employ         : chr  "A75" "A73" "A74" "A74" ...
    ##  $ installment    : int  4 2 2 2 3 2 3 2 2 4 ...
    ##  $ status         : chr  "A93" "A92" "A93" "A93" ...
    ##  $ others         : chr  "A101" "A101" "A101" "A103" ...
    ##  $ residence      : int  4 2 3 4 4 4 4 2 4 2 ...
    ##  $ property       : chr  "A121" "A121" "A121" "A122" ...
    ##  $ age            : int  67 22 49 45 53 35 53 35 61 28 ...
    ##  $ otherplans     : chr  "A143" "A143" "A143" "A143" ...
    ##  $ housing        : chr  "A152" "A152" "A152" "A153" ...
    ##  $ cards          : int  2 1 1 1 2 1 1 1 1 2 ...
    ##  $ job            : chr  "A173" "A173" "A172" "A173" ...
    ##  $ liable         : int  1 1 2 2 2 2 1 1 1 1 ...
    ##  $ tele           : chr  "A192" "A191" "A191" "A191" ...
    ##  $ foreign        : chr  "foreign" "foreign" "foreign" "foreign" ...
    ##  $ rent           : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...

    ##           Default
    ## history      0   1
    ##   good      36  53
    ##   poor     421 197
    ##   terrible 243  50

![](exercise_2_files/figure-markdown_strict/pressure-1.png)![](exercise_2_files/figure-markdown_strict/pressure-2.png)

    ##         (Intercept)            duration              amount         installment 
    ##               -0.73                0.01                0.00                0.28 
    ##                 age         historypoor     historyterrible          purposeedu 
    ##               -0.02               -1.09               -1.95                0.68 
    ## purposegoods/repair       purposenewcar      purposeusedcar       foreigngerman 
    ##                0.05                0.83               -1.21               -1.18

## Children and hotel reservations

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

    ##            acc ppv rmse_    auc
    ## Metrics 0.9233 NaN 3.102 0.6775

After fitting the baseline model 1 to the training set and assessing
out-of-sample accuracy, we see this model predicts with about 91%
accuracy, but the area under the curve (AUC) is quite low. Although we
would not consider “bad” model performance, the low AUC indicates that
this is not the best model.

#### Baseline Model 2:

    ##            acc    ppv rmse_    auc
    ## Metrics 0.9399 0.7307 4.008 0.8642

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

    ##            acc    ppv  rmse_    auc
    ## Metrics 0.9407 0.7229 0.2273 0.8618

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
    ## Metrics 0.9233 NaN 0.2619 0.6835

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
