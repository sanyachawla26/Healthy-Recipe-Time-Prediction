# Quick Fix or Healthy Mix?
By: Carolina Mondragon and Sanya Chawla
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

For the pivot table, we wanted to explore how the amount of time a recipe takes would affect its ratings and the count of high/low rating values it was given. Thus, we created a pivot table holding the index as `minutes`, the columns as `rating`, the values as `name`, and the aggfunc as `count`. This would allow us to visually produce a table that aggregates more information between columns. An example taken from this table would be if the index value of 2 minutes is analyzed, we would see that there were 2103 5-star ratings for all recipes combined that had a minutes value of 2 minutes. We can also see that by running the line pivot_count_ratings_time.loc[2, 5.0] = 2103. The following shows a snippet of the table: 

| rating | 1.0  | 2.0  | 3.0  | 4.0  | 5.0  |
|--------|------|------|------|------|------|
| minutes |      |      |      |      |      |
| 1      | 7    | 1    | 13   | 85   | 594  |
| 2      | 15   | 7    | 52   | 442  | 2103 |
| 3      | 10   | 8    | 41   | 232  | 1125 |
| 4      | 21   | 4    | 29   | 178  | 615  |
| 5      | 111  | 87   | 262  | 1635 | 8551 |

### Imputation

We decided to **not impute** any missing values. The `rating` column has missing values that were originally marked with 0 but we modified to NaN instead, as explained previously. We are deciding to not use any imputation techniques, such as mean imputation, to change the NaNs into a value. The `rating` column is irrelevant to our analysis and modeling. Therefore, it would be irrelevant to perform any imputation.

## Framing a Prediction Problem
## Baseline Model
## Final Model