# Quick Fix or Healthy Mix?
By: Carolina Mondragon and Sanya Chawla
## Introduction
Everyone gets busy at times, and for some, it’s all the time. Unconsciously, that drives our food choices and it leads us to opt for quicker and less healthy meals. In this analysis, we explore how cooking times differ between healthy and unhealthy recipes using the **"Recipes and Ratings"** dataset.

While there are many ways to define "healthy." For the purpose of this project, we define it based on the calorie-to-saturated-fat ratio. In other words, the ratio represents the number of calories per gram of saturated fat in a serving. Meaning, a high ratio generally indicates that the food is lower in saturated fat relative to its total calorie content, while a low ratio suggests the food is higher in saturated fat relative to its total calories."

For example, our first observation is a recipe for `name = brownies in the world best ever` with `calories = 138.4` and `saturated fat = 3.8`. The calorie-to-saturated-fat ratio would be 36.47. This means that for every gram of saturated fat in the food, there are approximately 36.47 calories. This high ratio suggests that the brownies per serving has less saturated fat relative to its calories and thereby "healthier." It is likely that most of the calories come from other macronutrients such as carbohydrates, rather than from saturated fat.

With that being said, the question guiding this exploration is "How do the cooking times of healthy recipes, as defined by a low calorie-to-saturated-fat ratio, compare to those of unhealthy recipes?" This analysis is important because it helps people find a balance between eating healthy and saving time. By understanding how cooking time affects the healthiness of a recipe, individuals can choose meals that fit their schedules without sacrificing nutrition. This way, people can make better food choices that are not only quick but also promote a healthier lifestyle.

Our dataset contains 232,665 rows after merging, where each observation represents a unique recipe. The relevant columns for this question include:

`name`: the name of the recipe.

`minutes`: the time (in minutes) it takes to prepare the recipe.

`n_steps`: the number of steps in the recipe.

`calories`: the calories per serving.

*`saturated_fat`: the grams of saturated fat per serving.

`calories_to_sfat`: the calories to grams of saturated fat ratio.

`rating`: the recipe's rating out of 5.

**The percentage daily value (PDV) for saturated fat is based on a standard 2,000-calorie diet. Typically, the recommended daily value for saturated fat is 20 grams.*

## Exploratory Data Analysis
### Data Cleaning
### Univariate Analysis
### Bivariate Analysis
### Interesting Aggregates
### Imputation
## Framing a Prediction Problem
## Baseline Model
## Final Model