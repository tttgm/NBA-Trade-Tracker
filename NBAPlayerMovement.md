
# NBA Player Movement data for Chord Chart plot

Aim to display "chord charts" for player movement (via trade) around the NBA.

### Methods

Data will be broken up by year, and then split into "in-season" and "off-season" trades.

Metrics tracked are usage (USG%), player efficiency rating (PER), and win-shares per 48 minutes (WS/48). Player must have played a minimum of 5 games at both of the teams (no minutes limit).

Note: once working, expand to include WORP, RPM, etc.


```python
import pandas as pd

# load the created .csv into a Pandas dataframe
df = pd.read_csv("PlayerMovement_16-17_in-season.csv")
```


```python
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
      <th>player_name</th>
      <th>team_1</th>
      <th>team_2</th>
      <th>gp_1</th>
      <th>gp_2</th>
      <th>usg_1</th>
      <th>usg_2</th>
      <th>per_1</th>
      <th>per_2</th>
      <th>ws_per_48_1</th>
      <th>ws_per_48_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nerlens Noel</td>
      <td>PHI</td>
      <td>DAL</td>
      <td>29</td>
      <td>22</td>
      <td>17.8</td>
      <td>17.3</td>
      <td>20.8</td>
      <td>19.8</td>
      <td>0.184</td>
      <td>0.178</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Justin Anderson</td>
      <td>DAL</td>
      <td>PHI</td>
      <td>51</td>
      <td>24</td>
      <td>23.6</td>
      <td>17.2</td>
      <td>14.7</td>
      <td>12.8</td>
      <td>0.067</td>
      <td>0.084</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Taj Gibson</td>
      <td>CHI</td>
      <td>OKC</td>
      <td>55</td>
      <td>23</td>
      <td>19.2</td>
      <td>19.5</td>
      <td>15.4</td>
      <td>13.8</td>
      <td>0.104</td>
      <td>0.076</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Doug McDermott</td>
      <td>CHI</td>
      <td>OKC</td>
      <td>44</td>
      <td>22</td>
      <td>17.6</td>
      <td>13.6</td>
      <td>11.3</td>
      <td>9.3</td>
      <td>0.084</td>
      <td>0.080</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joffrey Lauvergne</td>
      <td>OKC</td>
      <td>CHI</td>
      <td>50</td>
      <td>20</td>
      <td>17.8</td>
      <td>20.7</td>
      <td>12.9</td>
      <td>11.6</td>
      <td>0.091</td>
      <td>0.025</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['usg_1_norm'] = df['usg_1'] * 5
df['usg_2_norm'] = df['usg_2'] * 5

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
      <th>player_name</th>
      <th>team_1</th>
      <th>team_2</th>
      <th>gp_1</th>
      <th>gp_2</th>
      <th>usg_1</th>
      <th>usg_2</th>
      <th>per_1</th>
      <th>per_2</th>
      <th>ws_per_48_1</th>
      <th>ws_per_48_2</th>
      <th>usg_1_norm</th>
      <th>usg_2_norm</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nerlens Noel</td>
      <td>PHI</td>
      <td>DAL</td>
      <td>29</td>
      <td>22</td>
      <td>17.8</td>
      <td>17.3</td>
      <td>20.8</td>
      <td>19.8</td>
      <td>0.184</td>
      <td>0.178</td>
      <td>89.0</td>
      <td>86.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Justin Anderson</td>
      <td>DAL</td>
      <td>PHI</td>
      <td>51</td>
      <td>24</td>
      <td>23.6</td>
      <td>17.2</td>
      <td>14.7</td>
      <td>12.8</td>
      <td>0.067</td>
      <td>0.084</td>
      <td>118.0</td>
      <td>86.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Taj Gibson</td>
      <td>CHI</td>
      <td>OKC</td>
      <td>55</td>
      <td>23</td>
      <td>19.2</td>
      <td>19.5</td>
      <td>15.4</td>
      <td>13.8</td>
      <td>0.104</td>
      <td>0.076</td>
      <td>96.0</td>
      <td>97.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Doug McDermott</td>
      <td>CHI</td>
      <td>OKC</td>
      <td>44</td>
      <td>22</td>
      <td>17.6</td>
      <td>13.6</td>
      <td>11.3</td>
      <td>9.3</td>
      <td>0.084</td>
      <td>0.080</td>
      <td>88.0</td>
      <td>68.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joffrey Lauvergne</td>
      <td>OKC</td>
      <td>CHI</td>
      <td>50</td>
      <td>20</td>
      <td>17.8</td>
      <td>20.7</td>
      <td>12.9</td>
      <td>11.6</td>
      <td>0.091</td>
      <td>0.025</td>
      <td>89.0</td>
      <td>103.5</td>
    </tr>
  </tbody>
</table>
</div>




```python
teams = pd.read_csv("team_abbrevs.csv")

teams.head()
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
      <th>Franchise</th>
      <th>Acronym</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Atlanta Hawks</td>
      <td>ATL</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Brooklyn Nets</td>
      <td>BKN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Boston Celtics</td>
      <td>BOS</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Charlotte Hornets</td>
      <td>CHA</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Chicago Bulls</td>
      <td>CHI</td>
    </tr>
  </tbody>
</table>
</div>




```python
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
      <th>player_name</th>
      <th>team_1</th>
      <th>team_2</th>
      <th>gp_1</th>
      <th>gp_2</th>
      <th>usg_1</th>
      <th>usg_2</th>
      <th>per_1</th>
      <th>per_2</th>
      <th>ws_per_48_1</th>
      <th>ws_per_48_2</th>
      <th>usg_1_norm</th>
      <th>usg_2_norm</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nerlens Noel</td>
      <td>PHI</td>
      <td>DAL</td>
      <td>29</td>
      <td>22</td>
      <td>17.8</td>
      <td>17.3</td>
      <td>20.8</td>
      <td>19.8</td>
      <td>0.184</td>
      <td>0.178</td>
      <td>89.0</td>
      <td>86.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Justin Anderson</td>
      <td>DAL</td>
      <td>PHI</td>
      <td>51</td>
      <td>24</td>
      <td>23.6</td>
      <td>17.2</td>
      <td>14.7</td>
      <td>12.8</td>
      <td>0.067</td>
      <td>0.084</td>
      <td>118.0</td>
      <td>86.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Taj Gibson</td>
      <td>CHI</td>
      <td>OKC</td>
      <td>55</td>
      <td>23</td>
      <td>19.2</td>
      <td>19.5</td>
      <td>15.4</td>
      <td>13.8</td>
      <td>0.104</td>
      <td>0.076</td>
      <td>96.0</td>
      <td>97.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Doug McDermott</td>
      <td>CHI</td>
      <td>OKC</td>
      <td>44</td>
      <td>22</td>
      <td>17.6</td>
      <td>13.6</td>
      <td>11.3</td>
      <td>9.3</td>
      <td>0.084</td>
      <td>0.080</td>
      <td>88.0</td>
      <td>68.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joffrey Lauvergne</td>
      <td>OKC</td>
      <td>CHI</td>
      <td>50</td>
      <td>20</td>
      <td>17.8</td>
      <td>20.7</td>
      <td>12.9</td>
      <td>11.6</td>
      <td>0.091</td>
      <td>0.025</td>
      <td>89.0</td>
      <td>103.5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Anthony Morrow</td>
      <td>OKC</td>
      <td>CHI</td>
      <td>40</td>
      <td>9</td>
      <td>16.3</td>
      <td>17.0</td>
      <td>9.2</td>
      <td>16.0</td>
      <td>0.069</td>
      <td>0.190</td>
      <td>81.5</td>
      <td>85.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cameron Payne</td>
      <td>OKC</td>
      <td>CHI</td>
      <td>20</td>
      <td>11</td>
      <td>19.6</td>
      <td>23.9</td>
      <td>6.1</td>
      <td>4.0</td>
      <td>-0.012</td>
      <td>-0.086</td>
      <td>98.0</td>
      <td>119.5</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Tyler Ennis</td>
      <td>HOU</td>
      <td>LAL</td>
      <td>31</td>
      <td>22</td>
      <td>19.3</td>
      <td>19.0</td>
      <td>4.0</td>
      <td>14.3</td>
      <td>-0.061</td>
      <td>0.077</td>
      <td>96.5</td>
      <td>95.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>K.J. McDaniels</td>
      <td>HOU</td>
      <td>BKN</td>
      <td>29</td>
      <td>20</td>
      <td>16.4</td>
      <td>19.3</td>
      <td>10.2</td>
      <td>12.5</td>
      <td>0.058</td>
      <td>0.046</td>
      <td>82.0</td>
      <td>96.5</td>
    </tr>
    <tr>
      <th>9</th>
      <td>P.J. Tucker</td>
      <td>PHX</td>
      <td>TOR</td>
      <td>57</td>
      <td>24</td>
      <td>11.3</td>
      <td>10.9</td>
      <td>10.6</td>
      <td>10.4</td>
      <td>0.069</td>
      <td>0.099</td>
      <td>56.5</td>
      <td>54.5</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Roy Hibbert</td>
      <td>CHA</td>
      <td>DEN</td>
      <td>42</td>
      <td>6</td>
      <td>14.0</td>
      <td>15.5</td>
      <td>13.5</td>
      <td>16.3</td>
      <td>0.129</td>
      <td>0.119</td>
      <td>70.0</td>
      <td>77.5</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Lou Williams</td>
      <td>LAL</td>
      <td>HOU</td>
      <td>58</td>
      <td>23</td>
      <td>30.6</td>
      <td>25.3</td>
      <td>23.9</td>
      <td>15.4</td>
      <td>0.169</td>
      <td>0.096</td>
      <td>153.0</td>
      <td>126.5</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Corey Brewer</td>
      <td>HOU</td>
      <td>LAL</td>
      <td>58</td>
      <td>24</td>
      <td>12.9</td>
      <td>18.0</td>
      <td>7.5</td>
      <td>13.3</td>
      <td>0.045</td>
      <td>0.047</td>
      <td>64.5</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ersan Ilyasova</td>
      <td>PHI</td>
      <td>ATL</td>
      <td>53</td>
      <td>26</td>
      <td>23.6</td>
      <td>19.9</td>
      <td>15.4</td>
      <td>13.5</td>
      <td>0.088</td>
      <td>0.106</td>
      <td>118.0</td>
      <td>99.5</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bojan Bogdonavic</td>
      <td>BKN</td>
      <td>WAS</td>
      <td>55</td>
      <td>26</td>
      <td>22.5</td>
      <td>22.2</td>
      <td>13.0</td>
      <td>14.7</td>
      <td>0.052</td>
      <td>0.103</td>
      <td>112.5</td>
      <td>111.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Andrew Nicholson</td>
      <td>WAS</td>
      <td>BKN</td>
      <td>28</td>
      <td>10</td>
      <td>17.6</td>
      <td>15.8</td>
      <td>6.3</td>
      <td>5.0</td>
      <td>-0.014</td>
      <td>-0.030</td>
      <td>88.0</td>
      <td>79.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Buddy Hield</td>
      <td>NOP</td>
      <td>SAC</td>
      <td>57</td>
      <td>25</td>
      <td>20.4</td>
      <td>22.9</td>
      <td>9.9</td>
      <td>14.9</td>
      <td>0.020</td>
      <td>0.053</td>
      <td>102.0</td>
      <td>114.5</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Tyreke Evans</td>
      <td>NOP</td>
      <td>SAC</td>
      <td>26</td>
      <td>14</td>
      <td>27.2</td>
      <td>26.0</td>
      <td>15.8</td>
      <td>14.9</td>
      <td>0.053</td>
      <td>0.034</td>
      <td>136.0</td>
      <td>130.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Langston Galloway</td>
      <td>NOP</td>
      <td>SAC</td>
      <td>55</td>
      <td>19</td>
      <td>19.5</td>
      <td>15.4</td>
      <td>11.2</td>
      <td>8.0</td>
      <td>0.052</td>
      <td>0.007</td>
      <td>97.5</td>
      <td>77.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>DeMarcus Cousins</td>
      <td>SAC</td>
      <td>NOP</td>
      <td>55</td>
      <td>17</td>
      <td>37.5</td>
      <td>33.1</td>
      <td>26.5</td>
      <td>23.2</td>
      <td>0.153</td>
      <td>0.136</td>
      <td>187.5</td>
      <td>165.5</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Serge Ibaka</td>
      <td>ORL</td>
      <td>TOR</td>
      <td>56</td>
      <td>23</td>
      <td>20.8</td>
      <td>20.9</td>
      <td>17.5</td>
      <td>13.8</td>
      <td>0.108</td>
      <td>0.085</td>
      <td>104.0</td>
      <td>104.5</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Terrence Ross</td>
      <td>TOR</td>
      <td>ORL</td>
      <td>54</td>
      <td>24</td>
      <td>19.8</td>
      <td>18.5</td>
      <td>14.9</td>
      <td>11.1</td>
      <td>0.116</td>
      <td>0.037</td>
      <td>99.0</td>
      <td>92.5</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Jusuf Nurkic</td>
      <td>DEN</td>
      <td>POR</td>
      <td>45</td>
      <td>20</td>
      <td>22.6</td>
      <td>25.7</td>
      <td>15.0</td>
      <td>21.1</td>
      <td>0.034</td>
      <td>0.116</td>
      <td>113.0</td>
      <td>128.5</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Mason Plumlee</td>
      <td>POR</td>
      <td>DEN</td>
      <td>54</td>
      <td>27</td>
      <td>18.3</td>
      <td>17.5</td>
      <td>18.9</td>
      <td>16.3</td>
      <td>0.147</td>
      <td>0.107</td>
      <td>91.5</td>
      <td>87.5</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Spencer Hawes</td>
      <td>CHA</td>
      <td>MIL</td>
      <td>35</td>
      <td>19</td>
      <td>19.6</td>
      <td>20.3</td>
      <td>14.8</td>
      <td>18.6</td>
      <td>0.086</td>
      <td>0.160</td>
      <td>98.0</td>
      <td>101.5</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Miles Plumlee</td>
      <td>MIL</td>
      <td>CHA</td>
      <td>32</td>
      <td>13</td>
      <td>15.5</td>
      <td>8.7</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>0.021</td>
      <td>0.087</td>
      <td>77.5</td>
      <td>43.5</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Kyle Korver</td>
      <td>ATL</td>
      <td>CLE</td>
      <td>32</td>
      <td>35</td>
      <td>14.5</td>
      <td>15.8</td>
      <td>10.8</td>
      <td>13.5</td>
      <td>0.088</td>
      <td>0.114</td>
      <td>72.5</td>
      <td>79.0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Mike Dunleavy</td>
      <td>CLE</td>
      <td>ATL</td>
      <td>23</td>
      <td>30</td>
      <td>13.7</td>
      <td>14.3</td>
      <td>7.6</td>
      <td>11.9</td>
      <td>0.041</td>
      <td>0.120</td>
      <td>68.5</td>
      <td>71.5</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['traders'] = df['team_1'] + '_' + df['team_2']
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
      <th>player_name</th>
      <th>team_1</th>
      <th>team_2</th>
      <th>gp_1</th>
      <th>gp_2</th>
      <th>usg_1</th>
      <th>usg_2</th>
      <th>per_1</th>
      <th>per_2</th>
      <th>ws_per_48_1</th>
      <th>ws_per_48_2</th>
      <th>usg_1_norm</th>
      <th>usg_2_norm</th>
      <th>traders</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nerlens Noel</td>
      <td>PHI</td>
      <td>DAL</td>
      <td>29</td>
      <td>22</td>
      <td>17.8</td>
      <td>17.3</td>
      <td>20.8</td>
      <td>19.8</td>
      <td>0.184</td>
      <td>0.178</td>
      <td>89.0</td>
      <td>86.5</td>
      <td>PHI_DAL</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Justin Anderson</td>
      <td>DAL</td>
      <td>PHI</td>
      <td>51</td>
      <td>24</td>
      <td>23.6</td>
      <td>17.2</td>
      <td>14.7</td>
      <td>12.8</td>
      <td>0.067</td>
      <td>0.084</td>
      <td>118.0</td>
      <td>86.0</td>
      <td>DAL_PHI</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Taj Gibson</td>
      <td>CHI</td>
      <td>OKC</td>
      <td>55</td>
      <td>23</td>
      <td>19.2</td>
      <td>19.5</td>
      <td>15.4</td>
      <td>13.8</td>
      <td>0.104</td>
      <td>0.076</td>
      <td>96.0</td>
      <td>97.5</td>
      <td>CHI_OKC</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Doug McDermott</td>
      <td>CHI</td>
      <td>OKC</td>
      <td>44</td>
      <td>22</td>
      <td>17.6</td>
      <td>13.6</td>
      <td>11.3</td>
      <td>9.3</td>
      <td>0.084</td>
      <td>0.080</td>
      <td>88.0</td>
      <td>68.0</td>
      <td>CHI_OKC</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joffrey Lauvergne</td>
      <td>OKC</td>
      <td>CHI</td>
      <td>50</td>
      <td>20</td>
      <td>17.8</td>
      <td>20.7</td>
      <td>12.9</td>
      <td>11.6</td>
      <td>0.091</td>
      <td>0.025</td>
      <td>89.0</td>
      <td>103.5</td>
      <td>OKC_CHI</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Anthony Morrow</td>
      <td>OKC</td>
      <td>CHI</td>
      <td>40</td>
      <td>9</td>
      <td>16.3</td>
      <td>17.0</td>
      <td>9.2</td>
      <td>16.0</td>
      <td>0.069</td>
      <td>0.190</td>
      <td>81.5</td>
      <td>85.0</td>
      <td>OKC_CHI</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cameron Payne</td>
      <td>OKC</td>
      <td>CHI</td>
      <td>20</td>
      <td>11</td>
      <td>19.6</td>
      <td>23.9</td>
      <td>6.1</td>
      <td>4.0</td>
      <td>-0.012</td>
      <td>-0.086</td>
      <td>98.0</td>
      <td>119.5</td>
      <td>OKC_CHI</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Tyler Ennis</td>
      <td>HOU</td>
      <td>LAL</td>
      <td>31</td>
      <td>22</td>
      <td>19.3</td>
      <td>19.0</td>
      <td>4.0</td>
      <td>14.3</td>
      <td>-0.061</td>
      <td>0.077</td>
      <td>96.5</td>
      <td>95.0</td>
      <td>HOU_LAL</td>
    </tr>
    <tr>
      <th>8</th>
      <td>K.J. McDaniels</td>
      <td>HOU</td>
      <td>BKN</td>
      <td>29</td>
      <td>20</td>
      <td>16.4</td>
      <td>19.3</td>
      <td>10.2</td>
      <td>12.5</td>
      <td>0.058</td>
      <td>0.046</td>
      <td>82.0</td>
      <td>96.5</td>
      <td>HOU_BKN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>P.J. Tucker</td>
      <td>PHX</td>
      <td>TOR</td>
      <td>57</td>
      <td>24</td>
      <td>11.3</td>
      <td>10.9</td>
      <td>10.6</td>
      <td>10.4</td>
      <td>0.069</td>
      <td>0.099</td>
      <td>56.5</td>
      <td>54.5</td>
      <td>PHX_TOR</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Roy Hibbert</td>
      <td>CHA</td>
      <td>DEN</td>
      <td>42</td>
      <td>6</td>
      <td>14.0</td>
      <td>15.5</td>
      <td>13.5</td>
      <td>16.3</td>
      <td>0.129</td>
      <td>0.119</td>
      <td>70.0</td>
      <td>77.5</td>
      <td>CHA_DEN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Lou Williams</td>
      <td>LAL</td>
      <td>HOU</td>
      <td>58</td>
      <td>23</td>
      <td>30.6</td>
      <td>25.3</td>
      <td>23.9</td>
      <td>15.4</td>
      <td>0.169</td>
      <td>0.096</td>
      <td>153.0</td>
      <td>126.5</td>
      <td>LAL_HOU</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Corey Brewer</td>
      <td>HOU</td>
      <td>LAL</td>
      <td>58</td>
      <td>24</td>
      <td>12.9</td>
      <td>18.0</td>
      <td>7.5</td>
      <td>13.3</td>
      <td>0.045</td>
      <td>0.047</td>
      <td>64.5</td>
      <td>90.0</td>
      <td>HOU_LAL</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ersan Ilyasova</td>
      <td>PHI</td>
      <td>ATL</td>
      <td>53</td>
      <td>26</td>
      <td>23.6</td>
      <td>19.9</td>
      <td>15.4</td>
      <td>13.5</td>
      <td>0.088</td>
      <td>0.106</td>
      <td>118.0</td>
      <td>99.5</td>
      <td>PHI_ATL</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Bojan Bogdonavic</td>
      <td>BKN</td>
      <td>WAS</td>
      <td>55</td>
      <td>26</td>
      <td>22.5</td>
      <td>22.2</td>
      <td>13.0</td>
      <td>14.7</td>
      <td>0.052</td>
      <td>0.103</td>
      <td>112.5</td>
      <td>111.0</td>
      <td>BKN_WAS</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Andrew Nicholson</td>
      <td>WAS</td>
      <td>BKN</td>
      <td>28</td>
      <td>10</td>
      <td>17.6</td>
      <td>15.8</td>
      <td>6.3</td>
      <td>5.0</td>
      <td>-0.014</td>
      <td>-0.030</td>
      <td>88.0</td>
      <td>79.0</td>
      <td>WAS_BKN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Buddy Hield</td>
      <td>NOP</td>
      <td>SAC</td>
      <td>57</td>
      <td>25</td>
      <td>20.4</td>
      <td>22.9</td>
      <td>9.9</td>
      <td>14.9</td>
      <td>0.020</td>
      <td>0.053</td>
      <td>102.0</td>
      <td>114.5</td>
      <td>NOP_SAC</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Tyreke Evans</td>
      <td>NOP</td>
      <td>SAC</td>
      <td>26</td>
      <td>14</td>
      <td>27.2</td>
      <td>26.0</td>
      <td>15.8</td>
      <td>14.9</td>
      <td>0.053</td>
      <td>0.034</td>
      <td>136.0</td>
      <td>130.0</td>
      <td>NOP_SAC</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Langston Galloway</td>
      <td>NOP</td>
      <td>SAC</td>
      <td>55</td>
      <td>19</td>
      <td>19.5</td>
      <td>15.4</td>
      <td>11.2</td>
      <td>8.0</td>
      <td>0.052</td>
      <td>0.007</td>
      <td>97.5</td>
      <td>77.0</td>
      <td>NOP_SAC</td>
    </tr>
    <tr>
      <th>19</th>
      <td>DeMarcus Cousins</td>
      <td>SAC</td>
      <td>NOP</td>
      <td>55</td>
      <td>17</td>
      <td>37.5</td>
      <td>33.1</td>
      <td>26.5</td>
      <td>23.2</td>
      <td>0.153</td>
      <td>0.136</td>
      <td>187.5</td>
      <td>165.5</td>
      <td>SAC_NOP</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Serge Ibaka</td>
      <td>ORL</td>
      <td>TOR</td>
      <td>56</td>
      <td>23</td>
      <td>20.8</td>
      <td>20.9</td>
      <td>17.5</td>
      <td>13.8</td>
      <td>0.108</td>
      <td>0.085</td>
      <td>104.0</td>
      <td>104.5</td>
      <td>ORL_TOR</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Terrence Ross</td>
      <td>TOR</td>
      <td>ORL</td>
      <td>54</td>
      <td>24</td>
      <td>19.8</td>
      <td>18.5</td>
      <td>14.9</td>
      <td>11.1</td>
      <td>0.116</td>
      <td>0.037</td>
      <td>99.0</td>
      <td>92.5</td>
      <td>TOR_ORL</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Jusuf Nurkic</td>
      <td>DEN</td>
      <td>POR</td>
      <td>45</td>
      <td>20</td>
      <td>22.6</td>
      <td>25.7</td>
      <td>15.0</td>
      <td>21.1</td>
      <td>0.034</td>
      <td>0.116</td>
      <td>113.0</td>
      <td>128.5</td>
      <td>DEN_POR</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Mason Plumlee</td>
      <td>POR</td>
      <td>DEN</td>
      <td>54</td>
      <td>27</td>
      <td>18.3</td>
      <td>17.5</td>
      <td>18.9</td>
      <td>16.3</td>
      <td>0.147</td>
      <td>0.107</td>
      <td>91.5</td>
      <td>87.5</td>
      <td>POR_DEN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Spencer Hawes</td>
      <td>CHA</td>
      <td>MIL</td>
      <td>35</td>
      <td>19</td>
      <td>19.6</td>
      <td>20.3</td>
      <td>14.8</td>
      <td>18.6</td>
      <td>0.086</td>
      <td>0.160</td>
      <td>98.0</td>
      <td>101.5</td>
      <td>CHA_MIL</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Miles Plumlee</td>
      <td>MIL</td>
      <td>CHA</td>
      <td>32</td>
      <td>13</td>
      <td>15.5</td>
      <td>8.7</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>0.021</td>
      <td>0.087</td>
      <td>77.5</td>
      <td>43.5</td>
      <td>MIL_CHA</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Kyle Korver</td>
      <td>ATL</td>
      <td>CLE</td>
      <td>32</td>
      <td>35</td>
      <td>14.5</td>
      <td>15.8</td>
      <td>10.8</td>
      <td>13.5</td>
      <td>0.088</td>
      <td>0.114</td>
      <td>72.5</td>
      <td>79.0</td>
      <td>ATL_CLE</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Mike Dunleavy</td>
      <td>CLE</td>
      <td>ATL</td>
      <td>23</td>
      <td>30</td>
      <td>13.7</td>
      <td>14.3</td>
      <td>7.6</td>
      <td>11.9</td>
      <td>0.041</td>
      <td>0.120</td>
      <td>68.5</td>
      <td>71.5</td>
      <td>CLE_ATL</td>
    </tr>
  </tbody>
</table>
</div>



### Want to make a dictionary of possible trade partners
Can use this to see how often teams trade players/how many players they send. Maybe include directionality.


```python
# import 'team name' csv
team_names = pd.read_csv('team_abbrevs.csv')
team_names.head()
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
      <th>Franchise</th>
      <th>Acronym</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Atlanta Hawks</td>
      <td>ATL</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Brooklyn Nets</td>
      <td>BKN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Boston Celtics</td>
      <td>BOS</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Charlotte Hornets</td>
      <td>CHA</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Chicago Bulls</td>
      <td>CHI</td>
    </tr>
  </tbody>
</table>
</div>




```python
def trading_pairs(dataframe, col_name='Acronym'):
    '''
    Want to achieve something like this with empty lists for each pair (& direction) of trade partners:
    atl_bos = []
    atl_bkn = []
    atl_cha = []
    atl_chi = []
    atl_cle = []
    atl_dal = []
    atl_den = []
    atl_det = []
    atl_gsw = []
    atl_hou = []
    atl_ind = []
    atl_lac = []
    atl_lal = []
    atl_mem = []
    atl_mia = []
    atl_nop = []
    etc...
    '''
    trading_partners = []
    for team1 in dataframe[col_name]:
        for team2 in dataframe[col_name]:
            if team1 == team2:
                continue
            else:
                trading_partners.append(team1 + "_" + team2)
    
    return trading_partners
```


```python
list_of_trade_partners = trading_pairs(team_names)
list_of_trade_partners[:10]
```




    ['ATL_BKN',
     'ATL_BOS',
     'ATL_CHA',
     'ATL_CHI',
     'ATL_CLE',
     'ATL_DAL',
     'ATL_DEN',
     'ATL_DET',
     'ATL_GSW',
     'ATL_HOU']




```python
# Count up all of the trades between teams and assign them to the strings in the
# 'list_of_trade_partners'.
# Note this doesn't count the NUMBER OF TRADES, but rather the number of PLAYERS TRADED.
# Directionality is included (for now).

trade_counter = {}

for pair in list_of_trade_partners:
    trade_counter[pair] = 0
    for traders in df['traders']:
        if traders == pair:
            trade_counter[pair] += 1

# check is collecting the trades properly with a list comprehension!
unique_trade_partners = [val for val in trade_counter.itervalues() if (val != 0)]
players_involved = sum(unique_trade_partners)

print "Number of trade partners: " + str(len(unique_trade_partners))
print "Number of players involved: " + str(players_involved)
```

    Number of trade partners: 22
    Number of players involved: 28


That seems to be working well! (note: is only for the 2016-17 season at the moment - hence only 28 players traded)

# Visualization of the data

## Prepare code for use in D3: Convert to JSON object


```python
### Convert Pandas df to dict-of-dict to conform to JSON readability for use in D3.

# trade_dict = df.set_index('traders').T.to_dict()
trade_dict = df.T.to_dict() # '.T' is used to 'transpose' the df

trade_dict
```




    {0: {'gp_1': 29,
      'gp_2': 22,
      'per_1': 20.8,
      'per_2': 19.8,
      'player_name': 'Nerlens Noel',
      'team_1': 'PHI',
      'team_2': 'DAL',
      'traders': 'PHI_DAL',
      'usg_1': 17.8,
      'usg_1_norm': 89.0,
      'usg_2': 17.3,
      'usg_2_norm': 86.5,
      'ws_per_48_1': 0.184,
      'ws_per_48_2': 0.17800000000000002},
     1: {'gp_1': 51,
      'gp_2': 24,
      'per_1': 14.7,
      'per_2': 12.8,
      'player_name': 'Justin Anderson',
      'team_1': 'DAL',
      'team_2': 'PHI',
      'traders': 'DAL_PHI',
      'usg_1': 23.6,
      'usg_1_norm': 118.0,
      'usg_2': 17.2,
      'usg_2_norm': 86.0,
      'ws_per_48_1': 0.067,
      'ws_per_48_2': 0.084},
     2: {'gp_1': 55,
      'gp_2': 23,
      'per_1': 15.4,
      'per_2': 13.8,
      'player_name': 'Taj Gibson',
      'team_1': 'CHI',
      'team_2': 'OKC',
      'traders': 'CHI_OKC',
      'usg_1': 19.2,
      'usg_1_norm': 96.0,
      'usg_2': 19.5,
      'usg_2_norm': 97.5,
      'ws_per_48_1': 0.10400000000000001,
      'ws_per_48_2': 0.076},
     3: {'gp_1': 44,
      'gp_2': 22,
      'per_1': 11.3,
      'per_2': 9.3,
      'player_name': 'Doug McDermott',
      'team_1': 'CHI',
      'team_2': 'OKC',
      'traders': 'CHI_OKC',
      'usg_1': 17.6,
      'usg_1_norm': 88.0,
      'usg_2': 13.6,
      'usg_2_norm': 68.0,
      'ws_per_48_1': 0.084,
      'ws_per_48_2': 0.08},
     4: {'gp_1': 50,
      'gp_2': 20,
      'per_1': 12.9,
      'per_2': 11.6,
      'player_name': 'Joffrey Lauvergne',
      'team_1': 'OKC',
      'team_2': 'CHI',
      'traders': 'OKC_CHI',
      'usg_1': 17.8,
      'usg_1_norm': 89.0,
      'usg_2': 20.7,
      'usg_2_norm': 103.5,
      'ws_per_48_1': 0.091,
      'ws_per_48_2': 0.025},
     5: {'gp_1': 40,
      'gp_2': 9,
      'per_1': 9.2,
      'per_2': 16.0,
      'player_name': 'Anthony Morrow',
      'team_1': 'OKC',
      'team_2': 'CHI',
      'traders': 'OKC_CHI',
      'usg_1': 16.3,
      'usg_1_norm': 81.5,
      'usg_2': 17.0,
      'usg_2_norm': 85.0,
      'ws_per_48_1': 0.069,
      'ws_per_48_2': 0.19},
     6: {'gp_1': 20,
      'gp_2': 11,
      'per_1': 6.1,
      'per_2': 4.0,
      'player_name': 'Cameron Payne',
      'team_1': 'OKC',
      'team_2': 'CHI',
      'traders': 'OKC_CHI',
      'usg_1': 19.6,
      'usg_1_norm': 98.0,
      'usg_2': 23.9,
      'usg_2_norm': 119.5,
      'ws_per_48_1': -0.012,
      'ws_per_48_2': -0.086},
     7: {'gp_1': 31,
      'gp_2': 22,
      'per_1': 4.0,
      'per_2': 14.3,
      'player_name': 'Tyler Ennis',
      'team_1': 'HOU',
      'team_2': 'LAL',
      'traders': 'HOU_LAL',
      'usg_1': 19.3,
      'usg_1_norm': 96.5,
      'usg_2': 19.0,
      'usg_2_norm': 95.0,
      'ws_per_48_1': -0.061,
      'ws_per_48_2': 0.077},
     8: {'gp_1': 29,
      'gp_2': 20,
      'per_1': 10.2,
      'per_2': 12.5,
      'player_name': 'K.J. McDaniels',
      'team_1': 'HOU',
      'team_2': 'BKN',
      'traders': 'HOU_BKN',
      'usg_1': 16.4,
      'usg_1_norm': 82.0,
      'usg_2': 19.3,
      'usg_2_norm': 96.5,
      'ws_per_48_1': 0.057999999999999996,
      'ws_per_48_2': 0.046},
     9: {'gp_1': 57,
      'gp_2': 24,
      'per_1': 10.6,
      'per_2': 10.4,
      'player_name': 'P.J. Tucker',
      'team_1': 'PHX',
      'team_2': 'TOR',
      'traders': 'PHX_TOR',
      'usg_1': 11.3,
      'usg_1_norm': 56.5,
      'usg_2': 10.9,
      'usg_2_norm': 54.5,
      'ws_per_48_1': 0.069,
      'ws_per_48_2': 0.099},
     10: {'gp_1': 42,
      'gp_2': 6,
      'per_1': 13.5,
      'per_2': 16.3,
      'player_name': 'Roy Hibbert',
      'team_1': 'CHA',
      'team_2': 'DEN',
      'traders': 'CHA_DEN',
      'usg_1': 14.0,
      'usg_1_norm': 70.0,
      'usg_2': 15.5,
      'usg_2_norm': 77.5,
      'ws_per_48_1': 0.129,
      'ws_per_48_2': 0.11900000000000001},
     11: {'gp_1': 58,
      'gp_2': 23,
      'per_1': 23.9,
      'per_2': 15.4,
      'player_name': 'Lou Williams',
      'team_1': 'LAL',
      'team_2': 'HOU',
      'traders': 'LAL_HOU',
      'usg_1': 30.6,
      'usg_1_norm': 153.0,
      'usg_2': 25.3,
      'usg_2_norm': 126.5,
      'ws_per_48_1': 0.16899999999999998,
      'ws_per_48_2': 0.096},
     12: {'gp_1': 58,
      'gp_2': 24,
      'per_1': 7.5,
      'per_2': 13.3,
      'player_name': 'Corey Brewer',
      'team_1': 'HOU',
      'team_2': 'LAL',
      'traders': 'HOU_LAL',
      'usg_1': 12.9,
      'usg_1_norm': 64.5,
      'usg_2': 18.0,
      'usg_2_norm': 90.0,
      'ws_per_48_1': 0.045,
      'ws_per_48_2': 0.047},
     13: {'gp_1': 53,
      'gp_2': 26,
      'per_1': 15.4,
      'per_2': 13.5,
      'player_name': 'Ersan Ilyasova',
      'team_1': 'PHI',
      'team_2': 'ATL',
      'traders': 'PHI_ATL',
      'usg_1': 23.6,
      'usg_1_norm': 118.0,
      'usg_2': 19.9,
      'usg_2_norm': 99.5,
      'ws_per_48_1': 0.08800000000000001,
      'ws_per_48_2': 0.106},
     14: {'gp_1': 55,
      'gp_2': 26,
      'per_1': 13.0,
      'per_2': 14.7,
      'player_name': 'Bojan Bogdonavic',
      'team_1': 'BKN',
      'team_2': 'WAS',
      'traders': 'BKN_WAS',
      'usg_1': 22.5,
      'usg_1_norm': 112.5,
      'usg_2': 22.2,
      'usg_2_norm': 111.0,
      'ws_per_48_1': 0.052000000000000005,
      'ws_per_48_2': 0.10300000000000001},
     15: {'gp_1': 28,
      'gp_2': 10,
      'per_1': 6.3,
      'per_2': 5.0,
      'player_name': 'Andrew Nicholson',
      'team_1': 'WAS',
      'team_2': 'BKN',
      'traders': 'WAS_BKN',
      'usg_1': 17.6,
      'usg_1_norm': 88.0,
      'usg_2': 15.8,
      'usg_2_norm': 79.0,
      'ws_per_48_1': -0.013999999999999999,
      'ws_per_48_2': -0.03},
     16: {'gp_1': 57,
      'gp_2': 25,
      'per_1': 9.9,
      'per_2': 14.9,
      'player_name': 'Buddy Hield',
      'team_1': 'NOP',
      'team_2': 'SAC',
      'traders': 'NOP_SAC',
      'usg_1': 20.4,
      'usg_1_norm': 102.0,
      'usg_2': 22.9,
      'usg_2_norm': 114.5,
      'ws_per_48_1': 0.02,
      'ws_per_48_2': 0.053},
     17: {'gp_1': 26,
      'gp_2': 14,
      'per_1': 15.8,
      'per_2': 14.9,
      'player_name': 'Tyreke Evans',
      'team_1': 'NOP',
      'team_2': 'SAC',
      'traders': 'NOP_SAC',
      'usg_1': 27.2,
      'usg_1_norm': 136.0,
      'usg_2': 26.0,
      'usg_2_norm': 130.0,
      'ws_per_48_1': 0.053,
      'ws_per_48_2': 0.034},
     18: {'gp_1': 55,
      'gp_2': 19,
      'per_1': 11.2,
      'per_2': 8.0,
      'player_name': 'Langston Galloway',
      'team_1': 'NOP',
      'team_2': 'SAC',
      'traders': 'NOP_SAC',
      'usg_1': 19.5,
      'usg_1_norm': 97.5,
      'usg_2': 15.4,
      'usg_2_norm': 77.0,
      'ws_per_48_1': 0.052000000000000005,
      'ws_per_48_2': 0.006999999999999999},
     19: {'gp_1': 55,
      'gp_2': 17,
      'per_1': 26.5,
      'per_2': 23.2,
      'player_name': 'DeMarcus Cousins',
      'team_1': 'SAC',
      'team_2': 'NOP',
      'traders': 'SAC_NOP',
      'usg_1': 37.5,
      'usg_1_norm': 187.5,
      'usg_2': 33.1,
      'usg_2_norm': 165.5,
      'ws_per_48_1': 0.153,
      'ws_per_48_2': 0.136},
     20: {'gp_1': 56,
      'gp_2': 23,
      'per_1': 17.5,
      'per_2': 13.8,
      'player_name': 'Serge Ibaka',
      'team_1': 'ORL',
      'team_2': 'TOR',
      'traders': 'ORL_TOR',
      'usg_1': 20.8,
      'usg_1_norm': 104.0,
      'usg_2': 20.9,
      'usg_2_norm': 104.5,
      'ws_per_48_1': 0.10800000000000001,
      'ws_per_48_2': 0.085},
     21: {'gp_1': 54,
      'gp_2': 24,
      'per_1': 14.9,
      'per_2': 11.1,
      'player_name': 'Terrence Ross',
      'team_1': 'TOR',
      'team_2': 'ORL',
      'traders': 'TOR_ORL',
      'usg_1': 19.8,
      'usg_1_norm': 99.0,
      'usg_2': 18.5,
      'usg_2_norm': 92.5,
      'ws_per_48_1': 0.11599999999999999,
      'ws_per_48_2': 0.037000000000000005},
     22: {'gp_1': 45,
      'gp_2': 20,
      'per_1': 15.0,
      'per_2': 21.1,
      'player_name': 'Jusuf Nurkic',
      'team_1': 'DEN',
      'team_2': 'POR',
      'traders': 'DEN_POR',
      'usg_1': 22.6,
      'usg_1_norm': 113.0,
      'usg_2': 25.7,
      'usg_2_norm': 128.5,
      'ws_per_48_1': 0.034,
      'ws_per_48_2': 0.11599999999999999},
     23: {'gp_1': 54,
      'gp_2': 27,
      'per_1': 18.9,
      'per_2': 16.3,
      'player_name': 'Mason Plumlee',
      'team_1': 'POR',
      'team_2': 'DEN',
      'traders': 'POR_DEN',
      'usg_1': 18.3,
      'usg_1_norm': 91.5,
      'usg_2': 17.5,
      'usg_2_norm': 87.5,
      'ws_per_48_1': 0.147,
      'ws_per_48_2': 0.107},
     24: {'gp_1': 35,
      'gp_2': 19,
      'per_1': 14.8,
      'per_2': 18.6,
      'player_name': 'Spencer Hawes',
      'team_1': 'CHA',
      'team_2': 'MIL',
      'traders': 'CHA_MIL',
      'usg_1': 19.6,
      'usg_1_norm': 98.0,
      'usg_2': 20.3,
      'usg_2_norm': 101.5,
      'ws_per_48_1': 0.086,
      'ws_per_48_2': 0.16},
     25: {'gp_1': 32,
      'gp_2': 13,
      'per_1': 8.0,
      'per_2': 9.0,
      'player_name': 'Miles Plumlee',
      'team_1': 'MIL',
      'team_2': 'CHA',
      'traders': 'MIL_CHA',
      'usg_1': 15.5,
      'usg_1_norm': 77.5,
      'usg_2': 8.7,
      'usg_2_norm': 43.5,
      'ws_per_48_1': 0.021,
      'ws_per_48_2': 0.087},
     26: {'gp_1': 32,
      'gp_2': 35,
      'per_1': 10.8,
      'per_2': 13.5,
      'player_name': 'Kyle Korver',
      'team_1': 'ATL',
      'team_2': 'CLE',
      'traders': 'ATL_CLE',
      'usg_1': 14.5,
      'usg_1_norm': 72.5,
      'usg_2': 15.8,
      'usg_2_norm': 79.0,
      'ws_per_48_1': 0.08800000000000001,
      'ws_per_48_2': 0.114},
     27: {'gp_1': 23,
      'gp_2': 30,
      'per_1': 7.6,
      'per_2': 11.9,
      'player_name': 'Mike Dunleavy',
      'team_1': 'CLE',
      'team_2': 'ATL',
      'traders': 'CLE_ATL',
      'usg_1': 13.7,
      'usg_1_norm': 68.5,
      'usg_2': 14.3,
      'usg_2_norm': 71.5,
      'ws_per_48_1': 0.040999999999999995,
      'ws_per_48_2': 0.12}}


