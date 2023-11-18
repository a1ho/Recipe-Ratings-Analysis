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
For our research question, we focus on the popularity of a recipe. For the purposes of this project, we categorize a recipe's popularity by looking at how many reviews it receives, since we reason that recipes with more number of reviews have reached a wider audience, and hence are more popular. We used the `merged` DataFrame to calculate the number of reviews a recipe receives and add these values as a new column `n_reviews` to the `recipes` DataFrame.
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
Here we plotted the distribution of the `minutes` column. Notice that we temporarily dropped all minutes greater than 600 to have a better view of the plot. When exploring the `minutes` column, we found a recipe with more than 1 million minutes of cooking time titled _'how to preserve a husband'_. We think that many of the extreme outliers are likely caused by fake recipes like this one. Dropping these outliers temporarily did not affect our analysis here since only about 1% of the data has minutes great than 600. We see that most recipes have a cooking time of under 60 minutes, with 30 minutes being the most common. 
<iframe src="assets/minutes_hist.html" width=800 height=600 frameBorder=0></iframe>

We looked at the distribution of the `mean_rating` column. It shows that the reviews on `food.com` are overwhemingly positive as most of the mean ratings are above 4 and more than half of the mean ratings are 5.
<iframe src="assets/mean_rating_hist.html" width=800 height=600 frameBorder=0></iframe>

#### 2. Bivariate Distributions
The scatter plot below shows the `mean_rating` column vs. the `calories` column. Due to the extremely negatively skewed mean ratings, we cannot conclude any meaningful correlation between `mean_rating` and `calories`. We also noticed some outliers in `calories`, which may also be a result of fake recipes like the one mentioned in the above section so we temporarily dropped rows with calories above  20,000 for a better view of the relationship.
<iframe src="assets/rating_cal_scatter.html" width=800 height=600 frameBorder=0></iframe>

This second scatterplot shows the `calories` column vs. the `n_steps` column. As with the previous plot, we ignoed rows with calories above 20,000. Contrary to our intuition, recipes with more steps tend to have lower calories. This could still be due to the impact of outliers as there are many recipes with abnormally large calories.
<iframe src="assets/step_cal_scatter.html" width=800 height=600 frameBorder=0></iframe>

Here we see the conditional distribution of `mean_rating` for higher vs. lower calories. Again, for the sake of the analysis, we ignore all rows with calories higher than 20,000. We define "low calories" as calories lower than the median and "high calories" as higher than the median. We see that while the mean rating in both categories are still overwhemingly positive, lower calories food seem to have a larger varience in mean rating. This can be seen in the isolated blue bars around mean ratings of 1 - 3. We hypothesize that since the 'fake' recipes tend to have extreme calories, the recipes with lower calories are probably more legitimate, thus, having more meaningful ratings.

_Note: the y-axis is in log scale for better visualization._
<iframe src="assets/conditional_logcount.html" width=800 height=600 frameBorder=0></iframe>

#### 3. Interesting Aggregates
The histogram below shows the trend of average `minutes` when grouped by `n_ingredients`. For most of the plot, we can clearly see a positive correlation between average minutes requried for the recipe and the number of ingredients. The trend does not continue for number of ingredients higher than 28, however. This could be simply due to fewer recipes having more than 28 ingredients, thus, skewing the mean minutes for those recipes.
<iframe src="assets/ingredients_minutes_line.html" width=800 height=600 frameBorder=0></iframe>

## Assessment of Missingness
We used the `merged` DataFrame for the entirety of this section. Here is the count of missingness in the columns of `merged`:
....
### **NMAR Analysis**
We believe that the `'description'` column is NMAR because perhaps certain recipes do not have much to descibe, and therefore are left blank. For example, recipes for foods such as cookies or hot chocolate may not require much of an explanation, and thus their recipes are note accompanied by a description. We can collect data on how common each food item is, since we believe that more popular/well-known dishes may not need a description while more uncommon foods, like those that are specific to a culture, may be more likely to require a description.

### **Missingness Dependency**
From the missingness summary above, we notice that the `'rating'` column has a substantial amount of missing values as compared to the `'description'` and `'review'` columns. In this section, we conduct two separate permutation tests to analyze the dependence of the `'rating'` column's missingness on the `'saturated_fat'` column and the `'minutes'` column.
#### 1. Rating and Saturated Fat
For this permutation test we analyze if there is dependency between the missingness of the ratings and the saturated fat content. We use the difference in group means as our test statistic, as `'saturated_fat'` is numerical. For this test, we have the following:
- **Null Hypothesis**: The saturated fat for recipes with missing ratings and recipes with non-missing ratings are drawn from the same distribution (i.e. group_mean(missing) - group_mean(non-missing) = 0).
- **Alternate Hypothesis**: The mean saturated fat for recipes with missing ratings is greater than that of the recipes with non-missing ratings (i.e. group_mean(missing) - group_mean(non-missing) > 0).

Here is the plot of the results from our permutation test using 2000 permutations:
<iframe src="assets/missing_rating_fat_perm.html" width=800 height=600 frameBorder=0></iframe>

We get a p-value of 0.0, which is lower than the significance level 0.05, and therefore we reject the null hypothesis. As such, we can conclude that the missingness in the `'rating'` column is dependent on the `'saturated_fat'` column. In other words, **we conclude that ratings is MAR, conditional on saturated fat**.

#### 2. Rating and Minutes
For this permutation test we analyze if there is dependency between the missingness of the ratings and the minutes each recipe takes to make. We use the difference in group means as our test statistic, as `'minutes'` is numerical. For this test, we have the following:
- **Null Hypothesis**: The minutes for recipes with missing ratings and recipes with non-missing ratings are drawn from the same distribution (i.e. group_mean(missing) - group_mean(non-missing) = 0).
- **Alternate Hypothesis**: The mean minutes for recipes with missing ratings is greater than that of the recipes with non-missing ratings (i.e. group_mean(missing) - group_mean(non-missing) > 0).

Here is the plot of the results from our permutation test using 2000 permutations:
<iframe src="assets/missing_rating_min_perm.html" width=800 height=600 frameBorder=0></iframe>

We get a p-value of 0.1205, which is greater than the significance level 0.05, and therefore we fail to reject the null hypothesis. As such, we cannot conclude that the missingness in the `'rating'` column is dependent on the `'minutes'` column. In other words, **we conclude that ratings is MCAR with respect to the minutes**.


## Hypothesis Testing