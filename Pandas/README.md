# Pandas---My-learning-notes
Pandas---My-learning-notes

## Usefule links

http://pandas.pydata.org/pandas-docs/stable/10min.html

http://www.datasciencemadesimple.com/get-minimum-value-column-python-pandas/

https://github.com/IST256/learn-python/tree/master/content/lessons/12

https://stackoverflow.com/questions/tagged/pandas?sort=frequent&pageSize=15

http://seanlaw.github.io/2016/05/25/pandas-end-to-end-example/

https://github.com/GalvanizeOpenSource/live-coding-pandas-lda/blob/master/live_coding.ipynb

https://github.com/hyunhw/ml-pandas-examples/blob/master/Pandas/1_Pandas_basic.ipynb



## Notes:

## pandas iloc vs ix vs loc explanation, how are they different?
https://stackoverflow.com/questions/31593201/pandas-iloc-vs-ix-vs-loc-explanation-how-are-they-different
```
First, here's a recap of the three methods:

loc gets rows (or columns) with particular labels from the index.
iloc gets rows (or columns) at particular positions in the index (so it only takes integers).
ix usually tries to behave like loc but falls back to behaving like iloc if a label is not present in the index.
It's important to note some subtleties that can make ix slightly tricky to use:

if the index is of integer type, ix will only use label-based indexing and not fall back to position-based indexing. If the label is not in the index, an error is raised.

if the index does not contain only integers, then given an integer, ix will immediately use position-based indexing rather than label-based indexing. If however ix is given another type (e.g. a string), it can use label-based indexing.
```

### Change data type of columns

```python
b = [['a',10,100],['b',20,200]]
b_cols = ['c1','c2','c3']
df_b = pd.DataFrame(b,columns=b_cols)
print(df_b)
#apply
df_b[['c2','c3']] = df_b[['c2','c3']].apply(pd.to_numeric) 
print(df_b.dtypes)

#astype
df_b[['c2','c3']] = df_b[['c2','c3']].astype(float) 
print(df_b.dtypes)
```

      c1  c2   c3
    0  a  10  100
    1  b  20  200
    c1    object
    c2     int64
    c3     int64
    dtype: object
    c1     object
    c2    float64
    c3    float64
    dtype: object
    

```python
b = [['a',10,100.11],['b',20,200.22]]
b_cols = ['c1','c2','c3']
df_b = pd.DataFrame(b,columns=b_cols)
print(df_b)
#infer_objects
df_b = df_b.infer_objects()
df_b.dtypes
```

      c1  c2      c3
    0  a  10  100.11
    1  b  20  200.22
    

    c1     object
    c2      int64
    c3    float64
    dtype: object


## Delete column from pandas DataFrame

```python
import pandas as pd
import numpy as np
```


```python
df_a = pd.DataFrame({
    'b': np.random.choice([3,6,9,np.nan],5),
    'c': np.random.choice(['aa','bb','cc'],5)
})
df_a
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
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9.0</td>
      <td>bb</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.0</td>
      <td>bb</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.0</td>
      <td>bb</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9.0</td>
      <td>aa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3.0</td>
      <td>aa</td>
    </tr>
  </tbody>
</table>
</div>




```python
del df_a['b']
df_a
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
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>bb</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bb</td>
    </tr>
    <tr>
      <th>2</th>
      <td>bb</td>
    </tr>
    <tr>
      <th>3</th>
      <td>aa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>aa</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_b = df_a.columns.drop('c',1)
df_b
```

    Index([], dtype='object')

## Difference between map, applymap and apply methods 

```python
df_a = pd.DataFrame(np.random.randn(5,5),columns=list('abcde'),index=['i1','i2','i3','i4','i5'])
df_a
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>i1</th>
      <td>0.457927</td>
      <td>-1.367125</td>
      <td>0.098990</td>
      <td>0.209963</td>
      <td>-0.554169</td>
    </tr>
    <tr>
      <th>i2</th>
      <td>-1.804671</td>
      <td>1.012876</td>
      <td>-0.745989</td>
      <td>-0.287747</td>
      <td>-1.420975</td>
    </tr>
    <tr>
      <th>i3</th>
      <td>0.198437</td>
      <td>0.498339</td>
      <td>0.542231</td>
      <td>-0.427908</td>
      <td>0.482417</td>
    </tr>
    <tr>
      <th>i4</th>
      <td>0.492413</td>
      <td>-0.857842</td>
      <td>-0.978174</td>
      <td>-0.236399</td>
      <td>0.789442</td>
    </tr>
    <tr>
      <th>i5</th>
      <td>-0.845942</td>
      <td>-0.236043</td>
      <td>-1.652472</td>
      <td>-0.878454</td>
      <td>0.677147</td>
    </tr>
  </tbody>
</table>
</div>




```python
# applying a function on 1D arrays to each column or row
f_max = lambda x: x.max()
print(df_a.apply(f_max))

f_min = lambda x: x.min()
print(df_a.apply(f_min))
```

    a    0.492413
    b    1.012876
    c    0.542231
    d    0.209963
    e    0.789442
    dtype: float64
    a   -1.804671
    b   -1.367125
    c   -1.652472
    d   -0.878454
    e   -1.420975
    dtype: float64
    


```python
#  Element-wise Python functions
# applymap
f_format = lambda x: '%.2f' % x

print(df_a.applymap(f_format))
```

            a      b      c      d      e
    i1   0.46  -1.37   0.10   0.21  -0.55
    i2  -1.80   1.01  -0.75  -0.29  -1.42
    i3   0.20   0.50   0.54  -0.43   0.48
    i4   0.49  -0.86  -0.98  -0.24   0.79
    i5  -0.85  -0.24  -1.65  -0.88   0.68
    


```python
#a map method for applying an element-wise function:
df_a['a'].map(f_format)
```




    i1     0.46
    i2    -1.80
    i3     0.20
    i4     0.49
    i5    -0.85
    Name: a, dtype: object
    
## How to create sample datasets


```python
int_array_length = 6

df_sample = pd.DataFrame({
#1
    'a': np.random.randn(int_array_length),    
#2
    'b': np.random.choice([3,6,9,np.nan],int_array_length),
#3
    'c': np.random.choice(['aa','bb','cc'],int_array_length),
#4
    'd': np.repeat(range(3),2),
#5
    'e': np.tile(range(2),3),
#6
    'f': pd.date_range('1/1/2018',periods=int_array_length),
#7
    'g': np.random.choice(pd.date_range('1/1/2018',periods=365),6,replace=False)
    
})

#    'c': np.random.choice(['aa','bb','cc'],10)
#ValueError: arrays must all be same length
df_sample
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
      <th>f</th>
      <th>g</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.889095</td>
      <td>9.0</td>
      <td>bb</td>
      <td>0</td>
      <td>0</td>
      <td>2018-01-01</td>
      <td>2018-01-12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.158926</td>
      <td>3.0</td>
      <td>bb</td>
      <td>0</td>
      <td>1</td>
      <td>2018-01-02</td>
      <td>2018-05-31</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.224386</td>
      <td>9.0</td>
      <td>cc</td>
      <td>1</td>
      <td>0</td>
      <td>2018-01-03</td>
      <td>2018-08-24</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.422842</td>
      <td>9.0</td>
      <td>cc</td>
      <td>1</td>
      <td>1</td>
      <td>2018-01-04</td>
      <td>2018-10-05</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.425356</td>
      <td>6.0</td>
      <td>cc</td>
      <td>2</td>
      <td>0</td>
      <td>2018-01-05</td>
      <td>2018-09-09</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-1.196346</td>
      <td>6.0</td>
      <td>cc</td>
      <td>2</td>
      <td>1</td>
      <td>2018-01-06</td>
      <td>2018-10-12</td>
    </tr>
  </tbody>
</table>
</div>




```python
stocks = pd.DataFrame({ 
    'ticker':np.repeat( ['aapl','goog','yhoo','msft'], 25 ),
    'date':np.tile( pd.date_range('1/1/2011', periods=25, freq='D'), 4 ),
    'price':(np.random.randn(100).cumsum() + 10) })

stocks.head(5)
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
      <th>date</th>
      <th>price</th>
      <th>ticker</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2011-01-01</td>
      <td>10.133112</td>
      <td>aapl</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2011-01-02</td>
      <td>9.085344</td>
      <td>aapl</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2011-01-03</td>
      <td>9.125731</td>
      <td>aapl</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2011-01-04</td>
      <td>11.127179</td>
      <td>aapl</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2011-01-05</td>
      <td>11.721340</td>
      <td>aapl</td>
    </tr>
  </tbody>
</table>
</div>

## How to deal with SettingWithCopyWarning 

```python
df[df['a']>10]['b'] = 100
df
```

    c:\python36\lib\site-packages\ipykernel_launcher.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """Entry point for launching an IPython kernel.
    




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
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20</td>
      <td>22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>30</td>
      <td>30</td>
    </tr>
  </tbody>
</table>
</div>




```python
#.loc[] is primarily label based, but may also be used with a boolean array.
df.loc[df['a']>10,'b'] = 100
df
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
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20</td>
      <td>100</td>
    </tr>
    <tr>
      <th>2</th>
      <td>30</td>
      <td>100</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_a = pd.DataFrame(np.random.randn(5,2),columns=list('XY'))
df_a
df_b = df_a.iloc[:,[1,0]]
df_b.is_copy = True
df_b['X']/= 2

```

    c:\python36\lib\site-packages\ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """
    


```python
df_b.is_copy = False
df_b['X']/= 2
```


