# Draw Clustered Column Graph

## Sample Data
|stock|year|avg|
|---|---|---|
|x|2016|5|
|x|2017|6|
|x|2018|8|
|y|2015|1|
|y|2016|2|
|y|2017|4|
|y|2018|8|
|z|2015|1|
|z|2016|9|
|z|2018|7|

## Goal graph to achieve using Pandas
![target.PNG](.\target.PNG)

## Step By Step!

### Step 1: Prepare Data
```python
import pandas as pd
df = pd.DataFrame([['x',2016,5],['x',2017,6],['x',2018,8],['y',2015,1],
    ['y',2016,2],['y',2017,4],['y',2018,8],['z',2015,1],['z',2017,9],['z',2018,7]])
df.columns = ['stock','year','avg']
df.head()
```

```
  stock	year	avg
0	x	2016	5
1	x	2017	6
2	x	2018	8
3	y	2015	1
4	y	2016	2

```


### Step 2: Draw with Python By Default
```python
import matplotlib.pyplot as plt
df.plot.bar(x='year')
```


![sample.jpeg](.\sample.jpeg)


### Discussion: How to generate right clustered columns graph?
**Key is to form the right data strucutre.**

Each clustered column should be presented as one column, i.e. one column for x with avg value, one column for y with its avg value.

Data required to be as following:


|year|x|y|z|
|---|---|---|---|
|2015||1|1|
|2016|5|2||
|2017|6|4|9|
|2018|8|8|7|

Step 3: Transform Data

```python
df_new = pd.DataFrame()
df_new['year'] = df.year.unique()
df_new = df_new.set_index('year')
df = df.set_index('year')
for i in ['x','y','z']:
    df_new[i] = df[df.stock==i]['avg']
df_new = df_new.reset_index().sort_values(by='year')
df_new.head()

```
```

    year	x	y	z
3	2015	NaN	1	1.0
0	2016	5.0	2	9.0
1	2017	6.0	4	NaN
2	2018	8.0	8	7.0
```

Step 4: Draw Clustered Column

```python
df_new.plot.bar(x=['year'], figsize=(15,9))
```

![output.jpeg](D:\AnacondaProjects\adr_data_collection_2010_2017\output.jpeg)
