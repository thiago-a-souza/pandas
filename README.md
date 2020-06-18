# Pandas

Import Pandas library

```python
>>> import pandas as pd
```



## Creating DataFrames

Creating a DataFrame from a CSV file

```python
>>> df = pd.read_csv('/home/thiago/my_file.csv')
```

Creating a DataFrame from a dictionary

```python
>>> data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
            'year': [2000, 2001, 2002, 2001, 2002],
            'pop': [1.5, 1.7, 3.6, 2.4, 2.9]}
>>> df = pd.DataFrame(data)
>>> df
    state  year  pop
0    Ohio  2000  1.5
1    Ohio  2001  1.7
2    Ohio  2002  3.6
3  Nevada  2001  2.4
4  Nevada  2002  2.9
```

Creating a DataFrame with defined index values 

```python
>>> df = pd.DataFrame(data, index=['one', 'two', 'three', 'four', 'five'])
>>> df
        state  year  pop
one      Ohio  2000  1.5
two      Ohio  2001  1.7
three    Ohio  2002  3.6
four   Nevada  2001  2.4
five   Nevada  2002  2.9
```

As the name suggests *reset_index*, reset the DataFrame index. The parameter *drop=True* prevents adding the old index as a new column, and *inplace=True* applies the change in place instead of creating a new object.

```python
>>> df.reset_index(drop=True, inplace=True)
>>> df
    state  year  pop
0    Ohio  2000  1.5
1    Ohio  2001  1.7
2    Ohio  2002  3.6
3  Nevada  2001  2.4
4  Nevada  2002  2.9
>>>                   
```

## Showing data

Displaying the first N rows

```python
>>> df.head(2)
  state  year  pop
0  Ohio  2000  1.5
1  Ohio  2001  1.7
``` 

Displaying the last N rows

```python
>>> df.tail(3)
    state  year  pop
2    Ohio  2002  3.6
3  Nevada  2001  2.4
4  Nevada  2002  2.9
```
## Config 
Showing DataFrame column names

```python
>>> df.columns
Index(['state', 'year', 'pop'], dtype='object')
```

Showing DataFrame column types

```python
>>> df.dtypes
state     object
year       int64
pop      float64
```

Showing the number of rows and columns from a DataFrame
```python
>>> df.shape
(5, 3)
```

## Writing DataFrames

Writing DataFrame into CSV without exporting index values

```python
df.to_csv('data.csv', index = False)
```
