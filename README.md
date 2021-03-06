# ScrapingOLX
This project is an exercise project to improve my skill to scrape data from e-commerce / website. After get the data, I will try too to do data cleaning, EDA, and machine learning to predict the price.

# Web Scraping

### The Plan
For this web scraping, I will try to extract mercedes benz used car data (specification and price) from https://olx.co.id. I will use beautifulsoup as the tool to extract olx html. This web scraping is multipage web scraping. From the "mercedes benz" search result pages, I will extract the title, price, and link to item specifications. After I get them, I will use the link from each of extracted datas to direct me to item / car detail pages to get the car specifications like year, km, varian, model, etc.

### About The Data
#### Mercedes Benz Search Result Page
Form the result pages, there is noted that olx has 1978 mercedes benz used car data. At the result page, we could see that each page has 20 items / cars. So, olx may has around 99 pages of mercedes benz used car. Each of item has a link that will direct us to item page that has detail / car specification. If the items do not have title/price/link they will not be extracted. The following below is olx mercedes benz search result page :
<center><img src="https://github.com/agunggnug/PricePredict_ScrapingOLX/blob/master/Pictures/Screen%20Shot%202020-06-03%20at%2009.48.54.png?raw=true" alt="" width="950" height="550"></center>

#### Mercedes Benz Item Page
At the item page we could find detail which means car specification. From this specifications, I will not take all of them. I only need to take some specifications which are 'Merek','Varian', 'Model', 'Tahun', 'Jarak tempuh', 'Tipe bahan bakar', 'Warna', 'Transmisi', and 'Kapasitas mesin'. If the item doesn't have all of that specifications, that item will not be extracted. The following below is olx car detail or specification page :
<center><img src="https://github.com/agunggnug/PricePredict_ScrapingOLX/blob/master/Pictures/Screen%20Shot%202020-06-03%20at%2009.51.11.png?raw=true" alt="" width="950" height="550"></center>

#### The Result
After doing web scraping processes, it turns out that I need more than 100 loops to get existing data and more than one scraping process. I have extracted around 1440 items from 1978 items (~75%) which meet with our requirements. The web scraping process is availabe <a href="https://github.com/agunggnug/PricePredict_ScrapingOLX/blob/master/Scraping%20OLX.ipynb">here</a>. So, the next process is data cleaning and exploratory data analysis (EDA). The following below is the head of the dataset :
<center><img src="https://github.com/agunggnug/PricePredict_ScrapingOLX/blob/master/Pictures/Screen%20Shot%202020-06-04%20at%2015.52.43.png?raw=true" alt="" width="950" height="350"></center>

#### Future Improvement
For future improvement, it is possible to add more data to dataset. The data that will be added could be others brand data (like bmw, audi, etc) from olx or mercedes benz data that extracted from other website.


# Data Cleaning and EDA
For data cleaning, I check the dataset that that I got from web scraping, looking for unique value, data type, unique sample, and other information about the dataset. Change price type and remove the currency sign, change range of "Jarak Tempuh", remove some unnecessary columns, and others process for each features. After finishing data cleaning, to explore the data, I have checked the some distribution, correlation some features with price, correlation between features, and etc. This processes are very helpfull for me to get some insight from the dataset.

# Modelling
Since modelling is not the main objective of this project (Web Scraping), I only use some algorithms which are logistic regression, decission tree regressor, random forest regressor, and LGBM Regressor. The following below are my steps of work :

#### 1. Set features, set target, and split data

#### 2. Cross validation from all algorithms

|Algorithm               |MAE           |RMSE           |R2_Score       |
|------------------------|------------- |---------------|---------------|
|LinearRegression        | 4.158775e+16 |	4.104564e+17  | -1.694088e+18 |
|DecissionTree Regressor | 7.935544e+07 | 2.204110e+08	| 6.688851e-01  |
|RandomForest Regressor  | 6.811846e+07 | 1.954878e+08  | 7.409348e-01  |
|LGBM Regressor          | 9.314647e+07	| 2.049349e+08	| 7.260802e-01  |

Here we could see an interesting phenomena which is R2_score for LinearRegression is negative. The negative r2 score at linear regression means this model fits the data really poorly. So, I will tune RandomForest and LGBM.

#### 3. Tuning two of the best algorithms
After tuning Random Forest Regressor and LGBM Regressor, the best model / algorithm from both of them is Random Forest Regressor. The paramaters for tuning process available at the notebook.

#### 4. Evaluate the best model

```
Data range from 28000000 to 3500000000
MAE :  66148194.93014232
RMSE :  148579233.3999477
R2_Score :  0.8507351552286064
```

This model using the hyperparameter showing very good results. With price data range from 28M to 3.5B, it gives RMSE only 148M and MAE only 66M. And, for r2 score, it gives r2_score 0.85, which means this model fits good with the data, because r2 score tells how well the regression predictions approximate the real data points.

#### 5. Validate the best model
```
MAE mean from 20 fold :  75050559.38
RMSE mean from 20 fold :  162672183.43
R2_score mean from 20 fold :  0.82
```
I think it still have good result when validate this model with 20 Fold... But, this model absolutely need to be improved again with add more data or remove some outlier, because at the fold table we could see it shows not good stability when predicting the data. Since my main objective is to do web scraping, and doing some additional EDA and modelling, after looking this model is pretty good, I will not to continue to explore more or make this model nearly to pefect.


