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

For the pivot table, we wanted to explore how the amount of time a recipe takes would affect its ratings and the count of high/low rating values it was given. Thus, we created a pivot table holding the index as `minutes`, the columns as `rating`, the values as `name`, and the aggfunc as `count`. This would allow us to visually produce a table that aggregates more information between columns. An example taken from this table would be if the index value of 2 minutes is analyzed, we would see that there were 2103 5-star ratings for all recipes combined that had a minutes value of 2 minutes. We can also see that by running the line pivot_count_ratings_time.loc[2, 5.0] = 1098. The following shows a snippet of the table: 

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

As previously mentioned, we are investigating *how the cooking times of healthy recipes compare to those of unhealthy recipes.* Our problem is adequate for a **regression model** because the continuous numerical variable we are predicting, cooking time in minutes, can produce exact numerical outcomes. Ultimately, we are using the calories-to-saturated-fat ratio as the predictor variable to predict how long it takes to prepare or cook a recipe because we will have the ratio at the time of prediction as it is independent from cooking time.

The exploration for this problem comes from wanting to examine whether there is any correlation between the healthiness of a recipe and the time it takes to cook. Specifically, we want to determine if healthier recipes (with a higher ratio) take more or less time to prepare compared to unhealthier ones (with a lower ratio). This could provide valuable insights into whether healthier recipes usually require longer time for preparation and time-consuming steps or if there is no significant difference in cooking time between the two categories.

We chose **Mean Squared Error (MSE)** because it effectively accounts for the large variation in the calories-to-saturated-fat ratio, which might lead to significant differences in cooking times. MSE heavily penalizes large errors so the corresponding model will minimize significant deviations in the predictions. This is important for our continuous data because it ensures that extreme values in the ratio don't disproportionately affect model performance, which is something that metrics like R-squared can overlook.

## Baseline Model

Our target variable is `minutes`, and our baseline model consists of a RandomForestRegressor because our target variable is continuous. We found this to be our best choice for our baseline considering it is simple and can give more perspective as a reference for a complex model in our final model. 

Our features include `calories` and `n_steps`. The calories represent the energy content of a recipe and the number of steps can showcase how complex a recipe is, potentially impacting the cooking time. The variables `calories` and `n_steps` are quantitative. We also use the `rating` as a feature, as it may convey the cooking time based on high/low ratings. We used the `Ordinal Encoding` model on the ordinal `rating` feature, so our model takes in both quantitative and ordinal features. 

Finally, in order to evaluate our model, we use **Mean Squared Error** to understand how well our model was able to predict the cooking time. We obtained an MSE value of **3642.02**. We believe our current model is good because we scale the quantitative features and encode the ordinal feature, but there is room for improvement. We have not used our ratio feature and have also not introduced specific hyperparameter features to help decrease the Mean Squared Error further. We are able to generalize to unseen data because we performed a train test split, where we split the data into 80% training and 20% testing, and we then evaluated the Mean Squared Error based on the test data.

```
from sklearn.model_selection import train_test_split
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OrdinalEncoder, StandardScaler
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_squared_error

X = final_df[['calories', 'n_steps', 'rating']]
y = final_df['minutes']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

preprocessor = ColumnTransformer(
    transformers=[
        ('num', StandardScaler(), ['calories', 'n_steps']),
        ('cat', OrdinalEncoder(), ['rating'])
    ])

pipeline = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('regressor', RandomForestRegressor(random_state=42))
])

# train model on the test set
pipeline.fit(X_train, y_train)

# predict on the test set
y_pred = pipeline.predict(X_test)

mse = mean_squared_error(y_test, y_pred)
```

## Final Model

Our final model introduces new engineered features alongside our pre-existing features from our baseline model. Our first engineered feature is our previously engineered feature of the calories-to-saturated-fat-ratio, and our second engineered feature was looking at an interaction of ratings and the number of steps in a recipe. We wanted to see how this feature might depict a pattern in which complex recipes may align with a recipe being unhealthier based on our data, where the recipes with the most number of steps were conventionally unhealthy. Ratings multiplied by the number of steps also allows us to look at a more nuanced version of the ratings from the baseline model, where higher ratings could imply that complex foods might be more enjoyable even though they may take more time. 

We kept the Column Transformer similar from the baseline model, while adding our two new engineered features as part of the Standard Scaler transformation since both new features are quantitative. We also kept the original ordinal encoding on the rating feature as well from the baseline model. In order to ensure our train-test split data was the same from the baseline model as well in order to prevent the dataset from changing between models, we kept our random state the same. 

While continuing with our use of the RandomForestRegressor Model, we added n_estimators and max_depth as part of our hyperparameter tuning through the use of GridSearchCV. We used n_estimators because more trees improve accuracy by averaging out predictions and reducing variance. The focus is to find the efficiency sweet spot where there’s not too few trees where we risk underfitting and there’s not too many where we increase training time. We also used max_depth because it controls the complexity of each tree. The focus is to find the balance where the trees capture enough meaningful feature interactions, although they can’t be too deep to risk overfitting or too shallow to risk underfitting.

We wanted to capture as much from our data in our folds as well through our hyperparameter values, so we chose bigger numbers such as 200 for n_estimators. Finally, we performed GridSearchCV on our pipeline and parameter grid and explored the **MSE of our final model**, which gave us a final value of **2528.32**. As shown from our baseline to final model, our MSE decreased by 1101.55. The engineered features allowed us to explore new nonlinear relationships that our baseline model did otherwise not do, and the GridSearchCV uses cross-validation to ensure our model is trained on various splits of the training data for improved generalization and prediction on the testing data. 

```
from sklearn.model_selection import GridSearchCV

final_df = final_df.copy()

# create engineered features
final_df['calories_to_sfat'] # already engineered
final_df['rating_x_steps'] = final_df['rating'] * final_df['n_steps']

features = ['calories', 'saturated_fat', 'n_steps', 'rating', 'calories_to_sfat', 'rating_x_steps']
X = final_df[features]
y = final_df['minutes']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

preprocessor = ColumnTransformer(
    transformers=[
        ('num', StandardScaler(), ['calories', 'saturated_fat', 'n_steps', 'calories_to_sfat', 'rating_x_steps']),
        ('cat', OrdinalEncoder(), ['rating'])
    ]
)

pipeline = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('regressor', RandomForestRegressor(random_state=42))
])

param_grid = {
    'regressor__n_estimators': [100, 200],
    'regressor__max_depth': [10, 20, None],
}

grid_search = GridSearchCV(pipeline, param_grid, cv=5, scoring='neg_mean_squared_error', n_jobs=-1)
grid_search.fit(X_train, y_train)

best_model = grid_search.best_estimator_
y_pred = best_model.predict(X_test)
mse = mean_squared_error(y_test, y_pred)
```