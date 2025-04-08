# Quick Fix or Healthy Mix?
## Introduction
Everyone gets busy at times, and for some, it’s all the time. Unconsciously, that drives our food choices, often leading us to opt for quicker, less healthy meals. In this analysis, we explore how cooking times differ between healthy and unhealthy recipes. While there are many ways to define "healthy," for this project, we define it based on the calorie-to-fat ratio—the lower the ratio, the healthier the recipe, and vice versa. The question guiding this investigation is: How do the cooking times of healthy recipes, as defined by a low calorie-to-fat ratio, compare to those of unhealthy recipes? This analysis is important because understanding the relationship between recipe health and cooking time can help individuals make more informed food choices, potentially saving time while maintaining healthier diets.

The dataset contains 232,665 rows, each representing a unique recipe. The relevant columns for this question include:

**Name**: The name of the recipe.

**Minutes**: The time (in minutes) it takes to prepare the recipe.

**n_steps**: The number of steps required in the recipe.

**Calories**: The calories per serving.

**Total Fat**: The percentage of the daily value for fat per serving.

**Rating**: The recipe's rating out of 5.

Additionally, a new column, fat_calorie_ratio, has been created, which calculates the ratio of total fat to calories for each recipe, serving as an indicator of the recipe's healthiness. This column will help categorize the recipes into "healthy" or "unhealthy" based on their fat-to-calorie ratio.
## Exploratory Data Analysis
### Data Cleaning
### Univariate Analysis
### Bivariate Analysis
### Interesting Aggregates
### Imputation
## Framing a Prediction Problem
## Baseline Model
## Final Model