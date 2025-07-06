# Protein Content and Recipe Rating Analysis
By: Nayana Naineni

https://nayana1729.github.io/Protein-Content-and-Recipe-Rating-Analysis/

## Introduction
Protein content in food is something that has become incredibly trendy. Due to the fitness industry and influencers from the same pushing the narrative that high-protein foods will lead to more muscle growth and athletic performance, I wanted to explore the relationship between protein content and recipe rating. This question helps us better understand nutrition and its impact on consumer preferences. I will utilize two datasets, called ratings and recipes, from food.com, a popular recipe-sharing website containing detailed data from 2008 onwards. There are 731927 rows in the first dataset with 5 columns and 83782 rows in the second dataset with 12 columns. For this project, the relevant columns include:

Ratings:

'rating' - Rating given

Recipes:

'minutes' - Minutes taken to prepare recipe  
'nutrition - Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”  
'description' - User-provided description

## Data Cleaning and Exploratory Data Analysis
To clean the dataset, I first merged the two datasets to make working with the data easier and then replaced the 0 values in ratings with np.nan. This is because if calculating with 0s, they will seem like lower ratings even though in reality they are missing values. This helps us differentiate between actual ratings vs missing values. Then, I added a column with average ratings to understand the mean rating for each recipe. After that, I split nutrition into its individual values since we will utilize protein and calories for this project. Then, I kept only the relevant columns from this dataset. I also realized that some protein values were extreme (ex. 4000 grams) and I wanted to keep a threshold for the amount so I removed all rows that had a protein content of greater than 500 grams.

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>minutes</th>
      <th>nutrition</th>
      <th>description</th>
      <th>rating</th>
      <th>average_ratings</th>
      <th>calories</th>
      <th>protein</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>40</td>
      <td>[138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]</td>
      <td>these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>138.4</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>45</td>
      <td>[595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]</td>
      <td>this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>595.1</td>
      <td>13.0</td>
    </tr>
    <tr>
      <td>40</td>
      <td>[194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]</td>
      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>194.8</td>
      <td>22.0</td>
    </tr>
    <tr>
      <td>40</td>
      <td>[194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]</td>
      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>194.8</td>
      <td>22.0</td>
    </tr>
    <tr>
      <td>40</td>
      <td>[194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]</td>
      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>194.8</td>
      <td>22.0</td>
    </tr>
  </tbody>
</table>

### Univariate Analysis
<iframe src="assets/protein_uni.html" width="100%" height="600px"></iframe>

From this histogram, we can observe that the protein content is positively skewed with a right-tail. This tells us that the majority of the recipes have a moderate to low amount of protein content.

### Bivariate Analysis
<iframe src="assets/protein_ratings_box_plot.html" width="100%" height="600px"></iframe>

From this box plot, we can observe that a higher protein content means a higher rating. We can also see that there are many outliers, particularly for ratings 4 and 5. 

### Pivot Table
<iframe src="assets/pivot_table.html" width="100%" height="500px"></iframe>

I wanted to explore protein content and rating further by creating a range for protein and observing the average rating for those ranges. Interestingly, these averages are incredibly different from the plot relationship as the highest rating occurs for the protein range 400-500, however, the second highest is for the protein range 0-10. This shows that there may be other factors at play.

## Assessment of Missingness
I believe that the column 'description' is NMAR. This is because a user-provided description might be missing as a result of the user not wanting to provide a full-blown description perhaps due to feeling indifferent to the particular recipe. Moreover, for recipes with more steps or more time taken, these might require longer descriptions which may be difficult to obtain from every user. If we could obtain data about the number of steps and the minutes for the recipes, this can help us make the missingness of the description MAR.

Investigation of Rating and Minutes:
I was curious to see how the missingness of rating was related to minutes and decided to explore that through a permutation test. 

Null Hypothesis: The missingness of ratings does not depend on the minutes of the recipe.  
Alternate Hypothesis: The missingness of ratings does depend on the minutes of the recipe.  
Test Statistic: The difference in means of minutes between the group with missing ratings and the group without missing ratings.  
Significance Level: 0.05  

Outcome:
<iframe src="assets/rating_missing_vs_minutes.html" width="800" height="600" frameborder="0" ></iframe>

The observed test statistic came out to be 51.4 and after running the permutation test, the p-value came out to be 0.119 which means we will fail to reject the null hypothesis. Thus, we can understand that the missingness of rating is not dependent on the minutes the recipe takes.

Investigation of Rating and Protein Proportion:
I wanted to explore the missingness of rating and its relationship with protein proportion as a proportion of calories and decided to explore that through a permutation test. 

Null Hypothesis: The missingness of ratings does not depend on the protein proportion of the recipe.  
Alternate Hypothesis: The missingness of ratings does depend on the protein proportion of the recipe.  
Test Statistic: The difference in means of protein proportions between the group with missing ratings and the group without missing ratings.  
Significance Level: 0.05  

Outcome:
<iframe src="assets/rating_missing_vs_protein_proportion.html" width="800" height="600" frameborder="0" ></iframe>

The observed test statistic came out to be -0.007 and after running the permutation test, the p-value came out to be 0.0 which means we reject the null hypothesis. Thus, we can understand that the missingness of rating is dependent on the protein proportion of a recipe.

## Hypothesis Testing
Since we are exploring the relationship between protein content and ratings, I wanted to perform a permutation test regarding the same. In particular, I want to see if there is a difference in the ratings between high protein recipes and low protein recipes.

Null Hypothesis: There is no statistically significant difference in the ratings between high-protein and low-protein recipes.

Alternative Hypothesis: There is a statistically significant difference in the ratings between high-protein and low-protein recipes.

Test Statistic: Difference in rating means between high protein and low protein.

Significance Level: 0.05

I decided to set a threshold of 20 grams of protein to form groups of low and high. The reason I chose a permutation test is because we can test the actual observed difference from the data against a null distribution from shuffling. Moreover, we have two samples: ratings for high protein foods and ratings for low protein foods and we can use a permutation test to see if they come from the same population. The difference in means is suitable as a test statistic for this because we can observe which group has the higher average rating.

The observed difference in mean ratings was -0.02 which means the ratings for low-protein are higher on average and the p-value was 0.0, and thus we reject the null hypothesis. The higher ratings for low-protein foods may be due to taste.

## Framing a Prediction Problem
I want to predict the calories of a recipe which is a regression problem because the model will predict a continuous numerical value. I decided to choose calories as my response variable because I was interested in the nutritious aspect of this dataset and wanted to build a model that can predict the calories of recipes for people wanting to eat healthier. I decided to use RMSE as the metric to evaluate my model because it is suitable for this regression model and RMSE penalizes large errors more than small errors which is important for this model as large deviations can be misleading.

## Baseline Model
For my baseline model, I decided to use number of words in description and minutes taken as my features to predict the calories of a recipe. Number of words in description is quantitative and minutes is quantitative. I utilized a linear regression for this model and standardized minutes through StandardScaler to ensure that the model is easy to interpret because minutes is continuous whereas number of words in description is discrete. The RMSE is 552 for this model, which is quite high and thus not a very good model. With more features, the model can be better trained.

## Final Model
For my final model, I decided to add rating and protein content on top of the previously mentioned features. Moreover, I decided to log transform protein content due to the skew discovered previously for a better interpretation. These features are good for the data and prediction task because protein content is related to calories because a higher protein content leads to a higher number of calories. Rating is also related to calories because a higher rating might mean higher number of calories due to people enjoying calorically dense foods. Thus, these features allows the model to better predict calories of recipes. I decided to choose RandomForestRegressor because it utilizes multiple decision trees to improve prediction accuracy. I used GridSearchCV to tune the hyperparameters I chose which were n_estimators and max_depth. The best number of estimators is 100 and the best max depth is 3. The RMSE for this is 462 which is a lot less than before meaning that the model has improved due to these changes made. 

## Fairness Analysis
For the fairness analysis, I decided to choose high protein and low protein as my two groups. I decided to split them based on their mean because it allows for better interpretability and can help identify outliers in the data. I chose to evaluate the RMSE for both groups because it is a standardized metric that makes it easy to interpret the results as well as highlights the differences between the model's performance in both groups. I chose the difference in RMSE for my test statistic because it helps us understand the model's performance for both groups.

Null Hypothesis: The model is fair and its RMSE for high and low protein are around the same.  
Alternative Hypothesis: The model is unfair and its RMSE for high and low protein are different.  
Test Statistic: Difference in RMSE between high protein and low protein  
Significance Level: 0.05  

Outcome:
The observed difference in RMSE was 404 which tells us that the model has a higher RMSE score for the high protein group than the low protein group and a p-value of 0.0 and thus, we reject the null hypothesis.
