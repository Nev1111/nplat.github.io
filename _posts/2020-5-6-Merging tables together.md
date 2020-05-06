### Common questions - how to perform a merge or a join on two dataframes and indicate which table is the data source for each of the resulting fields in the final dataframe?

I have an auditing script that takes a sample from an excel file. I'm trying to make a comparison between two dataframes, the initial one and indicate which file they came from.  I want the final result to include all of the records in the first dataframe and only those records in the second dataframe that share the same identification key as in the first dataframe. Here is an example of the two dataframes:



```python
import pandas as pd
df1 = pd.DataFrame({'Employee_ID': ['1000', '2304', '5024', '9985'],'Title_code': ['1002', '3115', '1041', '3644'],'Title_description': ['System Invest Offcr', ' Analytics and Performance ', 'Internal Portfolio Mgt  ', 'Invest Offcr 2 Trading']},
 index=[0, 1, 2, 3])
```


```python
df1
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
      <th>Employee_ID</th>
      <th>Title_code</th>
      <th>Title_description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>1002</td>
      <td>System Invest Offcr</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2304</td>
      <td>3115</td>
      <td>Analytics and Performance</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5024</td>
      <td>1041</td>
      <td>Internal Portfolio Mgt</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9985</td>
      <td>3644</td>
      <td>Invest Offcr 2 Trading</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = pd.DataFrame({'Employee_ID': ['1000', '2304', '5024', '1263'],'Work_unit': ['IT', 'Risk', 'Investments', 'Investments'],'Unit_description': ['Systems_admin', ' Internal_audit ', 'Investments_unit  ', 'Investments_unit']},
 index=[0, 1, 2, 3])
```


```python

```


```python
df2
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
      <th>Employee_ID</th>
      <th>Work_unit</th>
      <th>Unit_description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>IT</td>
      <td>Systems_admin</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2304</td>
      <td>Risk</td>
      <td>Internal_audit</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5024</td>
      <td>Investments</td>
      <td>Investments_unit</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1263</td>
      <td>Investments</td>
      <td>Investments_unit</td>
    </tr>
  </tbody>
</table>
</div>




```python

```

The goal would be to merge or join these two dataframes by what they share in common (or Employee_ID) and indicate which dataframe each of the columns/fields came from in the resulting dataframe.  We can accomplish this by doing the following:


```python
resulting_df=pd.merge(df1,df2, on='Employee_ID', how='left', indicator=True)
```


```python

```


```python
resulting_df
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
      <th>Employee_ID</th>
      <th>Title_code</th>
      <th>Title_description</th>
      <th>Work_unit</th>
      <th>Unit_description</th>
      <th>_merge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>1002</td>
      <td>System Invest Offcr</td>
      <td>IT</td>
      <td>Systems_admin</td>
      <td>both</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2304</td>
      <td>3115</td>
      <td>Analytics and Performance</td>
      <td>Risk</td>
      <td>Internal_audit</td>
      <td>both</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5024</td>
      <td>1041</td>
      <td>Internal Portfolio Mgt</td>
      <td>Investments</td>
      <td>Investments_unit</td>
      <td>both</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9985</td>
      <td>3644</td>
      <td>Invest Offcr 2 Trading</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
  </tbody>
</table>
</div>



The resulting dataframe contains all of the records from the first table and only those records from the second table that share an identical "Employee_ID". Note the last column indicates 3 of the emloyees in the example dataframes were contained in both, left and right dataframes, while the last one (Employee_ID 9985) only existed in the first dataframe and not the second resuting in the "left_only" value under that column.

Merges can also be performed over the intersection of the entries contained within the dataframes.  If we were to look at the same two examples above, and our goal was to merge or join the dataframes over the same "Employee_ID" column, the command would change to this:


```python
resulting_df=pd.merge(df1,df2, on='Employee_ID', how='inner', indicator=True)
```


```python
resulting_df
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
      <th>Employee_ID</th>
      <th>Title_code</th>
      <th>Title_description</th>
      <th>Work_unit</th>
      <th>Unit_description</th>
      <th>_merge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>1002</td>
      <td>System Invest Offcr</td>
      <td>IT</td>
      <td>Systems_admin</td>
      <td>both</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2304</td>
      <td>3115</td>
      <td>Analytics and Performance</td>
      <td>Risk</td>
      <td>Internal_audit</td>
      <td>both</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5024</td>
      <td>1041</td>
      <td>Internal Portfolio Mgt</td>
      <td>Investments</td>
      <td>Investments_unit</td>
      <td>both</td>
    </tr>
  </tbody>
</table>
</div>



We see only the three employees with the common "Employee_ID" contained in the results. Similarly, merges can also be done by performing a "right" merge (although not very popular).  If we were to try this, we would expect to see the resulting table to contain all of the records from the second dataframe (df2) and only those records in first dataframe (df1) sharing the same "Employee_ID".


```python
resulting_df=pd.merge(df1,df2, on='Employee_ID', how='right', indicator=True)
```


```python
resulting_df
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
      <th>Employee_ID</th>
      <th>Title_code</th>
      <th>Title_description</th>
      <th>Work_unit</th>
      <th>Unit_description</th>
      <th>_merge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>1002</td>
      <td>System Invest Offcr</td>
      <td>IT</td>
      <td>Systems_admin</td>
      <td>both</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2304</td>
      <td>3115</td>
      <td>Analytics and Performance</td>
      <td>Risk</td>
      <td>Internal_audit</td>
      <td>both</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5024</td>
      <td>1041</td>
      <td>Internal Portfolio Mgt</td>
      <td>Investments</td>
      <td>Investments_unit</td>
      <td>both</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1263</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Investments</td>
      <td>Investments_unit</td>
      <td>right_only</td>
    </tr>
  </tbody>
</table>
</div>



The resulting dataframe contains all of the records from the second table and only those records from the first table that share an identical "Employee_ID". This is indicated in the last column by the "right_only" value.


```python

```


```python

```


```python

```
