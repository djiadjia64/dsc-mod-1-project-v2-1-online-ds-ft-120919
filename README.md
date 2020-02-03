```python
#Cleaned Code with comments
import os 

import numpy as np
import pandas as pd

from glob import glob

import matplotlib.pyplot as plt
```


```python
#initially had the question of which year/month produced the most profitable movie
#My approach involved joining the two tables tmdb.movies and tn.movie_budgets on a common title to obtain the date combined 
#with the gross and production budget columns for comparison.
```


```python
csv_files = glob("zippedData/*.csv.gz")
csv_files
#making a retrievable list of dataframes from zippedData
```




    ['zippedData\\bom.movie_gross.csv.gz',
     'zippedData\\imdb.name.basics.csv.gz',
     'zippedData\\imdb.title.akas.csv.gz',
     'zippedData\\imdb.title.basics.csv.gz',
     'zippedData\\imdb.title.crew.csv.gz',
     'zippedData\\imdb.title.principals.csv.gz',
     'zippedData\\imdb.title.ratings.csv.gz',
     'zippedData\\tmdb.movies.csv.gz',
     'zippedData\\tn.movie_budgets.csv.gz']




```python
#discovering usable dataframes for merging
```


```python
df1 = pd.read_csv('zippedData\\tmdb.movies.csv.gz')
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
      <th>Unnamed: 0</th>
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
      <td>0</td>
      <td>0</td>
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
      <td>1</td>
      <td>1</td>
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
      <td>2</td>
      <td>2</td>
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
      <td>3</td>
      <td>3</td>
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
      <td>4</td>
      <td>4</td>
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
    <tr>
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
      <td>26512</td>
      <td>26512</td>
      <td>[27, 18]</td>
      <td>488143</td>
      <td>en</td>
      <td>Laboratory Conditions</td>
      <td>0.600</td>
      <td>2018-10-13</td>
      <td>Laboratory Conditions</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>26513</td>
      <td>26513</td>
      <td>[18, 53]</td>
      <td>485975</td>
      <td>en</td>
      <td>_EXHIBIT_84xxx_</td>
      <td>0.600</td>
      <td>2018-05-01</td>
      <td>_EXHIBIT_84xxx_</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>26514</td>
      <td>26514</td>
      <td>[14, 28, 12]</td>
      <td>381231</td>
      <td>en</td>
      <td>The Last One</td>
      <td>0.600</td>
      <td>2018-10-01</td>
      <td>The Last One</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>26515</td>
      <td>26515</td>
      <td>[10751, 12, 28]</td>
      <td>366854</td>
      <td>en</td>
      <td>Trailer Made</td>
      <td>0.600</td>
      <td>2018-06-22</td>
      <td>Trailer Made</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>26516</td>
      <td>26516</td>
      <td>[53, 27]</td>
      <td>309885</td>
      <td>en</td>
      <td>The Church</td>
      <td>0.600</td>
      <td>2018-10-05</td>
      <td>The Church</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>26517 rows × 10 columns</p>
</div>




```python
df2 = pd.read_csv('zippedData\\bom.movie_gross.csv.gz')
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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Alice in Wonderland (2010)</td>
      <td>BV</td>
      <td>334200000.0</td>
      <td>691300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Harry Potter and the Deathly Hallows Part 1</td>
      <td>WB</td>
      <td>296000000.0</td>
      <td>664300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Shrek Forever After</td>
      <td>P/DW</td>
      <td>238700000.0</td>
      <td>513900000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>3382</td>
      <td>The Quake</td>
      <td>Magn.</td>
      <td>6200.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>3383</td>
      <td>Edward II (2018 re-release)</td>
      <td>FM</td>
      <td>4800.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>3384</td>
      <td>El Pacto</td>
      <td>Sony</td>
      <td>2500.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>3385</td>
      <td>The Swan</td>
      <td>Synergetic</td>
      <td>2400.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>3386</td>
      <td>An Actor Prepares</td>
      <td>Grav.</td>
      <td>1700.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
<p>3387 rows × 5 columns</p>
</div>




```python
df2.set_index(['title'])
#setting index to match other dataframe
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
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
    <tr>
      <th>title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>Alice in Wonderland (2010)</td>
      <td>BV</td>
      <td>334200000.0</td>
      <td>691300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>Harry Potter and the Deathly Hallows Part 1</td>
      <td>WB</td>
      <td>296000000.0</td>
      <td>664300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>Shrek Forever After</td>
      <td>P/DW</td>
      <td>238700000.0</td>
      <td>513900000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>The Quake</td>
      <td>Magn.</td>
      <td>6200.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>Edward II (2018 re-release)</td>
      <td>FM</td>
      <td>4800.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>El Pacto</td>
      <td>Sony</td>
      <td>2500.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>The Swan</td>
      <td>Synergetic</td>
      <td>2400.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>An Actor Prepares</td>
      <td>Grav.</td>
      <td>1700.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
<p>3387 rows × 4 columns</p>
</div>




```python
df1.set_index(['original_title'])
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
      <th>Unnamed: 0</th>
      <th>genre_ids</th>
      <th>id</th>
      <th>original_language</th>
      <th>popularity</th>
      <th>release_date</th>
      <th>title</th>
      <th>vote_average</th>
      <th>vote_count</th>
    </tr>
    <tr>
      <th>original_title</th>
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
      <td>Harry Potter and the Deathly Hallows: Part 1</td>
      <td>0</td>
      <td>[12, 14, 10751]</td>
      <td>12444</td>
      <td>en</td>
      <td>33.533</td>
      <td>2010-11-19</td>
      <td>Harry Potter and the Deathly Hallows: Part 1</td>
      <td>7.7</td>
      <td>10788</td>
    </tr>
    <tr>
      <td>How to Train Your Dragon</td>
      <td>1</td>
      <td>[14, 12, 16, 10751]</td>
      <td>10191</td>
      <td>en</td>
      <td>28.734</td>
      <td>2010-03-26</td>
      <td>How to Train Your Dragon</td>
      <td>7.7</td>
      <td>7610</td>
    </tr>
    <tr>
      <td>Iron Man 2</td>
      <td>2</td>
      <td>[12, 28, 878]</td>
      <td>10138</td>
      <td>en</td>
      <td>28.515</td>
      <td>2010-05-07</td>
      <td>Iron Man 2</td>
      <td>6.8</td>
      <td>12368</td>
    </tr>
    <tr>
      <td>Toy Story</td>
      <td>3</td>
      <td>[16, 35, 10751]</td>
      <td>862</td>
      <td>en</td>
      <td>28.005</td>
      <td>1995-11-22</td>
      <td>Toy Story</td>
      <td>7.9</td>
      <td>10174</td>
    </tr>
    <tr>
      <td>Inception</td>
      <td>4</td>
      <td>[28, 878, 12]</td>
      <td>27205</td>
      <td>en</td>
      <td>27.920</td>
      <td>2010-07-16</td>
      <td>Inception</td>
      <td>8.3</td>
      <td>22186</td>
    </tr>
    <tr>
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
      <td>Laboratory Conditions</td>
      <td>26512</td>
      <td>[27, 18]</td>
      <td>488143</td>
      <td>en</td>
      <td>0.600</td>
      <td>2018-10-13</td>
      <td>Laboratory Conditions</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>_EXHIBIT_84xxx_</td>
      <td>26513</td>
      <td>[18, 53]</td>
      <td>485975</td>
      <td>en</td>
      <td>0.600</td>
      <td>2018-05-01</td>
      <td>_EXHIBIT_84xxx_</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>The Last One</td>
      <td>26514</td>
      <td>[14, 28, 12]</td>
      <td>381231</td>
      <td>en</td>
      <td>0.600</td>
      <td>2018-10-01</td>
      <td>The Last One</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>Trailer Made</td>
      <td>26515</td>
      <td>[10751, 12, 28]</td>
      <td>366854</td>
      <td>en</td>
      <td>0.600</td>
      <td>2018-06-22</td>
      <td>Trailer Made</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>The Church</td>
      <td>26516</td>
      <td>[53, 27]</td>
      <td>309885</td>
      <td>en</td>
      <td>0.600</td>
      <td>2018-10-05</td>
      <td>The Church</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>26517 rows × 9 columns</p>
</div>




```python
d3 = pd.read_csv('zippedData\\imdb.title.ratings.csv.gz')
d3
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
      <th>tconst</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>tt10356526</td>
      <td>8.3</td>
      <td>31</td>
    </tr>
    <tr>
      <td>1</td>
      <td>tt10384606</td>
      <td>8.9</td>
      <td>559</td>
    </tr>
    <tr>
      <td>2</td>
      <td>tt1042974</td>
      <td>6.4</td>
      <td>20</td>
    </tr>
    <tr>
      <td>3</td>
      <td>tt1043726</td>
      <td>4.2</td>
      <td>50352</td>
    </tr>
    <tr>
      <td>4</td>
      <td>tt1060240</td>
      <td>6.5</td>
      <td>21</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>73851</td>
      <td>tt9805820</td>
      <td>8.1</td>
      <td>25</td>
    </tr>
    <tr>
      <td>73852</td>
      <td>tt9844256</td>
      <td>7.5</td>
      <td>24</td>
    </tr>
    <tr>
      <td>73853</td>
      <td>tt9851050</td>
      <td>4.7</td>
      <td>14</td>
    </tr>
    <tr>
      <td>73854</td>
      <td>tt9886934</td>
      <td>7.0</td>
      <td>5</td>
    </tr>
    <tr>
      <td>73855</td>
      <td>tt9894098</td>
      <td>6.3</td>
      <td>128</td>
    </tr>
  </tbody>
</table>
<p>73856 rows × 3 columns</p>
</div>




```python
d4 = pd.read_csv('zippedData\\imdb.title.basics.csv.gz')
d4
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
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>tt0063540</td>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
    </tr>
    <tr>
      <td>1</td>
      <td>tt0066787</td>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
    </tr>
    <tr>
      <td>2</td>
      <td>tt0069049</td>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
    </tr>
    <tr>
      <td>3</td>
      <td>tt0069204</td>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
    </tr>
    <tr>
      <td>4</td>
      <td>tt0100275</td>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>146139</td>
      <td>tt9916538</td>
      <td>Kuambil Lagi Hatiku</td>
      <td>Kuambil Lagi Hatiku</td>
      <td>2019</td>
      <td>123.0</td>
      <td>Drama</td>
    </tr>
    <tr>
      <td>146140</td>
      <td>tt9916622</td>
      <td>Rodolpho Teóphilo - O Legado de um Pioneiro</td>
      <td>Rodolpho Teóphilo - O Legado de um Pioneiro</td>
      <td>2015</td>
      <td>NaN</td>
      <td>Documentary</td>
    </tr>
    <tr>
      <td>146141</td>
      <td>tt9916706</td>
      <td>Dankyavar Danka</td>
      <td>Dankyavar Danka</td>
      <td>2013</td>
      <td>NaN</td>
      <td>Comedy</td>
    </tr>
    <tr>
      <td>146142</td>
      <td>tt9916730</td>
      <td>6 Gunn</td>
      <td>6 Gunn</td>
      <td>2017</td>
      <td>116.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>146143</td>
      <td>tt9916754</td>
      <td>Chico Albuquerque - Revelações</td>
      <td>Chico Albuquerque - Revelações</td>
      <td>2013</td>
      <td>NaN</td>
      <td>Documentary</td>
    </tr>
  </tbody>
</table>
<p>146144 rows × 6 columns</p>
</div>




```python
d3.set_index(d3['tconst'])
d3
#setting index to merge dataframes
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
      <th>tconst</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>tt10356526</td>
      <td>8.3</td>
      <td>31</td>
    </tr>
    <tr>
      <td>1</td>
      <td>tt10384606</td>
      <td>8.9</td>
      <td>559</td>
    </tr>
    <tr>
      <td>2</td>
      <td>tt1042974</td>
      <td>6.4</td>
      <td>20</td>
    </tr>
    <tr>
      <td>3</td>
      <td>tt1043726</td>
      <td>4.2</td>
      <td>50352</td>
    </tr>
    <tr>
      <td>4</td>
      <td>tt1060240</td>
      <td>6.5</td>
      <td>21</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>73851</td>
      <td>tt9805820</td>
      <td>8.1</td>
      <td>25</td>
    </tr>
    <tr>
      <td>73852</td>
      <td>tt9844256</td>
      <td>7.5</td>
      <td>24</td>
    </tr>
    <tr>
      <td>73853</td>
      <td>tt9851050</td>
      <td>4.7</td>
      <td>14</td>
    </tr>
    <tr>
      <td>73854</td>
      <td>tt9886934</td>
      <td>7.0</td>
      <td>5</td>
    </tr>
    <tr>
      <td>73855</td>
      <td>tt9894098</td>
      <td>6.3</td>
      <td>128</td>
    </tr>
  </tbody>
</table>
<p>73856 rows × 3 columns</p>
</div>




```python
d4.set_index(d4['tconst'])
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
      <th>tconst</th>
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
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>tt0063540</td>
      <td>tt0063540</td>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
    </tr>
    <tr>
      <td>tt0066787</td>
      <td>tt0066787</td>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
    </tr>
    <tr>
      <td>tt0069049</td>
      <td>tt0069049</td>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
    </tr>
    <tr>
      <td>tt0069204</td>
      <td>tt0069204</td>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
    </tr>
    <tr>
      <td>tt0100275</td>
      <td>tt0100275</td>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>tt9916538</td>
      <td>tt9916538</td>
      <td>Kuambil Lagi Hatiku</td>
      <td>Kuambil Lagi Hatiku</td>
      <td>2019</td>
      <td>123.0</td>
      <td>Drama</td>
    </tr>
    <tr>
      <td>tt9916622</td>
      <td>tt9916622</td>
      <td>Rodolpho Teóphilo - O Legado de um Pioneiro</td>
      <td>Rodolpho Teóphilo - O Legado de um Pioneiro</td>
      <td>2015</td>
      <td>NaN</td>
      <td>Documentary</td>
    </tr>
    <tr>
      <td>tt9916706</td>
      <td>tt9916706</td>
      <td>Dankyavar Danka</td>
      <td>Dankyavar Danka</td>
      <td>2013</td>
      <td>NaN</td>
      <td>Comedy</td>
    </tr>
    <tr>
      <td>tt9916730</td>
      <td>tt9916730</td>
      <td>6 Gunn</td>
      <td>6 Gunn</td>
      <td>2017</td>
      <td>116.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>tt9916754</td>
      <td>tt9916754</td>
      <td>Chico Albuquerque - Revelações</td>
      <td>Chico Albuquerque - Revelações</td>
      <td>2013</td>
      <td>NaN</td>
      <td>Documentary</td>
    </tr>
  </tbody>
</table>
<p>146144 rows × 6 columns</p>
</div>




```python
joineddd = pd.merge(d4, d3, on = 'tconst')
#merging two dataframes, still has a large amount of values and lets us compare year to averagerating
```


```python
joineddd
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
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>tt0063540</td>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.0</td>
      <td>77</td>
    </tr>
    <tr>
      <td>1</td>
      <td>tt0066787</td>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
      <td>7.2</td>
      <td>43</td>
    </tr>
    <tr>
      <td>2</td>
      <td>tt0069049</td>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
      <td>6.9</td>
      <td>4517</td>
    </tr>
    <tr>
      <td>3</td>
      <td>tt0069204</td>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
      <td>6.1</td>
      <td>13</td>
    </tr>
    <tr>
      <td>4</td>
      <td>tt0100275</td>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>6.5</td>
      <td>119</td>
    </tr>
    <tr>
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
      <td>73851</td>
      <td>tt9913084</td>
      <td>Diabolik sono io</td>
      <td>Diabolik sono io</td>
      <td>2019</td>
      <td>75.0</td>
      <td>Documentary</td>
      <td>6.2</td>
      <td>6</td>
    </tr>
    <tr>
      <td>73852</td>
      <td>tt9914286</td>
      <td>Sokagin Çocuklari</td>
      <td>Sokagin Çocuklari</td>
      <td>2019</td>
      <td>98.0</td>
      <td>Drama,Family</td>
      <td>8.7</td>
      <td>136</td>
    </tr>
    <tr>
      <td>73853</td>
      <td>tt9914642</td>
      <td>Albatross</td>
      <td>Albatross</td>
      <td>2017</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>8.5</td>
      <td>8</td>
    </tr>
    <tr>
      <td>73854</td>
      <td>tt9914942</td>
      <td>La vida sense la Sara Amat</td>
      <td>La vida sense la Sara Amat</td>
      <td>2019</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.6</td>
      <td>5</td>
    </tr>
    <tr>
      <td>73855</td>
      <td>tt9916160</td>
      <td>Drømmeland</td>
      <td>Drømmeland</td>
      <td>2019</td>
      <td>72.0</td>
      <td>Documentary</td>
      <td>6.5</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
<p>73856 rows × 8 columns</p>
</div>




```python
x = joineddd['start_year']
y = joineddd['averagerating']
```


```python
plt.scatter(x,y)
#not much correlation between time and rating
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-134-e3cfeb5b38a5> in <module>
    ----> 1 plt.scatter(x,y)
          2 #not much correlation between time and rating
    

    D:\asdf\jlkjkjconda\envs\learn-env\lib\site-packages\matplotlib\pyplot.py in scatter(x, y, s, c, marker, cmap, norm, vmin, vmax, alpha, linewidths, verts, edgecolors, plotnonfinite, data, **kwargs)
       2845         verts=verts, edgecolors=edgecolors,
       2846         plotnonfinite=plotnonfinite, **({"data": data} if data is not
    -> 2847         None else {}), **kwargs)
       2848     sci(__ret)
       2849     return __ret
    

    D:\asdf\jlkjkjconda\envs\learn-env\lib\site-packages\matplotlib\__init__.py in inner(ax, data, *args, **kwargs)
       1599     def inner(ax, *args, data=None, **kwargs):
       1600         if data is None:
    -> 1601             return func(ax, *map(sanitize_sequence, args), **kwargs)
       1602 
       1603         bound = new_sig.bind(ax, *args, **kwargs)
    

    D:\asdf\jlkjkjconda\envs\learn-env\lib\site-packages\matplotlib\axes\_axes.py in scatter(self, x, y, s, c, marker, cmap, norm, vmin, vmax, alpha, linewidths, verts, edgecolors, plotnonfinite, **kwargs)
       4442         y = np.ma.ravel(y)
       4443         if x.size != y.size:
    -> 4444             raise ValueError("x and y must be the same size")
       4445 
       4446         if s is None:
    

    ValueError: x and y must be the same size



![png](Cleaned%20Code%20with%20comments%20_files/Cleaned%20Code%20with%20comments%20_15_1.png)



```python
profit_df = pd.read_csv('zippedData\\bom.movie_gross.csv.gz')
profit_df
#setting up joining first dataframe with this one to compare values with profits
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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Alice in Wonderland (2010)</td>
      <td>BV</td>
      <td>334200000.0</td>
      <td>691300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Harry Potter and the Deathly Hallows Part 1</td>
      <td>WB</td>
      <td>296000000.0</td>
      <td>664300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Shrek Forever After</td>
      <td>P/DW</td>
      <td>238700000.0</td>
      <td>513900000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>3382</td>
      <td>The Quake</td>
      <td>Magn.</td>
      <td>6200.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>3383</td>
      <td>Edward II (2018 re-release)</td>
      <td>FM</td>
      <td>4800.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>3384</td>
      <td>El Pacto</td>
      <td>Sony</td>
      <td>2500.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>3385</td>
      <td>The Swan</td>
      <td>Synergetic</td>
      <td>2400.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>3386</td>
      <td>An Actor Prepares</td>
      <td>Grav.</td>
      <td>1700.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
<p>3387 rows × 5 columns</p>
</div>




```python

#joinedddd = pd.merge(joineddd, profit_df, on = 'title')
```


```python
profit_df.set_index('title')
#setting index to merge
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
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
    <tr>
      <th>title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>Alice in Wonderland (2010)</td>
      <td>BV</td>
      <td>334200000.0</td>
      <td>691300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>Harry Potter and the Deathly Hallows Part 1</td>
      <td>WB</td>
      <td>296000000.0</td>
      <td>664300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>Shrek Forever After</td>
      <td>P/DW</td>
      <td>238700000.0</td>
      <td>513900000</td>
      <td>2010</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>The Quake</td>
      <td>Magn.</td>
      <td>6200.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>Edward II (2018 re-release)</td>
      <td>FM</td>
      <td>4800.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>El Pacto</td>
      <td>Sony</td>
      <td>2500.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>The Swan</td>
      <td>Synergetic</td>
      <td>2400.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <td>An Actor Prepares</td>
      <td>Grav.</td>
      <td>1700.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
<p>3387 rows × 4 columns</p>
</div>




```python
joineddd.set_index('primary_title')
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
      <th>tconst</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
    <tr>
      <th>primary_title</th>
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
      <td>Sunghursh</td>
      <td>tt0063540</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.0</td>
      <td>77</td>
    </tr>
    <tr>
      <td>One Day Before the Rainy Season</td>
      <td>tt0066787</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
      <td>7.2</td>
      <td>43</td>
    </tr>
    <tr>
      <td>The Other Side of the Wind</td>
      <td>tt0069049</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
      <td>6.9</td>
      <td>4517</td>
    </tr>
    <tr>
      <td>Sabse Bada Sukh</td>
      <td>tt0069204</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
      <td>6.1</td>
      <td>13</td>
    </tr>
    <tr>
      <td>The Wandering Soap Opera</td>
      <td>tt0100275</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>6.5</td>
      <td>119</td>
    </tr>
    <tr>
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
      <td>Diabolik sono io</td>
      <td>tt9913084</td>
      <td>Diabolik sono io</td>
      <td>2019</td>
      <td>75.0</td>
      <td>Documentary</td>
      <td>6.2</td>
      <td>6</td>
    </tr>
    <tr>
      <td>Sokagin Çocuklari</td>
      <td>tt9914286</td>
      <td>Sokagin Çocuklari</td>
      <td>2019</td>
      <td>98.0</td>
      <td>Drama,Family</td>
      <td>8.7</td>
      <td>136</td>
    </tr>
    <tr>
      <td>Albatross</td>
      <td>tt9914642</td>
      <td>Albatross</td>
      <td>2017</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>8.5</td>
      <td>8</td>
    </tr>
    <tr>
      <td>La vida sense la Sara Amat</td>
      <td>tt9914942</td>
      <td>La vida sense la Sara Amat</td>
      <td>2019</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.6</td>
      <td>5</td>
    </tr>
    <tr>
      <td>Drømmeland</td>
      <td>tt9916160</td>
      <td>Drømmeland</td>
      <td>2019</td>
      <td>72.0</td>
      <td>Documentary</td>
      <td>6.5</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
<p>73856 rows × 7 columns</p>
</div>




```python
joineddd['title'] = joineddd['primary_title']
#making new column that matches column of other dataframe to merge
joineddd
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
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>tt0063540</td>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.0</td>
      <td>77</td>
      <td>Sunghursh</td>
    </tr>
    <tr>
      <td>1</td>
      <td>tt0066787</td>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
      <td>7.2</td>
      <td>43</td>
      <td>One Day Before the Rainy Season</td>
    </tr>
    <tr>
      <td>2</td>
      <td>tt0069049</td>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
      <td>6.9</td>
      <td>4517</td>
      <td>The Other Side of the Wind</td>
    </tr>
    <tr>
      <td>3</td>
      <td>tt0069204</td>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
      <td>6.1</td>
      <td>13</td>
      <td>Sabse Bada Sukh</td>
    </tr>
    <tr>
      <td>4</td>
      <td>tt0100275</td>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>6.5</td>
      <td>119</td>
      <td>The Wandering Soap Opera</td>
    </tr>
    <tr>
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
      <td>73851</td>
      <td>tt9913084</td>
      <td>Diabolik sono io</td>
      <td>Diabolik sono io</td>
      <td>2019</td>
      <td>75.0</td>
      <td>Documentary</td>
      <td>6.2</td>
      <td>6</td>
      <td>Diabolik sono io</td>
    </tr>
    <tr>
      <td>73852</td>
      <td>tt9914286</td>
      <td>Sokagin Çocuklari</td>
      <td>Sokagin Çocuklari</td>
      <td>2019</td>
      <td>98.0</td>
      <td>Drama,Family</td>
      <td>8.7</td>
      <td>136</td>
      <td>Sokagin Çocuklari</td>
    </tr>
    <tr>
      <td>73853</td>
      <td>tt9914642</td>
      <td>Albatross</td>
      <td>Albatross</td>
      <td>2017</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>8.5</td>
      <td>8</td>
      <td>Albatross</td>
    </tr>
    <tr>
      <td>73854</td>
      <td>tt9914942</td>
      <td>La vida sense la Sara Amat</td>
      <td>La vida sense la Sara Amat</td>
      <td>2019</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.6</td>
      <td>5</td>
      <td>La vida sense la Sara Amat</td>
    </tr>
    <tr>
      <td>73855</td>
      <td>tt9916160</td>
      <td>Drømmeland</td>
      <td>Drømmeland</td>
      <td>2019</td>
      <td>72.0</td>
      <td>Documentary</td>
      <td>6.5</td>
      <td>11</td>
      <td>Drømmeland</td>
    </tr>
  </tbody>
</table>
<p>73856 rows × 9 columns</p>
</div>




```python
joinedddd = pd.merge(profit_df, joineddd, on='title')
joinedddd
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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
      <td>tt0435761</td>
      <td>Toy Story 3</td>
      <td>Toy Story 3</td>
      <td>2010</td>
      <td>103.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>8.3</td>
      <td>682218</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
      <td>tt1375666</td>
      <td>Inception</td>
      <td>Inception</td>
      <td>2010</td>
      <td>148.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>8.8</td>
      <td>1841066</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Shrek Forever After</td>
      <td>P/DW</td>
      <td>238700000.0</td>
      <td>513900000</td>
      <td>2010</td>
      <td>tt0892791</td>
      <td>Shrek Forever After</td>
      <td>Shrek Forever After</td>
      <td>2010</td>
      <td>93.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>6.3</td>
      <td>167532</td>
    </tr>
    <tr>
      <td>3</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>Sum.</td>
      <td>300500000.0</td>
      <td>398000000</td>
      <td>2010</td>
      <td>tt1325004</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>2010</td>
      <td>124.0</td>
      <td>Adventure,Drama,Fantasy</td>
      <td>5.0</td>
      <td>211733</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Iron Man 2</td>
      <td>Par.</td>
      <td>312400000.0</td>
      <td>311500000</td>
      <td>2010</td>
      <td>tt1228705</td>
      <td>Iron Man 2</td>
      <td>Iron Man 2</td>
      <td>2010</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>657690</td>
    </tr>
    <tr>
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
      <td>3022</td>
      <td>Souvenir</td>
      <td>Strand</td>
      <td>11400.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt2387692</td>
      <td>Souvenir</td>
      <td>Souvenir</td>
      <td>2016</td>
      <td>90.0</td>
      <td>Drama,Music,Romance</td>
      <td>6.0</td>
      <td>823</td>
    </tr>
    <tr>
      <td>3023</td>
      <td>Souvenir</td>
      <td>Strand</td>
      <td>11400.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt2389092</td>
      <td>Souvenir</td>
      <td>Souvenir</td>
      <td>2014</td>
      <td>86.0</td>
      <td>Comedy,Romance</td>
      <td>5.9</td>
      <td>9</td>
    </tr>
    <tr>
      <td>3024</td>
      <td>Beauty and the Dogs</td>
      <td>Osci.</td>
      <td>8900.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt6776572</td>
      <td>Beauty and the Dogs</td>
      <td>Aala Kaf Ifrit</td>
      <td>2017</td>
      <td>100.0</td>
      <td>Crime,Drama,Thriller</td>
      <td>7.0</td>
      <td>1016</td>
    </tr>
    <tr>
      <td>3025</td>
      <td>The Quake</td>
      <td>Magn.</td>
      <td>6200.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt6523720</td>
      <td>The Quake</td>
      <td>Skjelvet</td>
      <td>2018</td>
      <td>106.0</td>
      <td>Action,Drama,Thriller</td>
      <td>6.2</td>
      <td>5270</td>
    </tr>
    <tr>
      <td>3026</td>
      <td>An Actor Prepares</td>
      <td>Grav.</td>
      <td>1700.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt5718046</td>
      <td>An Actor Prepares</td>
      <td>An Actor Prepares</td>
      <td>2018</td>
      <td>97.0</td>
      <td>Comedy</td>
      <td>5.0</td>
      <td>388</td>
    </tr>
  </tbody>
</table>
<p>3027 rows × 13 columns</p>
</div>




```python
joinedddd['studio'] = joinedddd['studio'].fillna(value='No_studio')
#filling in NaN in studio column to correctly filter the results
```


```python
joinedddd.sort_values(by='domestic_gross', ascending=False)[:50]
#choosing only the top 50 rows of domestic gross
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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2752</td>
      <td>Black Panther</td>
      <td>BV</td>
      <td>700100000.0</td>
      <td>646900000</td>
      <td>2018</td>
      <td>tt1825683</td>
      <td>Black Panther</td>
      <td>Black Panther</td>
      <td>2018</td>
      <td>134.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.3</td>
      <td>516148</td>
    </tr>
    <tr>
      <td>2751</td>
      <td>Avengers: Infinity War</td>
      <td>BV</td>
      <td>678800000.0</td>
      <td>1,369.5</td>
      <td>2018</td>
      <td>tt4154756</td>
      <td>Avengers: Infinity War</td>
      <td>Avengers: Infinity War</td>
      <td>2018</td>
      <td>149.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>8.5</td>
      <td>670926</td>
    </tr>
    <tr>
      <td>1616</td>
      <td>Jurassic World</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
      <td>2015</td>
      <td>tt0369610</td>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>539338</td>
    </tr>
    <tr>
      <td>2435</td>
      <td>Star Wars: The Last Jedi</td>
      <td>BV</td>
      <td>620200000.0</td>
      <td>712400000</td>
      <td>2017</td>
      <td>tt2527336</td>
      <td>Star Wars: The Last Jedi</td>
      <td>Star Wars: Episode VIII - The Last Jedi</td>
      <td>2017</td>
      <td>152.0</td>
      <td>Action,Adventure,Fantasy</td>
      <td>7.1</td>
      <td>462903</td>
    </tr>
    <tr>
      <td>2754</td>
      <td>Incredibles 2</td>
      <td>BV</td>
      <td>608600000.0</td>
      <td>634200000</td>
      <td>2018</td>
      <td>tt3606756</td>
      <td>Incredibles 2</td>
      <td>Incredibles 2</td>
      <td>2018</td>
      <td>118.0</td>
      <td>Action,Adventure,Animation</td>
      <td>7.7</td>
      <td>203510</td>
    </tr>
    <tr>
      <td>2031</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>BV</td>
      <td>532200000.0</td>
      <td>523900000</td>
      <td>2016</td>
      <td>tt3748528</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>Rogue One</td>
      <td>2016</td>
      <td>133.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.8</td>
      <td>478592</td>
    </tr>
    <tr>
      <td>2032</td>
      <td>Finding Dory</td>
      <td>BV</td>
      <td>486300000.0</td>
      <td>542300000</td>
      <td>2016</td>
      <td>tt2277860</td>
      <td>Finding Dory</td>
      <td>Finding Dory</td>
      <td>2016</td>
      <td>97.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>7.3</td>
      <td>213542</td>
    </tr>
    <tr>
      <td>1618</td>
      <td>Avengers: Age of Ultron</td>
      <td>BV</td>
      <td>459000000.0</td>
      <td>946400000</td>
      <td>2015</td>
      <td>tt2395427</td>
      <td>Avengers: Age of Ultron</td>
      <td>Avengers: Age of Ultron</td>
      <td>2015</td>
      <td>141.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.3</td>
      <td>665594</td>
    </tr>
    <tr>
      <td>598</td>
      <td>The Dark Knight Rises</td>
      <td>WB</td>
      <td>448100000.0</td>
      <td>636800000</td>
      <td>2012</td>
      <td>tt1345836</td>
      <td>The Dark Knight Rises</td>
      <td>The Dark Knight Rises</td>
      <td>2012</td>
      <td>164.0</td>
      <td>Action,Thriller</td>
      <td>8.4</td>
      <td>1387769</td>
    </tr>
    <tr>
      <td>958</td>
      <td>The Hunger Games: Catching Fire</td>
      <td>LGF</td>
      <td>424700000.0</td>
      <td>440300000</td>
      <td>2013</td>
      <td>tt1951264</td>
      <td>The Hunger Games: Catching Fire</td>
      <td>The Hunger Games: Catching Fire</td>
      <td>2013</td>
      <td>146.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.5</td>
      <td>575455</td>
    </tr>
    <tr>
      <td>2753</td>
      <td>Jurassic World: Fallen Kingdom</td>
      <td>Uni.</td>
      <td>417700000.0</td>
      <td>891800000</td>
      <td>2018</td>
      <td>tt4881806</td>
      <td>Jurassic World: Fallen Kingdom</td>
      <td>Jurassic World: Fallen Kingdom</td>
      <td>2018</td>
      <td>128.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>6.2</td>
      <td>219125</td>
    </tr>
    <tr>
      <td>0</td>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
      <td>tt0435761</td>
      <td>Toy Story 3</td>
      <td>Toy Story 3</td>
      <td>2010</td>
      <td>103.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>8.3</td>
      <td>682218</td>
    </tr>
    <tr>
      <td>2445</td>
      <td>Wonder Woman</td>
      <td>WB</td>
      <td>412600000.0</td>
      <td>409300000</td>
      <td>2017</td>
      <td>tt4283448</td>
      <td>Wonder Woman</td>
      <td>Wonder Woman</td>
      <td>2016</td>
      <td>75.0</td>
      <td>Documentary,Drama,Sport</td>
      <td>6.9</td>
      <td>13</td>
    </tr>
    <tr>
      <td>2444</td>
      <td>Wonder Woman</td>
      <td>WB</td>
      <td>412600000.0</td>
      <td>409300000</td>
      <td>2017</td>
      <td>tt4028068</td>
      <td>Wonder Woman</td>
      <td>Wonder Woman</td>
      <td>2014</td>
      <td>60.0</td>
      <td>Sci-Fi</td>
      <td>4.2</td>
      <td>20</td>
    </tr>
    <tr>
      <td>2443</td>
      <td>Wonder Woman</td>
      <td>WB</td>
      <td>412600000.0</td>
      <td>409300000</td>
      <td>2017</td>
      <td>tt0451279</td>
      <td>Wonder Woman</td>
      <td>Wonder Woman</td>
      <td>2017</td>
      <td>141.0</td>
      <td>Action,Adventure,Fantasy</td>
      <td>7.5</td>
      <td>487527</td>
    </tr>
    <tr>
      <td>955</td>
      <td>Iron Man 3</td>
      <td>BV</td>
      <td>409000000.0</td>
      <td>805800000</td>
      <td>2013</td>
      <td>tt1300854</td>
      <td>Iron Man 3</td>
      <td>Iron Man Three</td>
      <td>2013</td>
      <td>130.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.2</td>
      <td>692794</td>
    </tr>
    <tr>
      <td>2030</td>
      <td>Captain America: Civil War</td>
      <td>BV</td>
      <td>408100000.0</td>
      <td>745200000</td>
      <td>2016</td>
      <td>tt3498820</td>
      <td>Captain America: Civil War</td>
      <td>Captain America: Civil War</td>
      <td>2016</td>
      <td>147.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.8</td>
      <td>583507</td>
    </tr>
    <tr>
      <td>603</td>
      <td>The Hunger Games</td>
      <td>LGF</td>
      <td>408000000.0</td>
      <td>286400000</td>
      <td>2012</td>
      <td>tt1392170</td>
      <td>The Hunger Games</td>
      <td>The Hunger Games</td>
      <td>2012</td>
      <td>142.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.2</td>
      <td>795227</td>
    </tr>
    <tr>
      <td>2438</td>
      <td>Jumanji: Welcome to the Jungle</td>
      <td>Sony</td>
      <td>404500000.0</td>
      <td>557600000</td>
      <td>2017</td>
      <td>tt2283362</td>
      <td>Jumanji: Welcome to the Jungle</td>
      <td>Jumanji: Welcome to the Jungle</td>
      <td>2017</td>
      <td>119.0</td>
      <td>Action,Adventure,Comedy</td>
      <td>7.0</td>
      <td>242735</td>
    </tr>
    <tr>
      <td>952</td>
      <td>Frozen</td>
      <td>BV</td>
      <td>400700000.0</td>
      <td>875700000</td>
      <td>2013</td>
      <td>tt1323045</td>
      <td>Frozen</td>
      <td>Frozen</td>
      <td>2010</td>
      <td>93.0</td>
      <td>Adventure,Drama,Sport</td>
      <td>6.2</td>
      <td>62311</td>
    </tr>
    <tr>
      <td>954</td>
      <td>Frozen</td>
      <td>BV</td>
      <td>400700000.0</td>
      <td>875700000</td>
      <td>2013</td>
      <td>tt2294629</td>
      <td>Frozen</td>
      <td>Frozen</td>
      <td>2013</td>
      <td>102.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>7.5</td>
      <td>516998</td>
    </tr>
    <tr>
      <td>953</td>
      <td>Frozen</td>
      <td>BV</td>
      <td>400700000.0</td>
      <td>875700000</td>
      <td>2013</td>
      <td>tt1611845</td>
      <td>Frozen</td>
      <td>Wai nei chung ching</td>
      <td>2010</td>
      <td>92.0</td>
      <td>Fantasy,Romance</td>
      <td>5.4</td>
      <td>75</td>
    </tr>
    <tr>
      <td>2441</td>
      <td>Guardians of the Galaxy Vol. 2</td>
      <td>BV</td>
      <td>389800000.0</td>
      <td>473900000</td>
      <td>2017</td>
      <td>tt3896198</td>
      <td>Guardians of the Galaxy Vol. 2</td>
      <td>Guardians of the Galaxy Vol. 2</td>
      <td>2017</td>
      <td>136.0</td>
      <td>Action,Adventure,Comedy</td>
      <td>7.7</td>
      <td>482917</td>
    </tr>
    <tr>
      <td>2034</td>
      <td>The Secret Life of Pets</td>
      <td>Uni.</td>
      <td>368400000.0</td>
      <td>507100000</td>
      <td>2016</td>
      <td>tt2709768</td>
      <td>The Secret Life of Pets</td>
      <td>The Secret Life of Pets</td>
      <td>2016</td>
      <td>87.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>6.5</td>
      <td>161997</td>
    </tr>
    <tr>
      <td>956</td>
      <td>Despicable Me 2</td>
      <td>Uni.</td>
      <td>368100000.0</td>
      <td>602700000</td>
      <td>2013</td>
      <td>tt1690953</td>
      <td>Despicable Me 2</td>
      <td>Despicable Me 2</td>
      <td>2013</td>
      <td>98.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>7.4</td>
      <td>344230</td>
    </tr>
    <tr>
      <td>2036</td>
      <td>Deadpool</td>
      <td>Fox</td>
      <td>363100000.0</td>
      <td>420000000</td>
      <td>2016</td>
      <td>tt1431045</td>
      <td>Deadpool</td>
      <td>Deadpool</td>
      <td>2016</td>
      <td>108.0</td>
      <td>Action,Adventure,Comedy</td>
      <td>8.0</td>
      <td>820847</td>
    </tr>
    <tr>
      <td>1621</td>
      <td>Inside Out</td>
      <td>BV</td>
      <td>356500000.0</td>
      <td>501100000</td>
      <td>2015</td>
      <td>tt1640486</td>
      <td>Inside Out</td>
      <td>Inside Out</td>
      <td>2011</td>
      <td>93.0</td>
      <td>Crime,Drama</td>
      <td>4.6</td>
      <td>1566</td>
    </tr>
    <tr>
      <td>1622</td>
      <td>Inside Out</td>
      <td>BV</td>
      <td>356500000.0</td>
      <td>501100000</td>
      <td>2015</td>
      <td>tt2071483</td>
      <td>Inside Out</td>
      <td>Inside Out</td>
      <td>2011</td>
      <td>59.0</td>
      <td>Family</td>
      <td>7.3</td>
      <td>15</td>
    </tr>
    <tr>
      <td>1623</td>
      <td>Inside Out</td>
      <td>BV</td>
      <td>356500000.0</td>
      <td>501100000</td>
      <td>2015</td>
      <td>tt2096673</td>
      <td>Inside Out</td>
      <td>Inside Out</td>
      <td>2015</td>
      <td>95.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>8.2</td>
      <td>536181</td>
    </tr>
    <tr>
      <td>1624</td>
      <td>Inside Out</td>
      <td>BV</td>
      <td>356500000.0</td>
      <td>501100000</td>
      <td>2015</td>
      <td>tt2608638</td>
      <td>Inside Out</td>
      <td>Inside Out</td>
      <td>2013</td>
      <td>75.0</td>
      <td>Biography,Documentary,History</td>
      <td>7.5</td>
      <td>60</td>
    </tr>
    <tr>
      <td>1617</td>
      <td>Furious 7</td>
      <td>Uni.</td>
      <td>353000000.0</td>
      <td>1,163.0</td>
      <td>2015</td>
      <td>tt2820852</td>
      <td>Furious 7</td>
      <td>Furious Seven</td>
      <td>2015</td>
      <td>137.0</td>
      <td>Action,Crime,Thriller</td>
      <td>7.2</td>
      <td>335074</td>
    </tr>
    <tr>
      <td>240</td>
      <td>Transformers: Dark of the Moon</td>
      <td>P/DW</td>
      <td>352400000.0</td>
      <td>771400000</td>
      <td>2011</td>
      <td>tt1399103</td>
      <td>Transformers: Dark of the Moon</td>
      <td>Transformers: Dark of the Moon</td>
      <td>2011</td>
      <td>154.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>6.2</td>
      <td>366409</td>
    </tr>
    <tr>
      <td>1278</td>
      <td>American Sniper</td>
      <td>WB</td>
      <td>350100000.0</td>
      <td>197300000</td>
      <td>2014</td>
      <td>tt2179136</td>
      <td>American Sniper</td>
      <td>American Sniper</td>
      <td>2014</td>
      <td>133.0</td>
      <td>Action,Biography,Drama</td>
      <td>7.3</td>
      <td>401915</td>
    </tr>
    <tr>
      <td>2033</td>
      <td>Zootopia</td>
      <td>BV</td>
      <td>341300000.0</td>
      <td>682500000</td>
      <td>2016</td>
      <td>tt2948356</td>
      <td>Zootopia</td>
      <td>Zootopia</td>
      <td>2016</td>
      <td>108.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>8.0</td>
      <td>383446</td>
    </tr>
    <tr>
      <td>1270</td>
      <td>The Hunger Games: Mockingjay - Part 1</td>
      <td>LGF</td>
      <td>337100000.0</td>
      <td>418200000</td>
      <td>2014</td>
      <td>tt1951265</td>
      <td>The Hunger Games: Mockingjay - Part 1</td>
      <td>The Hunger Games: Mockingjay - Part 1</td>
      <td>2014</td>
      <td>123.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>6.6</td>
      <td>379050</td>
    </tr>
    <tr>
      <td>1619</td>
      <td>Minions</td>
      <td>Uni.</td>
      <td>336000000.0</td>
      <td>823400000</td>
      <td>2015</td>
      <td>tt2293640</td>
      <td>Minions</td>
      <td>Minions</td>
      <td>2015</td>
      <td>91.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>6.4</td>
      <td>193917</td>
    </tr>
    <tr>
      <td>2755</td>
      <td>Aquaman</td>
      <td>WB</td>
      <td>335100000.0</td>
      <td>812700000</td>
      <td>2018</td>
      <td>tt1477834</td>
      <td>Aquaman</td>
      <td>Aquaman</td>
      <td>2018</td>
      <td>143.0</td>
      <td>Action,Adventure,Fantasy</td>
      <td>7.1</td>
      <td>263328</td>
    </tr>
    <tr>
      <td>2439</td>
      <td>Spider-Man: Homecoming</td>
      <td>Sony</td>
      <td>334200000.0</td>
      <td>546000000</td>
      <td>2017</td>
      <td>tt2250912</td>
      <td>Spider-Man: Homecoming</td>
      <td>Spider-Man: Homecoming</td>
      <td>2017</td>
      <td>133.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.5</td>
      <td>426302</td>
    </tr>
    <tr>
      <td>1268</td>
      <td>Guardians of the Galaxy</td>
      <td>BV</td>
      <td>333200000.0</td>
      <td>440200000</td>
      <td>2014</td>
      <td>tt2015381</td>
      <td>Guardians of the Galaxy</td>
      <td>Guardians of the Galaxy</td>
      <td>2014</td>
      <td>121.0</td>
      <td>Action,Adventure,Comedy</td>
      <td>8.1</td>
      <td>948394</td>
    </tr>
    <tr>
      <td>2035</td>
      <td>Batman v Superman: Dawn of Justice</td>
      <td>WB</td>
      <td>330400000.0</td>
      <td>543300000</td>
      <td>2016</td>
      <td>tt2975590</td>
      <td>Batman v Superman: Dawn of Justice</td>
      <td>Batman v Superman: Dawn of Justice</td>
      <td>2016</td>
      <td>151.0</td>
      <td>Action,Adventure,Fantasy</td>
      <td>6.5</td>
      <td>576909</td>
    </tr>
    <tr>
      <td>2449</td>
      <td>It</td>
      <td>WB (NL)</td>
      <td>327500000.0</td>
      <td>372900000</td>
      <td>2017</td>
      <td>tt1396484</td>
      <td>It</td>
      <td>It</td>
      <td>2017</td>
      <td>135.0</td>
      <td>Horror,Thriller</td>
      <td>7.4</td>
      <td>359123</td>
    </tr>
    <tr>
      <td>2037</td>
      <td>Suicide Squad</td>
      <td>WB</td>
      <td>325100000.0</td>
      <td>421700000</td>
      <td>2016</td>
      <td>tt1386697</td>
      <td>Suicide Squad</td>
      <td>Suicide Squad</td>
      <td>2016</td>
      <td>123.0</td>
      <td>Action,Adventure,Fantasy</td>
      <td>6.0</td>
      <td>533039</td>
    </tr>
    <tr>
      <td>2758</td>
      <td>Deadpool 2</td>
      <td>Fox</td>
      <td>318500000.0</td>
      <td>460500000</td>
      <td>2018</td>
      <td>tt5463162</td>
      <td>Deadpool 2</td>
      <td>Deadpool 2</td>
      <td>2018</td>
      <td>119.0</td>
      <td>Action,Adventure,Comedy</td>
      <td>7.8</td>
      <td>391735</td>
    </tr>
    <tr>
      <td>2442</td>
      <td>Thor: Ragnarok</td>
      <td>BV</td>
      <td>315100000.0</td>
      <td>538900000</td>
      <td>2017</td>
      <td>tt3501632</td>
      <td>Thor: Ragnarok</td>
      <td>Thor: Ragnarok</td>
      <td>2017</td>
      <td>130.0</td>
      <td>Action,Adventure,Comedy</td>
      <td>7.9</td>
      <td>482995</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Iron Man 2</td>
      <td>Par.</td>
      <td>312400000.0</td>
      <td>311500000</td>
      <td>2010</td>
      <td>tt1228705</td>
      <td>Iron Man 2</td>
      <td>Iron Man 2</td>
      <td>2010</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>657690</td>
    </tr>
    <tr>
      <td>597</td>
      <td>Skyfall</td>
      <td>Sony</td>
      <td>304400000.0</td>
      <td>804200000</td>
      <td>2012</td>
      <td>tt1074638</td>
      <td>Skyfall</td>
      <td>Skyfall</td>
      <td>2012</td>
      <td>143.0</td>
      <td>Action,Adventure,Thriller</td>
      <td>7.8</td>
      <td>592221</td>
    </tr>
    <tr>
      <td>599</td>
      <td>The Hobbit: An Unexpected Journey</td>
      <td>WB (NL)</td>
      <td>303000000.0</td>
      <td>718100000</td>
      <td>2012</td>
      <td>tt0903624</td>
      <td>The Hobbit: An Unexpected Journey</td>
      <td>The Hobbit: An Unexpected Journey</td>
      <td>2012</td>
      <td>169.0</td>
      <td>Adventure,Family,Fantasy</td>
      <td>7.9</td>
      <td>719629</td>
    </tr>
    <tr>
      <td>3</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>Sum.</td>
      <td>300500000.0</td>
      <td>398000000</td>
      <td>2010</td>
      <td>tt1325004</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>2010</td>
      <td>124.0</td>
      <td>Adventure,Drama,Fantasy</td>
      <td>5.0</td>
      <td>211733</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
      <td>tt1375666</td>
      <td>Inception</td>
      <td>Inception</td>
      <td>2010</td>
      <td>148.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>8.8</td>
      <td>1841066</td>
    </tr>
    <tr>
      <td>962</td>
      <td>Man of Steel</td>
      <td>WB</td>
      <td>291000000.0</td>
      <td>377000000</td>
      <td>2013</td>
      <td>tt0770828</td>
      <td>Man of Steel</td>
      <td>Man of Steel</td>
      <td>2013</td>
      <td>143.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.1</td>
      <td>647288</td>
    </tr>
  </tbody>
</table>
</div>




```python
budget_df = pd.read_csv('zippedData\\tn.movie_budgets.csv.gz')
budget_df
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
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross</th>
      <th>worldwide_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>Dec 18, 2009</td>
      <td>Avatar</td>
      <td>$425,000,000</td>
      <td>$760,507,625</td>
      <td>$2,776,345,279</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>May 20, 2011</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>$410,600,000</td>
      <td>$241,063,875</td>
      <td>$1,045,663,875</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>Jun 7, 2019</td>
      <td>Dark Phoenix</td>
      <td>$350,000,000</td>
      <td>$42,762,350</td>
      <td>$149,762,350</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>May 1, 2015</td>
      <td>Avengers: Age of Ultron</td>
      <td>$330,600,000</td>
      <td>$459,005,868</td>
      <td>$1,403,013,963</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
      <td>Dec 15, 2017</td>
      <td>Star Wars Ep. VIII: The Last Jedi</td>
      <td>$317,000,000</td>
      <td>$620,181,382</td>
      <td>$1,316,721,747</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>5777</td>
      <td>78</td>
      <td>Dec 31, 2018</td>
      <td>Red 11</td>
      <td>$7,000</td>
      <td>$0</td>
      <td>$0</td>
    </tr>
    <tr>
      <td>5778</td>
      <td>79</td>
      <td>Apr 2, 1999</td>
      <td>Following</td>
      <td>$6,000</td>
      <td>$48,482</td>
      <td>$240,495</td>
    </tr>
    <tr>
      <td>5779</td>
      <td>80</td>
      <td>Jul 13, 2005</td>
      <td>Return to the Land of Wonders</td>
      <td>$5,000</td>
      <td>$1,338</td>
      <td>$1,338</td>
    </tr>
    <tr>
      <td>5780</td>
      <td>81</td>
      <td>Sep 29, 2015</td>
      <td>A Plague So Pleasant</td>
      <td>$1,400</td>
      <td>$0</td>
      <td>$0</td>
    </tr>
    <tr>
      <td>5781</td>
      <td>82</td>
      <td>Aug 5, 2005</td>
      <td>My Date With Drew</td>
      <td>$1,100</td>
      <td>$181,041</td>
      <td>$181,041</td>
    </tr>
  </tbody>
</table>
<p>5782 rows × 6 columns</p>
</div>




```python
budget_df['title'] = budget_df['movie']
budget_df
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
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross</th>
      <th>worldwide_gross</th>
      <th>title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>Dec 18, 2009</td>
      <td>Avatar</td>
      <td>$425,000,000</td>
      <td>$760,507,625</td>
      <td>$2,776,345,279</td>
      <td>Avatar</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>May 20, 2011</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>$410,600,000</td>
      <td>$241,063,875</td>
      <td>$1,045,663,875</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>Jun 7, 2019</td>
      <td>Dark Phoenix</td>
      <td>$350,000,000</td>
      <td>$42,762,350</td>
      <td>$149,762,350</td>
      <td>Dark Phoenix</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>May 1, 2015</td>
      <td>Avengers: Age of Ultron</td>
      <td>$330,600,000</td>
      <td>$459,005,868</td>
      <td>$1,403,013,963</td>
      <td>Avengers: Age of Ultron</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
      <td>Dec 15, 2017</td>
      <td>Star Wars Ep. VIII: The Last Jedi</td>
      <td>$317,000,000</td>
      <td>$620,181,382</td>
      <td>$1,316,721,747</td>
      <td>Star Wars Ep. VIII: The Last Jedi</td>
    </tr>
    <tr>
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
      <td>5777</td>
      <td>78</td>
      <td>Dec 31, 2018</td>
      <td>Red 11</td>
      <td>$7,000</td>
      <td>$0</td>
      <td>$0</td>
      <td>Red 11</td>
    </tr>
    <tr>
      <td>5778</td>
      <td>79</td>
      <td>Apr 2, 1999</td>
      <td>Following</td>
      <td>$6,000</td>
      <td>$48,482</td>
      <td>$240,495</td>
      <td>Following</td>
    </tr>
    <tr>
      <td>5779</td>
      <td>80</td>
      <td>Jul 13, 2005</td>
      <td>Return to the Land of Wonders</td>
      <td>$5,000</td>
      <td>$1,338</td>
      <td>$1,338</td>
      <td>Return to the Land of Wonders</td>
    </tr>
    <tr>
      <td>5780</td>
      <td>81</td>
      <td>Sep 29, 2015</td>
      <td>A Plague So Pleasant</td>
      <td>$1,400</td>
      <td>$0</td>
      <td>$0</td>
      <td>A Plague So Pleasant</td>
    </tr>
    <tr>
      <td>5781</td>
      <td>82</td>
      <td>Aug 5, 2005</td>
      <td>My Date With Drew</td>
      <td>$1,100</td>
      <td>$181,041</td>
      <td>$181,041</td>
      <td>My Date With Drew</td>
    </tr>
  </tbody>
</table>
<p>5782 rows × 7 columns</p>
</div>




```python
joined_2 = pd.merge(joinedddd, budget_df, on = 'title')
joined_2
#another merge to obtain profit values 
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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross_x</th>
      <th>foreign_gross</th>
      <th>year</th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross_y</th>
      <th>worldwide_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
      <td>tt0435761</td>
      <td>Toy Story 3</td>
      <td>Toy Story 3</td>
      <td>2010</td>
      <td>103.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>8.3</td>
      <td>682218</td>
      <td>47</td>
      <td>Jun 18, 2010</td>
      <td>Toy Story 3</td>
      <td>$200,000,000</td>
      <td>$415,004,880</td>
      <td>$1,068,879,522</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
      <td>tt1375666</td>
      <td>Inception</td>
      <td>Inception</td>
      <td>2010</td>
      <td>148.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>8.8</td>
      <td>1841066</td>
      <td>38</td>
      <td>Jul 16, 2010</td>
      <td>Inception</td>
      <td>$160,000,000</td>
      <td>$292,576,195</td>
      <td>$835,524,642</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Shrek Forever After</td>
      <td>P/DW</td>
      <td>238700000.0</td>
      <td>513900000</td>
      <td>2010</td>
      <td>tt0892791</td>
      <td>Shrek Forever After</td>
      <td>Shrek Forever After</td>
      <td>2010</td>
      <td>93.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>6.3</td>
      <td>167532</td>
      <td>27</td>
      <td>May 21, 2010</td>
      <td>Shrek Forever After</td>
      <td>$165,000,000</td>
      <td>$238,736,787</td>
      <td>$756,244,673</td>
    </tr>
    <tr>
      <td>3</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>Sum.</td>
      <td>300500000.0</td>
      <td>398000000</td>
      <td>2010</td>
      <td>tt1325004</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>2010</td>
      <td>124.0</td>
      <td>Adventure,Drama,Fantasy</td>
      <td>5.0</td>
      <td>211733</td>
      <td>53</td>
      <td>Jun 30, 2010</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>$68,000,000</td>
      <td>$300,531,751</td>
      <td>$706,102,828</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Iron Man 2</td>
      <td>Par.</td>
      <td>312400000.0</td>
      <td>311500000</td>
      <td>2010</td>
      <td>tt1228705</td>
      <td>Iron Man 2</td>
      <td>Iron Man 2</td>
      <td>2010</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>657690</td>
      <td>15</td>
      <td>May 7, 2010</td>
      <td>Iron Man 2</td>
      <td>$170,000,000</td>
      <td>$312,433,331</td>
      <td>$621,156,389</td>
    </tr>
    <tr>
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
      <td>1408</td>
      <td>Gotti</td>
      <td>VE</td>
      <td>4300000.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt1801552</td>
      <td>Gotti</td>
      <td>Gotti</td>
      <td>2018</td>
      <td>112.0</td>
      <td>Biography,Crime,Drama</td>
      <td>4.8</td>
      <td>10358</td>
      <td>64</td>
      <td>Jun 15, 2018</td>
      <td>Gotti</td>
      <td>$10,000,000</td>
      <td>$4,286,367</td>
      <td>$6,089,100</td>
    </tr>
    <tr>
      <td>1409</td>
      <td>Bilal: A New Breed of Hero</td>
      <td>VE</td>
      <td>491000.0</td>
      <td>1700000</td>
      <td>2018</td>
      <td>tt3576728</td>
      <td>Bilal: A New Breed of Hero</td>
      <td>Bilal: A New Breed of Hero</td>
      <td>2015</td>
      <td>105.0</td>
      <td>Action,Adventure,Animation</td>
      <td>8.0</td>
      <td>16854</td>
      <td>100</td>
      <td>Feb 2, 2018</td>
      <td>Bilal: A New Breed of Hero</td>
      <td>$30,000,000</td>
      <td>$490,973</td>
      <td>$648,599</td>
    </tr>
    <tr>
      <td>1410</td>
      <td>Mandy</td>
      <td>RLJ</td>
      <td>1200000.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt4995858</td>
      <td>Mandy</td>
      <td>Mandy</td>
      <td>2016</td>
      <td>113.0</td>
      <td>Drama,Thriller</td>
      <td>4.1</td>
      <td>39</td>
      <td>71</td>
      <td>Sep 14, 2018</td>
      <td>Mandy</td>
      <td>$6,000,000</td>
      <td>$1,214,525</td>
      <td>$1,427,656</td>
    </tr>
    <tr>
      <td>1411</td>
      <td>Mandy</td>
      <td>RLJ</td>
      <td>1200000.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt6998518</td>
      <td>Mandy</td>
      <td>Mandy</td>
      <td>2018</td>
      <td>121.0</td>
      <td>Action,Fantasy,Horror</td>
      <td>6.6</td>
      <td>44378</td>
      <td>71</td>
      <td>Sep 14, 2018</td>
      <td>Mandy</td>
      <td>$6,000,000</td>
      <td>$1,214,525</td>
      <td>$1,427,656</td>
    </tr>
    <tr>
      <td>1412</td>
      <td>Lean on Pete</td>
      <td>A24</td>
      <td>1200000.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt5340300</td>
      <td>Lean on Pete</td>
      <td>Lean on Pete</td>
      <td>2017</td>
      <td>121.0</td>
      <td>Adventure,Drama</td>
      <td>7.2</td>
      <td>8607</td>
      <td>13</td>
      <td>Apr 6, 2018</td>
      <td>Lean on Pete</td>
      <td>$8,000,000</td>
      <td>$1,163,056</td>
      <td>$2,455,027</td>
    </tr>
  </tbody>
</table>
<p>1413 rows × 19 columns</p>
</div>




```python
joined_2['month'] = joined_2['release_date'].str[:3]
joined_2
#setting up a column for only the month
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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross_x</th>
      <th>foreign_gross</th>
      <th>year</th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross_y</th>
      <th>worldwide_gross</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
      <td>tt0435761</td>
      <td>Toy Story 3</td>
      <td>Toy Story 3</td>
      <td>2010</td>
      <td>103.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>8.3</td>
      <td>682218</td>
      <td>47</td>
      <td>Jun 18, 2010</td>
      <td>Toy Story 3</td>
      <td>$200,000,000</td>
      <td>$415,004,880</td>
      <td>$1,068,879,522</td>
      <td>Jun</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
      <td>tt1375666</td>
      <td>Inception</td>
      <td>Inception</td>
      <td>2010</td>
      <td>148.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>8.8</td>
      <td>1841066</td>
      <td>38</td>
      <td>Jul 16, 2010</td>
      <td>Inception</td>
      <td>$160,000,000</td>
      <td>$292,576,195</td>
      <td>$835,524,642</td>
      <td>Jul</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Shrek Forever After</td>
      <td>P/DW</td>
      <td>238700000.0</td>
      <td>513900000</td>
      <td>2010</td>
      <td>tt0892791</td>
      <td>Shrek Forever After</td>
      <td>Shrek Forever After</td>
      <td>2010</td>
      <td>93.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>6.3</td>
      <td>167532</td>
      <td>27</td>
      <td>May 21, 2010</td>
      <td>Shrek Forever After</td>
      <td>$165,000,000</td>
      <td>$238,736,787</td>
      <td>$756,244,673</td>
      <td>May</td>
    </tr>
    <tr>
      <td>3</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>Sum.</td>
      <td>300500000.0</td>
      <td>398000000</td>
      <td>2010</td>
      <td>tt1325004</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>2010</td>
      <td>124.0</td>
      <td>Adventure,Drama,Fantasy</td>
      <td>5.0</td>
      <td>211733</td>
      <td>53</td>
      <td>Jun 30, 2010</td>
      <td>The Twilight Saga: Eclipse</td>
      <td>$68,000,000</td>
      <td>$300,531,751</td>
      <td>$706,102,828</td>
      <td>Jun</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Iron Man 2</td>
      <td>Par.</td>
      <td>312400000.0</td>
      <td>311500000</td>
      <td>2010</td>
      <td>tt1228705</td>
      <td>Iron Man 2</td>
      <td>Iron Man 2</td>
      <td>2010</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>657690</td>
      <td>15</td>
      <td>May 7, 2010</td>
      <td>Iron Man 2</td>
      <td>$170,000,000</td>
      <td>$312,433,331</td>
      <td>$621,156,389</td>
      <td>May</td>
    </tr>
    <tr>
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
      <td>1408</td>
      <td>Gotti</td>
      <td>VE</td>
      <td>4300000.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt1801552</td>
      <td>Gotti</td>
      <td>Gotti</td>
      <td>2018</td>
      <td>112.0</td>
      <td>Biography,Crime,Drama</td>
      <td>4.8</td>
      <td>10358</td>
      <td>64</td>
      <td>Jun 15, 2018</td>
      <td>Gotti</td>
      <td>$10,000,000</td>
      <td>$4,286,367</td>
      <td>$6,089,100</td>
      <td>Jun</td>
    </tr>
    <tr>
      <td>1409</td>
      <td>Bilal: A New Breed of Hero</td>
      <td>VE</td>
      <td>491000.0</td>
      <td>1700000</td>
      <td>2018</td>
      <td>tt3576728</td>
      <td>Bilal: A New Breed of Hero</td>
      <td>Bilal: A New Breed of Hero</td>
      <td>2015</td>
      <td>105.0</td>
      <td>Action,Adventure,Animation</td>
      <td>8.0</td>
      <td>16854</td>
      <td>100</td>
      <td>Feb 2, 2018</td>
      <td>Bilal: A New Breed of Hero</td>
      <td>$30,000,000</td>
      <td>$490,973</td>
      <td>$648,599</td>
      <td>Feb</td>
    </tr>
    <tr>
      <td>1410</td>
      <td>Mandy</td>
      <td>RLJ</td>
      <td>1200000.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt4995858</td>
      <td>Mandy</td>
      <td>Mandy</td>
      <td>2016</td>
      <td>113.0</td>
      <td>Drama,Thriller</td>
      <td>4.1</td>
      <td>39</td>
      <td>71</td>
      <td>Sep 14, 2018</td>
      <td>Mandy</td>
      <td>$6,000,000</td>
      <td>$1,214,525</td>
      <td>$1,427,656</td>
      <td>Sep</td>
    </tr>
    <tr>
      <td>1411</td>
      <td>Mandy</td>
      <td>RLJ</td>
      <td>1200000.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt6998518</td>
      <td>Mandy</td>
      <td>Mandy</td>
      <td>2018</td>
      <td>121.0</td>
      <td>Action,Fantasy,Horror</td>
      <td>6.6</td>
      <td>44378</td>
      <td>71</td>
      <td>Sep 14, 2018</td>
      <td>Mandy</td>
      <td>$6,000,000</td>
      <td>$1,214,525</td>
      <td>$1,427,656</td>
      <td>Sep</td>
    </tr>
    <tr>
      <td>1412</td>
      <td>Lean on Pete</td>
      <td>A24</td>
      <td>1200000.0</td>
      <td>NaN</td>
      <td>2018</td>
      <td>tt5340300</td>
      <td>Lean on Pete</td>
      <td>Lean on Pete</td>
      <td>2017</td>
      <td>121.0</td>
      <td>Adventure,Drama</td>
      <td>7.2</td>
      <td>8607</td>
      <td>13</td>
      <td>Apr 6, 2018</td>
      <td>Lean on Pete</td>
      <td>$8,000,000</td>
      <td>$1,163,056</td>
      <td>$2,455,027</td>
      <td>Apr</td>
    </tr>
  </tbody>
</table>
<p>1413 rows × 20 columns</p>
</div>




```python
#dump month = january - feburary, starts releasing good stuff during spring (april)
    #expect people to not show up as much, #during january, academy award eligibility rules

#joined_2['year_mine'] = joined_2['release_date'].str[:3]

x = joined_2['year']
y = joined_2['domestic_gross_x']
plt.barh(x,y)
#checking profits vs year to ensure growth in profits vs. time, good correlation with time vs. gross
```




    <BarContainer object of 1413 artists>




![png](Cleaned%20Code%20with%20comments%20_files/Cleaned%20Code%20with%20comments%20_28_1.png)



```python
#studio_joined_df['studio'] = studio_joined_df['studio'].fillna(value='No_studio')
joined_2['studio'] = joined_2['studio'].fillna(value = 'no_studio')
```


```python
joined_2['studio']
```




    0         BV
    1         WB
    2       P/DW
    3       Sum.
    4       Par.
            ... 
    1408      VE
    1409      VE
    1410     RLJ
    1411     RLJ
    1412     A24
    Name: studio, Length: 1413, dtype: object




```python
x = joined_2['studio']
y = joined_2['domestic_gross_x']
```


```python
plt.scatter(x,y)
```




    <matplotlib.collections.PathCollection at 0x224e5e667f0>




![png](Cleaned%20Code%20with%20comments%20_files/Cleaned%20Code%20with%20comments%20_32_1.png)



```python
final['worldwide_gross'] = final['worldwide_gross'].str.replace("$", "").str.replace(",","").astype('int')
#tn_movie_budgets_df['production_budget'] = tn_movie_budgets_df['production_budget'].str.replace("$", "").str.replace(",","").astype('int')
#final['profits'] = final['worldwide_gross'] - final['production_budget']

```


```python
final
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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross_x</th>
      <th>foreign_gross</th>
      <th>year</th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross_y</th>
      <th>worldwide_gross</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1303</td>
      <td>Black Panther</td>
      <td>BV</td>
      <td>700100000.0</td>
      <td>646900000</td>
      <td>2018</td>
      <td>tt1825683</td>
      <td>Black Panther</td>
      <td>Black Panther</td>
      <td>2018</td>
      <td>134.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.3</td>
      <td>516148</td>
      <td>42</td>
      <td>Feb 16, 2018</td>
      <td>Black Panther</td>
      <td>$200,000,000</td>
      <td>$700,059,566</td>
      <td>1348258224</td>
      <td>Feb</td>
    </tr>
    <tr>
      <td>1302</td>
      <td>Avengers: Infinity War</td>
      <td>BV</td>
      <td>678800000.0</td>
      <td>1,369.5</td>
      <td>2018</td>
      <td>tt4154756</td>
      <td>Avengers: Infinity War</td>
      <td>Avengers: Infinity War</td>
      <td>2018</td>
      <td>149.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>8.5</td>
      <td>670926</td>
      <td>7</td>
      <td>Apr 27, 2018</td>
      <td>Avengers: Infinity War</td>
      <td>$300,000,000</td>
      <td>$678,815,482</td>
      <td>2048134200</td>
      <td>Apr</td>
    </tr>
    <tr>
      <td>824</td>
      <td>Jurassic World</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
      <td>2015</td>
      <td>tt0369610</td>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>539338</td>
      <td>34</td>
      <td>Jun 12, 2015</td>
      <td>Jurassic World</td>
      <td>$215,000,000</td>
      <td>$652,270,625</td>
      <td>1648854864</td>
      <td>Jun</td>
    </tr>
    <tr>
      <td>1305</td>
      <td>Incredibles 2</td>
      <td>BV</td>
      <td>608600000.0</td>
      <td>634200000</td>
      <td>2018</td>
      <td>tt3606756</td>
      <td>Incredibles 2</td>
      <td>Incredibles 2</td>
      <td>2018</td>
      <td>118.0</td>
      <td>Action,Adventure,Animation</td>
      <td>7.7</td>
      <td>203510</td>
      <td>44</td>
      <td>Jun 15, 2018</td>
      <td>Incredibles 2</td>
      <td>$200,000,000</td>
      <td>$608,581,744</td>
      <td>1242520711</td>
      <td>Jun</td>
    </tr>
    <tr>
      <td>1005</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>BV</td>
      <td>532200000.0</td>
      <td>523900000</td>
      <td>2016</td>
      <td>tt3748528</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>Rogue One</td>
      <td>2016</td>
      <td>133.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.8</td>
      <td>478592</td>
      <td>45</td>
      <td>Dec 16, 2016</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>$200,000,000</td>
      <td>$532,177,324</td>
      <td>1049102856</td>
      <td>Dec</td>
    </tr>
    <tr>
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
      <td>806</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt8671762</td>
      <td>Jackpot</td>
      <td>Jackpot</td>
      <td>2018</td>
      <td>150.0</td>
      <td>Comedy,Romance</td>
      <td>7.8</td>
      <td>18</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>$400,000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
    </tr>
    <tr>
      <td>803</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt1809231</td>
      <td>Jackpot</td>
      <td>Arme Riddere</td>
      <td>2011</td>
      <td>86.0</td>
      <td>Action,Comedy,Crime</td>
      <td>6.6</td>
      <td>2941</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>$400,000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
    </tr>
    <tr>
      <td>804</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt3309662</td>
      <td>Jackpot</td>
      <td>Jackpot</td>
      <td>2013</td>
      <td>132.0</td>
      <td>Comedy,Thriller</td>
      <td>2.1</td>
      <td>647</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>$400,000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
    </tr>
    <tr>
      <td>805</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt4320966</td>
      <td>Jackpot</td>
      <td>Trung so</td>
      <td>2015</td>
      <td>92.0</td>
      <td>Comedy</td>
      <td>6.7</td>
      <td>55</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>$400,000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
    </tr>
    <tr>
      <td>166</td>
      <td>It's a Wonderful Afterlife</td>
      <td>UTV</td>
      <td>NaN</td>
      <td>1300000</td>
      <td>2010</td>
      <td>tt1319716</td>
      <td>It's a Wonderful Afterlife</td>
      <td>It's a Wonderful Afterlife</td>
      <td>2010</td>
      <td>100.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>5.4</td>
      <td>1361</td>
      <td>36</td>
      <td>Oct 8, 2010</td>
      <td>It's a Wonderful Afterlife</td>
      <td>$10,000,000</td>
      <td>$0</td>
      <td>1642939</td>
      <td>Oct</td>
    </tr>
  </tbody>
</table>
<p>1413 rows × 20 columns</p>
</div>




```python
final['production_budget'] = final['production_budget'].str.replace("$", "").str.replace(",","").astype('int')
```


```python
final
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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross_x</th>
      <th>foreign_gross</th>
      <th>year</th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross_y</th>
      <th>worldwide_gross</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1303</td>
      <td>Black Panther</td>
      <td>BV</td>
      <td>700100000.0</td>
      <td>646900000</td>
      <td>2018</td>
      <td>tt1825683</td>
      <td>Black Panther</td>
      <td>Black Panther</td>
      <td>2018</td>
      <td>134.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.3</td>
      <td>516148</td>
      <td>42</td>
      <td>Feb 16, 2018</td>
      <td>Black Panther</td>
      <td>200000000</td>
      <td>$700,059,566</td>
      <td>1348258224</td>
      <td>Feb</td>
    </tr>
    <tr>
      <td>1302</td>
      <td>Avengers: Infinity War</td>
      <td>BV</td>
      <td>678800000.0</td>
      <td>1,369.5</td>
      <td>2018</td>
      <td>tt4154756</td>
      <td>Avengers: Infinity War</td>
      <td>Avengers: Infinity War</td>
      <td>2018</td>
      <td>149.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>8.5</td>
      <td>670926</td>
      <td>7</td>
      <td>Apr 27, 2018</td>
      <td>Avengers: Infinity War</td>
      <td>300000000</td>
      <td>$678,815,482</td>
      <td>2048134200</td>
      <td>Apr</td>
    </tr>
    <tr>
      <td>824</td>
      <td>Jurassic World</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
      <td>2015</td>
      <td>tt0369610</td>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>539338</td>
      <td>34</td>
      <td>Jun 12, 2015</td>
      <td>Jurassic World</td>
      <td>215000000</td>
      <td>$652,270,625</td>
      <td>1648854864</td>
      <td>Jun</td>
    </tr>
    <tr>
      <td>1305</td>
      <td>Incredibles 2</td>
      <td>BV</td>
      <td>608600000.0</td>
      <td>634200000</td>
      <td>2018</td>
      <td>tt3606756</td>
      <td>Incredibles 2</td>
      <td>Incredibles 2</td>
      <td>2018</td>
      <td>118.0</td>
      <td>Action,Adventure,Animation</td>
      <td>7.7</td>
      <td>203510</td>
      <td>44</td>
      <td>Jun 15, 2018</td>
      <td>Incredibles 2</td>
      <td>200000000</td>
      <td>$608,581,744</td>
      <td>1242520711</td>
      <td>Jun</td>
    </tr>
    <tr>
      <td>1005</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>BV</td>
      <td>532200000.0</td>
      <td>523900000</td>
      <td>2016</td>
      <td>tt3748528</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>Rogue One</td>
      <td>2016</td>
      <td>133.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.8</td>
      <td>478592</td>
      <td>45</td>
      <td>Dec 16, 2016</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>200000000</td>
      <td>$532,177,324</td>
      <td>1049102856</td>
      <td>Dec</td>
    </tr>
    <tr>
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
      <td>806</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt8671762</td>
      <td>Jackpot</td>
      <td>Jackpot</td>
      <td>2018</td>
      <td>150.0</td>
      <td>Comedy,Romance</td>
      <td>7.8</td>
      <td>18</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>400000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
    </tr>
    <tr>
      <td>803</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt1809231</td>
      <td>Jackpot</td>
      <td>Arme Riddere</td>
      <td>2011</td>
      <td>86.0</td>
      <td>Action,Comedy,Crime</td>
      <td>6.6</td>
      <td>2941</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>400000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
    </tr>
    <tr>
      <td>804</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt3309662</td>
      <td>Jackpot</td>
      <td>Jackpot</td>
      <td>2013</td>
      <td>132.0</td>
      <td>Comedy,Thriller</td>
      <td>2.1</td>
      <td>647</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>400000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
    </tr>
    <tr>
      <td>805</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt4320966</td>
      <td>Jackpot</td>
      <td>Trung so</td>
      <td>2015</td>
      <td>92.0</td>
      <td>Comedy</td>
      <td>6.7</td>
      <td>55</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>400000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
    </tr>
    <tr>
      <td>166</td>
      <td>It's a Wonderful Afterlife</td>
      <td>UTV</td>
      <td>NaN</td>
      <td>1300000</td>
      <td>2010</td>
      <td>tt1319716</td>
      <td>It's a Wonderful Afterlife</td>
      <td>It's a Wonderful Afterlife</td>
      <td>2010</td>
      <td>100.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>5.4</td>
      <td>1361</td>
      <td>36</td>
      <td>Oct 8, 2010</td>
      <td>It's a Wonderful Afterlife</td>
      <td>10000000</td>
      <td>$0</td>
      <td>1642939</td>
      <td>Oct</td>
    </tr>
  </tbody>
</table>
<p>1413 rows × 20 columns</p>
</div>




```python
final['profits'] = final['worldwide_gross'] - final['production_budget']
final
#creating a column for profits
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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross_x</th>
      <th>foreign_gross</th>
      <th>year</th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>...</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross_y</th>
      <th>worldwide_gross</th>
      <th>month</th>
      <th>profits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1303</td>
      <td>Black Panther</td>
      <td>BV</td>
      <td>700100000.0</td>
      <td>646900000</td>
      <td>2018</td>
      <td>tt1825683</td>
      <td>Black Panther</td>
      <td>Black Panther</td>
      <td>2018</td>
      <td>134.0</td>
      <td>...</td>
      <td>7.3</td>
      <td>516148</td>
      <td>42</td>
      <td>Feb 16, 2018</td>
      <td>Black Panther</td>
      <td>200000000</td>
      <td>$700,059,566</td>
      <td>1348258224</td>
      <td>Feb</td>
      <td>1148258224</td>
    </tr>
    <tr>
      <td>1302</td>
      <td>Avengers: Infinity War</td>
      <td>BV</td>
      <td>678800000.0</td>
      <td>1,369.5</td>
      <td>2018</td>
      <td>tt4154756</td>
      <td>Avengers: Infinity War</td>
      <td>Avengers: Infinity War</td>
      <td>2018</td>
      <td>149.0</td>
      <td>...</td>
      <td>8.5</td>
      <td>670926</td>
      <td>7</td>
      <td>Apr 27, 2018</td>
      <td>Avengers: Infinity War</td>
      <td>300000000</td>
      <td>$678,815,482</td>
      <td>2048134200</td>
      <td>Apr</td>
      <td>1748134200</td>
    </tr>
    <tr>
      <td>824</td>
      <td>Jurassic World</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
      <td>2015</td>
      <td>tt0369610</td>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>...</td>
      <td>7.0</td>
      <td>539338</td>
      <td>34</td>
      <td>Jun 12, 2015</td>
      <td>Jurassic World</td>
      <td>215000000</td>
      <td>$652,270,625</td>
      <td>1648854864</td>
      <td>Jun</td>
      <td>1433854864</td>
    </tr>
    <tr>
      <td>1305</td>
      <td>Incredibles 2</td>
      <td>BV</td>
      <td>608600000.0</td>
      <td>634200000</td>
      <td>2018</td>
      <td>tt3606756</td>
      <td>Incredibles 2</td>
      <td>Incredibles 2</td>
      <td>2018</td>
      <td>118.0</td>
      <td>...</td>
      <td>7.7</td>
      <td>203510</td>
      <td>44</td>
      <td>Jun 15, 2018</td>
      <td>Incredibles 2</td>
      <td>200000000</td>
      <td>$608,581,744</td>
      <td>1242520711</td>
      <td>Jun</td>
      <td>1042520711</td>
    </tr>
    <tr>
      <td>1005</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>BV</td>
      <td>532200000.0</td>
      <td>523900000</td>
      <td>2016</td>
      <td>tt3748528</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>Rogue One</td>
      <td>2016</td>
      <td>133.0</td>
      <td>...</td>
      <td>7.8</td>
      <td>478592</td>
      <td>45</td>
      <td>Dec 16, 2016</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>200000000</td>
      <td>$532,177,324</td>
      <td>1049102856</td>
      <td>Dec</td>
      <td>849102856</td>
    </tr>
    <tr>
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
      <td>...</td>
    </tr>
    <tr>
      <td>806</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt8671762</td>
      <td>Jackpot</td>
      <td>Jackpot</td>
      <td>2018</td>
      <td>150.0</td>
      <td>...</td>
      <td>7.8</td>
      <td>18</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>400000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
      <td>-355548</td>
    </tr>
    <tr>
      <td>803</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt1809231</td>
      <td>Jackpot</td>
      <td>Arme Riddere</td>
      <td>2011</td>
      <td>86.0</td>
      <td>...</td>
      <td>6.6</td>
      <td>2941</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>400000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
      <td>-355548</td>
    </tr>
    <tr>
      <td>804</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt3309662</td>
      <td>Jackpot</td>
      <td>Jackpot</td>
      <td>2013</td>
      <td>132.0</td>
      <td>...</td>
      <td>2.1</td>
      <td>647</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>400000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
      <td>-355548</td>
    </tr>
    <tr>
      <td>805</td>
      <td>Jackpot</td>
      <td>DR</td>
      <td>800.0</td>
      <td>1100000</td>
      <td>2014</td>
      <td>tt4320966</td>
      <td>Jackpot</td>
      <td>Trung so</td>
      <td>2015</td>
      <td>92.0</td>
      <td>...</td>
      <td>6.7</td>
      <td>55</td>
      <td>17</td>
      <td>Jul 27, 2001</td>
      <td>Jackpot</td>
      <td>400000</td>
      <td>$44,452</td>
      <td>44452</td>
      <td>Jul</td>
      <td>-355548</td>
    </tr>
    <tr>
      <td>166</td>
      <td>It's a Wonderful Afterlife</td>
      <td>UTV</td>
      <td>NaN</td>
      <td>1300000</td>
      <td>2010</td>
      <td>tt1319716</td>
      <td>It's a Wonderful Afterlife</td>
      <td>It's a Wonderful Afterlife</td>
      <td>2010</td>
      <td>100.0</td>
      <td>...</td>
      <td>5.4</td>
      <td>1361</td>
      <td>36</td>
      <td>Oct 8, 2010</td>
      <td>It's a Wonderful Afterlife</td>
      <td>10000000</td>
      <td>$0</td>
      <td>1642939</td>
      <td>Oct</td>
      <td>-8357061</td>
    </tr>
  </tbody>
</table>
<p>1413 rows × 21 columns</p>
</div>




```python
x = final['studio']
y = final['profits']
plt.scatter(x,y)
```




    <matplotlib.collections.PathCollection at 0x224e9020fd0>




![png](Cleaned%20Code%20with%20comments%20_files/Cleaned%20Code%20with%20comments%20_38_1.png)



```python
finalfinal = final.sort_values(by = 'domestic_gross_x', ascending=False)[:200]
```


```python

finalfinal
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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross_x</th>
      <th>foreign_gross</th>
      <th>year</th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>...</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross_y</th>
      <th>worldwide_gross</th>
      <th>month</th>
      <th>profits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1303</td>
      <td>Black Panther</td>
      <td>BV</td>
      <td>700100000.0</td>
      <td>646900000</td>
      <td>2018</td>
      <td>tt1825683</td>
      <td>Black Panther</td>
      <td>Black Panther</td>
      <td>2018</td>
      <td>134.0</td>
      <td>...</td>
      <td>7.3</td>
      <td>516148</td>
      <td>42</td>
      <td>Feb 16, 2018</td>
      <td>Black Panther</td>
      <td>200000000</td>
      <td>$700,059,566</td>
      <td>1348258224</td>
      <td>Feb</td>
      <td>1148258224</td>
    </tr>
    <tr>
      <td>1302</td>
      <td>Avengers: Infinity War</td>
      <td>BV</td>
      <td>678800000.0</td>
      <td>1,369.5</td>
      <td>2018</td>
      <td>tt4154756</td>
      <td>Avengers: Infinity War</td>
      <td>Avengers: Infinity War</td>
      <td>2018</td>
      <td>149.0</td>
      <td>...</td>
      <td>8.5</td>
      <td>670926</td>
      <td>7</td>
      <td>Apr 27, 2018</td>
      <td>Avengers: Infinity War</td>
      <td>300000000</td>
      <td>$678,815,482</td>
      <td>2048134200</td>
      <td>Apr</td>
      <td>1748134200</td>
    </tr>
    <tr>
      <td>824</td>
      <td>Jurassic World</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
      <td>2015</td>
      <td>tt0369610</td>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>...</td>
      <td>7.0</td>
      <td>539338</td>
      <td>34</td>
      <td>Jun 12, 2015</td>
      <td>Jurassic World</td>
      <td>215000000</td>
      <td>$652,270,625</td>
      <td>1648854864</td>
      <td>Jun</td>
      <td>1433854864</td>
    </tr>
    <tr>
      <td>1305</td>
      <td>Incredibles 2</td>
      <td>BV</td>
      <td>608600000.0</td>
      <td>634200000</td>
      <td>2018</td>
      <td>tt3606756</td>
      <td>Incredibles 2</td>
      <td>Incredibles 2</td>
      <td>2018</td>
      <td>118.0</td>
      <td>...</td>
      <td>7.7</td>
      <td>203510</td>
      <td>44</td>
      <td>Jun 15, 2018</td>
      <td>Incredibles 2</td>
      <td>200000000</td>
      <td>$608,581,744</td>
      <td>1242520711</td>
      <td>Jun</td>
      <td>1042520711</td>
    </tr>
    <tr>
      <td>1005</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>BV</td>
      <td>532200000.0</td>
      <td>523900000</td>
      <td>2016</td>
      <td>tt3748528</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>Rogue One</td>
      <td>2016</td>
      <td>133.0</td>
      <td>...</td>
      <td>7.8</td>
      <td>478592</td>
      <td>45</td>
      <td>Dec 16, 2016</td>
      <td>Rogue One: A Star Wars Story</td>
      <td>200000000</td>
      <td>$532,177,324</td>
      <td>1049102856</td>
      <td>Dec</td>
      <td>849102856</td>
    </tr>
    <tr>
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
      <td>...</td>
    </tr>
    <tr>
      <td>378</td>
      <td>Life of Pi</td>
      <td>Fox</td>
      <td>125000000.0</td>
      <td>484000000</td>
      <td>2012</td>
      <td>tt0454876</td>
      <td>Life of Pi</td>
      <td>Life of Pi</td>
      <td>2012</td>
      <td>127.0</td>
      <td>...</td>
      <td>7.9</td>
      <td>535836</td>
      <td>88</td>
      <td>Nov 21, 2012</td>
      <td>Life of Pi</td>
      <td>120000000</td>
      <td>$124,987,022</td>
      <td>620912003</td>
      <td>Nov</td>
      <td>500912003</td>
    </tr>
    <tr>
      <td>403</td>
      <td>The Vow</td>
      <td>SGem</td>
      <td>125000000.0</td>
      <td>71100000</td>
      <td>2012</td>
      <td>tt1606389</td>
      <td>The Vow</td>
      <td>The Vow</td>
      <td>2012</td>
      <td>104.0</td>
      <td>...</td>
      <td>6.8</td>
      <td>173629</td>
      <td>42</td>
      <td>Feb 10, 2012</td>
      <td>The Vow</td>
      <td>30000000</td>
      <td>$125,014,030</td>
      <td>197618160</td>
      <td>Feb</td>
      <td>167618160</td>
    </tr>
    <tr>
      <td>200</td>
      <td>Rango</td>
      <td>Par.</td>
      <td>123500000.0</td>
      <td>122200000</td>
      <td>2011</td>
      <td>tt1192628</td>
      <td>Rango</td>
      <td>Rango</td>
      <td>2011</td>
      <td>107.0</td>
      <td>...</td>
      <td>7.2</td>
      <td>215761</td>
      <td>29</td>
      <td>Mar 4, 2011</td>
      <td>Rango</td>
      <td>135000000</td>
      <td>$123,477,607</td>
      <td>245724600</td>
      <td>Mar</td>
      <td>110724600</td>
    </tr>
    <tr>
      <td>843</td>
      <td>The Good Dinosaur</td>
      <td>BV</td>
      <td>123100000.0</td>
      <td>209100000</td>
      <td>2015</td>
      <td>tt1979388</td>
      <td>The Good Dinosaur</td>
      <td>The Good Dinosaur</td>
      <td>2015</td>
      <td>93.0</td>
      <td>...</td>
      <td>6.7</td>
      <td>91465</td>
      <td>73</td>
      <td>Nov 25, 2015</td>
      <td>The Good Dinosaur</td>
      <td>187500000</td>
      <td>$123,087,120</td>
      <td>333771037</td>
      <td>Nov</td>
      <td>146271037</td>
    </tr>
    <tr>
      <td>537</td>
      <td>G.I. Joe: Retaliation</td>
      <td>Par.</td>
      <td>122500000.0</td>
      <td>253200000</td>
      <td>2013</td>
      <td>tt1583421</td>
      <td>G.I. Joe: Retaliation</td>
      <td>G.I. Joe: Retaliation</td>
      <td>2013</td>
      <td>110.0</td>
      <td>...</td>
      <td>5.8</td>
      <td>165536</td>
      <td>13</td>
      <td>Mar 27, 2013</td>
      <td>G.I. Joe: Retaliation</td>
      <td>140000000</td>
      <td>$122,523,060</td>
      <td>375740705</td>
      <td>Mar</td>
      <td>235740705</td>
    </tr>
  </tbody>
</table>
<p>200 rows × 21 columns</p>
</div>




```python
plt.figure(figsize=(16, 12))
x = finalfinal['studio']
y = finalfinal['profits']
plt.barh(x,y)

plt.xlabel('Studio')
plt.ylabel('Profits')
plt.title("Studios vs. Profits")

#seeing studio vs. profits, which studio was most profitable, used only top 150 or else chart was very haphazard
```




    Text(0.5, 1.0, 'Studios vs. Profits')




![png](Cleaned%20Code%20with%20comments%20_files/Cleaned%20Code%20with%20comments%20_41_1.png)



```python
plt.figure(figsize=(15,9))
x = final['averagerating']
y = final['profits']
#correlations in that better grossing movies tended to also do better with audiences
plt.scatter(x,y, label = "Ratings vs. Profits")

plt.xlabel('Average Rating')
plt.ylabel('Profits')
plt.title("Ratings vs. Profits")
```




    Text(0.5, 1.0, 'Ratings vs. Profits')




![png](Cleaned%20Code%20with%20comments%20_files/Cleaned%20Code%20with%20comments%20_42_1.png)



```python
plt.figure(figsize=(15,9))
x = final['month']
y = final['profits']
plt.bar(x,y)

plt.xlabel('Month')
plt.ylabel('Profits')
plt.title("Month vs. Profits")

#seeing which month produced the most profits, reasons explored in powerpoint and down below
```




    Text(0.5, 1.0, 'Month vs. Profits')




![png](Cleaned%20Code%20with%20comments%20_files/Cleaned%20Code%20with%20comments%20_43_1.png)



```python
plt.figure(figsize=(15,9))
x = joined_2['year']
y = joined_2['domestic_gross_x']

plt.barh(x,y)

plt.xlabel('Year')
plt.ylabel('Profits')
plt.title("Year vs. Profits")

#seeing if the profits are on an uptrend 
```




    Text(0.5, 1.0, 'Year vs. Profits')




![png](Cleaned%20Code%20with%20comments%20_files/Cleaned%20Code%20with%20comments%20_44_1.png)



```python
Question 1: Does the release date affect the ratings or profits for the movie?
    Yes. There was definitely a positive correlation in year vs. profits, with profits exploding higher in the recent years. 
    This is probably due to the explicit growth in the industry itself, making it a viable industry to invest in the first
    place. There were also anomalies in the monthly profits. There was a huge decline in movie profits and movie releases
    on January while profits during June and April especially were very high. Also, there was a huge difference in January 
    vs. Febuary profits. 
```


```python
Question 2: Is there a better studio to work with? Are there studios that produce more profit than others?
    Yes. BV. and UNI made exponenially more than the others. Mostly through large movies that produced a disportionate 
    amount of profit. These studios are better to work with as they have more potential it seems. 
```


```python
Question 3: Is there a point in focusing on how well the movie does? Is there a correlation between rating and profit?
    Somewhat. Most of the movies did not produce much profit so those were pretty random in terms of rating. However, 
    a lot of movies that made a lot of money were always pretty well reviewed, showing us that there is significance in 
    a better movie, though it seems to only be for large movies that generate a lot of revenue.
```
