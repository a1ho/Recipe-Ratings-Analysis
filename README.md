# Recipe Ratings Analysis 🍱

**Authors**: Ashley Ho & Mizuho Fukuda
<br/>

**Course**: DSC80 at UCSD
<br/>

**Website**: <https://a1ho.github.io/Recipe-Ratings-Analysis/>

## Introduction 
In the vast realm of culinary ... ; We explore this central question:
> Are recipes that are lower in saturated fat content more popular among people than recipes that are higher in saturated fat content?
>

This research question seeks to explore whether individuals are concious about making healthy food choices when deciding on which recipes to try out. In essence, we are investigating whether recipes containing lower saturated fat contents are more popular amongst the general public than recipes higher in saturated fat. This research question may be of interest to people studying dietary habits and could provide insights into the factors influencing food choices in the ever-evolving landscape of nutrition and wellness.
<br/>

The dataset that we explore in this project contains recipes and ratings from [food.com](https://https://www.food.com/) that have been posted since 2008, with 83,782 different observations in the `recipes` dataset and 731,927 different reviews/ratings the `interacions` dataset. The columns from the `recipes` dataset that are of interest to us throughout this project:

| Column      | Description |
| ----------- | ----------- |
| `'id'`      | unique ID of recipe |
| `'name'`    | name of recipe |
| `'minutes'` | reported time each recipe takes to make, in minutes |
| `'nutrition'`| nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'` | number of steps to make recipe | 
| `'n_ingredients'`| number of ingredients to make recipe | 

Also, the columns from the `interactions` dataset that are of interest to us throughout this project:

| Column      | Description |
| ----------- | ----------- |
| `'recipe_id'`| unique ID of recipe |
| `'rating'`  | rating (out of 5) of recipe given a reviewer |

## Cleaning and EDA

### **Cleaning**
#### 1. Merge DataFrames
The `recipes` DataFrame contains one row per recipe, and the `interactions` dataframe contains one row per review of a recipe. The `id` column in `recipes` and the `recipe_id` columns in `interactions` are common columns, and thus we merge the two DataFrames using a left merge on `recipes`. This results in a DataFrame `merged` with one row per review of every recipe that appears in `recipes`.
#### 2. Add Average Ratings
From `merged` we calculate the average `rating` for each recipe in the DataFrame. Before doing so, we replace all ratings of 0 with `np.nan` since a rating of 0 may acutually mean that the rating is missing if a reviewer forget to add a rating at the end of their review. Thus, we replace with `np.nan` to ensure that when calculating the average rating, we do not factor in missing ratings as a rating of 0. Then, we add these average ratings as a new column `mean_rating` to the `recipes` DataFrame.
#### 3. Add Number of Reviews
For our research question, we focus on the 'popularity' of a recipe. For the purposes of this project, we categorize a recipe's popularity by looking at how many reviews it receives, since we reason that recipes with more number of reviews have reached a wider audience, and hence are more popular. We used the `merged` DataFrame to calculate the number of reviews a recipe receives and add these values as a new column `n_reviews` to the `recipes` DataFrame.
#### 4. Seperate `'nutrition'` Column 
After checking the data types of the columns, we notice that the `'nutrition'` column, which is present in both `recipes` and `merged`, actually contains strings that are formatted as lists instead of actual lists. For both DataFrames, we separate the values in the `'nutrition'` column into seperate these seperate columns: `'calories'`, `total_fat`, `sugar`, `sodium`, `'protein'`, `'saturated_fat'`, and `'carbohydrates'`. We note that all these values are in PDV units, or percentage of daily value.
#### 5. Drop and Sort Columns
For the purposes of our analysis, we drop the `'...'` columns from `recipes` and the `'...'` columns from `merged`. We also sort the `recipes` DataFrame by `id` for organizational purposes.
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


### **EDA**
#### 1. Univariate Distributions

Here we plotted the distribution of the `minutes` column. Notice that we temporarily dropped all minutes greater than 600 to have a better view of the plot. This did not affect our analysis since only about 1% of the data has minutes great than 600.
<iframe src="assets/minutes_hist.html" width=800 height=600 frameBorder=0></iframe>






## Assessment of Missingness


## Hypothesis Testing