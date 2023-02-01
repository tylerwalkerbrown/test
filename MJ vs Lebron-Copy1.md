```python
import requests
import pandas as pd
import matplotlib.pyplot as plt

```


```python
import requests
import pandas as pd
from bs4 import BeautifulSoup

def search(first, last):
    url = "https://www.basketball-reference.com/search/search.fcgi?hint={first}+{last}&search={first}+{last}".format(
        first=first, last=last)

    response = requests.get(url)
    soup = BeautifulSoup(response.text, "html.parser")

    results = soup.find_all("div")

    names = []
    links = []
    for result in results:
        name = result.find("div", class_="search-item-name")
        link = result.find("div", class_="search-item-url")
        names.append(name)
        links.append(link)

    data = {'Name': names, 'Link': links}
    df = pd.DataFrame(data)
    df = pd.DataFrame(df.Link[0])
    df = pd.DataFrame(df[0].str.split(".",expand = True)[0])
    
    return df
```


```python
first = input("Enter The Players First Name You want to search:")
last = input("Enter The Players Last Name You want to search:")
year_one = input("What is the first year you want to evaluate for the first player:")
year_two = input("What is the last year you want to evaluate for the first player:")
first2 = input("Enter The Players First Name You want to search:")
last2 = input("Enter The Players Last Name You want to search:")
year_one_1 = input("What is the first year you want to evaluate for the first player:")
year_two_2 = input("What is the last year you want to evaluate for the first player:")

```

    Enter The Players First Name You want to search:lebron
    Enter The Players Last Name You want to search:james
    What is the first year you want to evaluate for the first player:2004
    What is the last year you want to evaluate for the first player:2022
    Enter The Players First Name You want to search:michael
    Enter The Players Last Name You want to search:jordan
    What is the first year you want to evaluate for the first player:1984
    What is the last year you want to evaluate for the first player:1993



```python
year_one_1 = 1985

```


```python
player_info = search(first, last)
player_info_1 = search(first2, last2)
```


```python
print(player_info_1)
print(player_info)
```

                          0
    0  /players/j/jordami01
                          0
    0  /players/j/jamesle01



```python
#pd.concat(list(search(first, last)), list(search(first2, last2)))
```

Below I created a couple loops to first create a list of lengths with the attached ranges that the player year they played. I took in the years and attached the link in a string type then appended an empty list created called links. I then take those links and loop it through the pd.read_html function in python then append it so as it loops through the list of links pulling in all the data. I then have to concat the data at the end of the loops.


```python
players = [(first, last), (first2, last2)]
year_1 = range(int(year_one) ,int(year_two))
#year_2 = range(int(year_one_1),int(year_two_2))
year_2 = list(range(int(1985),int(1993)))+ list(range(1995,1999)) + list(range(2002,2004))

```


```python
len(year_1)
```




    18




```python
i = 0 
links = []
for i in year_1:
    string = str("https://www.basketball-reference.com" +player_info[0].unique()[0] + "/gamelog/"+str(i))
    links.append(string)
    i -=1
i = 0 
links_1 = []
for i in year_2:
    string = str("https://www.basketball-reference.com" +player_info_1[0].unique()[0] + "/gamelog/"+str(i))
    links_1.append(string)
    i -=1
```


```python
length = len(links) 
i = 0
data = []
for link in links:
    data.append(pd.read_html(str(link))[7])
lebron = pd.concat(data)
```


```python
lebron.head()
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
      <th>Rk</th>
      <th>G</th>
      <th>Date</th>
      <th>Age</th>
      <th>Tm</th>
      <th>Unnamed: 5</th>
      <th>Opp</th>
      <th>Unnamed: 7</th>
      <th>GS</th>
      <th>MP</th>
      <th>...</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>GmSc</th>
      <th>+/-</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>2003-10-29</td>
      <td>18-303</td>
      <td>CLE</td>
      <td>@</td>
      <td>SAC</td>
      <td>L (-14)</td>
      <td>1</td>
      <td>42:50</td>
      <td>...</td>
      <td>4</td>
      <td>6</td>
      <td>9</td>
      <td>4</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>25</td>
      <td>24.7</td>
      <td>-9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2</td>
      <td>2003-10-30</td>
      <td>18-304</td>
      <td>CLE</td>
      <td>@</td>
      <td>PHO</td>
      <td>L (-9)</td>
      <td>1</td>
      <td>40:21</td>
      <td>...</td>
      <td>10</td>
      <td>12</td>
      <td>8</td>
      <td>1</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
      <td>21</td>
      <td>14.7</td>
      <td>-3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>3</td>
      <td>2003-11-01</td>
      <td>18-306</td>
      <td>CLE</td>
      <td>@</td>
      <td>POR</td>
      <td>L (-19)</td>
      <td>1</td>
      <td>39:10</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>6</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>8</td>
      <td>5.0</td>
      <td>-21</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4</td>
      <td>2003-11-05</td>
      <td>18-310</td>
      <td>CLE</td>
      <td>NaN</td>
      <td>DEN</td>
      <td>L (-4)</td>
      <td>1</td>
      <td>41:06</td>
      <td>...</td>
      <td>9</td>
      <td>11</td>
      <td>7</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>7</td>
      <td>11.2</td>
      <td>-3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>5</td>
      <td>2003-11-07</td>
      <td>18-312</td>
      <td>CLE</td>
      <td>@</td>
      <td>IND</td>
      <td>L (-1)</td>
      <td>1</td>
      <td>43:44</td>
      <td>...</td>
      <td>5</td>
      <td>5</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>2</td>
      <td>23</td>
      <td>9.0</td>
      <td>-7</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>




```python
length = len(links_1) 
i = 0
data = []
for link in links_1:
    data.append(pd.read_html(str(link))[7])
jordan = pd.concat(data)

```


```python
jordan.head()
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
      <th>Rk</th>
      <th>G</th>
      <th>Date</th>
      <th>Age</th>
      <th>Tm</th>
      <th>Unnamed: 5</th>
      <th>Opp</th>
      <th>Unnamed: 7</th>
      <th>GS</th>
      <th>MP</th>
      <th>...</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>GmSc</th>
      <th>+/-</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>1984-10-26</td>
      <td>21-252</td>
      <td>CHI</td>
      <td>NaN</td>
      <td>WSB</td>
      <td>W (+16)</td>
      <td>1</td>
      <td>40:00</td>
      <td>...</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
      <td>2</td>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>16</td>
      <td>12.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2</td>
      <td>1984-10-27</td>
      <td>21-253</td>
      <td>CHI</td>
      <td>@</td>
      <td>MIL</td>
      <td>L (-2)</td>
      <td>1</td>
      <td>34:00</td>
      <td>...</td>
      <td>2</td>
      <td>5</td>
      <td>5</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>21</td>
      <td>19.4</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>3</td>
      <td>1984-10-29</td>
      <td>21-255</td>
      <td>CHI</td>
      <td>NaN</td>
      <td>MIL</td>
      <td>W (+6)</td>
      <td>1</td>
      <td>34:00</td>
      <td>...</td>
      <td>2</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>37</td>
      <td>32.9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4</td>
      <td>1984-10-30</td>
      <td>21-256</td>
      <td>CHI</td>
      <td>@</td>
      <td>KCK</td>
      <td>W (+5)</td>
      <td>1</td>
      <td>36:00</td>
      <td>...</td>
      <td>2</td>
      <td>4</td>
      <td>5</td>
      <td>3</td>
      <td>1</td>
      <td>6</td>
      <td>5</td>
      <td>25</td>
      <td>14.7</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>5</td>
      <td>1984-11-01</td>
      <td>21-258</td>
      <td>CHI</td>
      <td>@</td>
      <td>DEN</td>
      <td>L (-16)</td>
      <td>1</td>
      <td>33:00</td>
      <td>...</td>
      <td>2</td>
      <td>5</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>17</td>
      <td>13.2</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>



# Data Cleansing

I created a function to search for any variables that are not numeric to zero. I was having an issue with the data having strings such as "Did not play" so I created a function to seek out non numeric observations and created another function to loop through the different columns and automatically search.


```python
def clean_int_column(df, column):
    df[column] = pd.to_numeric(df[column], errors='coerce')
    df[column] = df[column].fillna(0).astype(int)
        
def clean_all_columns(df):
    for column in df.columns[10:]:
        clean_int_column(df, column)
```


```python
#Applying the function created above
clean_all_columns(jordan)
clean_all_columns(lebron)
```

Below we created a general long list of items to clean in the dataframes. Most of the datatypes were object instead of floats. I also split some columns so that I could extract the key elements in some of the columns. I also had to format the date to datetime so i could work on it as a time series if needed. 


```python
#Function to clean the set we are working with 
def clean_data(df):
    df['Age'] = pd.DataFrame(df['Age'].str.split('-', expand=True)[0])
    df['Year'] = pd.DataFrame(df['Date'].str.split('-', expand=True)[0])  
    df['MP'] = pd.DataFrame(df['MP'].str.split(':', expand=True)[0])    
    df = df[df['Date'] != 'Date']
    df.rename(columns={'Unnamed: 5': 'home_away'}, inplace=True)
    df.rename(columns={'Unnamed: 7': 'WL'}, inplace=True)
    df['Date'] = pd.to_datetime(df['Date'], format='%Y-%m-%d')
    df['home_away'].fillna("home", inplace=True)
    #Changing the data types 
    df['PTS'] = df['PTS'].astype(float)
    df['+/-'] = df['+/-'].astype(float)
    df['GmSc'] = df['GmSc'].astype(float)
    df['PF'] = df['PF'].astype(float)
    df['TOV'] = df['TOV'].astype(float)
    df['AST'] = df['AST'].astype(float)
    df['STL'] = df['STL'].astype(float)
    df['BLK'] = df['BLK'].astype(float)
    df['3P'] = df['3P'].astype(float)
    df['3PA'] = df['3PA'].astype(float)
    df['FG%'] = df['FG%'].astype(float)
    df['MP'] = df['MP'].replace('Did Not Dress', 0)
    df['MP'] = df['MP'].replace('Inactive', 0)
    df['MP'] = df['MP'].replace('Did Not Play', 0)
    df['MP'] = df['MP'].replace('Not With Team', 0)
    df['MP'] = df['MP'].astype(float)
    
    return df
```


```python
df_mj = clean_data(jordan)
df_lb = clean_data(lebron)

```

    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df.rename(columns={'Unnamed: 5': 'home_away'}, inplace=True)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:8: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df.rename(columns={'Unnamed: 7': 'WL'}, inplace=True)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:9: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['Date'] = pd.to_datetime(df['Date'], format='%Y-%m-%d')
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:10: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['home_away'].fillna("home", inplace=True)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:12: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['PTS'] = df['PTS'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:13: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['+/-'] = df['+/-'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:14: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['GmSc'] = df['GmSc'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:15: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['PF'] = df['PF'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:16: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['TOV'] = df['TOV'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:17: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['AST'] = df['AST'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:18: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['STL'] = df['STL'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:19: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['BLK'] = df['BLK'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:20: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['3P'] = df['3P'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:21: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['3PA'] = df['3PA'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:22: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['FG%'] = df['FG%'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:23: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['MP'] = df['MP'].replace('Did Not Dress', 0)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:24: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['MP'] = df['MP'].replace('Inactive', 0)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:25: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['MP'] = df['MP'].replace('Did Not Play', 0)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:26: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['MP'] = df['MP'].replace('Not With Team', 0)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:27: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['MP'] = df['MP'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df.rename(columns={'Unnamed: 5': 'home_away'}, inplace=True)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:8: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df.rename(columns={'Unnamed: 7': 'WL'}, inplace=True)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:9: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['Date'] = pd.to_datetime(df['Date'], format='%Y-%m-%d')
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:10: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['home_away'].fillna("home", inplace=True)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:12: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['PTS'] = df['PTS'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:13: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['+/-'] = df['+/-'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:14: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['GmSc'] = df['GmSc'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:15: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['PF'] = df['PF'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:16: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['TOV'] = df['TOV'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:17: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['AST'] = df['AST'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:18: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['STL'] = df['STL'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:19: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['BLK'] = df['BLK'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:20: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['3P'] = df['3P'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:21: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['3PA'] = df['3PA'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:22: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['FG%'] = df['FG%'].astype(float)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:23: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['MP'] = df['MP'].replace('Did Not Dress', 0)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:24: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['MP'] = df['MP'].replace('Inactive', 0)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:25: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['MP'] = df['MP'].replace('Did Not Play', 0)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:26: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['MP'] = df['MP'].replace('Not With Team', 0)
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/4285801742.py:27: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['MP'] = df['MP'].astype(float)


# SQL Connection
"""CREATE DATABASE nba_stats"""

```python
import mysql.connector
import sqlite3
# import the module
import pymysql
from sqlalchemy import create_engine
# create sqlalchemy engine
engine = create_engine("mysql+pymysql://{user}:{password}@localhost/{database}"
                       .format(user = 'root',
                              password = 'Coors1998',
                              database = 'nba_stats'))
```

Reading each of the clean datasets into SQL
df_lb.to_sql('lebron', con = engine, if_exists = 'append', chunksize = len(df_lb))
df_mj.to_sql('jordan', con = engine, if_exists = 'append', chunksize = len(df_mj))
# SQL Analysis 

# Querying H vs A Statistics


```python
lebron_sum = """SELECT COUNT({column0}) as {name}_game_count, 
AVG({column0}) as {name}_{column0}_avg,MAX({column0}) as {name}_{column0}_pts_max,   
MIN({column0}) as {name}_{column0}_min,SUM({column0}) as {name}_{column0}_sum,
AVG({column1}) as {name}_{column1}_avg,MAX({column1}) as {name}_{column1}_pts_max,   
MIN({column1}) as {name}_{column1}_min,SUM({column1}) as {name}_{column1}_sum,  
AVG({column2}) as {name}_{column2}_avg,MAX({column2}) as {name}_{column2}_pts_max,   
MIN({column2}) as {name}_{column2}_min,SUM({column2}) as {name}_{column2}_sum,  
AVG({column3}) as {name}_{column3}_avg,MAX({column3}) as {name}_{column3}_pts_max,   
MIN({column3}) as {name}_{column3}_min ,SUM({column3}) as {name}_{column3}_sum
FROM nba_stats.{name}""".format(column0 = "PTS",name = 'lebron',column1 = "BLK",column2 = "AST",column3 ="STL")
```


```python
pd.read_sql(lebron_sum,con=engine)
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
      <th>lebron_game_count</th>
      <th>lebron_PTS_avg</th>
      <th>lebron_PTS_pts_max</th>
      <th>lebron_PTS_min</th>
      <th>lebron_PTS_sum</th>
      <th>lebron_BLK_avg</th>
      <th>lebron_BLK_pts_max</th>
      <th>lebron_BLK_min</th>
      <th>lebron_BLK_sum</th>
      <th>lebron_AST_avg</th>
      <th>lebron_AST_pts_max</th>
      <th>lebron_AST_min</th>
      <th>lebron_AST_sum</th>
      <th>lebron_STL_avg</th>
      <th>lebron_STL_pts_max</th>
      <th>lebron_STL_min</th>
      <th>lebron_STL_sum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1521</td>
      <td>24.366864</td>
      <td>61.0</td>
      <td>0.0</td>
      <td>37062.0</td>
      <td>0.684418</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>1041.0</td>
      <td>6.604208</td>
      <td>19.0</td>
      <td>0.0</td>
      <td>10045.0</td>
      <td>1.404339</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>2136.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
jordan_sum = """SELECT COUNT({column0}) as {name}_game_count, 
AVG({column0}) as {name}_{column0}_avg,MAX({column0}) as {name}_{column0}_pts_max,   
MIN({column0}) as {name}_{column0}_min,SUM({column0}) as {name}_{column0}_sum,
AVG({column1}) as {name}_{column1}_avg,MAX({column1}) as {name}_{column1}_pts_max,   
MIN({column1}) as {name}_{column1}_min,SUM({column1}) as {name}_{column1}_sum,  
AVG({column2}) as {name}_{column2}_avg,MAX({column2}) as {name}_{column2}_pts_max,   
MIN({column2}) as {name}_{column2}_min,SUM({column2}) as {name}_{column2}_sum,  
AVG({column3}) as {name}_{column3}_avg,MAX({column3}) as {name}_{column3}_pts_max,   
MIN({column3}) as {name}_{column3}_min ,SUM({column3}) as {name}_{column3}_sum
FROM nba_stats.{name}""".format(column0 = "PTS",name = 'jordan',column1 = "BLK",column2 = "AST",column3 ="STL")
```


```python
pd.read_sql(jordan_sum,con=engine)
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
      <th>jordan_game_count</th>
      <th>jordan_PTS_avg</th>
      <th>jordan_PTS_pts_max</th>
      <th>jordan_PTS_min</th>
      <th>jordan_PTS_sum</th>
      <th>jordan_BLK_avg</th>
      <th>jordan_BLK_pts_max</th>
      <th>jordan_BLK_min</th>
      <th>jordan_BLK_sum</th>
      <th>jordan_AST_avg</th>
      <th>jordan_AST_pts_max</th>
      <th>jordan_AST_min</th>
      <th>jordan_AST_sum</th>
      <th>jordan_STL_avg</th>
      <th>jordan_STL_pts_max</th>
      <th>jordan_STL_min</th>
      <th>jordan_STL_sum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1086</td>
      <td>29.734807</td>
      <td>69.0</td>
      <td>0.0</td>
      <td>32292.0</td>
      <td>0.822284</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>893.0</td>
      <td>5.186924</td>
      <td>17.0</td>
      <td>0.0</td>
      <td>5633.0</td>
      <td>2.314917</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>2514.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
jordan_summary = """SELECT home_away ,COUNT({column0}) as {name}_game_count, 
AVG({column0}) as {name}_{column0}_avg,MAX({column0}) as {name}_{column0}_pts_max,   
MIN({column0}) as {name}_{column0}_min,SUM({column0}) as {name}_{column0}_sum,
AVG({column1}) as {name}_{column1}_avg,MAX({column1}) as {name}_{column1}_pts_max,   
MIN({column1}) as {name}_{column1}_min,SUM({column1}) as {name}_{column1}_sum,  
AVG({column2}) as {name}_{column2}_avg,MAX({column2}) as {name}_{column2}_pts_max,   
MIN({column2}) as {name}_{column2}_min,SUM({column2}) as {name}_{column2}_sum,  
AVG({column3}) as {name}_{column3}_avg,MAX({column3}) as {name}_{column3}_pts_max,   
MIN({column3}) as {name}_{column3}_min ,SUM({column3}) as {name}_{column3}_sum
FROM nba_stats.{name}
group by home_away""".format(column0 = "PTS",name = 'jordan',column1 = "BLK",column2 = "AST",column3 ="STL")
```


```python
pd.read_sql(jordan_summary,con=engine)
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
      <th>home_away</th>
      <th>jordan_game_count</th>
      <th>jordan_PTS_avg</th>
      <th>jordan_PTS_pts_max</th>
      <th>jordan_PTS_min</th>
      <th>jordan_PTS_sum</th>
      <th>jordan_BLK_avg</th>
      <th>jordan_BLK_pts_max</th>
      <th>jordan_BLK_min</th>
      <th>jordan_BLK_sum</th>
      <th>jordan_AST_avg</th>
      <th>jordan_AST_pts_max</th>
      <th>jordan_AST_min</th>
      <th>jordan_AST_sum</th>
      <th>jordan_STL_avg</th>
      <th>jordan_STL_pts_max</th>
      <th>jordan_STL_min</th>
      <th>jordan_STL_sum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>home</td>
      <td>541</td>
      <td>30.110906</td>
      <td>64.0</td>
      <td>0.0</td>
      <td>16290.0</td>
      <td>0.885397</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>479.0</td>
      <td>5.362292</td>
      <td>16.0</td>
      <td>0.0</td>
      <td>2901.0</td>
      <td>2.530499</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>1369.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@</td>
      <td>545</td>
      <td>29.361468</td>
      <td>69.0</td>
      <td>0.0</td>
      <td>16002.0</td>
      <td>0.759633</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>414.0</td>
      <td>5.012844</td>
      <td>17.0</td>
      <td>0.0</td>
      <td>2732.0</td>
      <td>2.100917</td>
      <td>9.0</td>
      <td>0.0</td>
      <td>1145.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
lebron_summary = """SELECT home_away , COUNT({column0}) as {name}_game_count, 
AVG({column0}) as {name}_{column0}_avg,MAX({column0}) as {name}_{column0}_pts_max,   MIN({column0}) as {name}_{column0}_max,  COUNT({column0}) as {name}_{column0}_count,
AVG({column1}) as {name}_{column1}_avg,MAX({column1}) as {name}_{column1}_pts_max,   MIN({column1}) as {name}_{column1}_max,  COUNT({column1}) as {name}_{column0}_count,
AVG({column2}) as {name}_{column2}_avg,MAX({column2}) as {name}_{column2}_pts_max,   MIN({column2}) as {name}_{column2}_max,  COUNT({column2}) as {name}_{column2}_count,
AVG({column3}) as {name}_{column3}_avg,MAX({column3}) as {name}_{column3}_pts_max,   MIN({column3}) as {name}_{column3}_min  
FROM nba_stats.{name}
group by home_away""".format(column0 = "PTS",name = 'lebron',column1 = "BLK",column2 = "AST",column3 ="STL")
```


```python
pd.read_sql(lebron_summary,con=engine)
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
      <th>home_away</th>
      <th>lebron_game_count</th>
      <th>lebron_PTS_avg</th>
      <th>lebron_PTS_pts_max</th>
      <th>lebron_PTS_max</th>
      <th>lebron_PTS_count</th>
      <th>lebron_BLK_avg</th>
      <th>lebron_BLK_pts_max</th>
      <th>lebron_BLK_max</th>
      <th>lebron_PTS_count</th>
      <th>lebron_AST_avg</th>
      <th>lebron_AST_pts_max</th>
      <th>lebron_AST_max</th>
      <th>lebron_AST_count</th>
      <th>lebron_STL_avg</th>
      <th>lebron_STL_pts_max</th>
      <th>lebron_STL_min</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>@</td>
      <td>761</td>
      <td>24.358739</td>
      <td>57.0</td>
      <td>0.0</td>
      <td>761</td>
      <td>0.649146</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>761</td>
      <td>6.346912</td>
      <td>19.0</td>
      <td>0.0</td>
      <td>761</td>
      <td>1.408673</td>
      <td>7.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>home</td>
      <td>760</td>
      <td>24.375000</td>
      <td>61.0</td>
      <td>0.0</td>
      <td>760</td>
      <td>0.719737</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>760</td>
      <td>6.861842</td>
      <td>19.0</td>
      <td>0.0</td>
      <td>760</td>
      <td>1.400000</td>
      <td>6.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



# Top/Bottom Career Performances by Team


```python
lebron_by_teams = """SELECT Opp , 
AVG({column0}) as {name}_{column0}_avg,MAX({column0}) as {name}_{column0}_pts_max,   MIN({column0}) as {name}_{column0}_max,  COUNT({column0}) as {name}_{column0}_count,
AVG({column1}) as {name}_{column1}_avg,MAX({column1}) as {name}_{column1}_pts_max,   MIN({column1}) as {name}_{column1}_max,  COUNT({column1}) as {name}_{column0}_count,
AVG({column2}) as {name}_{column2}_avg,MAX({column2}) as {name}_{column2}_pts_max,   MIN({column2}) as {name}_{column2}_max,  COUNT({column2}) as {name}_{column2}_count,
AVG({column3}) as {name}_{column3}_avg,MAX({column3}) as {name}_{column3}_pts_max,   MIN({column3}) as {name}_{column3}_max,  COUNT({column2}) as {name}_{column3}_count
FROM nba_stats.{name}
group by Opp
order by avg({column0}) desc""".format(column0 = "PTS",name = 'lebron',column1 = "BLK",column2 = "MP", column3 = "AST")
```


```python
lebron_by_teams = pd.read_sql(lebron_by_teams,con=engine)
lebron_by_teams.head()
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
      <th>Opp</th>
      <th>lebron_PTS_avg</th>
      <th>lebron_PTS_pts_max</th>
      <th>lebron_PTS_max</th>
      <th>lebron_PTS_count</th>
      <th>lebron_BLK_avg</th>
      <th>lebron_BLK_pts_max</th>
      <th>lebron_BLK_max</th>
      <th>lebron_PTS_count</th>
      <th>lebron_MP_avg</th>
      <th>lebron_MP_pts_max</th>
      <th>lebron_MP_max</th>
      <th>lebron_MP_count</th>
      <th>lebron_AST_avg</th>
      <th>lebron_AST_pts_max</th>
      <th>lebron_AST_max</th>
      <th>lebron_AST_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NOK</td>
      <td>28.250000</td>
      <td>35.0</td>
      <td>15.0</td>
      <td>4</td>
      <td>0.250000</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>4</td>
      <td>39.500000</td>
      <td>42.0</td>
      <td>33.0</td>
      <td>4</td>
      <td>6.000000</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>LAL</td>
      <td>27.633333</td>
      <td>41.0</td>
      <td>16.0</td>
      <td>30</td>
      <td>0.700000</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>30</td>
      <td>38.900000</td>
      <td>46.0</td>
      <td>30.0</td>
      <td>30</td>
      <td>7.266667</td>
      <td>12.0</td>
      <td>3.0</td>
      <td>30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NJN</td>
      <td>27.193548</td>
      <td>42.0</td>
      <td>0.0</td>
      <td>31</td>
      <td>0.612903</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>31</td>
      <td>37.516129</td>
      <td>46.0</td>
      <td>0.0</td>
      <td>31</td>
      <td>7.387097</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>31</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NOP</td>
      <td>27.000000</td>
      <td>41.0</td>
      <td>0.0</td>
      <td>24</td>
      <td>0.500000</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>24</td>
      <td>33.916667</td>
      <td>44.0</td>
      <td>0.0</td>
      <td>24</td>
      <td>7.958333</td>
      <td>15.0</td>
      <td>0.0</td>
      <td>24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BRK</td>
      <td>26.300000</td>
      <td>39.0</td>
      <td>0.0</td>
      <td>30</td>
      <td>0.466667</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>30</td>
      <td>34.433333</td>
      <td>49.0</td>
      <td>0.0</td>
      <td>30</td>
      <td>7.466667</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>30</td>
    </tr>
  </tbody>
</table>
</div>




```python
lebron_by_teams.tail()
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
      <th>Opp</th>
      <th>lebron_PTS_avg</th>
      <th>lebron_PTS_pts_max</th>
      <th>lebron_PTS_max</th>
      <th>lebron_PTS_count</th>
      <th>lebron_BLK_avg</th>
      <th>lebron_BLK_pts_max</th>
      <th>lebron_BLK_max</th>
      <th>lebron_PTS_count</th>
      <th>lebron_MP_avg</th>
      <th>lebron_MP_pts_max</th>
      <th>lebron_MP_max</th>
      <th>lebron_MP_count</th>
      <th>lebron_AST_avg</th>
      <th>lebron_AST_pts_max</th>
      <th>lebron_AST_max</th>
      <th>lebron_AST_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30</th>
      <td>HOU</td>
      <td>22.279070</td>
      <td>38.0</td>
      <td>0.0</td>
      <td>43</td>
      <td>0.697674</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>43</td>
      <td>32.813953</td>
      <td>48.0</td>
      <td>0.0</td>
      <td>43</td>
      <td>5.976744</td>
      <td>15.0</td>
      <td>0.0</td>
      <td>43</td>
    </tr>
    <tr>
      <th>31</th>
      <td>DET</td>
      <td>22.092308</td>
      <td>43.0</td>
      <td>0.0</td>
      <td>65</td>
      <td>0.492308</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>65</td>
      <td>34.046154</td>
      <td>50.0</td>
      <td>0.0</td>
      <td>65</td>
      <td>6.646154</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>65</td>
    </tr>
    <tr>
      <th>32</th>
      <td>SAC</td>
      <td>21.325581</td>
      <td>51.0</td>
      <td>0.0</td>
      <td>43</td>
      <td>0.720930</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>43</td>
      <td>30.697674</td>
      <td>49.0</td>
      <td>0.0</td>
      <td>43</td>
      <td>6.837209</td>
      <td>16.0</td>
      <td>0.0</td>
      <td>43</td>
    </tr>
    <tr>
      <th>33</th>
      <td>OKC</td>
      <td>21.264706</td>
      <td>44.0</td>
      <td>0.0</td>
      <td>34</td>
      <td>0.676471</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>34</td>
      <td>28.441176</td>
      <td>42.0</td>
      <td>0.0</td>
      <td>34</td>
      <td>5.882353</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>34</td>
    </tr>
    <tr>
      <th>34</th>
      <td>LAC</td>
      <td>20.750000</td>
      <td>39.0</td>
      <td>0.0</td>
      <td>44</td>
      <td>0.636364</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>44</td>
      <td>32.613636</td>
      <td>49.0</td>
      <td>0.0</td>
      <td>44</td>
      <td>5.954545</td>
      <td>12.0</td>
      <td>0.0</td>
      <td>44</td>
    </tr>
  </tbody>
</table>
</div>




```python
mj_by_teams = """SELECT Opp , 
AVG({column0}) as {name}_{column0}_avg,MAX({column0}) as {name}_{column0}_pts_max,   MIN({column0}) as {name}_{column0}_max,  COUNT({column0}) as {name}_{column0}_count,
AVG({column1}) as {name}_{column1}_avg,MAX({column1}) as {name}_{column1}_pts_max,   MIN({column1}) as {name}_{column1}_max,  COUNT({column1}) as {name}_{column0}_count,
AVG({column2}) as {name}_{column2}_avg,MAX({column2}) as {name}_{column2}_pts_max,   MIN({column2}) as {name}_{column2}_max,  COUNT({column2}) as {name}_{column2}_count,
AVG({column3}) as {name}_{column3}_avg,MAX({column3}) as {name}_{column3}_pts_max,   MIN({column3}) as {name}_{column3}_max,  COUNT({column2}) as {name}_{column3}_count
FROM nba_stats.{name}
group by Opp
order by avg({column0}) desc""".format(column0 = "PTS",name = 'jordan',column1 = "BLK",column2 = "MP",column3="AST")
```


```python
my_by_teams = pd.read_sql(mj_by_teams,con=engine)
my_by_teams.head()
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
      <th>Opp</th>
      <th>jordan_PTS_avg</th>
      <th>jordan_PTS_pts_max</th>
      <th>jordan_PTS_max</th>
      <th>jordan_PTS_count</th>
      <th>jordan_BLK_avg</th>
      <th>jordan_BLK_pts_max</th>
      <th>jordan_BLK_max</th>
      <th>jordan_PTS_count</th>
      <th>jordan_MP_avg</th>
      <th>jordan_MP_pts_max</th>
      <th>jordan_MP_max</th>
      <th>jordan_MP_count</th>
      <th>jordan_AST_avg</th>
      <th>jordan_AST_pts_max</th>
      <th>jordan_AST_max</th>
      <th>jordan_AST_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>UTA</td>
      <td>32.692308</td>
      <td>47.0</td>
      <td>11.0</td>
      <td>26</td>
      <td>0.807692</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>26</td>
      <td>38.538462</td>
      <td>56.0</td>
      <td>22.0</td>
      <td>26</td>
      <td>4.692308</td>
      <td>9.0</td>
      <td>1.0</td>
      <td>26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>PHO</td>
      <td>32.440000</td>
      <td>53.0</td>
      <td>14.0</td>
      <td>25</td>
      <td>0.880000</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>25</td>
      <td>37.880000</td>
      <td>44.0</td>
      <td>28.0</td>
      <td>25</td>
      <td>5.720000</td>
      <td>14.0</td>
      <td>1.0</td>
      <td>25</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MIL</td>
      <td>32.257576</td>
      <td>50.0</td>
      <td>11.0</td>
      <td>66</td>
      <td>0.909091</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>66</td>
      <td>38.181818</td>
      <td>48.0</td>
      <td>13.0</td>
      <td>66</td>
      <td>4.939394</td>
      <td>16.0</td>
      <td>0.0</td>
      <td>66</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CHH</td>
      <td>32.081081</td>
      <td>52.0</td>
      <td>19.0</td>
      <td>37</td>
      <td>0.621622</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>37</td>
      <td>37.594595</td>
      <td>43.0</td>
      <td>28.0</td>
      <td>37</td>
      <td>5.540541</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>37</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NYK</td>
      <td>31.566667</td>
      <td>55.0</td>
      <td>16.0</td>
      <td>60</td>
      <td>0.850000</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>60</td>
      <td>38.950000</td>
      <td>46.0</td>
      <td>19.0</td>
      <td>60</td>
      <td>4.800000</td>
      <td>10.0</td>
      <td>1.0</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
my_by_teams.tail()
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
      <th>Opp</th>
      <th>jordan_PTS_avg</th>
      <th>jordan_PTS_pts_max</th>
      <th>jordan_PTS_max</th>
      <th>jordan_PTS_count</th>
      <th>jordan_BLK_avg</th>
      <th>jordan_BLK_pts_max</th>
      <th>jordan_BLK_max</th>
      <th>jordan_PTS_count</th>
      <th>jordan_MP_avg</th>
      <th>jordan_MP_pts_max</th>
      <th>jordan_MP_max</th>
      <th>jordan_MP_count</th>
      <th>jordan_AST_avg</th>
      <th>jordan_AST_pts_max</th>
      <th>jordan_AST_max</th>
      <th>jordan_AST_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>28</th>
      <td>KCK</td>
      <td>25.500000</td>
      <td>26.0</td>
      <td>25.0</td>
      <td>2</td>
      <td>0.500000</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>37.000000</td>
      <td>38.0</td>
      <td>36.0</td>
      <td>2</td>
      <td>6.000000</td>
      <td>7.0</td>
      <td>5.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>29</th>
      <td>MEM</td>
      <td>23.000000</td>
      <td>33.0</td>
      <td>16.0</td>
      <td>3</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3</td>
      <td>32.000000</td>
      <td>35.0</td>
      <td>30.0</td>
      <td>3</td>
      <td>5.333333</td>
      <td>9.0</td>
      <td>2.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>30</th>
      <td>VAN</td>
      <td>22.500000</td>
      <td>29.0</td>
      <td>12.0</td>
      <td>6</td>
      <td>0.333333</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>6</td>
      <td>33.166667</td>
      <td>39.0</td>
      <td>26.0</td>
      <td>6</td>
      <td>3.666667</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>31</th>
      <td>TOR</td>
      <td>20.526316</td>
      <td>38.0</td>
      <td>2.0</td>
      <td>19</td>
      <td>0.684211</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>19</td>
      <td>34.157895</td>
      <td>43.0</td>
      <td>14.0</td>
      <td>19</td>
      <td>4.052632</td>
      <td>10.0</td>
      <td>1.0</td>
      <td>19</td>
    </tr>
    <tr>
      <th>32</th>
      <td>CHI</td>
      <td>12.125000</td>
      <td>29.0</td>
      <td>0.0</td>
      <td>8</td>
      <td>0.625000</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>8</td>
      <td>27.625000</td>
      <td>42.0</td>
      <td>0.0</td>
      <td>8</td>
      <td>3.750000</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



# Player in Game vs Out fo Game Results
in_vs_out_Lebron = """with table1 as (
SELECT home_away,
SUBSTRING( WL,1,1) AS WL 
FROM nba_stats.{name}
where MP <= {playing_time})
select home_away,WL, count(home_away) as number_of_WL
from table1
group by home_away,WL
order by home_away""".format(name = 'lebron', playing_time = 0)
This function uses the main query we see above to create custom queries when different players data is needed. The parameters that I outlines in the function are:

- Player- takes in only the last name of the player you wish to have displayed.
- Stating_time - this is the minutes you want to start at for playing time 
- max_p - max time you are exploring

This function takes in those three parameters to collect the range of times you selected to explore. So if you for example wanted to look at the data or winning percentages for lebrons teams you could have starting_time = 5 and max_p = 10 to get the winning percentage of when lebron is in the game for 5 - 10 minutes. It then uses .format to get all the calculations into a print statement that gives a read out of the analysis.


```python
def mins_of_play(player, starting_time,max_p):  
    in_vs_out = """with table1 as (
    SELECT home_away,
    SUBSTRING( WL,1,1) AS WL 
    FROM nba_stats.{name}
    where MP between {playing_time} and {max_play})
    select home_away,WL, count(home_away) as number_of_WL
    from table1
    group by home_away,WL
    order by home_away""".format(name = player, playing_time = starting_time, max_play = max_p)
    
    
    
    df = pd.read_sql(in_vs_out,con=engine)
    
    df['percent'] = df['number_of_WL']/df['number_of_WL'].sum()
    
    print("""When {Player} plays between {starting_time} and  {max_p} minutes the team loses {L}% of the time {LA}% coming from home games 
    and {LH}% of the losses at home.{Player}'s team whens {Player} plays between {starting_time} and 
    {max_p} minutes in the game
    {w}% of the time and {wa}% of them being away and {wh}% home.""".format(Player = player , 
                                            L = float(df['percent'][0]+df['percent'][2])*100,
                                              LA = df['percent'][0]*100,
                                           LH  = df['percent'][2]*100,
                                             wh = df['percent'][3]*100,
                                             w = float(df['percent'][2] + df['percent'][3])*100, 
                                            wa = df['percent'][1]*100,
                                             max_p = max_p,
                                                                           starting_time = starting_time))
    print(df.groupby(['WL']).sum())
    print(df)

```

# Players Overall W/L Out of Game


```python
mins_of_play("Jordan", 0, 0)
```

    When Jordan plays between 0 and  0 minutes the team loses 71.42857142857143% of the time 50.0% coming from home games 
        and 21.428571428571427% of the losses at home.Jordan's team whens Jordan plays between 0 and 
        0 minutes in the game
        35.71428571428571% of the time and 14.285714285714285% of them being away and 14.285714285714285% home.
        number_of_WL   percent
    WL                        
    L             10  0.714286
    W              4  0.285714
      home_away WL  number_of_WL   percent
    0         @  L             7  0.500000
    1         @  W             2  0.142857
    2      home  L             3  0.214286
    3      home  W             2  0.142857



```python
mins_of_play("lebron",0,0)
```

    When lebron plays between 0 and  0 minutes the team loses 65.16129032258064% of the time 42.58064516129032% coming from home games 
        and 22.58064516129032% of the losses at home.lebron's team whens lebron plays between 0 and 
        0 minutes in the game
        41.93548387096774% of the time and 15.483870967741936% of them being away and 19.35483870967742% home.
        number_of_WL   percent
    WL                        
    L            101  0.651613
    W             54  0.348387
      home_away WL  number_of_WL   percent
    0         @  L            66  0.425806
    1         @  W            24  0.154839
    2      home  L            35  0.225806
    3      home  W            30  0.193548


# Players Overall W/L in Game


```python
mins_of_play("lebron",1,100)
```

    When lebron plays between 1 and  100 minutes the team loses 34.55344070278185% of the time 22.035139092240115% coming from home games 
        and 12.518301610541727% of the losses at home.lebron's team whens lebron plays between 1 and 
        100 minutes in the game
        50.87847730600292% of the time and 27.086383601756953% of them being away and 38.3601756954612% home.
        number_of_WL   percent
    WL                        
    L            472  0.345534
    W            894  0.654466
      home_away WL  number_of_WL   percent
    0         @  L           301  0.220351
    1         @  W           370  0.270864
    2      home  L           171  0.125183
    3      home  W           524  0.383602



```python
mins_of_play("jordan",1,100)
```

    When jordan plays between 1 and  100 minutes the team loses 34.14179104477612% of the time 23.32089552238806% coming from home games 
        and 10.820895522388058% of the losses at home.jordan's team whens jordan plays between 1 and 
        100 minutes in the game
        50.0% of the time and 26.679104477611943% of them being away and 39.17910447761194% home.
        number_of_WL   percent
    WL                        
    L            366  0.341418
    W            706  0.658582
      home_away WL  number_of_WL   percent
    0         @  L           250  0.233209
    1         @  W           286  0.266791
    2      home  L           116  0.108209
    3      home  W           420  0.391791


# Year By Year Statistics


```python
jordan_y_y = """
select Age, avg(MP) as MP, avg(FGA) as FGA, avg(PTS) as PTS, avg(AST) as AST,
 avg(STL) as STL, avg(TRB) as TRB,(SUM(3P)/SUM(3PA)) AS  3PP,(SUM(FG)/SUM(FGA)) AS 2PP
FROM nba_stats.jordan
group by Age"""
```


```python
jordan_y_y = pd.read_sql(jordan_y_y, con = engine)

```


```python
lebron_y_y = """
select Age, avg(MP) as MP, avg(FGA) as FGA, avg(PTS) as PTS, avg(AST) as AST,
 avg(STL) as STL, avg(TRB) as TRB,(SUM(3P)/SUM(3PA)) AS  3PP,(SUM(FG)/SUM(FGA)) AS 2PP
FROM nba_stats.lebron
group by Age"""
```


```python
lebron_y_y = pd.read_sql(lebron_y_y, con = engine)

```


```python
def career_highs_lows(df,col,first):
    age = df[df[col] == max(df[col])]['Age'].unique()[0]
    print("""At age {age} {first} averaged {max_col} {col}'s which was his career high in the NBA 
    and low when he was {age1} years of age with {min_col} {col} per game.""".format(
                                                                              age = age,
                                                                              first = first,
                                                                              max_col = df[df[col] == max(df[col])][col].unique()[0],
                                                                              col = col,
                                                                              age1 =df[df[col] == min(df[col])]['Age'].unique()[0],
                                                                              min_col = df[df[col] == min(df[col])][col].unique()[0],

                                                                            ))
    
    
```


```python
lebron_y_y.head()
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
      <th>Age</th>
      <th>MP</th>
      <th>FGA</th>
      <th>PTS</th>
      <th>AST</th>
      <th>STL</th>
      <th>TRB</th>
      <th>3PP</th>
      <th>2PP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18</td>
      <td>39.806452</td>
      <td>17.9355</td>
      <td>20.161290</td>
      <td>6.064516</td>
      <td>1.580645</td>
      <td>5.9355</td>
      <td>0.310680</td>
      <td>0.4263</td>
    </tr>
    <tr>
      <th>1</th>
      <td>19</td>
      <td>37.862500</td>
      <td>18.4625</td>
      <td>21.712500</td>
      <td>6.062500</td>
      <td>1.912500</td>
      <td>5.6250</td>
      <td>0.306878</td>
      <td>0.4387</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20</td>
      <td>41.037500</td>
      <td>21.4000</td>
      <td>28.587500</td>
      <td>6.500000</td>
      <td>1.875000</td>
      <td>6.9125</td>
      <td>0.348571</td>
      <td>0.4761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>40.096386</td>
      <td>21.7831</td>
      <td>29.132530</td>
      <td>6.578313</td>
      <td>1.433735</td>
      <td>6.9398</td>
      <td>0.336986</td>
      <td>0.4746</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>35.858824</td>
      <td>19.1412</td>
      <td>24.952941</td>
      <td>5.788235</td>
      <td>1.552941</td>
      <td>6.1765</td>
      <td>0.295597</td>
      <td>0.4745</td>
    </tr>
  </tbody>
</table>
</div>




```python
career_highs_lows(lebron_y_y,'PTS',"Lebron")
career_highs_lows(lebron_y_y,'3PP',"Lebron")
career_highs_lows(lebron_y_y,'STL',"Lebron")
career_highs_lows(lebron_y_y,'AST',"Lebron")
career_highs_lows(lebron_y_y,'FGA',"Lebron")
career_highs_lows(lebron_y_y,'MP',"Lebron")
```

    At age 21 Lebron averaged 29.132530120481928 PTS's which was his career high in the NBA 
        and low when he was 36 years of age with 16.403846153846153 PTS per game.
    At age 28 Lebron averaged 0.3946360153256705 3PP's which was his career high in the NBA 
        and low when he was 22 years of age with 0.29559748427672955 3PP per game.
    At age 19 Lebron averaged 1.9125 STL's which was his career high in the NBA 
        and low when he was 37 years of age with 0.6956521739130435 STL per game.
    At age 35 Lebron averaged 8.642857142857142 AST's which was his career high in the NBA 
        and low when he was 37 years of age with 4.065217391304348 AST per game.
    At age 21 Lebron averaged 21.7831 FGA's which was his career high in the NBA 
        and low when he was 36 years of age with 11.9038 FGA per game.
    At age 20 Lebron averaged 41.0375 MP's which was his career high in the NBA 
        and low when he was 36 years of age with 21.557692307692307 MP per game.



```python
jordan_y_y.head()
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
      <th>Age</th>
      <th>MP</th>
      <th>FGA</th>
      <th>PTS</th>
      <th>AST</th>
      <th>STL</th>
      <th>TRB</th>
      <th>3PP</th>
      <th>2PP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21</td>
      <td>37.823529</td>
      <td>19.7451</td>
      <td>27.666667</td>
      <td>5.470588</td>
      <td>2.392157</td>
      <td>6.3922</td>
      <td>0.153846</td>
      <td>0.5204</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22</td>
      <td>38.617647</td>
      <td>19.9412</td>
      <td>28.705882</td>
      <td>6.264706</td>
      <td>2.323529</td>
      <td>6.5588</td>
      <td>0.192308</td>
      <td>0.5029</td>
    </tr>
    <tr>
      <th>2</th>
      <td>23</td>
      <td>36.187500</td>
      <td>26.5469</td>
      <td>33.609375</td>
      <td>3.718750</td>
      <td>2.281250</td>
      <td>4.6250</td>
      <td>0.172414</td>
      <td>0.4697</td>
    </tr>
    <tr>
      <th>3</th>
      <td>24</td>
      <td>39.963855</td>
      <td>24.1928</td>
      <td>34.662651</td>
      <td>6.168675</td>
      <td>3.542169</td>
      <td>5.4458</td>
      <td>0.181818</td>
      <td>0.5129</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25</td>
      <td>40.937500</td>
      <td>24.5125</td>
      <td>35.912500</td>
      <td>6.050000</td>
      <td>3.025000</td>
      <td>7.0125</td>
      <td>0.218391</td>
      <td>0.5497</td>
    </tr>
  </tbody>
</table>
</div>




```python
career_highs_lows(jordan_y_y,'PTS',"Jordan")
career_highs_lows(jordan_y_y,'3PP',"Jordan")
career_highs_lows(jordan_y_y,'STL',"Jordan")
career_highs_lows(jordan_y_y,'AST',"Jordan")
career_highs_lows(jordan_y_y,'FGA',"Jordan")
career_highs_lows(jordan_y_y,'MP',"Jordan")
```

    At age 25 Jordan averaged 35.9125 PTS's which was his career high in the NBA 
        and low when he was 39 years of age with 14.855263157894736 PTS per game.
    At age 32 Jordan averaged 0.42934782608695654 3PP's which was his career high in the NBA 
        and low when he was 21 years of age with 0.15384615384615385 3PP per game.
    At age 24 Jordan averaged 3.5421686746987953 STL's which was his career high in the NBA 
        and low when he was 39 years of age with 1.263157894736842 STL per game.
    At age 26 Jordan averaged 7.650602409638554 AST's which was his career high in the NBA 
        and low when he was 40 years of age with 3.433333333333333 AST per game.
    At age 23 Jordan averaged 26.5469 FGA's which was his career high in the NBA 
        and low when he was 39 years of age with 14.0395 FGA per game.
    At age 25 Jordan averaged 40.9375 MP's which was his career high in the NBA 
        and low when he was 39 years of age with 27.513157894736842 MP per game.


# Visualizations

This function is used to create a time series plot for two different players. I created each dataframe setting the length to the minimum so the time series would be able to equal x-axis lengths. I then wanted to get a general summary of game to game statistics of two players. In this case I chose Lebron and Jordan who are often highly debated. This function then creates a row number the enumerates through the given data to get lengths. I then plot each of the dataframes using row number on the x axis and chosen column on the Y axis giving the cumulative summary.


```python
def time_series(df,player1,df1,player2,column): 
    df = df.iloc[0:min(len(df),len(df1))]
    df1 = df1.iloc[0:min(len(df),len(df1))]
    df['row_num'] = [i for i,row in enumerate(df.itertuples())]
    df1['row_num'] = [i for i,row in enumerate(df.itertuples())]
    fig, ax = plt.subplots()

    plt.rcParams['figure.figsize'] = [16, 9]
    ax.plot(df['row_num'], df[column].cumsum(), label = "{name}".format(name = player1))
    ax.plot(df1['row_num'], df1[column].cumsum(), label = "{name1}".format(name1 = player2))
    plt.xlabel('Games')    
    plt.ylabel(column)
    plt.legend()
    plt.title('{name} vs {name1} {col} over Same Amount of Games'.format(name = player1, name1 = player2, col = column))    

```

We can see over the years when comparing statistics that Michael Jordan lead Lebron over the same amount of games. Jordan was ahead of Lebron by over 5 k points, over 800 steals, 100+ blocks, and with only around 500 extra minutes played. Lebron lead over Jordan with 100 more rebounds, over 100+ assists and 800 3 pointers. It is very evident with these visuals that Lebron and Jordan were different players. Lebron is more of an outside shooter that distributes the ball very well considering he still scores a lot with high assists. Michael Jordan was a scrapier defensive player with higher steals which also caused Jordan to get a lot of steals in his career. Jordan did not spend a plot of time beyond the 3 point line as he kept his game within and had a lot of success doing so. The difference is that Michael Jordan is 3 years older in this in comparison to Lebron who started to play straight out of high school at age 18. The next question is who was better by age? 


```python
time_series(df_mj,"Jordan", df_lb, "Lebron","PTS")
time_series(df_mj,"Jordan", df_lb, "Lebron","AST")
time_series(df_mj,"Jordan", df_lb, "Lebron","3P")
time_series(df_mj,"Jordan", df_lb, "Lebron","STL")
time_series(df_mj,"Jordan", df_lb, "Lebron","BLK")
time_series(df_mj,"Jordan", df_lb, "Lebron","TRB")
time_series(df_mj,"Jordan", df_lb, "Lebron","PF")
time_series(df_mj,"Jordan", df_lb, "Lebron","MP")
```

    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['row_num'] = [i for i,row in enumerate(df.itertuples())]
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/3530414438.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1['row_num'] = [i for i,row in enumerate(df.itertuples())]



    
![png](output_68_1.png)
    



    
![png](output_68_2.png)
    



    
![png](output_68_3.png)
    



    
![png](output_68_4.png)
    



    
![png](output_68_5.png)
    



    
![png](output_68_6.png)
    



    
![png](output_68_7.png)
    



    
![png](output_68_8.png)
    


This function is usewd to find the average percentages of given datasets for comparison. It does this by taking the given column then adding A to the end of the string to get the total attempts column of that stat. Then I plot the mean of the column divided my the attempts column for that stat. 


```python
def barplot(df,player, df1,player1, col):
# Create a figure and axes
    col2 = col + "A"
    df[player] = player
    df1[player1] = player1
    total = pd.concat([df,df1])
    fig, ax = plt.subplots()

# Plot the bar plots
    bar_width = 0.24
    plt.bar(df[player], (df[col]/df[col2]).mean(), width = bar_width, label=player)
    plt.bar(df1[player1], (df1[col]/df1[col2]).mean(), width = bar_width, label=player1)
    
# Add a title and labels
    ax.set_title('{value}%'.format(value = col))
    ax.set_xlabel('Players')
    ax.set_ylabel('{value}%'.format(value = col))

# Add a legend
    ax.legend()

# Display the plot
    plt.show()
    print("{p1}".format( p1 = player1), ":", (df1[col]/df1[col2]).mean())
    print("{p}".format( p = player), ":", (df[col]/df[col2]).mean())
```


```python
barplot(df_mj,"MJ",df_lb,"Lebron","FG")
barplot(df_mj,"MJ",df_lb,"Lebron","3P")
barplot(df_mj,"MJ",df_lb,"Lebron","FT")
```

    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/241579014.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df[player] = player
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/241579014.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1[player1] = player1



    
![png](output_71_1.png)
    


    Lebron : 0.505121996133121
    MJ : 0.4958998974271973


    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/241579014.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df[player] = player
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/241579014.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1[player1] = player1



    
![png](output_71_4.png)
    


    Lebron : 0.31181868000765556
    MJ : 0.2677875348040266


    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/241579014.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df[player] = player
    /var/folders/d5/yv3yty4s3y33ty4r_pc546j80000gn/T/ipykernel_52401/241579014.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df1[player1] = player1



    
![png](output_71_7.png)
    


    Lebron : 0.7271503188675675
    MJ : 0.8278913476447494


This box plot can be used to spot the outliers in the data of two different players. YUou can input any column and player into the data depending on who you are analyzing.


```python
def boxplot(df,player,column,df1,player1):
    # Create a figure and axes
    fig, ax = plt.subplots()

# Plot the box plots for each dataframe
    ax.boxplot([df[column], df1[column]], labels=[player, player1])

# Add a title and labels
    ax.set_title('{col} By Game {plyr} vs {plyr1}'.format(col = column, plyr = player, plyr1 = player1),fontsize = 40)
    ax.set_ylabel('{col}'.format(col = column),fontsize = 30)

# Display the plot
    plt.show()

```


```python
#boxplot(df_mj,"Jordan","PTS", df_lb, "Lebron")
#boxplot(df_mj,"Jordan","AST", df_lb, "Lebron")
#boxplot(df_mj,"Jordan","STL", df_lb, "Lebron")
#boxplot(df_mj,"Jordan","3P", df_lb, "Lebron")
```

![image.png](attachment:image.png)
![image-2.png](attachment:image-2.png)
![image-3.png](attachment:image-3.png)
![image-4.png](attachment:image-4.png)


```python
def histogram(df, column,Name, df1, Name2):
    import seaborn as sns
    import numpy as np
    import matplotlib.pyplot as plt

# Plot the first kdeplot
    ax = sns.kdeplot(np.array(df[column]), color='blue', label= Name)

# Plot the second kdeplot
    sns.kdeplot(np.array(df1[column]), color='red', label=Name2, ax=ax)
# Add a legend
    plt.legend(loc=2, prop={'size': 25})
    plt.show()
    
    
    
# Create a histogram of the first dataset
    plt.hist(df[column], bins=50, alpha=0.5, color='blue', label=Name)

# Create a histogram of the second dataset
    plt.hist(df1[column], bins=50, alpha=0.5, color='red', label=Name2)

# Add a title and labels to the x and y axes
    plt.title('{player} vs {player1} Histograms of {col}'.format(player = Name, player1 = Name2, col = column),fontsize = 40)
    plt.xlabel('Values')
    plt.ylabel('Frequency')

    mean1 = np.mean(df[column])
    mean2 = np.mean(df1[column])
    plt.axvline(mean1, color='blue', linestyle='dashed', linewidth=2, label=str(Name +" mean"))
    plt.axvline(mean2, color='orange', linestyle='dashed', linewidth=2, label=str(Name2+" mean"))

    plt.legend(loc=2, prop={'size': 25})

    plt.show()
    
    from scipy import stats

# Assuming that lebron_points and michael_points are two lists containing the points per game

# Conduct the t-test
    t_stat, p_value = stats.ttest_ind(df[column], df1[column])

# Specify the significance level
    alpha = 0.05

# Check if the p-value is less than the significance level
    if p_value < alpha:
        print("There is a significant difference in the {col} between {player} and {player2}.".format( player = Name, player2 = Name2, col = column))
    else:
        print("There is no significant difference in the {col} between {player} and {player2}.".format( player = Name, player2 = Name2, col = column))
    print("T-Stat: " , t_stat)
    print("P-value: ", p_value)
    print(Name, " Average {col}: ".format(col = column), df[column].mean())
    print(Name2, " Average {col}: ".format(col = column), df1[column].mean())
    

    
```

Below we did a T-Test to figure out the chance of Lebron and Jordan go out and put up similair amount of points. In our t test we created a plot to first look at the different in distributions amoungst the data sets. We noticed that Lebron had a lower mean value of points scored. The distributions were relatively similiar keeping. Jordan defiently had the edge on lebron when it came to those high scoring games that you notice a lot of when 50 <. 
The t-test we ran was to see if there was a significant difference between Lebron and Jordans spread. In this case we rejected the null hypothesis with a sub .05 p-value comparing the sets. Lebron also has a very large amount of games that he sat without injury compared to Jordan. Lebron does play consistantly showing a tighter distribtion compared to Micahel Jordan. 


```python
histogram(df_mj, 'PTS',"Jordan", df_lb, "Lebron")
```


    
![png](output_78_0.png)
    



    
![png](output_78_1.png)
    


    There is a significant difference in the PTS between Jordan and Lebron.
    T-Stat:  11.400789226841338
    P-value:  2.2553356676318803e-29
    Jordan  Average PTS:  29.514880952380953
    Lebron  Average PTS:  24.577484364141764



```python
import seaborn as sns
```


```python
plot_mj = df_mj.groupby(['3P', 'PTS', 'FG']).agg({'3PA':'mean',
                                           'FGA':'mean'}).sort_values('3P', ascending=False).reset_index().head(50)

plt.figure(figsize=(15,7))
plt.title("Jordan FGA vs PTS Kernel Density Estimate")
sns.kdeplot(x=plot_mj.FGA, y=plot_mj.PTS,cmap = "Reds",cbar=True, shade = True, shade_lowest = False)
```

    /Users/tylerbrown/opt/anaconda3/lib/python3.9/site-packages/seaborn/distributions.py:1718: UserWarning: `shade_lowest` is now deprecated in favor of `thresh`. Setting `thresh=0.05`, but please update your code.
      warnings.warn(msg, UserWarning)





    <AxesSubplot:title={'center':'Jordan FGA vs PTS Kernel Density Estimate'}, xlabel='FGA', ylabel='PTS'>




    
![png](output_80_2.png)
    



```python
plot_lb = df_lb.groupby(['3P', 'PTS', 'FG']).agg({'3PA':'mean',
                                           'FGA':'mean'}).sort_values('3P', ascending=False).reset_index().head(50)

plt.figure(figsize=(15,7))
plt.title("Lebron FGA vs PTS Kernel Density Estimate")
sns.kdeplot(x=plot_lb.FGA, y=plot_lb.PTS,cmap = "Reds", cbar=True,shade = True, shade_lowest = False)
```

    /Users/tylerbrown/opt/anaconda3/lib/python3.9/site-packages/seaborn/distributions.py:1718: UserWarning: `shade_lowest` is now deprecated in favor of `thresh`. Setting `thresh=0.05`, but please update your code.
      warnings.warn(msg, UserWarning)





    <AxesSubplot:title={'center':'Lebron FGA vs PTS Kernel Density Estimate'}, xlabel='FGA', ylabel='PTS'>




    
![png](output_81_2.png)
    



```python
#sns.set(rc={'figure.figsize':(40,38)})
sns.catplot(x='3PA', y='3P', data=df_lb)
plt.gcf().set_size_inches(15, 8)
plt.title("Lebron James Catplot")
plt.xticks(rotation=45)
```




    (array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13]),
     [Text(0, 0, '0.0'),
      Text(1, 0, '1.0'),
      Text(2, 0, '2.0'),
      Text(3, 0, '3.0'),
      Text(4, 0, '4.0'),
      Text(5, 0, '5.0'),
      Text(6, 0, '6.0'),
      Text(7, 0, '7.0'),
      Text(8, 0, '8.0'),
      Text(9, 0, '9.0'),
      Text(10, 0, '10.0'),
      Text(11, 0, '11.0'),
      Text(12, 0, '12.0'),
      Text(13, 0, '13.0')])




    
![png](output_82_1.png)
    



```python
#sns.set(rc={'figure.figsize':(40,38)})
sns.catplot(x='3PA', y='3P', data=df_mj)
plt.gcf().set_size_inches(15, 8)
plt.title("Michael Jordan Catplot")
plt.xticks(rotation=45)

```




    (array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]),
     [Text(0, 0, '0.0'),
      Text(1, 0, '1.0'),
      Text(2, 0, '2.0'),
      Text(3, 0, '3.0'),
      Text(4, 0, '4.0'),
      Text(5, 0, '5.0'),
      Text(6, 0, '6.0'),
      Text(7, 0, '7.0'),
      Text(8, 0, '8.0'),
      Text(9, 0, '12.0')])




    
![png](output_83_1.png)
    

