# Quick Fix or Healthy Mix?
By: Carolina Mondragon and Sanya Chawla
## Introduction
Everyone gets busy at times, and for some, it’s all the time. Unconsciously, that drives our food choices and it leads us to opt for quicker and less healthy meals. In this analysis, we explore how cooking times differ between healthy and unhealthy recipes using the **"Recipes and Ratings"** dataset.

While there are many ways to define "healthy," for this project we define it based on the calorie-to-saturated-fat ratio. We focus on saturated fat instead of total fat because saturated fats in foods like vegetable oils are linked to higher health risks and are typically associated with being "unhealthy." On the other hand, unsaturated fats like those in avocados are considered "healthier."

In other words, the ratio represents the number of calories per gram of saturated fat in a serving. Meaning, a high ratio generally indicates that the food is lower in saturated fat relative to its total calorie content, while a low ratio suggests the food is higher in saturated fat relative to its total calories.

For example, our first observation is a recipe for `name = brownies in the world best ever` with `calories = 138.4` and `saturated fat = 3.8`. The calorie-to-saturated-fat ratio would be 36. This means that for every gram of saturated fat in the food, there are approximately 36 calories. This high ratio suggests that the brownies per serving has less saturated fat relative to its calories and thereby "healthier." It is likely that most of the calories come from other macronutrients such as carbohydrates, rather than from saturated fat.

With that being said, the question guiding this exploration is **"how do the cooking times of healthy recipes compare to those of unhealthy recipes?"** This analysis is important because it helps people find a balance between eating healthy and saving time despite the rush of everyday living. By understanding how cooking time affects the healthiness of a recipe, people can choose meals that fit their schedules without sacrificing nutrition. This way, we can all make better food choices that are not only quick but also promote a healthier lifestyle for an increased quality of life.

Our dataset contains **232,566 rows** after merging, where each observation represents a unique recipe. The relevant columns for this question include:

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
### Univariate Analysis
### Bivariate Analysis
### Interesting Aggregates
### Imputation
## Framing a Prediction Problem
## Baseline Model
## Final Model