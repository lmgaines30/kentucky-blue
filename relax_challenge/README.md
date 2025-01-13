**Objective**

The objective of this project is to design a classification model to predict which users will become "adopted users". An "adopted user" is defined as a user who has logged into the application 3 times over any 7-day period.

**Data Wrangling**

The users dataset required some cleansing.  The datetime fields, creation\_time and last\_session\_creation\_time were imported with data types of object and float64, respectively.  Both were converted to datetime fields in order to leverage them as features within our prediction models. For the missing values in the last\_session\_creation\_time field, I utilized the fillna() method to replace them with the  value of the creation\_time field (assuming that the last login was at the time of creation).  I also dropped the columns that were not needed, such as name, email and invited\_by\_user\_id. 

To determine if the user should be classified as an “adopted” user, I had to create a new feature based on the login information in the engagement dataset.   I opted to use the window function in pandas to count the number of times visited within 7 days of each login for each record in the dataset.  I also captured the rolling sum for the number of visits within 7 days, and kept only 1 record for each user as represented by the max value of visits within 7 days for each record.  If that number was greater than or equal to 3, updating the “adopted\_user” column with “True”, otherwise “False”.

**Modeling**

Prior to running any prediction models.  I merged my dataframes and leveraged one hot encoding to transform my categorical fields.  I selected logistic regression, random forest, and K nearest neighbors for my predictions.  All of the models performed really well, with approximately 97% accuracy scores.  This would also suggest overfitting.

I ultimately chose the random forest model as the best.  When looking at the importance of features, it shows very clearly that the most important feature was the length of time between the initial account creation\_time and the last\_session\_creation\_time.  It accounts for over 80% of the variance.

**Potential for improvements**

When looking at our target variable, “adopted user” we can see that this dataset was imbalanced. With more time, I would have tried oversampling techniques to balance the dataset.

![Pie Chart with proportions for target variable](relax_challenge/adopted_user_pie_chart.svg) 
 
Lastly, I would have created more dimensions to see if the referring users or organizations may have had any importance with predicting adoption rates.  However, they would have greatly increased the dimensionality and complexity of this model.

