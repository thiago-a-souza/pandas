# Pandas

Import library

```python
>>> import pandas as pd
```

Read CSV and store into DataFrame

```python
>>> df = pd.read_csv('/home/thiago/my_file.csv')
```

Creating a DataFrame from a dictionary

```python
>>> data = { 'id' : [1, 2, 3], 'name' : ['john', 'peter', 'jack'] }
>>> df2 = pd.DataFrame(data)
>>> df2
   id   name
0   1   john
1   2  peter
2   3   jack
```
