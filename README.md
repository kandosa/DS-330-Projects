
# Group 4
## Hai Huang, Kuai Yu, Jiahua Ma

# Goal:
## How to become a top PUBG player

# PUBG
### PlayerUnknown's Battlegrounds (PUBG) is an online multiplayer battle royale game
### Which the last man survive win the game

# The majority of codes are in our jupyter notebook file.
### We put our graphs into webpage: http://personal.psu.edu/jkm5697/DS330/Final%20Project/DS330GraphDisplay.html

# Our goal
### Our goal is to analysis the top players and rest of the players, and trying to help players become top players

# The Data
### Get it from Kaggle Dataset
### webpage: https://www.kaggle.com/skihikingkevin/pubg-match-deaths

# Design
## we use jupyter notebook and python 3.7 to do the most of the code
## The visualization package we use are plotly seaborn and matplotlib

```python
import pandas as pd
import seaborn as sns
import matplotlib as plt
```

# Data concat
## Each csv file is around 2 GB, and agg_match file contain match information
## kill_match files contain information about players'death placement, killed by information


```python
#import and concat data for agg
agg_final = pd.concat([pd.read_csv('agg_match_stats_0.csv'),
                      pd.read_csv('agg_match_stats_1.csv')], ignore_index = True)
```


```python
#import and concat data for deaths
kill_final = pd.concat([pd.read_csv('kill_match_stats_final_0.csv'),
                      pd.read_csv('kill_match_stats_final_1.csv')], ignore_index = True)
```


```python
#Drop nan data from both datasets
agg_final = agg_final.dropna()
kill_final = kill_final.dropna()
```

# Sneak Peak of two datasets


```python
agg_final.head()
```



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>game_size</th>
      <th>match_id</th>
      <th>match_mode</th>
      <th>party_size</th>
      <th>player_assists</th>
      <th>player_dbno</th>
      <th>player_dist_ride</th>
      <th>player_dist_walk</th>
      <th>player_dmg</th>
      <th>player_kills</th>
      <th>player_name</th>
      <th>player_survive_time</th>
      <th>team_id</th>
      <th>team_placement</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2017-11-26T20:59:40+0000</td>
      <td>37</td>
      <td>2U4GBNA0YmnNZYkzjkfgN4ev-hXSrak_BSey_YEG6kIuDG...</td>
      <td>tpp</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>2870.72400</td>
      <td>1784.847780</td>
      <td>117</td>
      <td>1</td>
      <td>SnuffIes</td>
      <td>1106.320</td>
      <td>4</td>
      <td>18</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017-11-26T20:59:40+0000</td>
      <td>37</td>
      <td>2U4GBNA0YmnNZYkzjkfgN4ev-hXSrak_BSey_YEG6kIuDG...</td>
      <td>tpp</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>2938.40723</td>
      <td>1756.079710</td>
      <td>127</td>
      <td>1</td>
      <td>Ozon3r</td>
      <td>1106.315</td>
      <td>4</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2017-11-26T20:59:40+0000</td>
      <td>37</td>
      <td>2U4GBNA0YmnNZYkzjkfgN4ev-hXSrak_BSey_YEG6kIuDG...</td>
      <td>tpp</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0.00000</td>
      <td>224.157562</td>
      <td>67</td>
      <td>0</td>
      <td>bovize</td>
      <td>235.558</td>
      <td>5</td>
      <td>33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2017-11-26T20:59:40+0000</td>
      <td>37</td>
      <td>2U4GBNA0YmnNZYkzjkfgN4ev-hXSrak_BSey_YEG6kIuDG...</td>
      <td>tpp</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0.00000</td>
      <td>92.935150</td>
      <td>0</td>
      <td>0</td>
      <td>sbahn87</td>
      <td>197.553</td>
      <td>5</td>
      <td>33</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2017-11-26T20:59:40+0000</td>
      <td>37</td>
      <td>2U4GBNA0YmnNZYkzjkfgN4ev-hXSrak_BSey_YEG6kIuDG...</td>
      <td>tpp</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>2619.07739</td>
      <td>2510.447000</td>
      <td>175</td>
      <td>2</td>
      <td>GeminiZZZ</td>
      <td>1537.495</td>
      <td>14</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
kill_final.head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>killed_by</th>
      <th>killer_name</th>
      <th>killer_placement</th>
      <th>killer_position_x</th>
      <th>killer_position_y</th>
      <th>map</th>
      <th>match_id</th>
      <th>time</th>
      <th>victim_name</th>
      <th>victim_placement</th>
      <th>victim_position_x</th>
      <th>victim_position_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Grenade</td>
      <td>KrazyPortuguese</td>
      <td>5.0</td>
      <td>657725.10</td>
      <td>146275.2</td>
      <td>MIRAMAR</td>
      <td>2U4GBNA0YmnLSqvEycnTjo-KT000vfUnhSA2vfVhVPe1QB...</td>
      <td>823</td>
      <td>KrazyPortuguese</td>
      <td>5.0</td>
      <td>657725.10</td>
      <td>146275.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SCAR-L</td>
      <td>nide2Bxiaojiejie</td>
      <td>31.0</td>
      <td>93091.37</td>
      <td>722236.4</td>
      <td>MIRAMAR</td>
      <td>2U4GBNA0YmnLSqvEycnTjo-KT000vfUnhSA2vfVhVPe1QB...</td>
      <td>194</td>
      <td>X3evolution</td>
      <td>33.0</td>
      <td>92238.68</td>
      <td>723375.1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S686</td>
      <td>Ascholes</td>
      <td>43.0</td>
      <td>366921.40</td>
      <td>421623.9</td>
      <td>MIRAMAR</td>
      <td>2U4GBNA0YmnLSqvEycnTjo-KT000vfUnhSA2vfVhVPe1QB...</td>
      <td>103</td>
      <td>CtrlZee</td>
      <td>46.0</td>
      <td>367304.50</td>
      <td>421216.1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Down and Out</td>
      <td>Weirdo7777</td>
      <td>9.0</td>
      <td>472014.20</td>
      <td>313274.8</td>
      <td>MIRAMAR</td>
      <td>2U4GBNA0YmnLSqvEycnTjo-KT000vfUnhSA2vfVhVPe1QB...</td>
      <td>1018</td>
      <td>BlackDpre</td>
      <td>13.0</td>
      <td>476645.90</td>
      <td>316758.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M416</td>
      <td>Solayuki1</td>
      <td>9.0</td>
      <td>473357.80</td>
      <td>318340.5</td>
      <td>MIRAMAR</td>
      <td>2U4GBNA0YmnLSqvEycnTjo-KT000vfUnhSA2vfVhVPe1QB...</td>
      <td>1018</td>
      <td>Vjolt</td>
      <td>13.0</td>
      <td>473588.50</td>
      <td>318418.8</td>
    </tr>
  </tbody>
</table>
</div>



# Column features and shapes for both datasets


```python
agg_final.columns
```




    Index(['date', 'game_size', 'match_id', 'match_mode', 'party_size',
           'player_assists', 'player_dbno', 'player_dist_ride', 'player_dist_walk',
           'player_dmg', 'player_kills', 'player_name', 'player_survive_time',
           'team_id', 'team_placement'],
          dtype='object')




```python
kill_final.columns
```




    Index(['killed_by', 'killer_name', 'killer_placement', 'killer_position_x',
           'killer_position_y', 'map', 'match_id', 'time', 'victim_name',
           'victim_placement', 'victim_position_x', 'victim_position_y'],
          dtype='object')




```python
agg_final.shape
```




    (27653247, 15)




```python
kill_final.shape
```




    (24217596, 12)



# Data Preprocess:
## Match id is the only feature which are in both datasets and there are at most 100 same match id in the datasets. We have to group data by match id and some features we define the data such as party size and map

# Subset of data
## find each party size's match id
## find each map's match id


```python
solo_matches = agg_final.loc[agg_final['party_size'] == 1,'match_id'].drop_duplicates()
duo_matches = agg_final.loc[agg_final['party_size'] == 2,'match_id'].drop_duplicates()
squad_matches = agg_final.loc[agg_final['party_size'] == 4,'match_id'].drop_duplicates()
```


```python
er_matches = kill_final.loc[kill_final['map'] == 'ERANGEL','match_id'].drop_duplicates()
mr_matches = kill_final.loc[kill_final['map'] == 'MIRAMAR','match_id'].drop_duplicates()
```

# Match the match id from two dataset and create subsets of data. Here we have two maps of solo, duo and squad subsets of data


```python
er_agg = agg_final[agg_final['match_id'].isin(er_matches.values)]
top_solo_era = er_agg[(er_agg['party_size'] == 1) & (er_agg['team_placement'] < 6)]
top_duo_era = er_agg[(er_agg['party_size'] == 2) & (er_agg['team_placement'] < 6)]
top_squad_era = er_agg[(er_agg['party_size'] == 4) & (er_agg['team_placement'] < 6)]

rest_solo_era = er_agg[(er_agg['party_size'] == 1) & (er_agg['team_placement'] > 6)]
rest_duo_era = er_agg[(er_agg['party_size'] == 2) & (er_agg['team_placement'] > 6)]
rest_squad_era = er_agg[(er_agg['party_size'] == 4) & (er_agg['team_placement'] > 6)]
```


```python
mr_agg = agg_final[agg_final['match_id'].isin(mr_matches.values)]
top_solo_mir = mr_agg[(mr_agg['party_size'] == 1) & (mr_agg['team_placement'] < 6)]
top_duo_mir = mr_agg[(mr_agg['party_size'] == 2) & (mr_agg['team_placement'] < 6)]
top_squad_mir = mr_agg[(mr_agg['party_size'] == 4) & (mr_agg['team_placement'] < 6)]

rest_solo_mir = mr_agg[(mr_agg['party_size'] == 1) & (mr_agg['team_placement'] > 6)]
rest_duo_mir = mr_agg[(mr_agg['party_size'] == 2) & (mr_agg['team_placement'] > 6)]
rest_squad_mir = mr_agg[(mr_agg['party_size'] == 4) & (mr_agg['team_placement'] > 6)]
```

# Number of solo duo squad matches in two different maps


```python
print('Number of solo queue matches: %i' % len(solo_matches))
solo_deaths = kill_final[kill_final['match_id'].isin(solo_matches.values)]
deaths_solo_er = solo_deaths[solo_deaths['map'] == 'ERANGEL']
deaths_solo_mr = solo_deaths[solo_deaths['map'] == 'MIRAMAR']
print('  Number of Erangel solo matches: %i' % len(deaths_solo_er.groupby('match_id').first()))
print('  Number of Miramar solo matches: %i' % len(deaths_solo_mr.groupby('match_id').first()))
```

    Number of solo queue matches: 62702
      Number of Erangel solo matches: 51548
      Number of Miramar solo matches: 10090
    


```python
print('Number of duo queue matches: %i' % len(duo_matches))
duo_deaths = kill_final[kill_final['match_id'].isin(duo_matches.values)]
deaths_duo_er = duo_deaths[duo_deaths['map'] == 'ERANGEL']
deaths_duo_mr = duo_deaths[duo_deaths['map'] == 'MIRAMAR']
print('  Number of Erangel duo matches: %i' % len(deaths_duo_er.groupby('match_id').first()))
print('  Number of Miramar duo matches: %i' % len(deaths_duo_mr.groupby('match_id').first()))
```

    Number of duo queue matches: 95727
      Number of Erangel duo matches: 76717
      Number of Miramar duo matches: 16569
    


```python
print('Number of squad queue matches: %i' % len(squad_matches))
squad_deaths = kill_final[kill_final['match_id'].isin(squad_matches.values)]
deaths_squad_er = squad_deaths[squad_deaths['map'] == 'ERANGEL']
deaths_squad_mr = squad_deaths[squad_deaths['map'] == 'MIRAMAR']
print('  Number of Erangel squad matches: %i' % len(deaths_squad_er.groupby('match_id').first()))
print('  Number of Miramar squad matches: %i' % len(deaths_squad_mr.groupby('match_id').first()))
```

    Number of squad queue matches: 141555
      Number of Erangel squad matches: 109757
      Number of Miramar squad matches: 28460







```python

```
