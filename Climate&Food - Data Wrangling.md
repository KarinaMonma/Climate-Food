# <center>Climate & Food</center>

This is the first step of a project to understand the relationship between Climate & Food over the world. On this Notebook, the data will be imported, transformed and exported for the next step. After that, we'll be able to answer some questions, such as:

* There's any corelation between temperature and food production?
* Can prodution of food interfere on climate?
* What are the countries the have the most climate oscilations? 
* Does food seems to have any corelation with that?
* What are the countries with the least climate oscilations produce less food?
* Do they actually produce less food?
* There's any food that's more climate damaging?
* There's any food that's less climate damaging?
* There's any new insights found after visualization?

***

## <center>Summary</center>

0. [Introduction](#Climate-&-Food)
1. [Importing libraries](#Importing-Libraries)
2. [Save Function](#Save-Function)
3. [Food Dataset: Importing](#Importing-Food-Dataset)
4. [Food Dataset: Understanding the Dataset](#Understanding-the-Food-Dataset)
5. Food Dataset Table: </br>
    5.1. [Dimensional: Country](#Country's-Dimensional-Table) </br>
    5.2. [Dimensional: Item](#Item-Dimensional-Table) </br>
    5.3. [Dimensional: Element](#Element-Dimensional-Table) </br>
    5.4. [Fact: Food](#Food-Fact-Table)
6. [Temperature Dataset: Importing](#Importing-Global-Temperature-DataSet)
7. [Temperature Dataset: Understanding the Dataset](#Understanding-the-Global-Temperature-Dataset)
8. [Temperature Dataset: Transforming](#Transforming-the-Global-Temperature-Dataset)
9. [Extra: Pivot Table](#[Extra]-Pivot-Table)

***

###  <center>Importing Libraries

[Return to Summary](#Summary)

The first step is to import the libraries needed for this project:


```python
import os #module to navegate easily through the system 
import pandas as pd #library for Data Manipulation and Data Analysis
import numpy as np #library for scientific computing

from pathlib import Path #subclass to represent paths in system
from datetime import datetime #module to manipulate dates and times
```

### <center>Save Function</center>

[Return to Summary](#Summary)

The idea is to create a few different datasets and export them so it may seem usefull to create a function to check if there's any file with the given name. If it doesn't exists, it will export the file. Otherwise, it will just print that the file already exists and do nothing else.


```python
# defining the function and setting two parameters (DataFrame, str)
def save_file(file_data, file_name):

    # if the file already exists
    if Path(file_name).exists():
        return print('The file', repr(str(file_name)), 'already exist.') # a warning that the file exists will be printed
    else: # if the file doesn't exist
        file_data.to_csv(file_name) # the file will be exported under the given name,
        return print('The file', repr(str(file_name)), 'succesfully created.') # and return a warning of the file creation
```

###  <center>Importing Food Dataset

[Return to Summary](#Summary)

Now, it's time to import the datasets. The food production dataset will be the first one to be worked on, and here's a few information about it:

**Source:** [Kaggle](https://www.kaggle.com/)</br>
**Name:** *Who eats the food we grow?*     
**Link:** *[Click Here](https://www.kaggle.com/datasets/dorbicycle/world-foodfeed-production)*


```python
os.chdir('D:\Karina\Portfolio\Project_ClimateChange\Dataset') #setting the directory path

food_production = pd.read_csv('FAO.csv', encoding = 'latin1') #reading the csv file

food_production.head() #checking the first 5 rows of the dataset
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AFG</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AFG</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AFG</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AFG</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AFG</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 63 columns</p>
</div>



There's a lot of information on this dataset so might be interesting to create a few dimensional tables and a fact table from it, mainly for practice. Before this dataset is split, understanding a little more about it feels appropriate.

###  <center>Understanding the Food Dataset</center>

[Return to Summary](#Summary)

This step is important to have a bigger view of the dataset. It means check column's types, table's dimension, headers name, and even check some statistics summary and occasionaly find outliers that might need extra attention.


```python
food_production.shape #dimension of the dataset
```




    (21477, 63)




```python
food_production.dtypes #type of each dataset's column 
```




    Area Abbreviation     object
    Area Code              int64
    Area                  object
    Item Code              int64
    Item                  object
                          ...   
    Y2009                float64
    Y2010                float64
    Y2011                float64
    Y2012                  int64
    Y2013                  int64
    Length: 63, dtype: object




```python
food_production.describe() #statistical summary of the dataset
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Code</th>
      <th>Item Code</th>
      <th>Element Code</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>Y1961</th>
      <th>Y1962</th>
      <th>Y1963</th>
      <th>Y1964</th>
      <th>Y1965</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>21477.000000</td>
      <td>21477.000000</td>
      <td>21477.000000</td>
      <td>21477.000000</td>
      <td>21477.000000</td>
      <td>17938.000000</td>
      <td>17938.000000</td>
      <td>17938.000000</td>
      <td>17938.000000</td>
      <td>17938.000000</td>
      <td>...</td>
      <td>21128.000000</td>
      <td>21128.000000</td>
      <td>21373.000000</td>
      <td>21373.000000</td>
      <td>21373.000000</td>
      <td>21373.000000</td>
      <td>21373.000000</td>
      <td>21373.000000</td>
      <td>21477.000000</td>
      <td>21477.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>125.449411</td>
      <td>2694.211529</td>
      <td>5211.687154</td>
      <td>20.450613</td>
      <td>15.794445</td>
      <td>195.262069</td>
      <td>200.782250</td>
      <td>205.464600</td>
      <td>209.925577</td>
      <td>217.556751</td>
      <td>...</td>
      <td>486.690742</td>
      <td>493.153256</td>
      <td>496.319328</td>
      <td>508.482104</td>
      <td>522.844898</td>
      <td>524.581996</td>
      <td>535.492069</td>
      <td>553.399242</td>
      <td>560.569214</td>
      <td>575.557480</td>
    </tr>
    <tr>
      <th>std</th>
      <td>72.868149</td>
      <td>148.973406</td>
      <td>146.820079</td>
      <td>24.628336</td>
      <td>66.012104</td>
      <td>1864.124336</td>
      <td>1884.265591</td>
      <td>1861.174739</td>
      <td>1862.000116</td>
      <td>2014.934333</td>
      <td>...</td>
      <td>5001.782008</td>
      <td>5100.057036</td>
      <td>5134.819373</td>
      <td>5298.939807</td>
      <td>5496.697513</td>
      <td>5545.939303</td>
      <td>5721.089425</td>
      <td>5883.071604</td>
      <td>6047.950804</td>
      <td>6218.379479</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>2511.000000</td>
      <td>5142.000000</td>
      <td>-40.900000</td>
      <td>-172.100000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-169.000000</td>
      <td>-246.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>63.000000</td>
      <td>2561.000000</td>
      <td>5142.000000</td>
      <td>6.430000</td>
      <td>-11.780000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>120.000000</td>
      <td>2640.000000</td>
      <td>5142.000000</td>
      <td>20.590000</td>
      <td>19.150000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>...</td>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>8.000000</td>
      <td>8.000000</td>
      <td>8.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>188.000000</td>
      <td>2782.000000</td>
      <td>5142.000000</td>
      <td>41.150000</td>
      <td>46.870000</td>
      <td>21.000000</td>
      <td>22.000000</td>
      <td>23.000000</td>
      <td>24.000000</td>
      <td>25.000000</td>
      <td>...</td>
      <td>75.000000</td>
      <td>77.000000</td>
      <td>78.000000</td>
      <td>80.000000</td>
      <td>82.000000</td>
      <td>83.000000</td>
      <td>83.000000</td>
      <td>86.000000</td>
      <td>88.000000</td>
      <td>90.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>276.000000</td>
      <td>2961.000000</td>
      <td>5521.000000</td>
      <td>64.960000</td>
      <td>179.410000</td>
      <td>112227.000000</td>
      <td>109130.000000</td>
      <td>106356.000000</td>
      <td>104234.000000</td>
      <td>119378.000000</td>
      <td>...</td>
      <td>360767.000000</td>
      <td>373694.000000</td>
      <td>388100.000000</td>
      <td>402975.000000</td>
      <td>425537.000000</td>
      <td>434724.000000</td>
      <td>451838.000000</td>
      <td>462696.000000</td>
      <td>479028.000000</td>
      <td>489299.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 58 columns</p>
</div>




```python
food_production.info() #full information about the dataset, including a few metadata
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 21477 entries, 0 to 21476
    Data columns (total 63 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   Area Abbreviation  21477 non-null  object 
     1   Area Code          21477 non-null  int64  
     2   Area               21477 non-null  object 
     3   Item Code          21477 non-null  int64  
     4   Item               21477 non-null  object 
     5   Element Code       21477 non-null  int64  
     6   Element            21477 non-null  object 
     7   Unit               21477 non-null  object 
     8   latitude           21477 non-null  float64
     9   longitude          21477 non-null  float64
     10  Y1961              17938 non-null  float64
     11  Y1962              17938 non-null  float64
     12  Y1963              17938 non-null  float64
     13  Y1964              17938 non-null  float64
     14  Y1965              17938 non-null  float64
     15  Y1966              17938 non-null  float64
     16  Y1967              17938 non-null  float64
     17  Y1968              17938 non-null  float64
     18  Y1969              17938 non-null  float64
     19  Y1970              17938 non-null  float64
     20  Y1971              17938 non-null  float64
     21  Y1972              17938 non-null  float64
     22  Y1973              17938 non-null  float64
     23  Y1974              17938 non-null  float64
     24  Y1975              17938 non-null  float64
     25  Y1976              17938 non-null  float64
     26  Y1977              17938 non-null  float64
     27  Y1978              17938 non-null  float64
     28  Y1979              17938 non-null  float64
     29  Y1980              17938 non-null  float64
     30  Y1981              17938 non-null  float64
     31  Y1982              17938 non-null  float64
     32  Y1983              17938 non-null  float64
     33  Y1984              17938 non-null  float64
     34  Y1985              17938 non-null  float64
     35  Y1986              17938 non-null  float64
     36  Y1987              17938 non-null  float64
     37  Y1988              17938 non-null  float64
     38  Y1989              17938 non-null  float64
     39  Y1990              18062 non-null  float64
     40  Y1991              18062 non-null  float64
     41  Y1992              20490 non-null  float64
     42  Y1993              20865 non-null  float64
     43  Y1994              20865 non-null  float64
     44  Y1995              20865 non-null  float64
     45  Y1996              20865 non-null  float64
     46  Y1997              20865 non-null  float64
     47  Y1998              20865 non-null  float64
     48  Y1999              20865 non-null  float64
     49  Y2000              21128 non-null  float64
     50  Y2001              21128 non-null  float64
     51  Y2002              21128 non-null  float64
     52  Y2003              21128 non-null  float64
     53  Y2004              21128 non-null  float64
     54  Y2005              21128 non-null  float64
     55  Y2006              21373 non-null  float64
     56  Y2007              21373 non-null  float64
     57  Y2008              21373 non-null  float64
     58  Y2009              21373 non-null  float64
     59  Y2010              21373 non-null  float64
     60  Y2011              21373 non-null  float64
     61  Y2012              21477 non-null  int64  
     62  Y2013              21477 non-null  int64  
    dtypes: float64(53), int64(5), object(5)
    memory usage: 10.3+ MB
    

### <center>Country's Dimensional Table</center>

[Return to Summary](#Summary)

A Country Dimensional Table seems interesting. This table can retain a lot of information about the Country that's making redundant values on the table original and reduce the size of the final fact table as well. 


```python
# creating and setting the new dataframe
dim_country = pd.DataFrame(food_production, 
                           columns = ['Area Code', 'Area Abbreviation', 'Area', 'latitude', 'longitude'])

# removing duplicate values, applying the new dataframe, resetting the index
dim_country.drop_duplicates(keep = 'first', inplace = True, ignore_index = True) 

# display the first five rows
dim_country.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Code</th>
      <th>Area Abbreviation</th>
      <th>Area</th>
      <th>latitude</th>
      <th>longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>AFG</td>
      <td>Afghanistan</td>
      <td>33.94</td>
      <td>67.71</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>ALB</td>
      <td>Albania</td>
      <td>41.15</td>
      <td>20.17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>DZA</td>
      <td>Algeria</td>
      <td>28.03</td>
      <td>1.66</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>AGO</td>
      <td>Angola</td>
      <td>-11.20</td>
      <td>17.87</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>ATG</td>
      <td>Antigua and Barbuda</td>
      <td>17.06</td>
      <td>-61.80</td>
    </tr>
  </tbody>
</table>
</div>



For aesthetical reason, change the name of the columns and set a pattern for the names might do a subtle but nice result.


```python
# Renaming the columns and commiting the changes
dim_country.rename(columns = {'Area Code': 'Country Code', 'Area Abbreviation': 'Country Abv', 
                              'Area': 'Country', 'latitude': 'Lat', 
                              'longitude': 'Long'}, 
                   inplace = True)

dim_country.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Code</th>
      <th>Country Abv</th>
      <th>Country</th>
      <th>Lat</th>
      <th>Long</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>AFG</td>
      <td>Afghanistan</td>
      <td>33.94</td>
      <td>67.71</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>ALB</td>
      <td>Albania</td>
      <td>41.15</td>
      <td>20.17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>DZA</td>
      <td>Algeria</td>
      <td>28.03</td>
      <td>1.66</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>AGO</td>
      <td>Angola</td>
      <td>-11.20</td>
      <td>17.87</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>ATG</td>
      <td>Antigua and Barbuda</td>
      <td>17.06</td>
      <td>-61.80</td>
    </tr>
  </tbody>
</table>
</div>



This table seem to be finished, so it's time to export it for the visualization on another Notebook using the 'save_file' function, created before.


```python
# calling function
save_file(dim_country, 'Dim_Country.csv')
```

    The file 'Dim_Country.csv' succesfully created.
    

### <center>Item Dimensional Table</center>

[Return to Summary](#Summary)

A table with the Item information seem to be a good idea too, so just a code is needed on the fact table and downsize it a little bit more.


```python
dim_item = pd.DataFrame(food_production, columns = ['Item Code', 'Item'])
dim_item.drop_duplicates(keep = 'first', inplace = True, ignore_index = True)

dim_item.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Code</th>
      <th>Item</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2511</td>
      <td>Wheat and products</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2513</td>
      <td>Barley and products</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2514</td>
      <td>Maize and products</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2517</td>
      <td>Millet and products</td>
    </tr>
  </tbody>
</table>
</div>



It's time to save this dimensional table as well. For exercise reason, this file will be previously created so the function have to return so.


```python
save_file(dim_item, 'Dim_Item.csv')
```

    The file 'Dim_Item.csv' already exist.
    

### <center>Element Dimensional Table</center>

[Return to Summary](#Summary)

It's time to create and export a dimensional table for the propouse of the production. If it's food (for human consumption) or feed (for livestock).


```python
dim_element = pd.DataFrame(food_production, columns = ['Element Code', 'Element', 'Unit'])
dim_element.drop_duplicates(keep = 'first', inplace = True, ignore_index = True)

dim_element.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
    </tr>
  </tbody>
</table>
</div>




```python
save_file(dim_element, 'Dim_Element.csv')
```

    The file 'Dim_Element.csv' succesfully created.
    

### <center>Food Fact Table</center>

[Return to Summary](#Summary)

It's time to work on the Fact Table. Just a few years will be used for practice and to keep it a little bit simple. 

A 'for loop' would make all this a bit easier, but learn the manual work wouldn't hurt neither (I guess), so this time the longer way will be taken.


```python
fact_food = pd.DataFrame(food_production, columns = ['Area Abbreviation', 'Item Code', 'Element Code', 
                                                     'Y1980', 'Y1981', 'Y1982', 'Y1983', 'Y1984', 'Y1985', 
                                                     'Y1986', 'Y1987', 'Y1988', 'Y1989', 'Y1990', 'Y1991',
                                                     'Y1992', 'Y1993', 'Y1994', 'Y1995', 'Y1996', 'Y1997',
                                                     'Y1998', 'Y1999', 'Y2000', 'Y2001', 'Y2002', 'Y2003',
                                                     'Y2004', 'Y2005', 'Y2006', 'Y2007', 'Y2008', 'Y2009',
                                                     'Y2010', 'Y2011', 'Y2012', 'Y2013'])

fact_food.rename(columns = {'Area Abbreviation': 'Country Abv', 'Y1980' : '1980', 'Y1981' : '1981', 'Y1982' : '1982',
                           'Y1983' : '1983', 'Y1984' : '1984', 'Y1985' : '1985', 'Y1986' : '1986', 'Y1987' : '1987',
                           'Y1988' : '1988', 'Y1989' : '1989', 'Y1990' : '1990', 'Y1991' : '1991', 'Y1992' : '1992',
                           'Y1993' : '1993', 'Y1994' : '1994', 'Y1995' : '1995', 'Y1996' : '1996', 'Y1997' : '1997',
                           'Y1998' : '1998', 'Y1999' : '1999', 'Y2000' : '2000', 'Y2001' : '2001', 'Y2002' : '2002',
                           'Y2003' : '2003', 'Y2004' : '2004', 'Y2005' : '2005', 'Y2006' : '2006', 'Y2007' : '2007',
                           'Y2008' : '2008', 'Y2009' : '2009', 'Y2010' : '2010', 'Y2011' : '2011', 'Y2012' : '2012',
                           'Y2013' : '2013'}, inplace = True)

convert = {'2012' : float, '2013' : float} # creating a dictionary with the types to change on the dataset
fact_food = fact_food.astype(convert) # change the types 'int' to 'float'

fact_food.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Abv</th>
      <th>Item Code</th>
      <th>Element Code</th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>1984</th>
      <th>1985</th>
      <th>1986</th>
      <th>...</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AFG</td>
      <td>2511</td>
      <td>5142</td>
      <td>2129.0</td>
      <td>2133.0</td>
      <td>2068.0</td>
      <td>1994.0</td>
      <td>1851.0</td>
      <td>1791.0</td>
      <td>1683.0</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810.0</td>
      <td>4895.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AFG</td>
      <td>2805</td>
      <td>5142</td>
      <td>259.0</td>
      <td>248.0</td>
      <td>217.0</td>
      <td>217.0</td>
      <td>197.0</td>
      <td>186.0</td>
      <td>200.0</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425.0</td>
      <td>422.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AFG</td>
      <td>2513</td>
      <td>5521</td>
      <td>64.0</td>
      <td>60.0</td>
      <td>55.0</td>
      <td>53.0</td>
      <td>51.0</td>
      <td>48.0</td>
      <td>46.0</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367.0</td>
      <td>360.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AFG</td>
      <td>2513</td>
      <td>5142</td>
      <td>202.0</td>
      <td>189.0</td>
      <td>174.0</td>
      <td>167.0</td>
      <td>160.0</td>
      <td>151.0</td>
      <td>145.0</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78.0</td>
      <td>89.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AFG</td>
      <td>2514</td>
      <td>5521</td>
      <td>226.0</td>
      <td>210.0</td>
      <td>199.0</td>
      <td>192.0</td>
      <td>182.0</td>
      <td>173.0</td>
      <td>170.0</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200.0</td>
      <td>200.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 37 columns</p>
</div>



For better visualization it's best to keep a column with the year, instead of having one year per column, so it's time to unpivot this table before exporting it.


```python
# keep the columns in 'id_vars', unpivot the other columns and rename the resulting columns
fact_food = fact_food.melt(id_vars = ['Country Abv', 'Item Code', 'Element Code'], var_name = 'Year', value_name = 'Production')

fact_food.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country Abv</th>
      <th>Item Code</th>
      <th>Element Code</th>
      <th>Year</th>
      <th>Production</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AFG</td>
      <td>2511</td>
      <td>5142</td>
      <td>1980</td>
      <td>2129.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AFG</td>
      <td>2805</td>
      <td>5142</td>
      <td>1980</td>
      <td>259.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AFG</td>
      <td>2513</td>
      <td>5521</td>
      <td>1980</td>
      <td>64.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AFG</td>
      <td>2513</td>
      <td>5142</td>
      <td>1980</td>
      <td>202.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AFG</td>
      <td>2514</td>
      <td>5521</td>
      <td>1980</td>
      <td>226.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
save_file(fact_food, 'Fact_Food.csv')
```

    The file 'Fact_Food.csv' succesfully created.
    

### <Center>Importing Global Temperature DataSet</center>

[Return to Summary](#Summary)

It's time to check the Global Temperature dataset and here's a few information about it:

**Source:** *[Kaggle](https://www.kaggle.com/)*     
**Name:** *Climate Change: Earth Surface Temperature Data*     
**Link:** *[Click Here](https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data)*


```python
global_temperatures = pd.read_csv('GlobalLandTemperaturesByCountry.csv')

global_temperatures.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>dt</th>
      <th>AverageTemperature</th>
      <th>AverageTemperatureUncertainty</th>
      <th>Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1743-11-01</td>
      <td>4.384</td>
      <td>2.294</td>
      <td>Åland</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1743-12-01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Åland</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1744-01-01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Åland</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1744-02-01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Åland</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1744-03-01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Åland</td>
    </tr>
  </tbody>
</table>
</div>



### <center>Understanding the Global Temperature Dataset<Center> 

[Return to Summary](#Summary)


```python
global_temperatures.shape
```




    (577462, 4)




```python
global_temperatures.dtypes
```




    dt                                object
    AverageTemperature               float64
    AverageTemperatureUncertainty    float64
    Country                           object
    dtype: object




```python
global_temperatures.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AverageTemperature</th>
      <th>AverageTemperatureUncertainty</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>544811.000000</td>
      <td>545550.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>17.193354</td>
      <td>1.019057</td>
    </tr>
    <tr>
      <th>std</th>
      <td>10.953966</td>
      <td>1.201930</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-37.658000</td>
      <td>0.052000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>10.025000</td>
      <td>0.323000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>20.901000</td>
      <td>0.571000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>25.814000</td>
      <td>1.206000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>38.842000</td>
      <td>15.003000</td>
    </tr>
  </tbody>
</table>
</div>




```python
global_temperatures.info
```




    <bound method DataFrame.info of                 dt  AverageTemperature  AverageTemperatureUncertainty  \
    0       1743-11-01               4.384                          2.294   
    1       1743-12-01                 NaN                            NaN   
    2       1744-01-01                 NaN                            NaN   
    3       1744-02-01                 NaN                            NaN   
    4       1744-03-01                 NaN                            NaN   
    ...            ...                 ...                            ...   
    577457  2013-05-01              19.059                          1.022   
    577458  2013-06-01              17.613                          0.473   
    577459  2013-07-01              17.000                          0.453   
    577460  2013-08-01              19.759                          0.717   
    577461  2013-09-01                 NaN                            NaN   
    
             Country  
    0          Åland  
    1          Åland  
    2          Åland  
    3          Åland  
    4          Åland  
    ...          ...  
    577457  Zimbabwe  
    577458  Zimbabwe  
    577459  Zimbabwe  
    577460  Zimbabwe  
    577461  Zimbabwe  
    
    [577462 rows x 4 columns]>



This table has more dates, Countries and the structure is different. A little change is welcomed.

### <center>Transforming the Global Temperature Dataset</center>

[Return to Summary](#Summary)


```python
global_temperatures.rename(columns = {'dt' : 'Year'}, inplace = True) 

# changing the 'Year' column type from 'str' to 'datetime'
global_temperatures['Year'] = pd.to_datetime(global_temperatures.Year)
```


```python
# grouping 'Year' by 'Country' and getting the mean value of the average columns
temp_info = global_temperatures.groupby(
    [global_temperatures.Year.dt.strftime('%Y'), 'Country']
)[['AverageTemperature', 'AverageTemperatureUncertainty']].mean().reset_index()

# display the first and last five rows
temp_info
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Country</th>
      <th>AverageTemperature</th>
      <th>AverageTemperatureUncertainty</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1743</td>
      <td>Albania</td>
      <td>8.62000</td>
      <td>2.268000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1743</td>
      <td>Andorra</td>
      <td>7.55600</td>
      <td>2.188000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1743</td>
      <td>Austria</td>
      <td>2.48200</td>
      <td>2.116000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1743</td>
      <td>Belarus</td>
      <td>0.76700</td>
      <td>2.465000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1743</td>
      <td>Belgium</td>
      <td>7.10600</td>
      <td>1.855000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>48238</th>
      <td>2013</td>
      <td>Western Sahara</td>
      <td>23.74425</td>
      <td>1.133125</td>
    </tr>
    <tr>
      <th>48239</th>
      <td>2013</td>
      <td>Yemen</td>
      <td>28.12975</td>
      <td>1.335000</td>
    </tr>
    <tr>
      <th>48240</th>
      <td>2013</td>
      <td>Zambia</td>
      <td>21.19600</td>
      <td>0.825125</td>
    </tr>
    <tr>
      <th>48241</th>
      <td>2013</td>
      <td>Zimbabwe</td>
      <td>20.71075</td>
      <td>0.778500</td>
    </tr>
    <tr>
      <th>48242</th>
      <td>2013</td>
      <td>Åland</td>
      <td>6.22975</td>
      <td>0.536250</td>
    </tr>
  </tbody>
</table>
<p>48243 rows × 4 columns</p>
</div>




```python
# creating a 'for loop' to check every row
for item in temp_info.itertuples():
    
    # if the 'Year' is less than '1980'
    if item[1] < '1980':
        temp_info.drop(index = item[0], inplace = True) # the row will be removed from the dataframe
```


```python
# reset the index
temp_info.reset_index(drop = True)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Country</th>
      <th>AverageTemperature</th>
      <th>AverageTemperatureUncertainty</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1980</td>
      <td>Afghanistan</td>
      <td>14.887333</td>
      <td>0.400833</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1980</td>
      <td>Africa</td>
      <td>24.446667</td>
      <td>0.170417</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1980</td>
      <td>Albania</td>
      <td>12.162083</td>
      <td>0.294833</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1980</td>
      <td>Algeria</td>
      <td>23.160167</td>
      <td>0.382917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1980</td>
      <td>American Samoa</td>
      <td>26.990750</td>
      <td>0.326417</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>8257</th>
      <td>2013</td>
      <td>Western Sahara</td>
      <td>23.744250</td>
      <td>1.133125</td>
    </tr>
    <tr>
      <th>8258</th>
      <td>2013</td>
      <td>Yemen</td>
      <td>28.129750</td>
      <td>1.335000</td>
    </tr>
    <tr>
      <th>8259</th>
      <td>2013</td>
      <td>Zambia</td>
      <td>21.196000</td>
      <td>0.825125</td>
    </tr>
    <tr>
      <th>8260</th>
      <td>2013</td>
      <td>Zimbabwe</td>
      <td>20.710750</td>
      <td>0.778500</td>
    </tr>
    <tr>
      <th>8261</th>
      <td>2013</td>
      <td>Åland</td>
      <td>6.229750</td>
      <td>0.536250</td>
    </tr>
  </tbody>
</table>
<p>8262 rows × 4 columns</p>
</div>




```python
save_file(temp_info, 'Temp_Info.csv')
```

    The file 'Temp_Info.csv' succesfully created.
    

It's time to create some visuals and answer some questions!

I hope to see you again on the next Notebook!

Thank you for taking time to see this Notebook!

***

## <Center>[Extra] Pivot Table</Center>

[Return to Summary](#Summary)

Since the unpivot was needed in the project, let's do a pivot as well, just for fun (and practice).


```python
# pivot 'Year' column according to 'Country' to keep a similar format on the Food Dataset  
piv_temp = pd.pivot_table(temp_info, index = 'Country', columns = ('Year'), aggfunc = 'mean')

piv_temp
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="10" halign="left">AverageTemperature</th>
      <th>...</th>
      <th colspan="10" halign="left">AverageTemperatureUncertainty</th>
    </tr>
    <tr>
      <th>Year</th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>1984</th>
      <th>1985</th>
      <th>1986</th>
      <th>1987</th>
      <th>1988</th>
      <th>1989</th>
      <th>...</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
    </tr>
    <tr>
      <th>Country</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Afghanistan</th>
      <td>14.887333</td>
      <td>14.860083</td>
      <td>13.733083</td>
      <td>14.614833</td>
      <td>14.245833</td>
      <td>14.888750</td>
      <td>14.223167</td>
      <td>14.984000</td>
      <td>15.389000</td>
      <td>14.125750</td>
      <td>...</td>
      <td>0.482500</td>
      <td>0.528417</td>
      <td>0.488250</td>
      <td>0.396083</td>
      <td>0.517333</td>
      <td>0.346167</td>
      <td>0.290083</td>
      <td>0.316250</td>
      <td>0.365500</td>
      <td>0.570375</td>
    </tr>
    <tr>
      <th>Africa</th>
      <td>24.446667</td>
      <td>24.297417</td>
      <td>24.211583</td>
      <td>24.579417</td>
      <td>24.387167</td>
      <td>24.339833</td>
      <td>24.309417</td>
      <td>24.922000</td>
      <td>24.568333</td>
      <td>24.223833</td>
      <td>...</td>
      <td>0.199833</td>
      <td>0.179583</td>
      <td>0.192917</td>
      <td>0.171917</td>
      <td>0.165250</td>
      <td>0.174667</td>
      <td>0.168083</td>
      <td>0.200083</td>
      <td>0.257833</td>
      <td>0.248250</td>
    </tr>
    <tr>
      <th>Albania</th>
      <td>12.162083</td>
      <td>12.578667</td>
      <td>13.015250</td>
      <td>12.510917</td>
      <td>12.640750</td>
      <td>12.948167</td>
      <td>13.056417</td>
      <td>12.996583</td>
      <td>13.044083</td>
      <td>12.803833</td>
      <td>...</td>
      <td>0.284833</td>
      <td>0.247000</td>
      <td>0.319083</td>
      <td>0.280000</td>
      <td>0.283750</td>
      <td>0.340917</td>
      <td>0.326500</td>
      <td>0.350750</td>
      <td>0.493333</td>
      <td>0.544500</td>
    </tr>
    <tr>
      <th>Algeria</th>
      <td>23.160167</td>
      <td>23.579250</td>
      <td>23.094167</td>
      <td>23.683000</td>
      <td>23.063083</td>
      <td>23.526250</td>
      <td>23.256667</td>
      <td>24.247750</td>
      <td>23.815667</td>
      <td>23.724333</td>
      <td>...</td>
      <td>0.420417</td>
      <td>0.415667</td>
      <td>0.370417</td>
      <td>0.375167</td>
      <td>0.338250</td>
      <td>0.336417</td>
      <td>0.292000</td>
      <td>0.317917</td>
      <td>0.484583</td>
      <td>0.437625</td>
    </tr>
    <tr>
      <th>American Samoa</th>
      <td>26.990750</td>
      <td>26.745500</td>
      <td>26.902750</td>
      <td>26.928667</td>
      <td>26.879500</td>
      <td>26.949917</td>
      <td>26.980333</td>
      <td>26.898750</td>
      <td>27.089417</td>
      <td>26.752667</td>
      <td>...</td>
      <td>0.668917</td>
      <td>0.432750</td>
      <td>0.485250</td>
      <td>0.478750</td>
      <td>0.354167</td>
      <td>0.485750</td>
      <td>0.411417</td>
      <td>0.442250</td>
      <td>0.535750</td>
      <td>0.472250</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Western Sahara</th>
      <td>22.925667</td>
      <td>23.021667</td>
      <td>22.367917</td>
      <td>23.380833</td>
      <td>22.413000</td>
      <td>22.855917</td>
      <td>22.418083</td>
      <td>23.730750</td>
      <td>22.811833</td>
      <td>23.253750</td>
      <td>...</td>
      <td>0.635917</td>
      <td>0.785167</td>
      <td>0.662917</td>
      <td>0.680750</td>
      <td>0.588750</td>
      <td>0.499667</td>
      <td>0.702250</td>
      <td>0.586167</td>
      <td>0.880417</td>
      <td>1.133125</td>
    </tr>
    <tr>
      <th>Yemen</th>
      <td>26.855583</td>
      <td>26.542333</td>
      <td>26.082750</td>
      <td>26.244500</td>
      <td>25.851083</td>
      <td>25.882583</td>
      <td>26.054667</td>
      <td>26.612083</td>
      <td>26.783917</td>
      <td>25.894500</td>
      <td>...</td>
      <td>0.810583</td>
      <td>0.736500</td>
      <td>0.665750</td>
      <td>0.490417</td>
      <td>0.600917</td>
      <td>0.546500</td>
      <td>0.512500</td>
      <td>0.457583</td>
      <td>0.679250</td>
      <td>1.335000</td>
    </tr>
    <tr>
      <th>Zambia</th>
      <td>21.337667</td>
      <td>21.247833</td>
      <td>21.599583</td>
      <td>22.346083</td>
      <td>21.673833</td>
      <td>21.331833</td>
      <td>21.294083</td>
      <td>22.308583</td>
      <td>21.885083</td>
      <td>21.644000</td>
      <td>...</td>
      <td>0.423833</td>
      <td>0.474917</td>
      <td>0.423333</td>
      <td>0.490083</td>
      <td>0.485167</td>
      <td>0.491667</td>
      <td>0.483250</td>
      <td>0.495167</td>
      <td>0.586583</td>
      <td>0.825125</td>
    </tr>
    <tr>
      <th>Zimbabwe</th>
      <td>21.128833</td>
      <td>20.804167</td>
      <td>21.574917</td>
      <td>22.367167</td>
      <td>21.714583</td>
      <td>21.198083</td>
      <td>21.200500</td>
      <td>22.323500</td>
      <td>21.483000</td>
      <td>21.432250</td>
      <td>...</td>
      <td>0.329083</td>
      <td>0.272333</td>
      <td>0.315000</td>
      <td>0.320000</td>
      <td>0.433000</td>
      <td>0.436417</td>
      <td>0.409667</td>
      <td>0.393417</td>
      <td>0.538500</td>
      <td>0.778500</td>
    </tr>
    <tr>
      <th>Åland</th>
      <td>5.015667</td>
      <td>5.057750</td>
      <td>5.781917</td>
      <td>6.134083</td>
      <td>6.234750</td>
      <td>3.467167</td>
      <td>5.000083</td>
      <td>3.718250</td>
      <td>6.001583</td>
      <td>7.249417</td>
      <td>...</td>
      <td>0.307000</td>
      <td>0.307167</td>
      <td>0.348833</td>
      <td>0.320417</td>
      <td>0.367000</td>
      <td>0.366917</td>
      <td>0.397833</td>
      <td>0.419167</td>
      <td>0.372417</td>
      <td>0.536250</td>
    </tr>
  </tbody>
</table>
<p>243 rows × 68 columns</p>
</div>



***

## <center>Bibliography</center>

* Oppenheim, Dor (2018) "<i>Who eats the food we grow?</i>". Retrieved on: May 27, 2022 from [https://www.kaggle.com/datasets/dorbicycle/world-foodfeed-production](https://www.kaggle.com/datasets/dorbicycle/world-foodfeed-production)

* Earth, Berkeley (2017) "<i>Climate Change: Earth Surface Temperature Data</i>". Retrieved on: May 27, 2022 from [https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data/metadata](https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data/metadata)
