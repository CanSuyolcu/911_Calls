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
Firstly we will import numpy and pandas.
```python
import numpy as np
import pandas as pd
```
Now we import visualization libraries and set %matplotlib inline.
```python
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('whitegrid')
%matplotlib inline
```
Now we read in the csv file as a dataframe called df.
```python
df = pd.read_csv('911.csv')
```
Now we can check the informations about it.
```python
df.info()
```
<img src= "https://user-images.githubusercontent.com/66487971/87164397-6b28a280-c2d1-11ea-8306-6af45ea05a61.png" width = 300>
We can check the head of the dataframe for a quick grasp.



