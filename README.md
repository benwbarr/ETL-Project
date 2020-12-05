# **ETL Project**


![alt text](https://cdn.mapsinternational.co.uk/pub/media/catalog/product/cache/afad95d7734d2fa6d0a8ba78597182b7/k/i/kids-cartoon-world-map_wm01139.jpg)

### This was done using Jupyter Notebooks and Mongodb. You will need to install and import the following

- Jupyter Notebooks

- pandas 

- numpy

- os

- Mongodb

## Step 1 - Extract Data

The data used was extracted from these sites as CSVs

- https://www.kaggle.com/bitsnpieces/covid19-country-data?select=country_names_covid19_forecast.csv

- https://www.kaggle.com/darknez/gdp-among-world

- https://www.kaggle.com/pradeepp/economy-rankings

- https://datacatalog.worldbank.org/dataset/gdp-ranking

## Step 2 - Pull Data into Python

```python
Rankings_file = "Resources/Rankings.csv"
Rankings_df = pd.read_csv(Rankings_file)
```
Had to look up and reasurch encoding to get this CSV to import properly.

```python
GDP_file = "Resources/GDP.csv"
GDP_df = pd.read_csv(GDP_file, encoding="ISO-8859-1")
```


```python
Growth_file = "Resources/Growth.csv"
Growth_df = pd.read_csv(Growth_file)
```


# Transform

## Step 3 - Data cleaning and filtering 

### Cleaning

```python
GDP_drop = GDP_df.drop(GDP_df.columns[[0,2,5]], axis = 1)
```

![alt text](https://github.com/benwbarr/ETL-Project/blob/main/Images/Capture.PNG?raw=true)

```python
GDP_final= GDP_drop.rename(columns={"Ranking": "GDP Ranking",
                                    "Economy": "Country",
                                    "US dollars)": "GDP"}))
GDP_final.set_index("Country", inplace=True)                                    
```

![alt text](https://github.com/benwbarr/ETL-Project/blob/main/Images/Capture2.PNG?raw=true)
![alt text](https://github.com/benwbarr/ETL-Project/blob/main/Images/Capture3.PNG?raw=true)

Learned that I could convert df into a series so a drop with a range could be done.

```python
Covid_drop = Covid_df.drop(Covid_df.columns[0,], axis = 1)
Final_Covid_drop = Covid_drop.drop(Covid_drop.columns.to_series()["h1n1_Geographic_spread":"longitude"], axis=1)
```

![alt text](https://github.com/benwbarr/ETL-Project/blob/main/Images/Capture4.PNG?raw=true)

### Merging
```python
merge1 = pd.merge(GDP_final,Rankings_final, on = ["Country"])
merge2 = pd.merge(merge1,Growth_final, on = ["Country"])
merge_final = pd.merge(merge2,Health_final, on = ["Country"])
```

![alt text](https://github.com/benwbarr/ETL-Project/blob/main/Images/Capture5.PNG?raw=true)


```python
data_merge = pd.merge(Final,Covid_final, on = ["Country"])
```


![alt text](https://github.com/benwbarr/ETL-Project/blob/main/Images/Capture6.PNG?raw=true)


### Exporting
```python
Final.to_csv("Resources\Mongo_Data.csv")
```

```python
Final_Economy.to_csv("Resources\Economy_Data.csv")
```

## Mongodb
