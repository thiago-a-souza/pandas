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

As the name suggests *reset_index* reset the DataFrame index. The parameter *drop=True* prevents adding the old index as a new column, and *inplace=True* applies the change in place instead of creating a new object.

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

Displaying a specific column

```python
>>> df['state']
0      Ohio
1      Ohio
2      Ohio
3    Nevada
4    Nevada

# selecting a single column with a single pair of brackets return a Series
>>> type(df['state'])
<class 'pandas.core.series.Series'>
```

Displaying multiple columns

```python
>>> df[['state','pop']]
    state  pop
0    Ohio  1.5
1    Ohio  1.7
2    Ohio  3.6
3  Nevada  2.4
4  Nevada  2.9

# selecting a single column with a single pair of brackets return a DataFrame
>>> type(df[['state','pop']])
<class 'pandas.core.frame.DataFrame'>
```

Slicing rows 

```python
>>> df[0:2]
  state  year  pop
0  Ohio  2000  1.5
1  Ohio  2001  1.7

>>> df[3:5]
    state  year  pop
3  Nevada  2001  2.4
4  Nevada  2002  2.9
```

## Filtering rows

The output of conditional expressions is a pandas Series of boolean values with the same number of rows as the original DataFrame, and it can be used to filter DataFrames when enclosed in brackets, so only rows with *True* values are returned.

```python
>>> df['state'] == 'Ohio'
0     True
1     True
2     True
3    False
4    False

>>> df[df['state'] == 'Ohio']
  state  year  pop
0  Ohio  2000  1.5
1  Ohio  2001  1.7
2  Ohio  2002  3.6

>>> df[df['pop'] > 2.5]
    state  year  pop
2    Ohio  2002  3.6
4  Nevada  2002  2.9
```

Combining multiple conditional statements requires enclosing them by parenthesis. Also, it should use the logical operators & and | instead of and/or.

```python
>>> df[(df['year'] >= 2001) & (df['pop'] > 2.0)]
    state  year  pop
2    Ohio  2002  3.6
3  Nevada  2001  2.4
4  Nevada  2002  2.9
```

### iloc

```python
# accessing row number 2 and returning a Series
>>> df.iloc[2]
state    Ohio
year     2002
pop       3.6

# single pair of brackets returns a Series
>>> type(df.iloc[2])
<class 'pandas.core.series.Series'>

# accessing row number 2 and returning a DataFrame
>>> df.iloc[[2]]
  state  year  pop
2  Ohio  2002  3.6

# double pair of brackets returns a DataFrame
>>> type(df.iloc[[2]])
<class 'pandas.core.frame.DataFrame'>
```

Slicing with *iloc*

```python
# showing rows in the interval [0-2]
>>> df.iloc[:3]
  state  year  pop
0  Ohio  2000  1.5
1  Ohio  2001  1.7
2  Ohio  2002  3.6

# showing rows in the interval [1-2]
>>> df.iloc[1:3]
  state  year  pop
1  Ohio  2001  1.7
2  Ohio  2002  3.6
```

*iloc* also allows accessing a specific column position:

```python
# accessing cell (2, 1)
>>> df.iloc[2, 1]
2002

# slicing columns
>>> df.iloc[4, 1:3]
year    2002
pop      2.9

# colon without interval selects all rows or columns
>>> df.iloc[:, 1:3]
   year  pop
0  2000  1.5
1  2001  1.7
2  2002  3.6
3  2001  2.4
4  2002  2.9

# accessing rows/columns using an array of indexes
>>> df.iloc[[0,2], [0,2]]
  state  pop
0  Ohio  1.5
2  Ohio  3.6
```

### at

*at* is useful to set/get a single value, passing the row number and the column label as argument

```python
# returning the value for the column state at index 2
>>> df.at[2, 'state']
'Ohio'

# changing a cell value
>>> df.at[2, 'state'] = 'OHIO'
>>> df.at[2, 'state']
'OHIO'
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
