# Essentials of Film Production Industry


```python

```

## Importing relevant python libraries


```python
# importing relevant libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

## Data Analysis

 Reading the data


```python
df = pd.read_csv('Project data/tn.movie_budgets.csv', index_col=0)
df.head()
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
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross</th>
      <th>worldwide_gross</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Dec 18, 2009</td>
      <td>Avatar</td>
      <td>$425,000,000</td>
      <td>$760,507,625</td>
      <td>$2,776,345,279</td>
    </tr>
    <tr>
      <th>2</th>
      <td>May 20, 2011</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>$410,600,000</td>
      <td>$241,063,875</td>
      <td>$1,045,663,875</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Jun 7, 2019</td>
      <td>Dark Phoenix</td>
      <td>$350,000,000</td>
      <td>$42,762,350</td>
      <td>$149,762,350</td>
    </tr>
    <tr>
      <th>4</th>
      <td>May 1, 2015</td>
      <td>Avengers: Age of Ultron</td>
      <td>$330,600,000</td>
      <td>$459,005,868</td>
      <td>$1,403,013,963</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Dec 15, 2017</td>
      <td>Star Wars Ep. VIII: The Last Jedi</td>
      <td>$317,000,000</td>
      <td>$620,181,382</td>
      <td>$1,316,721,747</td>
    </tr>
  </tbody>
</table>
</div>



Calculating the Return on Investment(ROI)


```python
# Convert the currency columns from string to numeric values
df['production_budget'] = df['production_budget'].replace('[\$,]', '', regex=True).astype(float)
df['domestic_gross'] = df['domestic_gross'].replace('[\$,]', '', regex=True).astype(float)
df['worldwide_gross'] = df['worldwide_gross'].replace('[\$,]', '', regex=True).astype(float)

# Calculate ROI for each movie
df['ROI_domestic'] = (df['domestic_gross'] - df['production_budget']) / df['production_budget'] * 100
df['ROI_worldwide'] = (df['worldwide_gross'] - df['production_budget']) / df['production_budget'] * 100

# Display the calculated ROI for each movie
print(df[['movie', 'ROI_domestic', 'ROI_worldwide']])
```

                                              movie  ROI_domestic  ROI_worldwide
    id                                                                          
    1                                        Avatar     78.942971     553.257713
    2   Pirates of the Caribbean: On Stranger Tides    -41.289850     154.667286
    3                                  Dark Phoenix    -87.782186     -57.210757
    4                       Avengers: Age of Ultron     38.840250     324.384139
    5             Star Wars Ep. VIII: The Last Jedi     95.640815     315.369636
    ..                                          ...           ...            ...
    78                                       Red 11   -100.000000    -100.000000
    79                                    Following    708.033333    3908.250000
    80                Return to the Land of Wonders    -73.240000     -73.240000
    81                         A Plague So Pleasant   -100.000000    -100.000000
    82                            My Date With Drew  16358.272727   16358.272727
    
    [5782 rows x 3 columns]
    


```python
# Rank movies based on ROI
df = df.sort_values(by='ROI_worldwide', ascending=False)

# Display the ranked movies with best ROI
ranked_movies = df[['movie', 'ROI_domestic', 'ROI_worldwide']]
print(ranked_movies)
```

                                    movie   ROI_domestic  ROI_worldwide
    id                                                                 
    46                        Deep Throat  179900.000000  179900.000000
    14                            Mad Max    4275.000000   49775.000000
    93                Paranormal Activity   23881.957778   43051.785333
    80                        The Gallows   22664.410000   41556.474000
    7             The Blair Witch Project   23323.183167   41283.333333
    ..                                ...            ...            ...
    23                           Pancakes    -100.000000    -100.000000
    22                            Show Me    -100.000000    -100.000000
    21            My Beautiful Laundrette    -100.000000    -100.000000
    17                          Checkmate    -100.000000    -100.000000
    83  No Man's Land: The Rise of Reeker    -100.000000    -100.000000
    
    [5782 rows x 3 columns]
    


```python
df = pd.read_csv('Project data/tmdb.movies.csv', index_col=0)
df.head()
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
      <th>genre_ids</th>
      <th>id</th>
      <th>original_language</th>
      <th>original_title</th>
      <th>popularity</th>
      <th>release_date</th>
      <th>title</th>
      <th>vote_average</th>
      <th>vote_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[12, 14, 10751]</td>
      <td>12444</td>
      <td>en</td>
      <td>Harry Potter and the Deathly Hallows: Part 1</td>
      <td>33.533</td>
      <td>2010-11-19</td>
      <td>Harry Potter and the Deathly Hallows: Part 1</td>
      <td>7.7</td>
      <td>10788</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[14, 12, 16, 10751]</td>
      <td>10191</td>
      <td>en</td>
      <td>How to Train Your Dragon</td>
      <td>28.734</td>
      <td>2010-03-26</td>
      <td>How to Train Your Dragon</td>
      <td>7.7</td>
      <td>7610</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[12, 28, 878]</td>
      <td>10138</td>
      <td>en</td>
      <td>Iron Man 2</td>
      <td>28.515</td>
      <td>2010-05-07</td>
      <td>Iron Man 2</td>
      <td>6.8</td>
      <td>12368</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[16, 35, 10751]</td>
      <td>862</td>
      <td>en</td>
      <td>Toy Story</td>
      <td>28.005</td>
      <td>1995-11-22</td>
      <td>Toy Story</td>
      <td>7.9</td>
      <td>10174</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[28, 878, 12]</td>
      <td>27205</td>
      <td>en</td>
      <td>Inception</td>
      <td>27.920</td>
      <td>2010-07-16</td>
      <td>Inception</td>
      <td>8.3</td>
      <td>22186</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.info()

```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 26517 entries, 0 to 26516
    Data columns (total 9 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   genre_ids          26517 non-null  object 
     1   id                 26517 non-null  int64  
     2   original_language  26517 non-null  object 
     3   original_title     26517 non-null  object 
     4   popularity         26517 non-null  float64
     5   release_date       26517 non-null  object 
     6   title              26517 non-null  object 
     7   vote_average       26517 non-null  float64
     8   vote_count         26517 non-null  int64  
    dtypes: float64(2), int64(2), object(5)
    memory usage: 2.0+ MB
    


```python
# Genre Analysis
genres = df['genre_ids'].apply(lambda x: x.strip('[]').split(', '))

# Flatten the list of genres
all_genres = [genre for sublist in genres for genre in sublist]

# Calculate the genre distribution
genre_distribution = pd.Series(all_genres).value_counts()

# Identify the most popular genres
most_popular_genres = genre_distribution.head()

# Print the genre distribution
print("Genre Distribution:")
print(genre_distribution)

# Print the most popular genres
print("\nMost Popular Genres:")
print(most_popular_genres)
```

    Genre Distribution:
    18       8303
    35       5652
    99       4965
    53       4207
    27       3683
    28       2612
             2479
    10749    2321
    878      1762
    10751    1565
    80       1515
    16       1486
    12       1400
    10402    1267
    9648     1237
    14       1139
    10770    1084
    36        622
    10752     330
    37        205
    dtype: int64
    
    Most Popular Genres:
    18    8303
    35    5652
    99    4965
    53    4207
    27    3683
    dtype: int64
    


```python
df = pd.read_csv('Project data/title.basics.csv', index_col=0)
df.head(10)

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
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
    </tr>
    <tr>
      <th>tconst</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>tt0063540</th>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
    </tr>
    <tr>
      <th>tt0066787</th>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
    </tr>
    <tr>
      <th>tt0069049</th>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
    </tr>
    <tr>
      <th>tt0069204</th>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
    </tr>
    <tr>
      <th>tt0100275</th>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
    </tr>
    <tr>
      <th>tt0111414</th>
      <td>A Thin Life</td>
      <td>A Thin Life</td>
      <td>2018</td>
      <td>75.0</td>
      <td>Comedy</td>
    </tr>
    <tr>
      <th>tt0112502</th>
      <td>Bigfoot</td>
      <td>Bigfoot</td>
      <td>2017</td>
      <td>NaN</td>
      <td>Horror,Thriller</td>
    </tr>
    <tr>
      <th>tt0137204</th>
      <td>Joe Finds Grace</td>
      <td>Joe Finds Grace</td>
      <td>2017</td>
      <td>83.0</td>
      <td>Adventure,Animation,Comedy</td>
    </tr>
    <tr>
      <th>tt0139613</th>
      <td>O Silêncio</td>
      <td>O Silêncio</td>
      <td>2012</td>
      <td>NaN</td>
      <td>Documentary,History</td>
    </tr>
    <tr>
      <th>tt0144449</th>
      <td>Nema aviona za Zagreb</td>
      <td>Nema aviona za Zagreb</td>
      <td>2012</td>
      <td>82.0</td>
      <td>Biography</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = pd.read_csv('Project data/title.ratings.csv', index_col=0)
df.head()

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
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
    <tr>
      <th>tconst</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>tt10356526</th>
      <td>8.3</td>
      <td>31</td>
    </tr>
    <tr>
      <th>tt10384606</th>
      <td>8.9</td>
      <td>559</td>
    </tr>
    <tr>
      <th>tt1042974</th>
      <td>6.4</td>
      <td>20</td>
    </tr>
    <tr>
      <th>tt1043726</th>
      <td>4.2</td>
      <td>50352</td>
    </tr>
    <tr>
      <th>tt1060240</th>
      <td>6.5</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Load the first table into a DataFrame
df1 = pd.read_csv('Project data/title.basics.csv', index_col='tconst')

# Load the second table into a DataFrame
df2 = pd.read_csv('Project data/title.ratings.csv', index_col='tconst')

# Perform an inner join on the common column "tconst"
joined_df = df1.merge(df2, left_index=True, right_index=True)

# Print the joined DataFrame
joined_df.head()
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
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
    <tr>
      <th>tconst</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>tt0063540</th>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.0</td>
      <td>77</td>
    </tr>
    <tr>
      <th>tt0066787</th>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
      <td>7.2</td>
      <td>43</td>
    </tr>
    <tr>
      <th>tt0069049</th>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
      <td>6.9</td>
      <td>4517</td>
    </tr>
    <tr>
      <th>tt0069204</th>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
      <td>6.1</td>
      <td>13</td>
    </tr>
    <tr>
      <th>tt0100275</th>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>6.5</td>
      <td>119</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Split the 'genres' column into separate genre columns
genres_columns = joined_df['genres'].str.get_dummies(',')

# Concatenate the genre columns with the original DataFrame
df = pd.concat([joined_df, genres_columns], axis=1)

# Print the updated DataFrame
(df)
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
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>Action</th>
      <th>Adult</th>
      <th>Adventure</th>
      <th>...</th>
      <th>Mystery</th>
      <th>News</th>
      <th>Reality-TV</th>
      <th>Romance</th>
      <th>Sci-Fi</th>
      <th>Short</th>
      <th>Sport</th>
      <th>Thriller</th>
      <th>War</th>
      <th>Western</th>
    </tr>
    <tr>
      <th>tconst</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>tt0063540</th>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.0</td>
      <td>77</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tt0066787</th>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
      <td>7.2</td>
      <td>43</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tt0069049</th>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
      <td>6.9</td>
      <td>4517</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tt0069204</th>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
      <td>6.1</td>
      <td>13</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tt0100275</th>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>6.5</td>
      <td>119</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>tt9913084</th>
      <td>Diabolik sono io</td>
      <td>Diabolik sono io</td>
      <td>2019</td>
      <td>75.0</td>
      <td>Documentary</td>
      <td>6.2</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tt9914286</th>
      <td>Sokagin Çocuklari</td>
      <td>Sokagin Çocuklari</td>
      <td>2019</td>
      <td>98.0</td>
      <td>Drama,Family</td>
      <td>8.7</td>
      <td>136</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tt9914642</th>
      <td>Albatross</td>
      <td>Albatross</td>
      <td>2017</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>8.5</td>
      <td>8</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tt9914942</th>
      <td>La vida sense la Sara Amat</td>
      <td>La vida sense la Sara Amat</td>
      <td>2019</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.6</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tt9916160</th>
      <td>Drømmeland</td>
      <td>Drømmeland</td>
      <td>2019</td>
      <td>72.0</td>
      <td>Documentary</td>
      <td>6.5</td>
      <td>11</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>73856 rows × 33 columns</p>
</div>




```python
# View the column headers
column_heads = df.columns
# Print the column headers
print(column_heads)
```

    Index(['primary_title', 'original_title', 'start_year', 'runtime_minutes',
           'genres', 'averagerating', 'numvotes', 'Action', 'Adult', 'Adventure',
           'Animation', 'Biography', 'Comedy', 'Crime', 'Documentary', 'Drama',
           'Family', 'Fantasy', 'Game-Show', 'History', 'Horror', 'Music',
           'Musical', 'Mystery', 'News', 'Reality-TV', 'Romance', 'Sci-Fi',
           'Short', 'Sport', 'Thriller', 'War', 'Western'],
          dtype='object')
    


```python
# Calculate the sum of each genre
genre_counts = df.iloc[:, 6:].sum()

# Create a histogram of genre 
plt.figure(figsize=(10, 6))
genre_counts.plot(kind='bar')
plt.xlabel('Genre')
plt.ylabel('Frequency')
plt.title('Histogram of Genres')
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_18_0.png)
    



```python

```
