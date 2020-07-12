# 911_Calls_Project
This is an exercise project in order to practice my Pandas, NumPy, Seaborn and Matplotlib skills in Python.

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

```python
df.head(3)
```
<img src= "https://user-images.githubusercontent.com/66487971/87165507-ea6aa600-c2d2-11ea-967d-01efb7f81627.JPG" width = 800>

## Creating new features

In the titles column there are "Reasons/Departments" specified before the title code. These are EMS, Fire, and Traffic. We are going to use .apply() with a custom lambda expression to create a new column called "Reason" that contains this string value.

```python
df['Reason'] = df['title'].apply(lambda title: title.split(':')[0])
```

Now we can check  the most common Reason for a 911 call based off of this new column.

```python
df['Reason'].value_counts()
```
<img src= "https://user-images.githubusercontent.com/66487971/87167251-5cdc8580-c2d5-11ea-9948-8c1b1c30b287.png" width = 250>

Now we will start using Seaborn to create a countplot of 911 calls by Reason.

```python
sns.countplot(x='Reason',data=df,palette='viridis')
```

<img src= "https://user-images.githubusercontent.com/66487971/87167454-a5943e80-c2d5-11ea-8854-5aee3c5641be.png" width = 500>

We will now use pd.to_datetime to convert the column from strings to DateTime objects.

```python
df['timeStamp'] = pd.to_datetime(df['timeStamp'])
```

Now that the timestamp column are actually DateTime objects, we can use .apply() to create 3 new columns called Hour, Month, and Day of Week.
```python
df['Hour'] = df['timeStamp'].apply(lambda time: time.hour)
df['Month'] = df['timeStamp'].apply(lambda time: time.month)
df['Day of Week'] = df['timeStamp'].apply(lambda time: time.dayofweek)
```
We will use the .map() with this dictionary to map the actual string names to the day of the week.

```python
dmap = {0:'Mon',1:'Tue',2:'Wed',3:'Thu',4:'Fri',5:'Sat',6:'Sun'}
df['Day of Week'] = df['Day of Week'].map(dmap)
```
Now we use seaborn to create a countplot of the Day of Week column with the hue based off of the Reason column.

```python
sns.countplot(x='Day of Week',data=df,hue='Reason',palette='viridis')

# To relocate the legend
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```
<img src= "https://user-images.githubusercontent.com/66487971/87168177-c1e4ab00-c2d6-11ea-9d0b-c35bd5e5b12b.png" width = 500>

Now we do the same for Month.

```python
sns.countplot(x='Month',data=df,hue='Reason',palette='viridis')

# To relocate the legend
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```

<img src= "https://user-images.githubusercontent.com/66487971/87168361-0c662780-c2d7-11ea-9893-586aec71d6aa.png" width = 500>

We notice that it was missing some Months, now we will fill in this information by plotting a simple line plot that fills in the missing months.

```python
byMonth = df.groupby('Month').count()
byMonth.head()
```
<img src= "https://user-images.githubusercontent.com/66487971/87168595-6bc43780-c2d7-11ea-9c84-780e699fef5e.png" width = 500>

 Now we create a simple plot off of the dataframe indicating the count of calls per month.
 
 ```python
 byMonth['twp'].plot()
 ```
 <img src= "https://user-images.githubusercontent.com/66487971/87168734-9c0bd600-c2d7-11ea-984f-25949d26c02a.png" width = 500>
 
  Now we will use seaborn's lmplot() to create a linear fit on the number of calls per month. 
  
  ```python
  sns.lmplot(x='Month',y='twp',data=byMonth.reset_index())
  ```
  <img src= "https://user-images.githubusercontent.com/66487971/87168893-d4abaf80-c2d7-11ea-9400-99c4f4d413a4.png" width = 500>
  
  We create a new column called 'Date' that contains the date from the timeStamp column and groupby this Date column with the count() aggregate and create a plot of counts of 911 calls.
  
  ```python
  df['Date']=df['timeStamp'].apply(lambda t: t.date())
  df.groupby('Date').count()['twp'].plot()
plt.tight_layout()
```

<img src= "https://user-images.githubusercontent.com/66487971/87169082-205e5900-c2d8-11ea-865a-137e5b4031d2.png" width = 500>

Now we will recreate this plot but create 3 separate plots with each plot representing a Reason for the 911 call.

```python
df[df['Reason']=='Traffic'].groupby('Date').count()['twp'].plot()
plt.title('Traffic')
plt.tight_layout()
```
<img src= "https://user-images.githubusercontent.com/66487971/87169346-82b75980-c2d8-11ea-889b-d42ee988332b.png" width = 500>

```python
df[df['Reason']=='Fire'].groupby('Date').count()['twp'].plot()
plt.title('Fire')
plt.tight_layout()
```
<img src= "https://user-images.githubusercontent.com/66487971/87170171-a929c480-c2d9-11ea-8ae1-d919614b5f11.png" width = 500>

```python
df[df['Reason']=='EMS'].groupby('Date').count()['twp'].plot()
plt.title('EMS')
plt.tight_layout()
```

<img src= "https://user-images.githubusercontent.com/66487971/87170297-d37b8200-c2d9-11ea-9ff6-a6de191c3765.png" width = 500>

 Now we will create heatmaps with seaborn and our data. We'll first need to restructure the dataframe so that the columns become the Hours and the Index becomes the Day of the Week.
 
 ```python
 dayHour = df.groupby(by=['Day of Week','Hour']).count()['Reason'].unstack()
dayHour.head()
```

<img src= "https://user-images.githubusercontent.com/66487971/87170562-29e8c080-c2da-11ea-8adb-05bdc54a0610.png" width = 800>

Now we create a HeatMap using this new DataFrame.

```python
plt.figure(figsize=(12,6))
sns.heatmap(dayHour,cmap='viridis')
```
<img src= "(https://user-images.githubusercontent.com/66487971/87170766-716f4c80-c2da-11ea-8f71-9c599be5201f.png" width = 500>

Now we create a clustermap using this DataFrame.

```python
sns.clustermap(dayHour,cmap='viridis')
```
<img src= "https://user-images.githubusercontent.com/66487971/87171290-26096e00-c2db-11ea-9fe5-17ba8be4cd00.png" width = 500>

Now we repeat these same plots and operations, for a DataFrame that shows the Month as the column.

```python
dayMonth = df.groupby(by=['Day of Week','Month']).count()['Reason'].unstack()
dayMonth.head()
```
<img src= "https://user-images.githubusercontent.com/66487971/87171453-5f41de00-c2db-11ea-9393-d21fe8414dee.png" width = 800>

```python
plt.figure(figsize=(12,6))
sns.heatmap(dayMonth,cmap='viridis')
```
<img src= "https://user-images.githubusercontent.com/66487971/87171593-8bf5f580-c2db-11ea-99f4-3d7bdd87cb3d.png" width = 500>

```python
sns.clustermap(dayMonth,cmap='viridis')
```
<img src= "https://user-images.githubusercontent.com/66487971/87171684-b182ff00-c2db-11ea-9ea5-5ff4985feda8.png" width = 500>

### This concludes my project here. Thanks for reading all the way through.










  








