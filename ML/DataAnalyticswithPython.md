## 100 Data Analytics with Python Interview Questions and Answers

### 1. How do you read a CSV file into a DataFrame using pandas?
**Answer:**  
You can read a CSV file using the `pd.read_csv` function. *(Self-contained mock setup below)*:
```python
import pandas as pd
import io

# Mocking a CSV file using StringIO
csv_data = """column
5
12
3
18"""

df = pd.read_csv(io.StringIO(csv_data))
print(df.head())
# Output:
#    column
# 0       5
# 1      12
# 2       3
# 3      18
```
---

### 2. How do you handle missing values in a DataFrame using pandas?
**Answer:**  
You can handle missing values using the `fillna` or `dropna` methods.
```python
import pandas as pd
import numpy as np

# Concrete DataFrame with missing values
df = pd.DataFrame({'column': [5, np.nan, 3, np.nan]})

df_filled = df.fillna(0)
print(df_filled)
# Output:
#    column
# 0     5.0
# 1     0.0
# 2     3.0
# 3     0.0

df_dropped = df.dropna()
print(df_dropped)
# Output:
#    column
# 0     5.0
# 2     3.0
```
---

### 3. How do you filter rows in a DataFrame based on a condition?
**Answer:**  
You can filter rows using boolean indexing.
```python
import pandas as pd

df = pd.DataFrame({'column': [5, 12, 3, 18]})
filtered_df = df[df['column'] > 10]
print(filtered_df)
# Output:
#    column
# 1      12
# 3      18
```
---

### 4. How do you compute summary statistics for a DataFrame using pandas?
**Answer:**  
You can compute summary statistics using the `describe` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [10, 20, 30]})
summary_stats = df.describe()
print(summary_stats)
# Output:
#           column
# count   3.000000
# mean   20.000000
# std    10.000000
# min    10.000000
# 25%    15.000000
# 50%    20.000000
# 75%    25.000000
# max    30.000000
```
---

### 5. How do you group data in a DataFrame and compute aggregate statistics?
**Answer:**  
You can group data using the `groupby` method and compute aggregate statistics with `agg`.
```python
import pandas as pd

df = pd.DataFrame({'column': ['A', 'A', 'B', 'B'], 'another_column': [10, 20, 30, 40]})
grouped_df = df.groupby('column').agg({'another_column': 'mean'})
print(grouped_df)
# Output:
#         another_column
# column                
# A                 15.0
# B                 35.0
```
---

### 6. How do you merge two DataFrames in pandas?
**Answer:**  
You can merge DataFrames using the `merge` function.
```python
import pandas as pd

df1 = pd.DataFrame({'common_column': [1, 2], 'val1': [10, 20]})
df2 = pd.DataFrame({'common_column': [1, 2], 'val2': [100, 200]})
merged_df = pd.merge(df1, df2, on='common_column')
print(merged_df.head())
# Output:
#    common_column  val1  val2
# 0              1    10   100
# 1              2    20   200
```
---

### 7. How do you concatenate multiple DataFrames in pandas?
**Answer:**  
You can concatenate DataFrames using the `pd.concat` function.
```python
import pandas as pd

df1 = pd.DataFrame({'column': [1, 2]})
df2 = pd.DataFrame({'column': [3, 4]})
concatenated_df = pd.concat([df1, df2], axis=0)
print(concatenated_df.head())
# Output:
#    column
# 0       1
# 1       2
# 0       3
# 1       4
```
---

### 8. How do you pivot a DataFrame in pandas?
**Answer:**  
You can pivot a DataFrame using the `pivot_table` method.
```python
import pandas as pd

df = pd.DataFrame({'column1': ['A', 'A', 'B'], 'column2': ['X', 'Y', 'X'], 'value_column': [1, 2, 3]})
pivot_df = df.pivot_table(index='column1', columns='column2', values='value_column')
print(pivot_df)
# Output:
# column2    X    Y
# column1          
# A        1.0  2.0
# B        3.0  NaN
```
---

### 9. How do you melt a DataFrame in pandas?
**Answer:**  
You can melt a DataFrame using the `melt` method.
```python
import pandas as pd

df = pd.DataFrame({'id': [1], 'column1': [10], 'column2': [20]})
melted_df = df.melt(id_vars=['id'], value_vars=['column1', 'column2'])
print(melted_df)
# Output:
#    id variable  value
# 0   1  column1     10
# 1   1  column2     20
```
---

### 10. How do you create a pivot table in pandas?
**Answer:**  
You can create a pivot table using the `pivot_table` method.
```python
import pandas as pd

df = pd.DataFrame({'column1': ['A', 'A'], 'column2': ['X', 'X'], 'value_column': [10, 20]})
pivot_table = df.pivot_table(index='column1', columns='column2', values='value_column', aggfunc='mean')
print(pivot_table)
# Output:
# column2     X
# column1      
# A        15.0
```
---

### 11. How do you apply a function to a DataFrame column using pandas?
**Answer:**  
You can apply a function using the `apply` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [1, 2, 3]})
df['new_column'] = df['column'].apply(lambda x: x * 2)
print(df.head())
# Output:
#    column  new_column
# 0       1           2
# 1       2           4
# 2       3           6
```
---

### 12. How do you apply a function to a DataFrame row using pandas?
**Answer:**  
You can apply a function using the `apply` method with `axis=1`.
```python
import pandas as pd

df = pd.DataFrame({'column1': [1, 2], 'column2': [10, 20]})
df['new_column'] = df.apply(lambda row: row['column1'] + row['column2'], axis=1)
print(df.head())
# Output:
#    column1  column2  new_column
# 0        1       10          11
# 1        2       20          22
```
---

### 13. How do you apply a function to a group in a DataFrame using pandas?
**Answer:**  
You can aggregate groups directly using group functions like `.mean()` or by using `.agg()`.
```python
import pandas as pd

df = pd.DataFrame({'column': ['A', 'A', 'B'], 'value': [10, 20, 30]})
grouped_df = df.groupby('column').mean(numeric_only=True)
print(grouped_df)
# Output:
#         value
# column       
# A        15.0
# B        30.0
```
---

### 14. How do you create a new column based on conditions in pandas?
**Answer:**  
You can create a new column using `np.where` or `pd.Series.apply`.
```python
import pandas as pd
import numpy as np

df = pd.DataFrame({'column': [5, 15]})
df['new_column'] = np.where(df['column'] > 10, 'high', 'low')
print(df.head())
# Output:
#    column new_column
# 0       5        low
# 1      15       high
```
---

### 15. How do you create a scatter plot using pandas?
**Answer:**  
You can create a scatter plot using the `plot.scatter` method.
```python
import pandas as pd

df = pd.DataFrame({'column1': [1, 2, 3], 'column2': [10, 20, 30]})
# Generates a matplotlib Axes object mapping points (1,10), (2,20), (3,30)
ax = df.plot.scatter(x='column1', y='column2')
```
---

### 16. How do you create a line plot using pandas?
**Answer:**  
You can create a line plot using the `plot.line` method.
```python
import pandas as pd

df = pd.DataFrame({'column1': [1, 2, 3], 'column2': [5, 15, 10]})
# Draws a line connecting coordinates across index or columns
ax = df.plot.line(x='column1', y='column2')
```
---

### 17. How do you create a histogram using pandas?
**Answer:**  
You can create a histogram using the `plot.hist` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [1, 2, 2, 3, 3, 3, 4]})
# Visualizes the frequency distribution of 'column'
ax = df['column'].plot.hist()
```
---

### 18. How do you create a box plot using pandas?
**Answer:**  
You can create a box plot using the `plot.box` method.
```python
import pandas as pd

df = pd.DataFrame({'column1': [1, 2, 3, 4, 5], 'column2': [10, 20, 30, 40, 50]})
# Displays the distribution statistics (median, quartiles, outliers)
ax = df.plot.box()
```
---

### 19. How do you create a bar plot using pandas?
**Answer:**  
You can create a bar plot using the `plot.bar` method.
```python
import pandas as pd

df = pd.DataFrame({'column1': ['A', 'B', 'C'], 'column2': [10, 30, 20]})
# Renders a vertical bar chart matching category values
ax = df.plot.bar(x='column1', y='column2')
```
---

### 20. How do you create a pie chart using pandas?
**Answer:**  
You can create a pie chart using the `plot.pie` method.
```python
import pandas as pd

df = pd.DataFrame({'column': ['Yes', 'Yes', 'No', 'Yes']})
# Generates a visual breakdown proportional to value distributions (Yes: 75%, No: 25%)
ax = df['column'].value_counts().plot.pie()
```
---

### 21. How do you save a DataFrame to a CSV file using pandas?
**Answer:**  
You can save a DataFrame using the `to_csv` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [1, 2, 3]})
# Writes contents out to 'output.csv'
df.to_csv('output.csv', index=False)
```
---

### 22. How do you save a DataFrame to an Excel file using pandas?
**Answer:**  
You can save a DataFrame using the `to_excel` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [1, 2, 3]})
# Saves table structured as spreadsheet rows
# df.to_excel('output.xlsx', index=False)
```
---

### 23. How do you load an Excel file into a DataFrame using pandas?
**Answer:**  
You can load an Excel file using the `pd.read_excel` function.
```python
import pandas as pd

# Mocking the loaded structure of an Excel workbook
df = pd.DataFrame({'Excel_Col': [100, 200]})
print(df.head())
# Output:
#    Excel_Col
# 0        100
# 1        200
```
---

### 24. How do you read specific columns from a CSV file using pandas?
**Answer:**  
You can read specific columns using the `usecols` parameter in `pd.read_csv`.
```python
import pandas as pd
import io

csv_data = "column1,column2,column3\n1,10,100\n2,20,200"
df = pd.read_csv(io.StringIO(csv_data), usecols=['column1', 'column2'])
print(df.head())
# Output:
#    column1  column2
# 0        1       10
# 1        2       20
```
---

### 25. How do you rename columns in a DataFrame using pandas?
**Answer:**  
You can rename columns using the `rename` method by assigning the returned DataFrame.
```python
import pandas as pd

df = pd.DataFrame({'old_name': [1, 2]})
df = df.rename(columns={'old_name': 'new_name'})
print(df.head())
# Output:
#    new_name
# 0         1
# 1         2
```
---

### 26. How do you drop columns from a DataFrame using pandas?
**Answer:**  
You can drop columns using the `drop` method.
```python
import pandas as pd

df = pd.DataFrame({'keep': [1, 2], 'column_to_drop': [10, 20]})
df = df.drop(columns=['column_to_drop'])
print(df.head())
# Output:
#    keep
# 0     1
# 1     2
```
---

### 27. How do you drop duplicate rows in a DataFrame using pandas?
**Answer:**  
You can drop duplicate rows using the `drop_duplicates` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [1, 2, 2, 3]})
df = df.drop_duplicates()
print(df.head())
# Output:
#    column
# 0       1
# 1       2
# 3       3
```
---

### 28. How do you sort a DataFrame by a column in ascending order using pandas?
**Answer:**  
You can sort a DataFrame using the `sort_values` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [3, 1, 2]})
df = df.sort_values(by='column', ascending=True)
print(df.head())
# Output:
#    column
# 1       1
# 2       2
# 0       3
```
---

### 29. How do you sort a DataFrame by multiple columns using pandas?
**Answer:**  
You can sort a DataFrame by multiple columns using the `sort_values` method.
```python
import pandas as pd

df = pd.DataFrame({'column1': [1, 1, 2], 'column2': [10, 20, 5]})
df = df.sort_values(by=['column1', 'column2'], ascending=[True, False])
print(df.head())
# Output:
#    column1  column2
# 1        1       20
# 0        1       10
# 2        2        5
```
---

### 30. How do you set a column as the index of a DataFrame using pandas?
**Answer:**  
You can set a column as the index using the `set_index` method.
```python
import pandas as pd

df = pd.DataFrame({'column': ['idx1', 'idx2'], 'value': [10, 20]})
df = df.set_index('column')
print(df.head())
# Output:
#         value
# column       
# idx1       10
# idx2       20
```
---

### 31. How do you reset the index of a DataFrame using pandas?
**Answer:**  
You can reset the index using the `reset_index` method.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20]}, index=['idx1', 'idx2'])
df = df.reset_index()
print(df.head())
# Output:
#   index  value
# 0  idx1     10
# 1  idx2     20
```
---

### 32. How do you calculate the correlation matrix of a DataFrame using pandas?
**Answer:**  
You can calculate the correlation matrix using the `corr` method, specifying `numeric_only=True` to prevent type errors on non-numeric columns.
```python
import pandas as pd

df = pd.DataFrame({'A': [1, 2, 3], 'B': [10, 20, 29]})
correlation_matrix = df.corr(numeric_only=True)
print(correlation_matrix)
# Output:
#           A         B
# A  1.000000  0.993399
# B  0.993399  1.000000
```
---

### 33. How do you calculate the covariance matrix of a DataFrame using pandas?
**Answer:**  
You can calculate the covariance matrix using the `cov` method with `numeric_only=True`.
```python
import pandas as pd

df = pd.DataFrame({'A': [1, 2, 3], 'B': [10, 20, 30]})
covariance_matrix = df.cov(numeric_only=True)
print(covariance_matrix)
# Output:
#      A     B
# A  1.0  10.0
# B 10.0 100.0
```
---

### 34. How do you calculate the rolling mean of a DataFrame column using pandas?
**Answer:**  
You can calculate the rolling mean using the `rolling` and `mean` methods.
```python
import pandas as pd

df = pd.DataFrame({'column': [10, 20, 30, 40]})
df['rolling_mean'] = df['column'].rolling(window=3).mean()
print(df.head())
# Output:
#    column  rolling_mean
# 0      10           NaN
# 1      20           NaN
# 2      30          20.0
# 3      40          30.0
```
---

### 35. How do you calculate the exponential moving average of a DataFrame column using pandas?
**Answer:**  
You can calculate the exponential moving average using the `ewm` and `mean` methods.
```python
import pandas as pd

df = pd.DataFrame({'column': [10, 20, 30]})
df['ema'] = df['column'].ewm(span=3, adjust=False).mean()
print(df.head())
# Output:
#    column   ema
# 0      10  10.0
# 1      20  15.0
# 2      30  22.5
```
---

### 36. How do you calculate the cumulative sum of a DataFrame column using pandas?
**Answer:**  
You can calculate the cumulative sum using the `cumsum` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [1, 2, 3]})
df['cumsum'] = df['column'].cumsum()
print(df.head())
# Output:
#    column  cumsum
# 0       1       1
# 1       2       3
# 2       3       6
```
---

### 37. How do you calculate the cumulative product of a DataFrame column using pandas?
**Answer:**  
You can calculate the cumulative product using the `cumprod` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [1, 2, 3, 4]})
df['cumprod'] = df['column'].cumprod()
print(df.head())
# Output:
#    column  cumprod
# 0       1        1
# 1       2        2
# 2       3        6
# 3       4       24
```
---

### 38. How do you calculate the cumulative minimum of a DataFrame column using pandas?
**Answer:**  
You can calculate the cumulative minimum using the `cummin` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [5, 3, 8, 2]})
df['cummin'] = df['column'].cummin()
print(df.head())
# Output:
#    column  cummin
# 0       5       5
# 1       3       3
# 2       8       3
# 3       2       2
```
---

### 39. How do you calculate the cumulative maximum of a DataFrame column using pandas?
**Answer:**  
You can calculate the cumulative maximum using the `cummax` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [2, 7, 4, 9]})
df['cummax'] = df['column'].cummax()
print(df.head())
# Output:
#    column  cummax
# 0       2       2
# 1       7       7
# 2       4       7
# 3       9       9
```
---

### 40. How do you resample a time series DataFrame using pandas?
**Answer:**  
You can resample a time series using the `resample` method (using standard frequency aliases such as `'ME'` for month-end).
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20]}, index=pd.to_datetime(['2026-01-15', '2026-01-31']))
resampled_df = df.resample('ME').mean()
print(resampled_df.head())
# Output:
#             value
# 2026-01-31   15.0
```
---

### 41. How do you interpolate missing values in a DataFrame using pandas?
**Answer:**  
You can interpolate missing values using the `interpolate` method.
```python
import pandas as pd
import numpy as np

df = pd.DataFrame({'column': [1.0, np.nan, 3.0]})
df['column'] = df['column'].interpolate(method='linear')
print(df.head())
# Output:
#    column
# 0     1.0
# 1     2.0
# 2     3.0
```
---

### 42. How do you calculate the rank of a DataFrame column using pandas?
**Answer:**  
You can calculate the rank using the `rank` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [40, 10, 30]})
df['rank'] = df['column'].rank()
print(df.head())
# Output:
#    column  rank
# 0      40   3.0
# 1      10   1.0
# 2      30   2.0
```
---

### 43. How do you perform a rolling window correlation between two DataFrame columns using pandas?
**Answer:**  
You can perform a rolling window correlation using the `rolling` and `corr` methods.
```python
import pandas as pd

df = pd.DataFrame({
    'column1': [1, 2, 3, 4, 5],
    'column2': [2, 4, 5, 8, 10]
})
rolling_corr = df['column1'].rolling(window=3).corr(df['column2'])
print(rolling_corr.head())
# Output:
# 0         NaN
# 1         NaN
# 2    0.981981
# 3    0.981981
# 4    1.000000
```
---

### 44. How do you shift the values of a DataFrame column using pandas?
**Answer:**  
You can shift the values using the `shift` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [10, 20, 30]})
df['shifted_column'] = df['column'].shift(1)
print(df.head())
# Output:
#    column  shifted_column
# 0      10             NaN
# 1      20            10.0
# 2      30            20.0
```
---

### 45. How do you calculate the difference between successive rows of a DataFrame column using pandas?
**Answer:**  
You can calculate the difference using the `diff` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [10, 15, 30]})
df['diff'] = df['column'].diff()
print(df.head())
# Output:
#    column  diff
# 0      10   NaN
# 1      15   5.0
# 2      30  15.0
```
---

### 46. How do you calculate the percentage change between successive rows of a DataFrame column using pandas?
**Answer:**  
You can calculate the percentage change using the `pct_change` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [10, 20, 30]})
df['pct_change'] = df['column'].pct_change()
print(df.head())
# Output:
#    column  pct_change
# 0      10         NaN
# 1      20         1.0
# 2      30         0.5
```
---

### 47. How do you apply a lambda function to each row of a DataFrame using pandas?
**Answer:**  
You can apply a lambda function to each row using the `apply` method with `axis=1`.
```python
import pandas as pd

df = pd.DataFrame({'column1': [1, 2], 'column2': [3, 4]})
df['new_column'] = df.apply(lambda row: row['column1'] + row['column2'], axis=1)
print(df.head())
# Output:
#    column1  column2  new_column
# 0        1        3           4
# 1        2        4           6
```
---

### 48. How do you apply a lambda function to each element of a DataFrame column using pandas?
**Answer:**  
You can apply a lambda function to each element using the `apply` method.
```python
import pandas as pd

df = pd.DataFrame({'column': [10, 20]})
df['new_column'] = df['column'].apply(lambda x: x * 2)
print(df.head())
# Output:
#    column  new_column
# 0      10          20
# 1      20          40
```
---

### 49. How do you filter a DataFrame based on a string condition in a column using pandas?
**Answer:**  
You can filter based on a string condition using boolean indexing.
```python
import pandas as pd

df = pd.DataFrame({'column': ['apple', 'banana', 'apricot']})
filtered_df = df[df['column'].str.contains('ap')]
print(filtered_df.head())
# Output:
#     column
# 0    apple
# 2  apricot
```
---

### 50. How do you create dummy variables from a categorical column in a DataFrame using pandas?
**Answer:**  
You can create dummy variables using the `pd.get_dummies` function.
```python
import pandas as pd

df = pd.DataFrame({'categorical_column': ['A', 'B', 'A']})
dummies = pd.get_dummies(df['categorical_column'])
print(dummies.head())
# Output:
#        A      B
# 0   True  False
# 1  False   True
# 2   True  False
```
---

### 51. How do you merge DataFrames with different keys using pandas?
**Answer:**  
You can merge DataFrames with different keys using the `pd.merge` function and specifying the `left_on` and `right_on` parameters.
```python
import pandas as pd

df1 = pd.DataFrame({'key1': [1, 2, 3], 'value1': ['a', 'b', 'c']})
df2 = pd.DataFrame({'key2': [1, 2, 3], 'value2': ['x', 'y', 'z']})
merged_df = pd.merge(df1, df2, left_on='key1', right_on='key2')
print(merged_df)
# Output:
#    key1 value1  key2 value2
# 0     1      a     1      x
# 1     2      b     2      y
# 2     3      c     3      z
```
---

### 52. How do you apply a function to multiple columns of a DataFrame using pandas?
**Answer:**  
You can apply a function to multiple columns using the `apply` method with `axis=1`.
```python
import pandas as pd

df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})
df['C'] = df.apply(lambda row: row['A'] + row['B'], axis=1)
print(df)
# Output:
#    A  B  C
# 0  1  4  5
# 1  2  5  7
# 2  3  6  9
```
---

### 53. How do you compute the moving average of a DataFrame column using pandas?
**Answer:**  
You can compute the moving average using the `rolling` and `mean` methods.
```python
import pandas as pd

df = pd.DataFrame({'A': [1, 2, 3, 4, 5]})
df['moving_avg'] = df['A'].rolling(window=3).mean()
print(df)
# Output:
#    A  moving_avg
# 0  1         NaN
# 1  2         NaN
# 2  3         2.0
# 3  4         3.0
# 4  5         4.0
```
---

### 54. How do you merge multiple DataFrames on a common key using pandas?
**Answer:**  
You can merge multiple DataFrames using the `pd.merge` function in a loop or reduce.
```python
import pandas as pd
from functools import reduce

dfs = [pd.DataFrame({'key': [1, 2, 3], 'value_1': ['a', 'b', 'c']}), 
       pd.DataFrame({'key': [1, 2, 3], 'value_2': ['x', 'y', 'z']}), 
       pd.DataFrame({'key': [1, 2, 3], 'value_3': ['u', 'v', 'w']})]
merged_df = reduce(lambda left, right: pd.merge(left, right, on='key'), dfs)
print(merged_df)
# Output:
#    key value_1 value_2 value_3
# 0    1       a       x       u
# 1    2       b       y       v
# 2    3       c       z       w
```
---

### 55. How do you remove outliers from a DataFrame using pandas?
**Answer:**  
You can remove outliers by filtering based on a condition such as the Interquartile Range (IQR).
```python
import pandas as pd

df = pd.DataFrame({'A': [1, 2, 3, 4, 100]})
q1 = df['A'].quantile(0.25)
q3 = df['A'].quantile(0.75)
iqr = q3 - q1
filtered_df = df[(df['A'] >= (q1 - 1.5 * iqr)) & (df['A'] <= (q3 + 1.5 * iqr))]
print(filtered_df)
# Output:
#    A
# 0  1
# 1  2
# 2  3
# 3  4
```
---

### 56. How do you reshape a DataFrame from long to wide format using pandas?
**Answer:**  
You can reshape using the `pivot` method.
```python
import pandas as pd

df = pd.DataFrame({'date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02'],
                   'variable': ['A', 'B', 'A', 'B'], 'value': [1, 2, 3, 4]})
wide_df = df.pivot(index='date', columns='variable', values='value')
print(wide_df)
# Output:
# variable    A  B
# date            
# 2021-01-01  1  2
# 2021-01-02  3  4
```
---

### 57. How do you stack a DataFrame from wide to long format using pandas?
**Answer:**  
You can stack using the `melt` method.
```python
import pandas as pd

df = pd.DataFrame({'date': ['2021-01-01', '2021-01-02'],
                   'A': [1, 3], 'B': [2, 4]})
long_df = df.melt(id_vars='date', value_vars=['A', 'B'])
print(long_df)
# Output:
#          date variable  value
# 0  2021-01-01        A      1
# 1  2021-01-02        A      3
# 2  2021-01-01        B      2
# 3  2021-01-02        B      4
```
---

### 58. How do you impute missing values with the mean of a column using pandas?
**Answer:**  
You can impute missing values using `fillna` reassignment.
```python
import pandas as pd

df = pd.DataFrame({'A': [1, 2, None, 4, 5]})
df['A'] = df['A'].fillna(df['A'].mean())
print(df)
# Output:
#      A
# 0  1.0
# 1  2.0
# 2  3.0
# 3  4.0
# 4  5.0
```
---

### 59. How do you group by multiple columns and apply a function using pandas?
**Answer:**  
You can group by multiple columns using the `groupby` method and apply aggregate operations directly.
```python
import pandas as pd

df = pd.DataFrame({'A': ['foo', 'bar', 'foo', 'bar'], 'B': ['one', 'one', 'two', 'two'], 'C': [1, 2, 3, 4]})
grouped_df = df.groupby(['A', 'B']).sum()
print(grouped_df)
# Output:
#          C
# A   B     
# bar one  2
#     two  4
# foo one  1
#     two  3
```
---

### 60. How do you concatenate DataFrames with different indices using pandas?
**Answer:**  
You can concatenate DataFrames with different indices using the `pd.concat` function.
```python
import pandas as pd

df1 = pd.DataFrame({'A': [1, 2, 3]}, index=[0, 1, 2])
df2 = pd.DataFrame({'B': [4, 5, 6]}, index=[2, 3, 4])
concatenated_df = pd.concat([df1, df2], axis=1)
print(concatenated_df)
# Output:
#      A    B
# 0  1.0  NaN
# 1  2.0  NaN
# 2  3.0  4.0
# 3  NaN  5.0
# 4  NaN  6.0
```
---

### 61. How do you convert a column to datetime in pandas?
**Answer:**  
You can convert a column to datetime using the `pd.to_datetime` function.
```python
import pandas as pd

df = pd.DataFrame({'date': ['2021-01-01', '2021-01-02']})
df['date'] = pd.to_datetime(df['date'])
print(df)
# Output:
#         date
# 0 2021-01-01
# 1 2021-01-02
```
---

### 62. How do you extract the year from a datetime column in pandas?
**Answer:**  
You can extract the year using the `.dt` accessor.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01', '2022-01-02'])})
df['year'] = df['date'].dt.year
print(df)
# Output:
#         date  year
# 0 2021-01-01  2021
# 1 2022-01-02  2022
```
---

### 63. How do you extract the month from a datetime column in pandas?
**Answer:**  
You can extract the month using the `.dt` accessor.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01', '2021-02-01'])})
df['month'] = df['date'].dt.month
print(df)
# Output:
#         date  month
# 0 2021-01-01      1
# 1 2021-02-01      2
```
---

### 64. How do you extract the day from a datetime column in pandas?
**Answer:**  
You can extract the day using the `.dt` accessor.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-05', '2021-01-06'])})
df['day'] = df['date'].dt.day
print(df)
# Output:
#         date  day
# 0 2021-01-05    5
# 1 2021-01-06    6
```
---

### 65. How do you calculate the time difference between two datetime columns in pandas?
**Answer:**  
You can calculate the time difference using subtraction.
```python
import pandas as pd

df = pd.DataFrame({'start': pd.to_datetime(['2021-01-01', '2021-01-02']), 'end': pd.to_datetime(['2021-01-02', '2021-01-04'])})
df['diff'] = df['end'] - df['start']
print(df)
# Output:
#        start        end   diff
# 0 2021-01-01 2021-01-02 1 days
# 1 2021-01-02 2021-01-04 2 days
```
---

### 66. How do you round a datetime column to the nearest hour in pandas?
**Answer:**  
You can round a datetime column using the `dt.round` method with lowercase offset string `'h'`.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01 12:30:00', '2021-01-01 12:45:00'])})
df['rounded_date'] = df['date'].dt.round('h')
print(df)
# Output:
#                  date        rounded_date
# 0 2021-01-01 12:30:00 2021-01-01 12:00:00
# 1 2021-01-01 12:45:00 2021-01-01 13:00:00
```
---

### 67. How do you round a datetime column to the nearest day in pandas?
**Answer:**  
You can round a datetime column using the `dt.round` method with lowercase offset string `'d'`.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01 11:30:00', '2021-01-01 23:45:00'])})
df['rounded_date'] = df['date'].dt.round('d')
print(df)
# Output:
#                  date rounded_date
# 0 2021-01-01 11:30:00   2021-01-01
# 1 2021-01-01 23:45:00   2021-01-02
```
---

### 68. How do you round a datetime column to the nearest minute in pandas?
**Answer:**  
You can round a datetime column using the `dt.round` method.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01 12:30:29', '2021-01-01 12:45:45'])})
df['rounded_date'] = df['date'].dt.round('min')
print(df)
# Output:
#                  date         rounded_date
# 0 2021-01-01 12:30:29  2021-01-01 12:30:00
# 1 2021-01-01 12:45:45  2021-01-01 12:46:00
```
---

### 69. How do you set a datetime column as the index of a DataFrame in pandas?
**Answer:**  
You can set a datetime column as the index using the `set_index` method.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01', '2021-01-02']), 'value': [1, 2]})
df = df.set_index('date')
print(df)
# Output:
#             value
# date             
# 2021-01-01      1
# 2021-01-02      2
```
---

### 70. How do you create a time series plot using pandas?
**Answer:**  
You can create a time series plot using the `plot` method.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01', '2021-01-02']), 'value': [1, 2]})
df = df.set_index('date')
# Plots dynamic data tracking over index datetime timestamps
ax = df.plot()
```
---

### 71. How do you calculate the year-to-date (YTD) sum of a DataFrame column using pandas?
**Answer:**  
You can calculate the YTD sum using the `cumsum` method.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01', '2021-01-02', '2021-01-03']), 'value': [10, 20, 30]})
df['ytd_sum'] = df['value'].cumsum()
print(df)
# Output:
#         date  value  ytd_sum
# 0 2021-01-01     10       10
# 1 2021-01-02     20       30
# 2 2021-01-03     30       60
```
---

### 72. How do you calculate the month-to-date (MTD) sum of a DataFrame column using pandas?
**Answer:**  
You can calculate the MTD sum using the `groupby` and `cumsum` methods.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01', '2021-01-02', '2021-02-01', '2021-02-02']), 'value': [1, 2, 3, 4]})
df['month'] = df['date'].dt.to_period('M')
df['mtd_sum'] = df.groupby('month')['value'].cumsum()
print(df)
# Output:
#         date  value   month  mtd_sum
# 0 2021-01-01      1 2021-01        1
# 1 2021-01-02      2 2021-01        3
# 2 2021-02-01      3 2021-02        3
# 3 2021-02-02      4 2021-02        7
```
---

### 73. How do you calculate the week-to-date (WTD) sum of a DataFrame column using pandas?
**Answer:**  
You can calculate the WTD sum using the `groupby` and `cumsum` methods.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01', '2021-01-02', '2021-01-04', '2021-01-05']), 'value': [1, 2, 3, 4]})
df['week'] = df['date'].dt.to_period('W')
df['wtd_sum'] = df.groupby('week')['value'].cumsum()
print(df)
# Output:
#         date  value                  week  wtd_sum
# 0 2021-01-01      1 2020-12-28/2021-01-03        1
# 1 2021-01-02      2 2020-12-28/2021-01-03        3
# 2 2021-01-04      3 2021-01-04/2021-01-10        3
# 3 2021-01-05      4 2021-01-04/2021-01-10        7
```
---

### 74. How do you calculate the quarter-to-date (QTD) sum of a DataFrame column using pandas?
**Answer:**  
You can calculate the QTD sum using the `groupby` and `cumsum` methods.
```python
import pandas as pd

df = pd.DataFrame({'date': pd.to_datetime(['2021-01-01', '2021-01-02', '2021-04-01', '2021-04-02']), 'value': [1, 2, 3, 4]})
df['quarter'] = df['date'].dt.to_period('Q')
df['qtd_sum'] = df.groupby('quarter')['value'].cumsum()
print(df)
# Output:
#         date  value  quarter  qtd_sum
# 0 2021-01-01      1   2021Q1        1
# 1 2021-01-02      2   2021Q1        3
# 2 2021-04-01      3   2021Q2        3
# 3 2021-04-02      4   2021Q2        7
```
---

### 75. How do you calculate the rolling sum of a DataFrame column using pandas?
**Answer:**  
You can calculate the rolling sum using the `rolling` and `sum` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [1, 2, 3, 4, 5]})
df['rolling_sum'] = df['value'].rolling(window=3).sum()
print(df)
# Output:
#    value  rolling_sum
# 0      1          NaN
# 1      2          NaN
# 2      3          6.0
# 3      4          9.0
# 4      5         12.0
```
---

### 76. How do you calculate the rolling average of a DataFrame column using pandas?
**Answer:**  
You can calculate the rolling average using the `rolling` and `mean` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [1, 2, 3, 4, 5]})
df['rolling_avg'] = df['value'].rolling(window=3).mean()
print(df)
# Output:
#    value  rolling_avg
# 0      1          NaN
# 1      2          NaN
# 2      3          2.0
# 3      4          3.0
# 4      5          4.0
```
---

### 77. How do you calculate the rolling standard deviation of a DataFrame column using pandas?
**Answer:**  
You can calculate the rolling standard deviation using the `rolling` and `std` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 10, 20, 10]})
df['rolling_std'] = df['value'].rolling(window=3).std()
print(df)
# Output:
#    value  rolling_std
# 0     10          NaN
# 1     20          NaN
# 2     10     5.773503
# 3     20     5.773503
# 4     10     5.773503
```
---

### 78. How do you calculate the exponentially weighted mean of a DataFrame column using pandas?
**Answer:**  
You can calculate the exponentially weighted mean using the `ewm` and `mean` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30]})
df['ewm_mean'] = df['value'].ewm(span=3, adjust=False).mean()
print(df)
# Output:
#    value  ewm_mean
# 0     10      10.0
# 1     20      15.0
# 2     30      22.5
```
---

### 79. How do you calculate the exponentially weighted standard deviation of a DataFrame column using pandas?
**Answer:**  
You can calculate the exponentially weighted standard deviation using the `ewm` and `std` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30]})
df['ewm_std'] = df['value'].ewm(span=3, adjust=False).std()
print(df)
# Output:
#    value    ewm_std
# 0     10        NaN
# 1     20   7.071068
# 2     30  10.606602
```
---

### 80. How do you calculate the lagged difference of a DataFrame column using pandas?
**Answer:**  
You can calculate the lagged difference using the `shift` and `diff` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 15, 25, 40]})
df['lagged_diff'] = df['value'].shift(1).diff()
print(df)
# Output:
#    value  lagged_diff
# 0     10          NaN
# 1     15          NaN
# 2     25          5.0
# 3     40         10.0
```
---

### 81. How do you calculate the autocorrelation of a DataFrame column using pandas?
**Answer:**  
You can calculate the autocorrelation using the `autocorr` method.
```python
import pandas as pd

df = pd.DataFrame({'value': [1, 2, 3, 4, 5]})
autocorrelation = df['value'].autocorr()
print(autocorrelation)
# Output: 1.0
```
---

### 82. How do you create a time-lagged feature in a DataFrame using pandas?
**Answer:**  
You can create a time-lagged feature using the `shift` method.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30]})
df['lagged_value'] = df['value'].shift(1)
print(df)
# Output:
#    value  lagged_value
# 0     10           NaN
# 1     20          10.0
# 2     30          20.0
```
---

### 83. How do you calculate the rank of values within each group of a DataFrame column using pandas?
**Answer:**  
You can calculate the rank within each group using the `groupby` and `rank` methods.
```python
import pandas as pd

df = pd.DataFrame({'group': ['A', 'A', 'B', 'B'], 'value': [20, 10, 40, 30]})
df['rank_within_group'] = df.groupby('group')['value'].rank()
print(df)
# Output:
#   group  value  rank_within_group
# 0     A     20                2.0
# 1     A     10                1.0
# 2     B     40                2.0
# 3     B     30                1.0
```
---

### 84. How do you calculate the z-score of a DataFrame column using pandas?
**Answer:**  
You can calculate the z-score using the `mean` and `std` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30]})
df['z_score'] = (df['value'] - df['value'].mean()) / df['value'].std()
print(df)
# Output:
#    value   z_score
# 0     10 -1.000000
# 1     20  0.000000
# 2     30  1.000000
```
---

### 85. How do you calculate the rolling z-score of a DataFrame column using pandas?
**Answer:**  
You can calculate the rolling z-score using the `rolling`, `mean`, and `std` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 10, 20, 30]})
rolling_mean = df['value'].rolling(window=3).mean()
rolling_std = df['value'].rolling(window=3).std()
df['rolling_z_score'] = (df['value'] - rolling_mean) / rolling_std
print(df)
# Output:
#    value  rolling_z_score
# 0     10              NaN
# 1     20              NaN
# 2     10        -0.577350
# 3     20         0.577350
# 4     30         1.154701
```
---

### 86. How do you create a lagged feature for multiple time steps in a DataFrame using pandas?
**Answer:**  
You can create multiple lagged features using the `shift` method in a loop.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30, 40]})
for lag in range(1, 3):
    df[f'lag_{lag}'] = df['value'].shift(lag)
print(df)
# Output:
#    value  lag_1  lag_2
# 0     10    NaN    NaN
# 1     20   10.0    NaN
# 2     30   20.0   10.0
# 3     40   30.0   20.0
```
---

### 87. How do you calculate the cumulative sum of values within each group of a DataFrame column using pandas?
**Answer:**  
You can calculate the cumulative sum within each group using the `groupby` and `cumsum` methods.
```python
import pandas as pd

df = pd.DataFrame({'group': ['A', 'A', 'B', 'B'], 'value': [1, 2, 3, 4]})
df['cumsum_within_group'] = df.groupby('group')['value'].cumsum()
print(df)
# Output:
#   group  value  cumsum_within_group
# 0     A      1                    1
# 1     A      2                    3
# 2     B      3                    3
# 3     B      4                    7
```
---

### 88. How do you normalize a DataFrame column to a 0-1 range using pandas?
**Answer:**  
You can normalize a column using the min-max normalization formula.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30, 50]})
df['normalized'] = (df['value'] - df['value'].min()) / (df['value'].max() - df['value'].min())
print(df)
# Output:
#    value  normalized
# 0     10        0.00
# 1     20        0.25
# 2     30        0.50
# 3     50        1.00
```
---

### 89. How do you standardize a DataFrame column to have a mean of 0 and a standard deviation of 1 using pandas?
**Answer:**  
You can standardize a column using the z-score formula.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30]})
df['standardized'] = (df['value'] - df['value'].mean()) / df['value'].std()
print(df)
# Output:
#    value  standardized
# 0     10          -1.0
# 1     20           0.0
# 2     30           1.0
```
---

### 90. How do you apply a custom function to a rolling window in a DataFrame using pandas?
**Answer:**  
You can apply a custom function using the `rolling` and `apply` methods, passing `raw=True` for fast NumPy array executions.
```python
import pandas as pd

df = pd.DataFrame({'value': [1, 5, 2, 10]})
df['custom_rolling'] = df['value'].rolling(window=3).apply(lambda x: x.max() - x.min(), raw=True)
print(df)
# Output:
#    value  custom_rolling
# 0      1             NaN
# 1      5             NaN
# 2      2             4.0
# 3     10             8.0
```
---

### 91. How do you calculate the exponentially weighted variance of a DataFrame column using pandas?
**Answer:**  
You can calculate the exponentially weighted variance using the `ewm` and `var` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30]})
df['ewm_var'] = df['value'].ewm(span=3, adjust=False).var()
print(df)
# Output:
#    value  ewm_var
# 0     10      NaN
# 1     20     50.0
# 2     30    112.5
```
---

### 92. How do you calculate the rolling correlation between two DataFrame columns using pandas?
**Answer:**  
You can calculate the rolling correlation using the `rolling` and `corr` methods.
```python
import pandas as pd

df = pd.DataFrame({'value1': [1, 2, 3, 4], 'value2': [4, 3, 2, 1]})
df['rolling_corr'] = df['value1'].rolling(window=3).corr(df['value2'])
print(df)
# Output:
#    value1  value2  rolling_corr
# 0       1       4           NaN
# 1       2       3           NaN
# 2       3       2          -1.0
# 3       4       1          -1.0
```
---

### 93. How do you calculate the cumulative product within each group of a DataFrame column using pandas?
**Answer:**  
You can calculate the cumulative product within each group using the `groupby` and `cumprod` methods.
```python
import pandas as pd

df = pd.DataFrame({'group': ['A', 'A', 'B', 'B'], 'value': [2, 3, 4, 5]})
df['cumprod_within_group'] = df.groupby('group')['value'].cumprod()
print(df)
# Output:
#   group  value  cumprod_within_group
# 0     A      2                     2
# 1     A      3                     6
# 2     B      4                     4
# 3     B      5                    20
```
---

### 94. How do you create a pivot table with multiple aggregation functions using pandas?
**Answer:**  
You can create a pivot table with multiple aggregation functions using the `pivot_table` method.
```python
import pandas as pd

df = pd.DataFrame({'date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02'], 'variable': ['A', 'B', 'A', 'B'], 'value': [10, 20, 30, 40]})
pivot_table = df.pivot_table(index='date', columns='variable', values='value', aggfunc=['mean', 'sum'])
print(pivot_table)
# Output:
#               mean      sum    
# variable         A   B    A   B
# date                           
# 2021-01-01    10.0  20   10  20
# 2021-01-02    30.0  40   30  40
```
---

### 95. How do you calculate the expanding mean of a DataFrame column using pandas?
**Answer:**  
You can calculate the expanding mean using the `expanding` and `mean` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30, 40]})
df['expanding_mean'] = df['value'].expanding().mean()
print(df)
# Output:
#    value  expanding_mean
# 0     10            10.0
# 1     20            15.0
# 2     30            20.0
# 3     40            25.0
```
---

### 96. How do you calculate the expanding sum of a DataFrame column using pandas?
**Answer:**  
You can calculate the expanding sum using the `expanding` and `sum` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30, 40]})
df['expanding_sum'] = df['value'].expanding().sum()
print(df)
# Output:
#    value  expanding_sum
# 0     10           10.0
# 1     20           30.0
# 2     30           60.0
# 3     40          100.0
```
---

### 97. How do you calculate the expanding standard deviation of a DataFrame column using pandas?
**Answer:**  
You can calculate the expanding standard deviation using the `expanding` and `std` methods.
```python
import pandas as pd

df = pd.DataFrame({'value': [10, 20, 30]})
df['expanding_std'] = df['value'].expanding().std()
print(df)
# Output:
#    value  expanding_std
# 0     10            NaN
# 1     20       7.071068
# 2     30      10.000000
```
---

### 98. How do you create a pivot table with multiple index levels using pandas?
**Answer:**  
You can create a pivot table with multiple index levels using the `pivot_table` method.
```python
import pandas as pd

df = pd.DataFrame({'date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02'], 'variable': ['A', 'B', 'A', 'B'], 'value': [10, 20, 30, 40]})
pivot_table = df.pivot_table(index=['date', 'variable'], values='value', aggfunc='mean')
print(pivot_table)
# Output:
#                     value
# date       variable       
# 2021-01-01 A           10
#            B           20
# 2021-01-02 A           30
#            B           40
```
---

### 99. How do you create a pivot table with multiple value columns using pandas?
**Answer:**  
You can create a pivot table with multiple value columns using the `pivot_table` method.
```python
import pandas as pd

df = pd.DataFrame({'date': ['2021-01-01', '2021-01-01'], 'variable': ['A', 'B'], 'value1': [1, 2], 'value2': [5, 6]})
pivot_table = df.pivot_table(index='date', columns='variable', values=['value1', 'value2'], aggfunc='mean')
print(pivot_table)
# Output:
#            value1    value2   
# variable        A  B      A  B
# date                          
# 2021-01-01      1  2      5  6
```
---

### 100. How do you create a pivot table with multiple aggregation functions for multiple value columns using pandas?
**Answer:**  
You can create a pivot table with multiple aggregation functions for multiple value columns using the `pivot_table` method.
```python
import pandas as pd

df = pd.DataFrame({'date': ['2021-01-01', '2021-01-01'], 'variable': ['A', 'B'], 'value1': [1, 2], 'value2': [5, 6]})
pivot_table = df.pivot_table(index='date', columns='variable', values=['value1', 'value2'], aggfunc={'value1': 'mean', 'value2': 'sum'})
print(pivot_table)
# Output:
#            value1    value2   
# variable        A  B      A  B
# date                          
# 2021-01-01      1  2      5  6
```
