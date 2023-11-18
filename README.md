# Recipe Ratings Analysis 🍱

Authors: Ashley Ho & Mizuho Fukuda
<br/>

Course: DSC80 Project 3!
<br/>

Website: https://a1ho.github.io/Recipe-Ratings-Analysis/

## Introduction 
In the vast realm of culinary ... ; We explore this central question:
> Do people tend to try recipes with lower saturated fat content more often than recipes with higher saturated fat content?
>

This research question seeks to explore whether individuals are concious about making healthy food choices when deciding on which recipes to try out. In essence, we are investigating whether recipes containing lower saturated fat contents are more popular amongst the general public than recipes higher in saturated fat. This research question may be of interest to people studying dietary habits and could provide insights into the factors influencing food choices in the ever-evolving landscape of nutrition and wellness.
<br/>

The dataset that we explore in this project contains recipes and ratings from [food.com](https://https://www.food.com/) that have been posted since 2008, with 83,782 different observations in the `raw_recipes` dataset and 731,927 different reviews/ratings the `raw_interacions` dataset. The columns from the `raw_recipes` dataset that are of interest to us throughout this project:

| Column      | Description |
| ----------- | ----------- |
| `'id'`      | unique ID of recipe |
| `'name'`    | name of recipe |
| `'minutes'` | reported time each recipe takes to make, in minutes |
| `'nutrition'`| nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'` | number of steps to make recipe | 
| `'n_ingredients'`| number of ingredients to make recipe | 

Also, the columns from the `raw_interactions` dataset that are of interest to us throughout this project:

| Column      | Description |
| ----------- | ----------- |
| `'recipe_id'`| unique ID of recipe |
| `'rating'`  | rating (out of 5) of recipe given a reviewer |

## Cleaning and EDA


#### 6. Cleaned DataFrames
Here are the first 5 rows of the cleaned `recipes` DataFrame:

|     id | name                                  |   minutes |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbohydrates |   n_ingredients |   n_steps |   mean_rating |   n_reviews |
|-------:|:--------------------------------------|----------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|----------------:|----------:|--------------:|------------:|
| 275022 | impossible macaroni and cheese pie    |        50 |      386.1 |          34 |       7 |       24 |        41 |              62 |               8 |               7 |        11 |             3 |           3 |
| 275024 | impossible rhubarb pie                |        55 |      377.1 |          18 |     208 |       13 |        13 |              30 |              20 |               8 |         6 |             3 |           1 |
| 275026 | impossible seafood pie                |        45 |      326.6 |          30 |      12 |       27 |        37 |              51 |               5 |               9 |         7 |             3 |           2 |
| 275030 | paula deen s caramel apple cheesecake |        45 |      577.7 |          53 |     149 |       19 |        14 |              67 |              21 |               9 |        11 |             5 |          10 |
| 275032 | midori poached pears                  |        25 |      386.9 |           0 |     347 |        0 |         1 |               0 |              33 |               9 |         8 |             5 |           1 |



Here are the first 5 rows of the cleaned `merged` DataFrame:

|     id | name                                 | description             |   minutes |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbohydrates |   n_ingredients |   n_steps |   rating | review                  |
|-------:|:-------------------------------------|:------------------------|----------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|----------------:|----------:|---------:|:------------------------|
| 333281 | 1 brownies in the world    best ever | these are the most; ... |        40 |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 |               9 |        10 |        4 | These were pretty go... |
| 453467 | 1 in canada chocolate chip cookies   | this is the recipe t... |        45 |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 |              11 |        12 |        5 | Originally I was gon... |
| 306168 | 412 broccoli casserole               | since there are alre... |        40 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |               9 |         6 |        5 | This was one of the ... |
| 306168 | 412 broccoli casserole               | since there are alre... |        40 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |               9 |         6 |        5 | I made this for my s... |
| 306168 | 412 broccoli casserole               | since there are alre... |        40 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |               9 |         6 |        5 | Loved this.  Be sure... |



## Assessment of Missingness


## Hypothesis Testing