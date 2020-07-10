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

