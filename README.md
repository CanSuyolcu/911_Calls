# 911_Calls
This is an exercise project for my Pandas, NumPy, Seaborn and Matplotlib skills 

For this capstone project I will be analyzing some 911 call data from Kaggle. The data contains the following fields:
- lat : String variable, Latitude
- lng: String variable, Longitude
- desc: String variable, Description of the Emergency Call
- zip: String variable, Zipcode
- title: String variable, Title
- timeStamp: String variable, YYYY-MM-DD HH:MM:SS
- twp: String variable, Township
- addr: String variable, Address
- String variable, Dummy variable (always 1)
## Data and Setup
Firstly I import numpy and pandas
```python
import numpy as np
import pandas as pd
```
Now I import visualization libraries and set %matplotlib inline.
```python
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('whitegrid')
%matplotlib inline
```
Now I  read in the csv file as a dataframe called df
```python
df = pd.read_csv('911.csv')
```
Now I can check the informations about it
```python
df.info()
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 99492 entries, 0 to 99491
Data columns (total 9 columns):
 #   Column     Non-Null Count  Dtype  
---  ------     --------------  -----  
 0   lat        99492 non-null  float64
 1   lng        99492 non-null  float64
 2   desc       99492 non-null  object 
 3   zip        86637 non-null  float64
 4   title      99492 non-null  object 
 5   timeStamp  99492 non-null  object 
 6   twp        99449 non-null  object 
 7   addr       98973 non-null  object 
 8   e          99492 non-null  int64  
dtypes: float64(3), int64(1), object(5)
memory usage: 6.8+ MB
and now I check the head of to get some more information about it
```python
df.head(3)
```

