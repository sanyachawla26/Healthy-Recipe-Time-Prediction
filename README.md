# Quick Fix or Healthy Mix?
By: Carolina Mondragon (cmondrag@umich.edu) and Sanya Chawla (csanya@umich.edu)
## Introduction
Everyone gets busy at times, and for some, it’s all the time. Unconsciously, that drives our food choices and it leads us to opt for quicker and less healthy meals. In this analysis, we explore how cooking times differ between healthy and unhealthy recipes using the **"Recipes and Ratings"** dataset.

While there are many ways to define "healthy," for this project we define it based on the calorie-to-saturated-fat ratio. We focus on saturated fat instead of total fat because saturated fats in foods like vegetable oils are linked to higher health risks and are typically associated with being "unhealthy." On the other hand, unsaturated fats like those in avocados are considered "healthier."

In other words, the ratio represents the number of calories per gram of saturated fat in a serving. Meaning, a high ratio generally indicates that the food is lower in saturated fat relative to its total calorie content which is "healthier," while a low ratio suggests the food is higher in saturated fat relative to its total calories which is "unhealthier."

For example, our first observation is a recipe for `name = brownies in the world best ever` with `calories = 138.4` and `saturated fat = 3.8`. The calorie-to-saturated-fat ratio would be 36. This means that for every gram of saturated fat in the food, there are approximately 36 calories. This high ratio suggests that the brownies per serving has less saturated fat relative to its calories and thereby "healthier." It is likely that most of the calories come from other macronutrients such as carbohydrates, rather than from saturated fat.

With that being said, the question guiding this exploration is **"how do the cooking times of healthy recipes compare to those of unhealthy recipes?"** This analysis is important because it helps people find a balance between eating healthy and saving time despite the rush of everyday living. By understanding how cooking time affects the healthiness of a recipe, people can choose meals that fit their schedules without sacrificing nutrition. This way, we can all make better food choices that are not only quick but also promote a healthier lifestyle for an increased quality of life.

Our dataset contains **212,894 rows** after merging, where each observation represents a unique recipe. The relevant columns for this question include:

`name`: the name of the recipe.

`minutes`: the time it takes to prepare the recipe.

`n_steps`: the number of steps in the recipe.

`calories`: the calories per serving.

*`saturated_fat`: the grams of saturated fat per serving.

`calories_to_sfat`: the calories to grams of saturated fat ratio.

`rating`: the recipe's rating out of 5.

**The percentage daily value (PDV) for saturated fat is based on a standard 2,000-calorie diet. Typically, the recommended daily value for saturated fat is 20 grams.*

## Exploratory Data Analysis
### Data Cleaning

In order to conduct our data cleaning, we started off by imputing any missing values in our “interactions” dataset and “recipes” dataset with NaN values. Next, we converted the `date` column in the “interactions” dataframe to datetime values, following the same method for the `submitted` column in the “recipes” dataframe. Our next step was to split the nutrition column into the various elements it holds, including `calories`, `total_fat`, `sugar`, `sodium`, `protein`, `saturated_fat`, and `carbohydrates`. We also decided to drop the original `nutrition` column after because it would have been redundant to keep. 

Next, we calculated the saturated fat values in units of grams from the original percentage daily value provided in the dataframe. We use this transformed column later while adding new columns to benefit and target towards our own analysis and prediction problem we are attempting to answer. We then merged the two dataframes and within the rating column, we imputed any values of 0 with NaN values. Within the original dataframe, a rating of 0  means that the recipe has not been rated at all. This is misleading since it could be interpretted as someone rating the recipe with 0 to express that the recipe was bad. Therefore, it cannot be used in the data analysis we have since keeping any 0 values in the `ratings` columns can largely affect any calculations involving the columns, such as average ratings because we would see a drop in the average. Afterwards, we created a threshold of 1000 `minutes` and 5000 `calories` to avoid and drop any rare rows with outlier values of minutes and calories that would be unreasonable and skew our data heavily. 

Adding new columns, we created an average ratings column as mentioned briefly above of the average ratings of all the recipes using the non-NaN values. We also added a new column in which we calculated the ratio of the calorie values to the saturated fat values for each row, and we used this ratio in order to identify what recipes can be considered more unhealthy or more healthy. Within this column, we dropped any rows in which the ratio was NaN. For more comprehensive analysis, we chose to use the median of the calories-to-saturated fat ratio as a threshold to determine what would be considered a high ratio and low ratio. Using these values, we created a new column where each recipe was given a “high” value or “low” value relatively depending on whether it was above or below the median ratio threshold that we mentioned before. We then dropped the columns we considered to be irrelevant, leaving columns such as `n_steps`, `calories`, `saturated_fat`, etc. 

Finally, we would like to clarify that there are **two versions** of our dataframes. The first version `full_df` is the fully cleaned dataframe, using the methods mentioned earlier. The second version `final_df` is a copy of `full_df`, but with all recipes containing an "inf" calories-to-saturated-fat ratio removed. For the purposes of data cleaning and exploration, we have decided to keep the recipes with an "inf" calories-to-saturated-fat ratio in `full_df` because their presence is intuitive in the sense that dividing a recipe with many calories by 0 grams of saturated fat will result in an infinite value. From an interpretive standpoint, this very high ratio can indicate a very healthy recipe. However for visualization and modeling purposes, we will not use `full_df` with these "inf" values, since creating models or visualizations based on infinite values would not be appropriate. Therefore, we will proceed with `final_df` for the analysis moving forward.

| name                                 |   minutes |   n_steps |   calories |   saturated_fat |   calories_to_sfat |   rating | ratio_category   |
|:-------------------------------------|----------:|----------:|-----------:|----------------:|-------------------:|---------:|:-----------------|
| 1 brownies in the world    best ever |        40 |        10 |      138.4 |             3.8 |            36.4211 |        4 | Low Ratio        |
| 1 in canada chocolate chip cookies   |        45 |        12 |      595.1 |            10.2 |            58.3431 |        5 | Low Ratio        |
| 412 broccoli casserole               |        40 |         6 |      194.8 |             7.2 |            27.0556 |        5 | Low Ratio        |
| 412 broccoli casserole               |        40 |         6 |      194.8 |             7.2 |            27.0556 |        5 | Low Ratio        |
| 412 broccoli casserole               |        40 |         6 |      194.8 |             7.2 |            27.0556 |        5 | Low Ratio        |

### Univariate Analysis

 <iframe
 src="plotly/uni.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

 In our pie chart, we see an almost even split between the amount of recipes that were categorized as “Low Ratio” and “High Ratio.” There’s a slightly larger quantity of “Low Ratio” recipes which we are defining as being unhealthy recipes. The difference is around 10% which isn’t much but worth noting.

### Bivariate Analysis

 <iframe
 src="plotly/biv.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

 In our scatter plot, we showcased a relationship between the calories-to-saturated-fat ratio and the minutes. We see a trend in which the majority of lower ratios took less time, but some lower ratios also took more time. As the ratios increased, we see all of these specific recipes had relatively low minute values.

### Interesting Aggregates

For the pivot table, we wanted to explore how the amount of time a recipe takes would affect its ratings and the count of high/low rating values it was given. Thus, we created a pivot table holding the index as `minutes`, the columns as `rating`, the values as `name`, and the aggfunc as `count`. This would allow us to visually produce a table that aggregates more information between columns. An example taken from this table would be if the index value of 2 minutes is analyzed, we would see that there were 1098 5-star ratings for all recipes combined that had a minutes value of 2 minutes. We can also see that by running the line pivot_count_ratings_time.loc[2, 5.0] = 1098. The following shows a snippet of the table: 

|   rating  |   1.0 |   2.0 |   3.0 |   4.0 |   5.0 |
|-----------|-------|-------|-------|-------|-------|
|   minutes |       |       |       |       |       |
|         1 |     7 |     0 |     6 |    41 |   266 |
|         2 |     8 |     5 |    26 |   237 |  1098 |
|         3 |     5 |     7 |    26 |   138 |   714 |
|         4 |    20 |     4 |    27 |   145 |   519 |
|         5 |    92 |    66 |   199 |  1257 |  6136 |


### Imputation

We decided to use **probabilistic imputation** on the `rating` column because rating is an integral part of our baseline model. We wanted to ensure we could capture all the data from that feature. Therefore, we chose to use probabilistic imputation as our method of imputation because it imposes the least bias in how the imputed values are determined because random values are pulled from the sample of prexisting rating. We avoided mean imputation as we felt the average rating would be on the higher end with the increased amount of 5.0 ratings in the data. Below is a plot of the data before and after probabilistic imputation. As observed, there isn't much of a noticable difference before vs. after imputation but **we will proceed with imputation to ensure consistency across the data**.

 <iframe
 src="plotly/fig_before.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

 <iframe
 src="plotly/fig_after.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

## Framing a Prediction Problem

As previously mentioned, we are investigating *how the cooking times of healthy recipes compare to those of unhealthy recipes.* Our problem is adequate for a **regression model** because the continuous numerical **variable we are predicting, cooking time in minutes**, can produce exact numerical outcomes.

The exploration for this problem comes from wanting to examine whether there is any correlation between the healthiness of a recipe and the time it takes to cook. Specifically, we want to determine if healthier recipes (with a higher ratio) take more or less time to prepare compared to unhealthier ones (with a lower ratio). This could provide valuable insights into whether healthier recipes usually require longer time for preparation and time-consuming steps or if there is no significant difference in cooking time between the two categories.

We chose **Mean Squared Error (MSE)** because it effectively accounts for large variations in the data, which might lead to significant differences in cooking time. MSE heavily penalizes large errors which ensures that the model minimizes significant deviations. This is important for our continuous data because it ensures that extreme values in the ratio don't disproportionately affect model performance, which is something that metrics like R-squared can overlook.

## Baseline Model

Our target variable is `minutes`, and our baseline model consists of a `RandomForestRegressor()` because our target variable is continuous. We found this to be the best choice for our baseline since it's simple and gives us a good foundation for building a more complex model later on.

Our features include `calories` and `n_steps` and `rating`. The variables `calories` and `n_steps` are quantitative while `rating` is qualitative ordinal. Furthermore, we used `OrdinalEncoding()` on `rating` to account for it being the only categorical variable in our baseline.

We use **Mean Squared Error** to understand how well our model was able to predict the cooking time. We obtained an MSE value of **3640.66**. We believe our current model is good because we scaled the quantitative features and encoded the ordinal feature, but there is room for improvement. We have not used the `calories_to_sfat` feature and have also not introduced specific hyperparameter features to help decrease the Mean Squared Error further. 

Furthermore, to ensure the model generalizes well to unseen data, we performed a train-test split. This split divides the dataset into 80% for training and 20% for testing which allows us to evaluate the model’s performance on data it hasn't seen before.

## Final Model

Our final model introduces new engineered features alongside our pre-existing features from our baseline model. Our first engineered feature is the `calories_to_sfat`, which was made previously. Our second engineered feature was looking at an interaction of ratings and the number of steps in a recipe. The driving force behind creating this feature was to explore the relationship between complex recipes and their "unhealthiness," and vice versa. Thus, using a variable where `rating` is multiplied by `n_steps`, we are able to look at a more nuanced version of the ratings from the baseline model. 

For our modeling algorithm, we kept the `ColumnTransformer()` from the baseline model. However, we added our two new engineered features as part of the `StandardScaler()` transformation since both new features are quantitative. We also kept the original `OrdinalEncoding()` on `rating`, also from the baseline model. In order to ensure our train-test split data was consistent with that of the baseline model, we kept our `random_state` the same. 

For our hyperparameter tuning, we continued with the `RandomForestRegressor()` model and added `n_estimators` and `max_depth` through the use of `GridSearchCV()`. By using `n_estimators`, we are able to improve accuracy by averaging out predictions and reducing variance while keeping in mind that too few trees can lead to underfitting and too many trees can lead to an increase in training time. By using `max_depth`, we are able to control the complexity of each tree to guarantee there are enough trees to capture meaningful feature interactions. These trees shall not be too deep to risk overfitting or too shallow to risk underfitting.

For our performance analysis, we wanted to capture sufficient data in our folds and hyperparameter values by selecting larger values such as 200 for `n_estimators`. We performed `GridSearchCV()` on our pipeline and parameter grid and explored the **MSE of our final model** which was **2538.86**. As shown from our baseline to final model, our **MSE decreased by 1101.80**. The engineered features allowed us to explore new nonlinear relationships that our baseline model did not. Additionally, `GridSearchCV()` used cross-validation to ensure our model was trained on various splits of the training data which improved its generalization and prediction on the testing data.