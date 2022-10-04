# module_14_challenge
Step 1:<br>
This challenge employs a predictive model to visualize cumulative returns of a security's actual returns against cumulative returns of a trading strategy that uses predicted trading signals. The trading strategy used an SVM model with the prior day's short SMA and long SMA as the features, and the current day's binary signal as the target (a value of 1 reflects positive returns (buy), and -1 reflects negative returns (sell). By analyzing one day's moving averages and the next day's actual returns on the training data, the model made predictions of the trading signal on the testing data. So, the trained model should analyze one day's small moving averages and predict if the next day's returns are positive or negative. If the prediction is positive (buy) and the actual returns are positive, then the 'Strategy Returns' are positive (1 * positive number = positive number). If the prediction is negative (sell) and the actual returns are negative, then again the 'Strategy Returns are positive (-1 * negative number = positive number).

SVC machine learning algorithim with the following parameters and resulting plot:
#- SMA short window: 4
#- SMA long window: 100 
#- training data slice: first 3 months of ohlcv dataframe (2015-04-02 : 2015-07-02). 
![3-month-training-window](https://github.com/danporeda/module_14_challenge/blob/main/Resources/3_month_train.png)



Step 2:<br>
Adjust the machine learning's training data to a bigger timeframe. The OHLCV dataframe spans six years, and in the first approach, only three months of data were used to train the model. Compared to a standard train-test-split of 70% data used for training, three months out of six years seems very undersampled. So, for this step we adjusted the training data to be 70% of the dataframe by changing the 'DateOffset' function's parameter from 'months=3' to 'years=3'. However, the resulting classification report seemed to predict the signal 100% as '1'. So we further refined the training timeframe by testing several timeframes from as large as 3 years down to 6 months, and then reading the classification report. the results determined that specifically 9 months seemed to perform best. All samples sized 1-year or longer yeilded recall score 0 for the -1 target, and every sample around 9 months had low recall scores. The 9-month window for training data yeilded: (-1) precision: 45%, recall: 31%, and (+1) precision: 57%, recall: 70%. However, the well-rounded classification report did not reflect performace in the cumulative-returns plot. after iterating through 3 to 36 months, the highest returns for the Strategy Returns came from a training window of 20 months. Observe the plot below:

Adjusted training data, parameters, and resulting plot:
#- SMA short window: 4
#- SMA long window: 100
#- training data slice: 20 months from the head of the dataframe (2015-04-02 : 2016-12-02).
![20-month-training-window](https://github.com/danporeda/module_14_challenge/blob/main/Resources/20_month_train.png)



Step 3:<br>
Adjust the Short and Long SMA windows. At this step, we kept the training sample window of 20 months, and then iterated through samples of increasing and decreasing Short and Long SMA windows. However, the findings from our experimented samples revealed that the original SMA windows (short = 4 days, long = 100 days) performed best, so no changes to the SMA windows are recommended. The above plot in Step 2 reflects the concluded optimal parameters: 20-month training window, and SMA-short window = 4, SMA-long window = 100. 


Step 4:<br>
The above results were performed using the SVC model. With the arrived training and testing data, we proceeded to run it through different models and analyze the results.

First was the Logistic Regression model, Which produced the following camparative plot of cumulative returns:
![Logistic Regression](https://github.com/danporeda/module_14_challenge/blob/main/Resources/LogisticRegression.png)
As this shows, the resultsing Strategy Returns seemed to be doing well until the end of the dataset, the Strategy returns plummeted, finishing lower than the Actual Returns.

Finally ran the training data through the AdaBoostClassifier as above, which produced the following plot:
![Ada Boost Classifier](https://github.com/danporeda/module_14_challenge/blob/main/Resources/adaboost.png)
The results were modest, as the Strategy Returns superceeded the Actual Returns. However, these strategy Returns did not outperform the original Strategy Returns from the SVC Model. 

In conclusion, from the sampled permutations of input data, training samples, and predictive models, the best performance was found with the SVC model, taking 20 months of training data from the OHLCV dataframe, and using an SMA short window of 4 days, and an SMA long window of 100 days. 