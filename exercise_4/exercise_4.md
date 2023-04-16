## 1. Clustering and PCA

We use the K-means algorithm as our selected clustering algorithm.
K-means tries to minimize within-cluster SSE. The K value used for the
K-means clustering is chosen using the Gap Statistic. The default
selection method looks for the first local peak up to the standard error
in estimating En\*\[log(WK)\]. The result for the best K value is also
presented in the clusGap function output. In both cases, we get that
K=5. The plot of the Gap Statistic selection method is presented below:

![](exercise_4_files/figure-markdown_strict/k_value.png)

Before running clustering algorithms, we note a few facts about red and
white wine:

-   White wine is more acidic and denser than red wine
-   Red wine generally has less sugar and sulfur dioxide than white wine
-   Red wine tends to have higher pH and alcohol content than white wine

We run the K means clustering algorithm and PCA on 11 chemical
properties of 6500 different bottles of vinho verde wine. Clustering
assumes that each data point is a member of one, and only one, cluster.
That is, clusters are mutually exclusive. The results of the K mean
clustering algorithm are presented below:

![](exercise_4_files/figure-markdown_strict/K Means feature plots-1.png)![](exercise_4_files/figure-markdown_strict/K Means feature plots-2.png)![](exercise_4_files/figure-markdown_strict/K Means feature plots-3.png)

Recall the three facts listed above.

-   White wine is more acidic and denser than red wine, so clusters with
    higher density and citric acid values (i.e. clusters in the
    upper-right side of the plot) should represent white wine. The plot
    suggests cluster 1 and 4 appear to be white wine.
-   Red wine generally has less sugar and sulfur dioxide than white
    wine, so clusters with lower less sugar and sulfur dioxide
    (i.e. clusters in the lower-left side of the plot) should represent
    red wine. The plot suggests cluster 4 and 5 appear to be red wine.
-   Red wine tends to have higher pH and alcohol content than white
    wine, so clusters with higher pH and alcohol content (i.e. clusters
    in the upper-right side of the plot) should represent red wine. The
    plot suggests cluster 5 appears to be red wine.

To check the accuracy of the K means clustering algorithm, we examine
citric acid and total sulfur dioxide content between red and white wine.
We observe that the clustering algorithm was fairly accurate grouping
red and white wine. We see that clusters 4 and 5 were grouped red wine.
While cluster 1 had a few red wines, generally white wine was clustered
in groups 1, 2, and 3. Additionally, while there was overlap in citric
acid and total sulfur dioxide measurement content, we see white wine had
outlier values that exceeded outlier values for red wine. Note this may
be attributed to the fact that generally white wine is more acidic and
red wine has lower sulfur dioxide (i.e. white wine has higher sulfur
dioxide). Overall, these results are consistent with the data and graphs
previously presented. The plots of citric acid and total sulfur dioxide
content between red and white wine are presented below:

![](exercise_4_files/K Means feature plots-1.png)![](exercise_4_files/K Means feature plots-2.png)

We examine each of the 11 chemical properties by quality. As we see in
the plots below, the clusters are not concentrated by quality ratings.
That is, it does not appear that the K means algorithm is capable of
distinguishing higher quality wine from lower quality wine. The 11
chemical properties by quality plots are presented below:

![](exercise_4_files/K means distinguishing high quality wine plots-1.png)![](exercise_4_files/K means distinguishing high quality wine plots-2.png)![](exercise_4_files/K means distinguishing high quality wine plots-3.png)

The goal of PCA is to find low-dimensional summaries of high-dimensional
data sets. PCA assumes that each data point is like a combination of
multiple basic ingredients where the ingredients are not mutually
exclusive. We can fiddle with ingredients continuously, but cluster
membership only discretely. PCA assumes we use the same basic
ingredients, just at differing amounts.

We would expect PCA to be the better dimensionality reduction technique
for data on 11 chemical properties of 6500 bottles of wine. This is
because red and white wine both contain the 11 characteristics, the wine
types just differ in amounts. That is, red and white wine share the same
ingredients, just at differing amounts. For example, as stated above,
white wine is generally more acidic while red wine tends to have higher
pH. But both wines will have both acidic and pH levels. PCA allows us to
fiddle with ingredients continuously. The K means clustering algorithm
is limited in that clustering algorithm cluster membership only
discretely. In the wine type PCA plot below, we see red and white wine
are clearly distinguished by component 2. That is, PCA was very clearly
able to sort red wines to the left and white wines on the right. So very
clearly, PCA was able to successfully pick up wine quality as a
component. Furthermore, in the wine quality PCA plot below, we see a
general vertical gradient in color (a gradient for Component 2). This
indicates PCA was also able to pick up wine quality as a component.
Hence, as expected, PCA was better at distinguishing red wine from white
wine and identify wine quality using only the unsupervised information
contained in the data on chemical properties.

![](exercise_4_files/PCA-1.png)![](exercise_4_files/PCA-2.png)

## 2. Market segmentation

## ***Introduction***

We want to use market-research data based on tweets for NutrientH20 to
come up with how the brand may position itself to different market
segments for maximum appeal. So what are we working with? NutrientH20’s
advertising firm took a sample of the brand’s Twitter followers, and
collected each tweet by a follower over a seven-day period in June 2014.
Amazon’s Mechanical Turk service parsed through each tweet and allocated
different categories to it, such as family or sports. Each tweet can
have more than 1 category. There are a total of 36 pre-specified
categories.

    social_marketing <- read.csv("social_marketing.csv")

    ls(social_marketing)

    ##  [1] "adult"            "art"              "automotive"       "beauty"          
    ##  [5] "business"         "chatter"          "college_uni"      "computers"       
    ##  [9] "cooking"          "crafts"           "current_events"   "dating"          
    ## [13] "eco"              "family"           "fashion"          "food"            
    ## [17] "health_nutrition" "home_and_garden"  "music"            "news"            
    ## [21] "online_gaming"    "outdoors"         "parenting"        "personal_fitness"
    ## [25] "photo_sharing"    "politics"         "religion"         "school"          
    ## [29] "shopping"         "small_business"   "spam"             "sports_fandom"   
    ## [33] "sports_playing"   "travel"           "tv_film"          "uncategorized"   
    ## [37] "X"

The original dataset has 7882 observations with 37 variables (you can
find a more detailed breakdown in the Appendix).

Our big task? Identifying market segments. Are NutrientH20’s followers
more sports-oriented, family-focused, or even fashion-obsessed? Getting
these segments correct will help us tailor the firm’s advertising
strategies.

## ***Methodology***

Before we run our magic, we need to make sure our data is centered and
scaled so that we can get meaningful insights. Since there are still
spam, adult, uncategorised and chatter categories that may not be useful
for our overall analysis, I will drop these from dataset.

I then center the data by subtracting the mean value of each variable
from all of its values. This will ensure that the mean of each variable
is zero. Centering the data makes sure that the clustering algorithm
focuses on the patterns and differences in the data, rather than the
absolute values of each variable. To scale the data, I divide each
variable by its standard deviation. This will ensure that each variable
has a similar scale and range. Scaling the data makes sure that the PCA
and clustering algorithms treats each variable equally, regardless of
its magnitude or unit of measurement.

I will then check for correlation within my data. I will first use
principal component analysis (PCA) to identify which variables are most
relevant in the dataset. Since this is a large dataset, PCA tries to
simplify it by reducing the number of variables or dimensions involved.
It is somewhat like taking a 1000-piece puzzle and contracting it into a
500-piece puzzle of the same picture. Based on this, I will check for
appropriate clusters in the data. Clustering is what it sounds like. It
groups similar objects together. So for example, if multiple tweets are
revolving around sports, the algorithm would classify them into one
cluster.

### Correlation matrix

    #Clean data
    X = social_marketing[,-c(1, 2, 6, 36, 37)]

    # Center and scale the data
    X = scale(X, center=TRUE, scale=TRUE)

    # Extract the centers and scales from the rescaled data (which are named attributes)
    mu = attr(X,"scaled:center")
    sigma = attr(X,"scaled:scale")

    #Correlation matrix

    corr_matrix <- cor(X)
    ggcorrplot(corr_matrix)

![](exercise_4_files/figure-markdown_strict/unnamed-chunk-2-1.png) We
can see that there are some variables that are highly correlated with
each other. Therefore, we can use PCA to explain the data.

### Principal Component Analysis (PCA)

I generate 10 principal components of the data, since PC10 ends up
explaining 65% of our data. Beyond this, we had little marginal increase
in variation explained by summaries.

    # Now run PCA on the social marketing data
    pc_sm = prcomp(X, rank=10, center = TRUE, scale=TRUE)

    # these are the linear combinations of station-level data that define the PCs
    # each column is a different PC, i.e. a different linear summary of the stations
    #head(pc_sm$rotation)

    # overall variation in 256 original features
    summary(pc_sm)

    ## Importance of first k=10 (out of 32) components:
    ##                           PC1     PC2     PC3     PC4     PC5     PC6     PC7
    ## Standard deviation     2.0987 1.66483 1.59119 1.52912 1.46728 1.28477 1.21616
    ## Proportion of Variance 0.1376 0.08661 0.07912 0.07307 0.06728 0.05158 0.04622
    ## Cumulative Proportion  0.1376 0.22426 0.30338 0.37645 0.44372 0.49531 0.54153
    ##                            PC8     PC9    PC10
    ## Standard deviation     1.17377 1.05325 0.99329
    ## Proportion of Variance 0.04305 0.03467 0.03083
    ## Cumulative Proportion  0.58458 0.61925 0.65008

    plot(pc_sm)

![](exercise_4_files/figure-markdown_strict/unnamed-chunk-3-1.png)

    # plotting variation 
    pr_var <-  pc_sm$sdev ^ 2
    pve <- pr_var / sum(pr_var)
    plot(pve, xlab = "Principal Component", ylab = "Proportion of Variance Explained", ylim = c(0,1), type = 'b')

![](exercise_4_files/figure-markdown_strict/unnamed-chunk-3-2.png)

    plot(cumsum(pve), xlab = "Principal Component", ylab = "Cummulative Proportion of Variance Explained", ylim = c(0,1), type = 'b')

![](exercise_4_files/figure-markdown_strict/unnamed-chunk-3-3.png)

    # Checking loadings on PCA
    round(pc_sm$rotation[,1:10],2) 

    ##                   PC1   PC2   PC3   PC4   PC5   PC6   PC7   PC8   PC9  PC10
    ## current_events   0.09 -0.05  0.05 -0.02  0.04 -0.13  0.19 -0.11  0.00  0.07
    ## travel           0.12 -0.05  0.44  0.11  0.05  0.00 -0.31 -0.16  0.12 -0.04
    ## photo_sharing    0.16 -0.27 -0.03 -0.13  0.20 -0.06  0.31 -0.29  0.04 -0.12
    ## tv_film          0.09 -0.08  0.10 -0.12 -0.18 -0.50 -0.05  0.30  0.12  0.07
    ## sports_fandom    0.30  0.30 -0.07 -0.03  0.02  0.06  0.09  0.07  0.05  0.05
    ## politics         0.13 -0.02  0.50  0.17  0.09  0.10 -0.04  0.01  0.05 -0.03
    ## food             0.31  0.20 -0.11  0.08 -0.09 -0.02 -0.10 -0.04  0.13 -0.02
    ## family           0.25  0.19 -0.06 -0.05 -0.01  0.06  0.09 -0.05  0.09 -0.08
    ## home_and_garden  0.11 -0.05  0.03  0.00 -0.04 -0.11  0.05  0.09 -0.36  0.12
    ## music            0.12 -0.15  0.00 -0.11 -0.04 -0.15  0.02  0.08  0.28  0.71
    ## news             0.13  0.02  0.35  0.16  0.04  0.16  0.27  0.39 -0.05 -0.01
    ## online_gaming    0.07 -0.10  0.07 -0.29 -0.44  0.27  0.01 -0.07 -0.02 -0.15
    ## shopping         0.11 -0.16  0.02 -0.06  0.11 -0.18  0.43 -0.41  0.02 -0.10
    ## health_nutrition 0.13 -0.21 -0.19  0.44 -0.23  0.02 -0.02 -0.02  0.03 -0.02
    ## college_uni      0.09 -0.13  0.10 -0.33 -0.44  0.17  0.01 -0.05  0.05  0.05
    ## sports_playing   0.13 -0.13  0.06 -0.24 -0.33  0.21 -0.01 -0.06 -0.07 -0.01
    ## cooking          0.18 -0.37 -0.17 -0.05  0.22  0.17 -0.13  0.17  0.12 -0.09
    ## eco              0.14 -0.10 -0.02  0.12 -0.06 -0.10  0.15 -0.19  0.00 -0.08
    ## computers        0.14 -0.05  0.38  0.11  0.09  0.07 -0.27 -0.25  0.11 -0.02
    ## business         0.13 -0.09  0.11 -0.01  0.05 -0.15  0.00 -0.19 -0.11  0.18
    ## outdoors         0.14 -0.17 -0.11  0.39 -0.19  0.06  0.03  0.08  0.00  0.09
    ## crafts           0.19  0.02  0.00 -0.02 -0.04 -0.25 -0.06  0.01 -0.08 -0.34
    ## automotive       0.13  0.04  0.19  0.04  0.04  0.16  0.51  0.35 -0.07 -0.01
    ## art              0.10 -0.07  0.06 -0.09 -0.13 -0.46 -0.11  0.29 -0.02 -0.40
    ## religion         0.32  0.30 -0.11 -0.04  0.02  0.01 -0.10 -0.06  0.09  0.05
    ## beauty           0.20 -0.25 -0.14 -0.17  0.29  0.14 -0.14  0.19  0.05 -0.03
    ## parenting        0.31  0.28 -0.11 -0.02  0.03  0.05 -0.04 -0.04  0.05  0.01
    ## dating           0.10 -0.07  0.03  0.03  0.01  0.01 -0.16 -0.05 -0.76  0.17
    ## school           0.29  0.19 -0.10 -0.06  0.08 -0.01 -0.05 -0.05 -0.25  0.04
    ## personal_fitness 0.14 -0.21 -0.18  0.42 -0.22  0.01  0.00 -0.03  0.03 -0.02
    ## fashion          0.18 -0.32 -0.12 -0.17  0.28  0.14 -0.16  0.15 -0.04 -0.06
    ## small_business   0.11 -0.09  0.10 -0.09 -0.01 -0.23 -0.01 -0.02 -0.07  0.21

    # create a tidy summary of the loadings
    loadings_summary = pc_sm$rotation %>%
      as.data.frame() %>%
      rownames_to_column('Category')

    #PC1
    loadings_summary %>%
      select(Category, PC1) %>%
      arrange(desc(PC1))

    ##            Category        PC1
    ## 1          religion 0.31550368
    ## 2              food 0.31296463
    ## 3         parenting 0.31039913
    ## 4     sports_fandom 0.30489680
    ## 5            school 0.28912549
    ## 6            family 0.25453224
    ## 7            beauty 0.19941180
    ## 8            crafts 0.19406911
    ## 9           cooking 0.18455349
    ## 10          fashion 0.17802974
    ## 11    photo_sharing 0.15674558
    ## 12        computers 0.14411277
    ## 13         outdoors 0.14405229
    ## 14              eco 0.14082664
    ## 15 personal_fitness 0.13750846
    ## 16             news 0.13320952
    ## 17         politics 0.13289089
    ## 18       automotive 0.13100966
    ## 19         business 0.12906125
    ## 20   sports_playing 0.12843880
    ## 21 health_nutrition 0.12504946
    ## 22           travel 0.11841396
    ## 23            music 0.11787525
    ## 24  home_and_garden 0.11368173
    ## 25   small_business 0.11305712
    ## 26         shopping 0.10891732
    ## 27           dating 0.09773030
    ## 28              art 0.09677326
    ## 29          tv_film 0.09398281
    ## 30   current_events 0.09161265
    ## 31      college_uni 0.09135998
    ## 32    online_gaming 0.07400221

    #PC2
    loadings_summary %>%
      select(Category, PC2) %>%
      arrange(desc(PC2))

    ##            Category         PC2
    ## 1     sports_fandom  0.30225848
    ## 2          religion  0.29547112
    ## 3         parenting  0.27584941
    ## 4              food  0.20299642
    ## 5            school  0.19192085
    ## 6            family  0.18591469
    ## 7        automotive  0.03804935
    ## 8              news  0.02319302
    ## 9            crafts  0.01662037
    ## 10         politics -0.02061149
    ## 11        computers -0.04658779
    ## 12           travel -0.05016670
    ## 13  home_and_garden -0.05327205
    ## 14   current_events -0.05412578
    ## 15           dating -0.06513053
    ## 16              art -0.06774581
    ## 17          tv_film -0.07860944
    ## 18   small_business -0.08939350
    ## 19         business -0.09354319
    ## 20              eco -0.09722265
    ## 21    online_gaming -0.09997424
    ## 22   sports_playing -0.12539108
    ## 23      college_uni -0.12562310
    ## 24            music -0.15086771
    ## 25         shopping -0.15707705
    ## 26         outdoors -0.17228803
    ## 27 personal_fitness -0.20592080
    ## 28 health_nutrition -0.21136587
    ## 29           beauty -0.24633528
    ## 30    photo_sharing -0.27274368
    ## 31          fashion -0.31651895
    ## 32          cooking -0.37059755

    #PC3
    loadings_summary %>%
      select(Category, PC3) %>%
      arrange(desc(PC3))

    ##            Category          PC3
    ## 1          politics  0.503026288
    ## 2            travel  0.440747471
    ## 3         computers  0.378667428
    ## 4              news  0.346959527
    ## 5        automotive  0.185678239
    ## 6          business  0.105636488
    ## 7    small_business  0.103268148
    ## 8       college_uni  0.100511636
    ## 9           tv_film  0.099224961
    ## 10    online_gaming  0.069171916
    ## 11              art  0.061842233
    ## 12   sports_playing  0.057123483
    ## 13   current_events  0.046424285
    ## 14           dating  0.034640135
    ## 15  home_and_garden  0.026326200
    ## 16         shopping  0.021516637
    ## 17           crafts  0.001664916
    ## 18            music -0.002235723
    ## 19              eco -0.024276275
    ## 20    photo_sharing -0.025581039
    ## 21           family -0.064035662
    ## 22    sports_fandom -0.070152584
    ## 23           school -0.098669019
    ## 24        parenting -0.105311156
    ## 25         outdoors -0.105563558
    ## 26         religion -0.108724285
    ## 27             food -0.113632532
    ## 28          fashion -0.121633440
    ## 29           beauty -0.137129678
    ## 30          cooking -0.165556638
    ## 31 personal_fitness -0.181925626
    ## 32 health_nutrition -0.187678435

    #PC4
    loadings_summary %>%
      select(Category, PC4) %>%
      arrange(desc(PC4))

    ##            Category           PC4
    ## 1  health_nutrition  0.4371115574
    ## 2  personal_fitness  0.4206179779
    ## 3          outdoors  0.3859715023
    ## 4          politics  0.1674941042
    ## 5              news  0.1561362328
    ## 6               eco  0.1233297213
    ## 7         computers  0.1119142324
    ## 8            travel  0.1075494609
    ## 9              food  0.0828505656
    ## 10       automotive  0.0433350965
    ## 11           dating  0.0271898446
    ## 12  home_and_garden -0.0001320125
    ## 13         business -0.0145817471
    ## 14        parenting -0.0209143621
    ## 15   current_events -0.0219596318
    ## 16           crafts -0.0237588733
    ## 17    sports_fandom -0.0293878348
    ## 18         religion -0.0420267671
    ## 19          cooking -0.0490697934
    ## 20           family -0.0537300618
    ## 21           school -0.0563855491
    ## 22         shopping -0.0585366808
    ## 23              art -0.0926893062
    ## 24   small_business -0.0939938563
    ## 25            music -0.1052579141
    ## 26          tv_film -0.1249958527
    ## 27    photo_sharing -0.1270976587
    ## 28          fashion -0.1668855555
    ## 29           beauty -0.1691033762
    ## 30   sports_playing -0.2355179502
    ## 31    online_gaming -0.2913256954
    ## 32      college_uni -0.3294348289

    #PC5
    loadings_summary %>%
      select(Category, PC5) %>%
      arrange(desc(PC5))

    ##            Category          PC5
    ## 1            beauty  0.291337733
    ## 2           fashion  0.276386057
    ## 3           cooking  0.216412286
    ## 4     photo_sharing  0.198792569
    ## 5          shopping  0.109540286
    ## 6         computers  0.091181045
    ## 7          politics  0.087582339
    ## 8            school  0.081768221
    ## 9            travel  0.054415953
    ## 10         business  0.048945968
    ## 11       automotive  0.043109219
    ## 12             news  0.041778850
    ## 13   current_events  0.038639159
    ## 14        parenting  0.031992855
    ## 15    sports_fandom  0.020018243
    ## 16         religion  0.019357440
    ## 17           dating  0.007288697
    ## 18           family -0.005291282
    ## 19   small_business -0.014786279
    ## 20            music -0.036039134
    ## 21  home_and_garden -0.036321428
    ## 22           crafts -0.042489248
    ## 23              eco -0.062654876
    ## 24             food -0.088403899
    ## 25              art -0.132862035
    ## 26          tv_film -0.178194848
    ## 27         outdoors -0.193891124
    ## 28 personal_fitness -0.223967227
    ## 29 health_nutrition -0.232871495
    ## 30   sports_playing -0.333492078
    ## 31    online_gaming -0.438510437
    ## 32      college_uni -0.440562756

    #PC6
    loadings_summary %>%
      select(Category, PC6) %>%
      arrange(desc(PC6))

    ##            Category          PC6
    ## 1     online_gaming  0.274855669
    ## 2    sports_playing  0.214815476
    ## 3       college_uni  0.171620860
    ## 4           cooking  0.167575393
    ## 5        automotive  0.164839655
    ## 6              news  0.156656341
    ## 7           fashion  0.142772889
    ## 8            beauty  0.140530136
    ## 9          politics  0.104456580
    ## 10        computers  0.071899034
    ## 11    sports_fandom  0.064324509
    ## 12           family  0.063592882
    ## 13         outdoors  0.060012935
    ## 14        parenting  0.051348153
    ## 15 health_nutrition  0.015945202
    ## 16 personal_fitness  0.011765235
    ## 17           dating  0.009108887
    ## 18         religion  0.005360473
    ## 19           travel -0.002012466
    ## 20           school -0.012207628
    ## 21             food -0.021835310
    ## 22    photo_sharing -0.057086160
    ## 23              eco -0.103184406
    ## 24  home_and_garden -0.105416052
    ## 25   current_events -0.134490170
    ## 26         business -0.145666815
    ## 27            music -0.149386348
    ## 28         shopping -0.181931173
    ## 29   small_business -0.229238495
    ## 30           crafts -0.254014558
    ## 31              art -0.463671706
    ## 32          tv_film -0.495466799

    #PC7
    loadings_summary %>%
      select(Category, PC7) %>%
      arrange(desc(PC7))

    ##            Category          PC7
    ## 1        automotive  0.513814864
    ## 2          shopping  0.429126108
    ## 3     photo_sharing  0.311754823
    ## 4              news  0.269924462
    ## 5    current_events  0.194128989
    ## 6               eco  0.145591788
    ## 7            family  0.092416515
    ## 8     sports_fandom  0.088698263
    ## 9   home_and_garden  0.051855842
    ## 10         outdoors  0.033327230
    ## 11            music  0.019132033
    ## 12    online_gaming  0.013884338
    ## 13      college_uni  0.008465957
    ## 14         business -0.002456088
    ## 15 personal_fitness -0.004809660
    ## 16   sports_playing -0.012054815
    ## 17   small_business -0.013940768
    ## 18 health_nutrition -0.023350150
    ## 19        parenting -0.042703167
    ## 20         politics -0.044488027
    ## 21          tv_film -0.046186185
    ## 22           school -0.053443744
    ## 23           crafts -0.063849751
    ## 24             food -0.097001267
    ## 25         religion -0.099016970
    ## 26              art -0.106519349
    ## 27          cooking -0.133268175
    ## 28           beauty -0.140192766
    ## 29          fashion -0.157449326
    ## 30           dating -0.162953626
    ## 31        computers -0.272049627
    ## 32           travel -0.308157072

    #PC8
    loadings_summary %>%
      select(Category, PC8) %>%
      arrange(desc(PC8))

    ##            Category         PC8
    ## 1              news  0.38680527
    ## 2        automotive  0.34869879
    ## 3           tv_film  0.29580327
    ## 4               art  0.28598568
    ## 5            beauty  0.19041563
    ## 6           cooking  0.16845770
    ## 7           fashion  0.15446987
    ## 8   home_and_garden  0.08603592
    ## 9          outdoors  0.08159401
    ## 10            music  0.07622512
    ## 11    sports_fandom  0.07089552
    ## 12         politics  0.01481782
    ## 13           crafts  0.01086300
    ## 14 health_nutrition -0.01917216
    ## 15   small_business -0.02130123
    ## 16 personal_fitness -0.03438445
    ## 17             food -0.03795077
    ## 18        parenting -0.04249528
    ## 19           family -0.04509193
    ## 20           school -0.04963686
    ## 21      college_uni -0.05091982
    ## 22           dating -0.05156922
    ## 23   sports_playing -0.05563322
    ## 24         religion -0.05580622
    ## 25    online_gaming -0.06688220
    ## 26   current_events -0.11025385
    ## 27           travel -0.15575665
    ## 28              eco -0.18878818
    ## 29         business -0.19461431
    ## 30        computers -0.24752091
    ## 31    photo_sharing -0.29056982
    ## 32         shopping -0.40553891

    #PC9
    loadings_summary %>%
      select(Category, PC9) %>%
      arrange(desc(PC9))

    ##            Category          PC9
    ## 1             music  0.284279948
    ## 2              food  0.127720993
    ## 3           tv_film  0.122832532
    ## 4            travel  0.122003067
    ## 5           cooking  0.121866751
    ## 6         computers  0.112397690
    ## 7            family  0.089183461
    ## 8          religion  0.088982654
    ## 9         parenting  0.054026060
    ## 10         politics  0.053886934
    ## 11           beauty  0.053815603
    ## 12    sports_fandom  0.053411507
    ## 13      college_uni  0.046188828
    ## 14    photo_sharing  0.038638293
    ## 15 health_nutrition  0.034935986
    ## 16 personal_fitness  0.025058969
    ## 17         shopping  0.021214599
    ## 18   current_events  0.004680002
    ## 19         outdoors  0.001006533
    ## 20              eco -0.001993157
    ## 21    online_gaming -0.015023080
    ## 22              art -0.016067188
    ## 23          fashion -0.041649909
    ## 24             news -0.047651857
    ## 25   sports_playing -0.066257527
    ## 26       automotive -0.073238451
    ## 27   small_business -0.073947453
    ## 28           crafts -0.076024809
    ## 29         business -0.111690647
    ## 30           school -0.253618359
    ## 31  home_and_garden -0.357393715
    ## 32           dating -0.762890247

    #PC10
    loadings_summary %>%
      select(Category, PC10) %>%
      arrange(desc(PC10))

    ##            Category         PC10
    ## 1             music  0.709074449
    ## 2    small_business  0.211262792
    ## 3          business  0.177074659
    ## 4            dating  0.172415312
    ## 5   home_and_garden  0.117273272
    ## 6          outdoors  0.085516623
    ## 7           tv_film  0.073880982
    ## 8    current_events  0.070874945
    ## 9          religion  0.050808947
    ## 10    sports_fandom  0.046080185
    ## 11      college_uni  0.045612004
    ## 12           school  0.044161616
    ## 13        parenting  0.012891826
    ## 14       automotive -0.006419145
    ## 15   sports_playing -0.009465990
    ## 16             news -0.010019083
    ## 17             food -0.015859861
    ## 18 health_nutrition -0.021034445
    ## 19 personal_fitness -0.021097189
    ## 20        computers -0.022782271
    ## 21           beauty -0.031536846
    ## 22         politics -0.032574932
    ## 23           travel -0.041028276
    ## 24          fashion -0.056298341
    ## 25           family -0.078145763
    ## 26              eco -0.084193350
    ## 27          cooking -0.091981050
    ## 28         shopping -0.102855668
    ## 29    photo_sharing -0.122539274
    ## 30    online_gaming -0.149131071
    ## 31           crafts -0.340810983
    ## 32              art -0.395737791

Based on these loadings, we can see that a few clear segments emerge.
PC1 and PC2 signal families with kids. Variables such as beauty, crafts,
cooking and fashion suggest that mothers are most likely an active
audience of the brand, and sports and school indicate that they are
focused on making sure their kids get enough nutrition for athletic and
school activities.

PC3 identifies a different group, that is most likely working adults in
their 20s and college students. Variables such as politics, travel, and
computers contribute the most to this summary.

PC4 shows a new segment that is focused on fitness and the outdoors.
This also reflects probably an audience in their 20s and 30s, since
politics and news are also positive contributors.

PC5 tells us about people who are focused on living an “influencer”
lifestyle, where they emphasise beauty, fashion, cooking, and
photo-sharing on their social media.

PC6 brings us back to college students but those who are specifically
into gaming and sports.

It is difficult to glean a segment from PC7 and PC8, but seem to be
well-paid homeowners probably in their 30s and 40s.

PC9 and PC10 also seem similar, indicating those with a more “bohemian”
art-oriented lifestyle, where they like to support small businesses,
maintain their home and gardens, and are interested in food, film, and
especially music.

Therefore, NutrientH20 should cater to the following segments: families
with kids, college students, working adults, fitness enthusiasts,
art-lovers and small-business supporters, and lifestyle influencers.

    # Run k-means with 6 clusters and 25 starts
    clust1 = kmeans(X, 6, nstart=25)

    scores = pc_sm$x

    Y <- as.data.frame(cbind(X, scores))

    ggplot(Y) + geom_point(aes(x=PC1, y=PC2, fill=factor(clust1$cluster)),
    size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

![](exercise_4_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    #ggplot(Y) + geom_point(aes(x=PC1, y=PC3, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

    #ggplot(Y) + geom_point(aes(x=PC1, y=PC4, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

    #ggplot(Y) + geom_point(aes(x=PC1, y=PC5, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

    #ggplot(Y) + geom_point(aes(x=PC1, y=PC6, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

    #ggplot(Y) + geom_point(aes(x=PC1, y=PC7, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

    #ggplot(Y) + geom_point(aes(x=PC1, y=PC8, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

    #ggplot(Y) + geom_point(aes(x=PC2, y=PC3, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

    #ggplot(Y) + geom_point(aes(x=PC3, y=PC4, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

    #ggplot(Y) + geom_point(aes(x=PC5, y=PC6, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

    #ggplot(Y) + geom_point(aes(x=PC3, y=PC6, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

    #ggplot(Y) + geom_point(aes(x=PC4, y=PC6, fill=factor(clust1$cluster)),
    #size=3, col="#7f7f7f", shape=21) + theme_bw(base_family="Helvetica")

When we check clusters within the PCAs, we can see the same patterns
emerge. As an example, we can see the clusters within PC1 and PC2. We
can see that the first cluster is found more within PC2. This is close
to the group of families with kids identified earlier. Comparing PC1 and
PC3, cluster 5 is clearly in PC3, while Cluster 1 is clearly not, giving
us the working adults. Checking PC4 and PC1, we can see Cluster 6 is in
PC4, that is fitness enthusiasts. Cluster 4 is more towards PC5 of
“influencers”, but there is a slight lack of clarity. Cluster 3 is in
PC6, college students, but overlaps with some other clusters.

## 3. Association rules for grocery purchases

In this section, we use data on grocery purchases to find some
interesting association rules for customers’ shopping baskets. The data
that we have is in a `txt` file. Every row represents one customer’s
basket in which multiple items per row are separated by commas. First,
we transform the `txt` file into a readable dataframe, so we can perform
analysis.

A sample of our data can be seen below:

    ##    id               items
    ## 1   1        citrus fruit
    ## 2   1 semi-finished bread
    ## 3   1           margarine
    ## 4   1         ready soups
    ## 5   2      tropical fruit
    ## 6   2              yogurt
    ## 7   2              coffee
    ## 8   3          whole milk
    ## 9   4           pip fruit
    ## 10  4              yogurt

First, we split data into a list of grocery items for each customer and
remove any duplicates. Then, we run the `apriori` algorithm to look at
rules, where support is greater than 0.005 and confidence is greater
than 0.1.

    ## Apriori
    ## 
    ## Parameter specification:
    ##  confidence minval smax arem  aval originalSupport maxtime support minlen
    ##         0.1    0.1    1 none FALSE            TRUE       5   0.005      1
    ##  maxlen target  ext
    ##       4  rules TRUE
    ## 
    ## Algorithmic control:
    ##  filter tree heap memopt load sort verbose
    ##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
    ## 
    ## Absolute minimum support count: 49 
    ## 
    ## set item appearances ...[0 item(s)] done [0.00s].
    ## set transactions ...[169 item(s), 9835 transaction(s)] done [0.00s].
    ## sorting and recoding items ... [120 item(s)] done [0.00s].
    ## creating transaction tree ... done [0.00s].
    ## checking subsets of size 1 2 3 4 done [0.00s].
    ## writing ... [1582 rule(s)] done [0.00s].
    ## creating S4 object  ... done [0.00s].

Inspecting at the rules, we see that 1,582 rules have been generated. We
generate plots to visualize the rules in order to provide us additional
insight on confidence, support, and lift.

![](exercise_4_files/figure-markdown_strict/rules_plots-1.png)![](exercise_4_files/figure-markdown_strict/rules_plots-2.png)![](exercise_4_files/figure-markdown_strict/rules_plots-3.png)

In the second graph, it appears that confidence is greater when lift is
greater than 2. In the first graph, it also seems like higher confidence
rules were cluster near the bottom, while in the third graph, we see
that order 1 is also clustered at the bottom. As a result, it may be
better to keep the confidence at about 0.15 to ensure that all orders
are represented. Based on the plots above, a good threshold may be when
lift is greater than 2 and when confidence is above 0.15. Under these
conditions, we would have a subset of 495 rules.

Next, we will produce a visualization for our associations through a
program called Gephi.

![](gephi_graph.png)

The item sets we discovered makes sense. For example, the items
associated with “shopping bags” include soda, vegetables, rolls/buns,
and sausage, which are all items necessary for a picnic or barbecue.

![](itemset.png)

From this section, we analyzed the association between different grocery
items based on customers’ purchase baskets. We found interesting
association rules and produce a visualization of that network of items
using Gephi.
