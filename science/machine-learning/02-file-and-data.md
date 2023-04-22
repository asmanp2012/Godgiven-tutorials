# 02 - file and data

Most machine learning models are trained using data from files. The most popular file formats for ML, are .tfrecords, .csv, .npy, and .petastorm, as well as the file formats used to store models, such as .pb and .pkl .

For this reason first of things, we should learn how to read files and data.

I prefer two way for read file and data.

## 1- with `open` function of python

You should open the file with `open` function like below

```python
path = r"./file.txt"

with open(path,'r') as f:
   text = f.readlines()
print(text)
```

### detect row's and column of csv with `numpy`

some time's the pandas not working correctly because file's have some `custom conditions` then `numpy is better` for that but when you have a `standard csv` you can do that easier with `panda` lib

### Detect row's and column's with `numpy` lib

```python
import csv
import numpy as np
path = r"./file.csv"

with open(path,'r') as f:
   reader = csv.reader(f,delimiter = ',')
   headers = next(reader)
   data = list(reader)
   data = np.array(data)
print(headers)
print(data.shape)
```

numpy can select custom `row` and `columns` with give params between `bracket`(`[]`)

- the first index is for row
- the seceond index is for column

attention to bellow semi code for better realized

```python
data = np.array()
data[row,column]
```

two way are for chose row and column

- a array with index of selects
- a string with this syntax `start:end:step` each one don't exist don't apply

attention to bellow semi code for better realized

```python
data = np.array()
print(data[0::1,0:5])
print(data[[0,3,4],0:])
print(data[0:5:2])
```

----

- The `0::1` string mean i want select row's `from 0 index` `to end` with `1 step by step`
- The `0:5` string mean i want select `column's that start from 0 index to end from 5 index`

----

- the `[0,3,4]` array mean i want select `row's that index's are 0, 3, 4`
- The `0:` string mean i want select `column's that start from 0 to end`

----

- The `0:5:2` string mean i want select `row's that start from 0 to end index 5 by 2 step by step`

## 2- with `panda` lib

```python
from pandas import read_csv
path = r"./file.csv"

data = read_csv(path, encoding='utf-8')
print(data.shape)
```
