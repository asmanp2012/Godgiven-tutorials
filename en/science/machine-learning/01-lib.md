# 01 - Library Dependencies

courses:

- [Tutorials point Machine Learning with Python](https://www.tutorialspoint.com/machine_learning_with_python/machine_learning_with_python_ecosystem.htm)
- [Python Machine Learning](https://www.w3schools.com/python/python_ml_getting_started.asp)

## Requirement

At the start of learning machine learning, we should install some packages that I put command of that below:

```bash
pip install notebook
pip install jupyter
pip install NumPy
pip install Pandas
pip install -U scikit-learn
pip install matplotlib
pip install seaborn
```

### jupyter notebook

Jupyter notebook is a server that you can run that and make markdown document and run code online on that. ( I hate from that because manage document and code manually is easier for me)

```bash
jupyter notebook
```

After the jupyter server was run you can see jupyter on the browser.

with click on the `new` button you can make `python` tab and start your writing code on that or make `Markdown`  tab and make document.

`new` -> `python`

### NumPy

NumPy is a library for the Python programming language, adding support for large, multi-dimensional arrays and matrices, along with a large collection of high-level mathematical functions to operate on these arrays.

```python
import numpy

arr = numpy.array([1, 2, 3, 4, 5])

print(arr)
```

You can learn more about numpy on [`w3s`](https://www.w3schools.com/python/numpy/default.asp).

### Pandas

Pandas is a software library written for the Python programming language for data manipulation and analysis. In particular, it offers data structures and operations for manipulating numerical tables and time series. It is free software released under the three-clause BSD license.

```python
import pandas

mydataset = {
  'cars': ["BMW", "Volvo", "Ford"],
  'passings': [3, 7, 2]
}

myvar = pandas.DataFrame(mydataset)

print(myvar)
```

You can learn more about panda on [`w3s`](https://www.w3schools.com/python/pandas/default.asp)

### scikit-learn

scikit-learn is a free software machine learning library for the Python programming language. It features various classification, regression and clustering algorithms including support-vector machines, random forests, gradient boosting, k-means and DBSCAN, and is designed to interoperate with the Python numerical and scientific libraries NumPy and SciPy. Scikit-learn is a NumFOCUS fiscally sponsored project.

```python
from sklearn.datasets import load_iris
iris = load_iris()
X = iris.data
y = iris.target
feature_names = iris.feature_names
target_names = iris.target_names
print("Feature names:", feature_names)
print("Target names:", target_names)
print("\nFirst 10 rows of X:\n", X[:10])
```

You can learn more about scikit-learn on [`Tutorials point`](https://www.tutorialspoint.com/scikit_learn/scikit_learn_modelling_process.htm)

### Matplotlib

Matplotlib is a plotting library for the Python programming language and its numerical mathematics extension NumPy. It provides an object-oriented API for embedding plots into applications using general-purpose GUI toolkits like Tkinter, wxPython, Qt, or GTK.

```python
import matplotlib.pyplot as plt
import numpy as np

xpoints = np.array([0, 6])
ypoints = np.array([0, 250])

plt.plot(xpoints, ypoints)
plt.show()
```

You can learn more about matplotlib on [`w3s`](https://www.w3schools.com/python/matplotlib_getting_started.asp)

### Seaborn

Seaborn is a Python data visualization library based on matplotlib. It provides a high-level interface for drawing attractive and informative statistical graphics.

```python
import matplotlib.pyplot as plt
import seaborn as sns

sns.distplot([0, 1, 2, 3, 4, 5])

plt.show()
```

You can learn more about Seaborn on [`Tutorials point`](https://www.tutorialspoint.com/seaborn/index.htm)
