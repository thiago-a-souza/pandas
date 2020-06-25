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

# accessing a specific row/column
>>> df['year'].iloc[2]
2002
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

### isin

*isin* allows filtering values from an array

```python
>>> df[df['year'].isin([2000, 2001])]
    state  year  pop
0    Ohio  2000  1.5
1    Ohio  2001  1.7
3  Nevada  2001  2.4
```
## Sorting data

```python
>>> df.sort_values(by='year')
    state  year  pop
0    Ohio  2000  1.5
1    Ohio  2001  1.7
3  Nevada  2001  2.4
2    Ohio  2002  3.6
4  Nevada  2002  2.9

>>> df.sort_values(by=['state', 'pop'], ascending=False)
    state  year  pop
2    Ohio  2002  3.6
1    Ohio  2001  1.7
0    Ohio  2000  1.5
4  Nevada  2002  2.9
3  Nevada  2001  2.4
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

## Grouping data

The Pandas *groupby* function basically splits the data into groups passed as arguments, and then all numeric fields can be aggregated using functions such as *mean*, *sum*, *size*, etc.

```python
# splitting data with groupby
>>> groups = df.groupby(['state'])
>>> type(groups)
<class 'pandas.core.groupby.generic.DataFrameGroupBy'>

# if no column is selected from the dataframe, it applies the function (e.g. sum) to all numeric fields
>>> df.groupby(['state']).sum()
        year  pop
state
Nevada  4003  5.3
Ohio    6003  6.8

# selecting columns and applying an aggregation
>>> df[['pop', 'state']].groupby(['state']).sum()
        pop
state
Nevada  5.3
Ohio    6.8

# checking the size of each group
>>> df.groupby(['state']).size()
state
Nevada    2
Ohio      3

# finding the min population per year
>>> df[['year', 'pop']].groupby(['year']).min()
      pop
year
2000  1.5
2001  1.7
2002  2.9
```


Counting occurrences grouped by year. The *size* function returns the size of each group and creates a new column named that can be renamed to a more meaningful name.

```python
>>> df.groupby(['year']).size().reset_index().rename(columns={0 : 'count'})
   year  count
0  2000      1
1  2001      2
2  2002      2
```
## Merging data

Merging in Pandas is similar to SQL joins. There are serveral parameters allowed, but the follow examples cover some common use cases.

```python
# creating source dataframes
>>> emp = pd.DataFrame({'name': ['john', 'peter', 'andrew', 'david', 'paul', 'philip'],
...                     'department_id' : [3, 1, 1, 2, 3, None]})
>>> dept = pd.DataFrame({'deptname' : ['it', 'marketing', 'hr'], 'deptid' : [1, 2, 3] })


# inner join between emp and dept
>>> inner_df = pd.merge(emp, dept, how='inner', left_on=['department_id'], right_on=['deptid'])
>>> inner_df.sort_values(by='deptid')
     name  department_id   deptname  deptid
2   peter            1.0         it       1
3  andrew            1.0         it       1
4   david            2.0  marketing       2
0    john            3.0         hr       3
1    paul            3.0         hr       3

# left outer join between emp and dept
>>> left_outer_df = pd.merge(emp, dept, how='left', left_on=['department_id'], right_on=['deptid'])
>>> left_outer_df.sort_values(by='deptid')
     name  department_id   deptname  deptid
1   peter            1.0         it     1.0
2  andrew            1.0         it     1.0
3   david            2.0  marketing     2.0
0    john            3.0         hr     3.0
4    paul            3.0         hr     3.0
5  philip            NaN        NaN     NaN                                   
```


## Writing DataFrames

Writing DataFrame into CSV without exporting index values

```python
df.to_csv('data.csv', index = False)
```
