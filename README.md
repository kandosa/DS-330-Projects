
# Group 4
## Hai Huang, Kuai Yu, Jiahua Ma
## How to become a top PUBG player

# PUBG
### PlayerUnknown's Battlegrounds (PUBG) is an online multiplayer battle royale game
### Which the last man survive win the game

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
## Match id is the only feature which are in both datasets

## and there are at most 100 same match id in the datasets

## we have to group data by match id and some features we define the data such as party size and map

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

# Match the match id from two dataset and create subsets of data 
## Here we have two maps of solo, duo and squad subsets of data


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
import gc
gc.collect()
```




    7




```python
#top 5 erangle stats
#top_solo_era = era.loc[(era['party_size'] ==1) & (era['team_placement'] < 6)]
#top_duo_era = era.loc[(era['party_size'] ==2) & (era['team_placement'] < 6)]
#top_squad_era = era.loc[(era['party_size'] ==4) & (era['team_placement'] < 6)]

#top 5 solo erangle stats
topsolokill_era = top_solo_era['player_kills'].mean()

topsolodbno_era = top_solo_era['player_dbno'].mean()

topsolodmg_era = top_solo_era['player_dmg'].mean()

topsolosurvive_era = top_solo_era['player_survive_time'].mean() / 60

#top 5 duo erangle stats
topduokill_era = top_duo_era['player_kills'].mean()

topduodbno_era = top_duo_era['player_dbno'].mean()

topduodmg_era = top_duo_era['player_dmg'].mean()

topduosurvive_era = top_duo_era['player_survive_time'].mean() / 60

#top 5 squad erangle stats
topsquadkill_era = top_squad_era['player_kills'].mean()

topsquaddbno_era = top_squad_era['player_dbno'].mean()

topsquaddmg_era = top_squad_era['player_dmg'].mean()

topsquadsurvive_era = top_squad_era['player_survive_time'].mean() /60

```


```python
#rest erangle stats
#rest_solo_era = era.loc[(era['party_size'] ==1) & (era['team_placement'] > 5)]
#rest_duo_era = era.loc[(era['party_size'] ==2) & (era['team_placement'] > 5)]
#rest_squad_era = era.loc[(era['party_size'] ==4) & (era['team_placement'] > 5)]

#rest solo erangle stats
restsolokill_era = rest_solo_era['player_kills'].mean()

restsolodbno_era = rest_solo_era['player_dbno'].mean()

restsolodmg_era = rest_solo_era['player_dmg'].mean()

rest_solo_era.drop(rest_solo_era.loc[rest_solo_era['player_survive_time'] > 3600].index, inplace=True)

restsolosurvive_era = rest_solo_era['player_survive_time'].mean() / 60

#rest duo erangle stats
restduokill_era = rest_duo_era['player_kills'].mean()

restduodbno_era = rest_duo_era['player_dbno'].mean()

restduodmg_era = rest_duo_era['player_dmg'].mean()

restduosurvive_era = rest_duo_era['player_survive_time'].mean() / 60

#rest squad erangle stats
restsquadkill_era = rest_squad_era['player_kills'].mean()

restsquaddbno_era = rest_squad_era['player_dbno'].mean()

restsquaddmg_era = rest_squad_era['player_dmg'].mean()

restsquadsurvive_era = rest_squad_era['player_survive_time'].mean() / 60
```

    c:\users\kando\appdata\local\programs\python\python37\lib\site-packages\pandas\core\frame.py:3697: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      errors=errors)
    


```python
#top 5 MIRAMAR stats
#top_solo_mir = mir.loc[(mir['party_size'] ==1) & (mir['team_placement'] < 6)]
#top_duo_mir = mir.loc[(mir['party_size'] ==2) & (mir['team_placement'] < 6)]
#top_squad_mir = mir.loc[(mir['party_size'] ==4) & (mir['team_placement'] < 6)]

#top 5 solo MIRAMAR stats
topsolokill_mir = top_solo_mir['player_kills'].mean()

topsolodbno_mir = top_solo_mir['player_dbno'].mean()

topsolodmg_mir = top_solo_mir['player_dmg'].mean()

topsolosurvive_mir = top_solo_mir['player_survive_time'].mean() / 60

#top 5 duo MIRAMAR stats
topduokill_mir = top_duo_mir['player_kills'].mean()

topduodbno_mir = top_duo_mir['player_dbno'].mean()

topduodmg_mir = top_duo_mir['player_dmg'].mean()

topduosurvive_mir = top_duo_mir['player_survive_time'].mean() / 60

#top 5 squad MIRAMAR stats
topsquadkill_mir = top_squad_mir['player_kills'].mean()

topsquaddbno_mir = top_squad_mir['player_dbno'].mean()

topsquaddmg_mir = top_squad_mir['player_dmg'].mean()

topsquadsurvive_mir = top_squad_mir['player_survive_time'].mean() / 60
```


```python
#rest MIRAMAR stats
#rest_solo_mir = mir.loc[(mir['party_size'] ==1) & (mir['team_placement'] > 5)]
#rest_duo_mir = mir.loc[(mir['party_size'] ==2) & (mir['team_placement'] > 5)]
#rest_squad_mir = mir.loc[(mir['party_size'] ==4) & (mir['team_placement'] > 5)]

#rest solo MIRAMAR stats
restsolokill_mir = rest_solo_mir['player_kills'].mean()

restsolodbno_mir = rest_solo_mir['player_dbno'].mean()

restsolodmg_mir = rest_solo_mir['player_dmg'].mean()

restsolosurvive_mir = rest_solo_mir['player_survive_time'].mean() / 60

#rest duo MIRAMAR stats
restduokill_mir = rest_duo_mir['player_kills'].mean()

restduodbno_mir = rest_duo_mir['player_dbno'].mean()

restduodmg_mir = rest_duo_mir['player_dmg'].mean()

restduosurvive_mir = rest_duo_mir['player_survive_time'].mean() / 60

#rest squad MIRAMAR stats
restsquadkill_mir = rest_squad_mir['player_kills'].mean()

restsquaddbno_mir = rest_squad_mir['player_dbno'].mean()

restsquaddmg_mir = rest_squad_mir['player_dmg'].mean()

restsquadsurvive_mir = rest_squad_mir['player_survive_time'].mean() / 60
```


```python
import plotly.graph_objs as go
from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
init_notebook_mode(connected=True)
import numpy as np
```


<script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script><script type="text/javascript">if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script><script>requirejs.config({paths: { 'plotly': ['https://cdn.plot.ly/plotly-latest.min']},});if(!window._Plotly) {require(['plotly'],function(plotly) {window._Plotly=plotly;});}</script>



```python
#making barchart for erangle top&rest solo
trace1 = go.Bar(
    x=['kills', 'damage', 'number of player knocked', 'survive time'],
    y=[topsolokill_era, topsolodmg_era, topsolodbno_era, topsolosurvive_era],
    name='erangle top 5 solo'
)
trace2 = go.Bar(
    x=['kills', 'damage', 'number of player knocked', 'survive time'],
    y=[restsolokill_era,restsolodmg_era,restsolodbno_era,restsolosurvive_era],
    name='erangle solo'
)

data = [trace1, trace2]
layout = go.Layout(
    barmode='group'
)

# Stats of top 5 players and rest of the players in different map and different party size


```python
layout = dict(title='Maps gamemode stats difference', showlegend=False,
              updatemenus=updatemenus)
figmenu = dict(data=menu_data, layout=layout)
iplot(figmenu, filename='update_button')
```


<div id="174ee27a-3e07-4e8f-8f74-3bf661ffe2c6" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("174ee27a-3e07-4e8f-8f74-3bf661ffe2c6", [{"name": "erangle top 5 solo", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [3.802782677772456, 411.6037468409637, 0.0, 30.432280752691106], "type": "bar", "uid": "9b451a33-5f3b-4e27-8e78-f2e7f234533f"}, {"name": "erangle solo", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [0.716783078248595, 93.67493624280584, 0.0, 10.868979388918422], "type": "bar", "uid": "4003426f-985b-4846-8e10-2a3913cfe15c"}, {"name": "erangle top 5 duo", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [2.6192155389195597, 314.99098581852866, 1.3590786904802572, 28.350402664226458], "type": "bar", "uid": "f7394c37-a974-4325-a39f-c15d5c472bf7"}, {"name": "erangle duo", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [0.6731185238244994, 99.803011322684, 0.48505195416077895, 10.618317072441641], "type": "bar", "uid": "f85f4ebc-b74e-429e-b492-9cf3aaddd440"}, {"name": "erangle top 5 squad", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [1.8122685086127257, 237.4238649150055, 1.456481758843682, 25.731036183063523], "type": "bar", "uid": "41da4f45-23da-46a5-b7d4-faadbd64af45"}, {"name": "erangle squad", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [0.6270023838948171, 99.18477304960088, 0.6759187935947286, 10.510012288918656], "type": "bar", "uid": "dae0d7cf-3156-4d24-b63f-7ad7b700c646"}, {"name": "miramar top 5 solo", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [3.5243644911945173, 389.93429755358994, 0.0, 30.13379038203575], "type": "bar", "uid": "ca6a2564-10a3-41bb-bc70-b81c642f18dc"}, {"name": "miramar solo", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [0.7068443183421989, 93.57659373068519, 0.0, 10.506427771295566], "type": "bar", "uid": "8cc8e00b-a0bc-45ce-a22f-019429b4ef5f"}, {"name": "miramar top 5 duo", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [2.4238038066100693, 298.8477477861814, 1.2745196028839063, 27.9620662412722], "type": "bar", "uid": "5690ae1c-50d5-4f63-a1bc-b29c5a1e7fe9"}, {"name": "miramar duo", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [0.6668025312039079, 99.23229584635148, 0.487672275704566, 10.218438167005008], "type": "bar", "uid": "9bc20cbc-7f7a-4daf-8e7b-f18b588804d4"}, {"name": "miramar top 5 squad", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [1.6860280116868656, 226.5049031215203, 1.372112774730382, 25.296235932415005], "type": "bar", "uid": "e135005d-b659-49f7-b601-5855dc8652a8"}, {"name": "miramar squad", "x": ["kills", "damage", "number of player knocked", "survive time"], "y": [0.6338528462869127, 99.15335352028052, 0.6849492403433165, 9.908132335634281], "type": "bar", "uid": "e7f67206-8a45-4a67-b238-fa39ab4ead23"}], {"showlegend": false, "title": "Maps gamemode stats difference", "updatemenus": [{"active": -1, "buttons": [{"args": [{"visible": [true, true, false, false, false, false, false, false, false, false, false, false]}, {"title": "Erangle solo stats comparison"}], "label": "Erangle solo", "method": "update"}, {"args": [{"visible": [false, false, true, true, false, false, false, false, false, false, false, false]}, {"title": "Erangle solo stats comparison"}], "label": "Erangle duo", "method": "update"}, {"args": [{"visible": [false, false, false, false, true, true, false, false, false, false, false, false]}, {"title": "Erangle solo stats comparison"}], "label": "Erangle squad", "method": "update"}, {"args": [{"visible": [false, false, false, false, false, false, true, true, false, false, false, false]}, {"title": "Miramar solo stats comparison"}], "label": "Miramar solo", "method": "update"}, {"args": [{"visible": [false, false, false, false, false, false, false, false, true, true, false, false]}, {"title": "Miramar duo stats comparison"}], "label": "Miramar duo", "method": "update"}, {"args": [{"visible": [false, false, false, false, false, false, false, false, false, false, true, true]}, {"title": "Miramar squad stats comparison"}], "label": "Miramar squad", "method": "update"}], "type": "buttons"}]}, {"showLink": true, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly"})});</script><script type="text/javascript">window.addEventListener("resize", function(){window._Plotly.Plots.resize(document.getElementById("174ee27a-3e07-4e8f-8f74-3bf661ffe2c6"));});</script>


# Top 20 weapons usage between top players and rest of the players


```python
waysOfDeath = kill_final['killed_by'].unique()
nwaysOfDeath = kill_final['killed_by'].nunique()
rank = 20
nwaysOfDeath
```




    56




```python
#deaths matches on maps and different party size
deaths_tsolo_mr = deaths_solo_mr[deaths_solo_mr['killer_placement'] < 6]
deaths_tsolo_er = deaths_solo_er[deaths_solo_er['killer_placement'] < 6]
deaths_tduo_mr = deaths_duo_mr[deaths_duo_mr['killer_placement'] < 6]
deaths_tduo_er = deaths_duo_er[deaths_duo_er['killer_placement'] < 6]
deaths_tsquad_mr = deaths_squad_mr[deaths_squad_mr['killer_placement'] < 6]
deaths_tsquad_er = deaths_squad_er[deaths_squad_er['killer_placement'] < 6]

deaths_rsolo_mr = deaths_solo_mr[deaths_solo_mr['killer_placement'] > 5]
deaths_rsolo_er = deaths_solo_er[deaths_solo_er['killer_placement'] > 5]
deaths_rduo_mr = deaths_duo_mr[deaths_duo_mr['killer_placement'] > 5]
deaths_rduo_er = deaths_duo_er[deaths_duo_er['killer_placement'] > 5]
deaths_rsquad_mr = deaths_squad_mr[deaths_squad_mr['killer_placement'] > 5]
deaths_rsquad_er = deaths_squad_er[deaths_squad_er['killer_placement'] > 5]

#count deaths by each map and each party size
dtopsolocount_mir = deaths_tsolo_mr['killed_by'].value_counts()
dtopsolocount_era = deaths_tsolo_er['killed_by'].value_counts()
dtopduocount_mir = deaths_tduo_mr['killed_by'].value_counts()
dtopduocount_era = deaths_tduo_er['killed_by'].value_counts()
dtopsquadcount_mir = deaths_tsquad_mr['killed_by'].value_counts()
dtopsquadcount_era = deaths_tsquad_er['killed_by'].value_counts()

drestsolocount_mir = deaths_rsolo_mr['killed_by'].value_counts()
drestsolocount_era = deaths_rsolo_er['killed_by'].value_counts()
drestduocount_mir = deaths_rduo_mr['killed_by'].value_counts()
drestduocount_era = deaths_rduo_er['killed_by'].value_counts()
drestsquadcount_mir = deaths_rsquad_mr['killed_by'].value_counts()
drestsquadcount_era = deaths_rsquad_er['killed_by'].value_counts()
```


```python
#top solo mir weapons usage and counts
tsmlabels = [dtopsolocount_mir[:rank].index]
tsmvalues = [dtopsolocount_mir[:rank].values]
tsmlabels
```




    [Index(['M416', 'SCAR-L', 'M16A4', 'AKM', 'Kar98k', 'SKS', 'UMP9', 'Mini 14',
            'Grenade', 'M24', 'AWM', 'S1897', 'AUG', 'Micro UZI', 'S12K', 'S686',
            'Groza', 'Hit by Car', 'Tommy Gun', 'Punch'],
           dtype='object')]




```python
tsmvalues
```




    [array([47756, 32208, 17047, 16487, 13128,  7799,  7307,  6545,  3212,
             2697,  2467,  2219,  2035,  2025,  1659,  1643,  1510,  1393,
             1084,  1069], dtype=int64)]




```python
#top solo era weapons usage and counts
tselabels = [dtopsolocount_era[:rank].index]
tsevalues = [dtopsolocount_era[:rank].values]
tselabels
```




    [Index(['M416', 'SCAR-L', 'M16A4', 'AKM', 'Kar98k', 'Mini 14', 'UMP9', 'SKS',
            'S1897', 'Grenade', 'M24', 'Hit by Car', 'S12K', 'Groza', 'S686',
            'Micro UZI', 'AWM', 'M249', 'Tommy Gun', 'Punch'],
           dtype='object')]




```python
tsevalues
```




    [array([217515, 161618, 118504, 102224,  67875,  50639,  43826,  36998,
             18517,  16278,  13629,  13391,  12592,  12232,  12162,  11961,
             11146,   7454,   7157,   7070], dtype=int64)]




```python
#top duo mir weapons usage and counts
tdmlabels = [dtopduocount_mir[:rank].index]
tdmvalues = [dtopduocount_mir[:rank].values]
tdmlabels
```




    [Index(['Down and Out', 'M416', 'SCAR-L', 'M16A4', 'AKM', 'Kar98k', 'UMP9',
            'SKS', 'Mini 14', 'Grenade', 'S1897', 'Micro UZI', 'M24', 'S686',
            'S12K', 'Punch', 'Bluezone', 'AUG', 'AWM', 'Tommy Gun'],
           dtype='object')]




```python
tdmvalues
```




    [array([93864, 75326, 53104, 30896, 30853, 18705, 14559, 13752, 12126,
             7120,  4883,  3633,  3392,  3320,  3149,  2997,  2934,  2724,
             2549,  2313], dtype=int64)]




```python
#top duo era weapons usage and counts
tdelabels = [dtopduocount_era[:rank].index]
tdevalues = [dtopduocount_era[:rank].values]
tdelabels
```




    [Index(['Down and Out', 'M416', 'SCAR-L', 'M16A4', 'AKM', 'Kar98k', 'Mini 14',
            'UMP9', 'SKS', 'Grenade', 'S1897', 'S686', 'S12K', 'Micro UZI', 'Punch',
            'Bluezone', 'Groza', 'M24', 'Tommy Gun', 'AWM'],
           dtype='object')]




```python
tdevalues
```




    [array([474015, 307733, 238174, 187236, 161880,  92873,  88778,  77702,
             61099,  35630,  34870,  21588,  21396,  19871,  17698,  15594,
             15573,  15085,  13026,  11121], dtype=int64)]




```python
#top squad mir weapons usage and counts
tsqmlabels = [dtopsquadcount_mir[:rank].index]
tsqmvalues = [dtopsquadcount_mir[:rank].values]
tsqmlabels
```




    [Index(['Down and Out', 'M416', 'SCAR-L', 'AKM', 'M16A4', 'UMP9', 'Kar98k',
            'SKS', 'Mini 14', 'Grenade', 'Bluezone', 'S1897', 'Punch', 'Micro UZI',
            'S686', 'S12K', 'Win94', 'Tommy Gun', 'Pickup Truck', 'M24'],
           dtype='object')]




```python
tsqmvalues
```




    [array([213359, 140152, 102074,  66375,  65734,  34213,  33353,  27503,
             23762,  13583,  12578,  12542,   9987,   8264,   7932,   7921,
              5608,   5308,   5226,   4954], dtype=int64)]




```python
#top squad era weapons usage and counts
tsqelabels = [dtopsquadcount_era[:rank].index]
tsqevalues = [dtopsquadcount_era[:rank].values]
tsqelabels
```




    [Index(['Down and Out', 'M416', 'SCAR-L', 'M16A4', 'AKM', 'Mini 14', 'UMP9',
            'Kar98k', 'SKS', 'S1897', 'Grenade', 'Bluezone', 'Punch', 'S686',
            'S12K', 'Micro UZI', 'Tommy Gun', 'Uaz', 'Groza', 'M24'],
           dtype='object')]




```python
tsqevalues
```




    [array([907634, 472601, 375299, 323583, 279922, 146238, 142158, 138467,
            101818,  69076,  58348,  56743,  50201,  40333,  39381,  36414,
             24008,  22684,  20005,  18772], dtype=int64)]




```python
#rest solo mir weapons usage and counts
rsmlabels = [drestsolocount_mir[:rank].index]
rsmvalues = [drestsolocount_mir[:rank].values]
rsmlabels
```




    [Index(['M416', 'SCAR-L', 'M16A4', 'AKM', 'UMP9', 'S1897', 'S686', 'Micro UZI',
            'SKS', 'Mini 14', 'S12K', 'Kar98k', 'Punch', 'Hit by Car', 'P92',
            'Tommy Gun', 'P1911', 'Grenade', 'Win94', 'death.WeapSawnoff_C'],
           dtype='object')]




```python
rsmvalues
```




    [array([93974, 75471, 72418, 68893, 47115, 30839, 20341, 19734, 18943,
            18847, 18277, 18230, 16197, 15361, 12847, 11254, 10468, 10193,
             9688,  5781], dtype=int64)]




```python
#rest solo era weapons usage and counts
rselabels = [drestsolocount_era[:rank].index]
rsevalues = [drestsolocount_era[:rank].values]
rselabels
```




    [Index(['M416', 'M16A4', 'AKM', 'SCAR-L', 'UMP9', 'S1897', 'S686', 'Punch',
            'Mini 14', 'S12K', 'Micro UZI', 'Kar98k', 'SKS', 'Hit by Car', 'P92',
            'P1911', 'Tommy Gun', 'Grenade', 'Vector', 'Falling'],
           dtype='object')]




```python
rsevalues
```




    [array([437134, 422328, 379210, 368276, 252111, 219649, 133063, 127039,
            124171, 117021, 109851,  93205,  89103,  77145,  73646,  72934,
             63461,  48887,  28010,  25945], dtype=int64)]




```python
#rest duo mir weapons usage and counts
rdmlabels = [drestduocount_mir[:rank].index]
rdmvalues = [drestduocount_mir[:rank].values]
rdmlabels
```




    [Index(['Down and Out', 'M416', 'SCAR-L', 'M16A4', 'AKM', 'UMP9', 'S1897',
            'S686', 'Punch', 'S12K', 'Micro UZI', 'Mini 14', 'SKS', 'Grenade',
            'Kar98k', 'Tommy Gun', 'Win94', 'P92', 'P1911', 'Hit by Car'],
           dtype='object')]




```python
rdmvalues
```




    [array([176536, 103075,  84314,  83178,  82335,  60025,  41313,  25558,
             25073,  23850,  22361,  20865,  20683,  20355,  16003,  14000,
             12906,  12753,  11083,   9341], dtype=int64)]




```python
#rest duo era weapons usage and counts
rdelabels = [drestduocount_era[:rank].index]
rdevalues = [drestduocount_era[:rank].values]
rdelabels
```




    [Index(['Down and Out', 'M416', 'M16A4', 'AKM', 'SCAR-L', 'UMP9', 'S1897',
            'Punch', 'S686', 'S12K', 'Mini 14', 'Micro UZI', 'SKS', 'Grenade',
            'Kar98k', 'Tommy Gun', 'P1911', 'P92', 'Hit by Car', 'Vector'],
           dtype='object')]




```python
rdevalues
```




    [array([884693, 437930, 437327, 407342, 377983, 292610, 264868, 152123,
            149785, 133135, 129616, 114622,  91949,  91272,  78790,  74532,
             70345,  66375,  37798,  32399], dtype=int64)]




```python
#rest squad mir weapons usage and counts
rsqmlabels = [drestsquadcount_mir[:rank].index]
rsqmvalues = [drestsquadcount_mir[:rank].values]
rsqmlabels
```




    [Index(['Down and Out', 'M416', 'AKM', 'M16A4', 'SCAR-L', 'UMP9', 'S1897',
            'Punch', 'S686', 'S12K', 'Micro UZI', 'Grenade', 'Mini 14', 'SKS',
            'Tommy Gun', 'Kar98k', 'Win94', 'P92', 'P1911', 'Hit by Car'],
           dtype='object')]




```python
rsqmvalues
```




    [array([247614, 150344, 128970, 127097, 125073,  96352,  69577,  50693,
             41379,  39280,  34309,  32156,  30254,  30113,  22974,  22120,
             21640,  18709,  16579,  12409], dtype=int64)]




```python
##rest squad era weapons usage and counts
rsqelabels = [drestsquadcount_era[:rank].index]
rsqevalues = [drestsquadcount_era[:rank].values]
rsqelabels
```




    [Index(['Down and Out', 'M16A4', 'M416', 'AKM', 'SCAR-L', 'UMP9', 'S1897',
            'Punch', 'S686', 'S12K', 'Mini 14', 'Micro UZI', 'Grenade', 'SKS',
            'Tommy Gun', 'Kar98k', 'P1911', 'P92', 'Hit by Car', 'Vector'],
           dtype='object')]




```python
rsqevalues
```




    [array([1002043,  532945,  510026,  506331,  442624,  375390,  347723,
             234019,  188828,  167546,  147983,  142569,  117836,  106565,
              92435,   86043,   83043,   77622,   41951,   38464], dtype=int64)]




```python
# Miramar Solo donut chart

fig1 = {
  "data": [
    {
      "values": [47756, 32208, 17047, 16487, 13128,  7799,  7307,  6545,  3212,
         2697,  2467,  2219,  2035,  2025,  1659,  1643,  1510,  1393,
         1084,  1069],
      "labels": [
'M416', 'SCAR-L', 'M16A4', 'AKM', 'Kar98k', 'SKS', 'UMP9', 'Mini 14',
        'Grenade', 'M24', 'AWM', 'S1897', 'AUG', 'Micro UZI', 'S12K', 'S686',
        'Groza', 'Hit by Car', 'Tommy Gun', 'Punch'
      ],
      "domain": {"x": [0, .48]},
      "name": "Miramar Solo Top 5",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    },
    {
      "values": [93974, 75471, 72418, 68893, 47115, 30839, 20341, 19734, 18943,
        18847, 18277, 18230, 16197, 15361, 12847, 11254, 10468, 10193,
         9688,  5781],
      "labels": [
      'M416', 'SCAR-L', 'M16A4', 'AKM', 'UMP9', 'S1897', 'S686', 'Micro UZI',
        'SKS', 'Mini 14', 'S12K', 'Kar98k', 'Punch', 'Hit by Car', 'P92',
        'Tommy Gun', 'P1911', 'Grenade', 'Win94', 'death.WeapSawnoff_C'
      ],
      "textposition":"inside",
      "domain": {"x": [.52, 1]},
      "name": "Miramar Solo Rest",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    }],
  "layout": {
        "title":"Miramar Solo Queue Ways of Death",
        "annotations": [
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Top5",
                "x": 0.20,
                "y": 0.5
            },
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Rest",
                "x": 0.8,
                "y": 0.5
            }
        ]
    }
}

```


```python
iplot(fig1, filename='donut6')
```


<div id="4a67d161-21d8-421a-8b6b-839cf383d44c" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("4a67d161-21d8-421a-8b6b-839cf383d44c", [{"domain": {"x": [0, 0.48]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["M416", "SCAR-L", "M16A4", "AKM", "Kar98k", "SKS", "UMP9", "Mini 14", "Grenade", "M24", "AWM", "S1897", "AUG", "Micro UZI", "S12K", "S686", "Groza", "Hit by Car", "Tommy Gun", "Punch"], "name": "Miramar Solo Top 5", "values": [47756, 32208, 17047, 16487, 13128, 7799, 7307, 6545, 3212, 2697, 2467, 2219, 2035, 2025, 1659, 1643, 1510, 1393, 1084, 1069], "type": "pie", "uid": "d3be850b-6699-4291-b551-9e00335e1a2c"}, {"domain": {"x": [0.52, 1]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["M416", "SCAR-L", "M16A4", "AKM", "UMP9", "S1897", "S686", "Micro UZI", "SKS", "Mini 14", "S12K", "Kar98k", "Punch", "Hit by Car", "P92", "Tommy Gun", "P1911", "Grenade", "Win94", "death.WeapSawnoff_C"], "name": "Miramar Solo Rest", "textposition": "inside", "values": [93974, 75471, 72418, 68893, 47115, 30839, 20341, 19734, 18943, 18847, 18277, 18230, 16197, 15361, 12847, 11254, 10468, 10193, 9688, 5781], "type": "pie", "uid": "3c99c0fc-d0c4-4202-bff9-2cbd5812181f"}], {"annotations": [{"font": {"size": 20}, "showarrow": false, "text": "Top5", "x": 0.2, "y": 0.5}, {"font": {"size": 20}, "showarrow": false, "text": "Rest", "x": 0.8, "y": 0.5}], "title": "Miramar Solo Queue Ways of Death"}, {"showLink": true, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly"})});</script><script type="text/javascript">window.addEventListener("resize", function(){window._Plotly.Plots.resize(document.getElementById("4a67d161-21d8-421a-8b6b-839cf383d44c"));});</script>



```python
# Miramar Duo donut chart

fig2 = {
  "data": [
    {
      "values": [93864, 75326, 53104, 30896, 30853, 18705, 14559, 13752, 12126,
         7120,  4883,  3633,  3392,  3320,  3149,  2997,  2934,  2724,
         2549,  2313],
      "labels": [
'Down and Out', 'M416', 'SCAR-L', 'M16A4', 'AKM', 'Kar98k', 'UMP9',
        'SKS', 'Mini 14', 'Grenade', 'S1897', 'Micro UZI', 'M24', 'S686',
        'S12K', 'Punch', 'Bluezone', 'AUG', 'AWM', 'Tommy Gun'
      ],
      "domain": {"x": [0, .48]},
      "name": "Miramar Duo Top 5",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    },
    {
      "values": [176536, 103075,  84314,  83178,  82335,  60025,  41313,  25558,
         25073,  23850,  22361,  20865,  20683,  20355,  16003,  14000,
         12906,  12753,  11083,   9341],
      "labels": [
      'Down and Out', 'M416', 'SCAR-L', 'M16A4', 'AKM', 'UMP9', 'S1897',
        'S686', 'Punch', 'S12K', 'Micro UZI', 'Mini 14', 'SKS', 'Grenade',
        'Kar98k', 'Tommy Gun', 'Win94', 'P92', 'P1911', 'Hit by Car'
      ],
      "textposition":"inside",
      "domain": {"x": [.52, 1]},
      "name": "Miramar Duo Rest",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    }],
  "layout": {
        "title":"Miramar Duo Queue Ways of Death",
        "annotations": [
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Top5",
                "x": 0.20,
                "y": 0.5
            },
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Rest",
                "x": 0.8,
                "y": 0.5
            }
        ]
    }
}

```


```python
iplot(fig2, filename='donut1')
```


<div id="d67b8189-aa41-4fc5-b377-c7cbca4d9c1c" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("d67b8189-aa41-4fc5-b377-c7cbca4d9c1c", [{"domain": {"x": [0, 0.48]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["Down and Out", "M416", "SCAR-L", "M16A4", "AKM", "Kar98k", "UMP9", "SKS", "Mini 14", "Grenade", "S1897", "Micro UZI", "M24", "S686", "S12K", "Punch", "Bluezone", "AUG", "AWM", "Tommy Gun"], "name": "Miramar Duo Top 5", "values": [93864, 75326, 53104, 30896, 30853, 18705, 14559, 13752, 12126, 7120, 4883, 3633, 3392, 3320, 3149, 2997, 2934, 2724, 2549, 2313], "type": "pie", "uid": "948a60b3-85a3-4358-bf68-4ea1c526fee7"}, {"domain": {"x": [0.52, 1]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["Down and Out", "M416", "SCAR-L", "M16A4", "AKM", "UMP9", "S1897", "S686", "Punch", "S12K", "Micro UZI", "Mini 14", "SKS", "Grenade", "Kar98k", "Tommy Gun", "Win94", "P92", "P1911", "Hit by Car"], "name": "Miramar Duo Rest", "textposition": "inside", "values": [176536, 103075, 84314, 83178, 82335, 60025, 41313, 25558, 25073, 23850, 22361, 20865, 20683, 20355, 16003, 14000, 12906, 12753, 11083, 9341], "type": "pie", "uid": "533e1f3a-2c26-4fb0-8417-7af9cccbce3f"}], {"annotations": [{"font": {"size": 20}, "showarrow": false, "text": "Top5", "x": 0.2, "y": 0.5}, {"font": {"size": 20}, "showarrow": false, "text": "Rest", "x": 0.8, "y": 0.5}], "title": "Miramar Duo Queue Ways of Death"}, {"showLink": true, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly"})});</script><script type="text/javascript">window.addEventListener("resize", function(){window._Plotly.Plots.resize(document.getElementById("d67b8189-aa41-4fc5-b377-c7cbca4d9c1c"));});</script>



```python
# Miramar Squad donut chart

fig3 = {
  "data": [
    {
      "values": [5666, 4744, 3734, 3204, 3085, 2064, 1233,  966,  923,  914,  854,
         774,  747,  717,  676,  455,  395,  393,  334,  326],
      "labels": [
'Down and Out', 'M416', 'SCAR-L', 'AKM', 'M16A4', 'UMP9', 'Kar98k',
        'SKS', 'Mini 14', 'Grenade', 'Bluezone', 'S1897', 'Punch', 'Micro UZI',
        'S686', 'S12K', 'Win94', 'Tommy Gun', 'Pickup Truck', 'M24'
      ],
      "domain": {"x": [0, .48]},
      "name": "Miramar Squad Top 5",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    },
    {
      "values": [247614, 150344, 128970, 127097, 125073,  96352,  69577,  50693,
         41379,  39280,  34309,  32156,  30254,  30113,  22974,  22120,
         21640,  18709,  16579,  12409],
      "labels": [
       'Down and Out', 'M416', 'AKM', 'M16A4', 'SCAR-L', 'UMP9', 'S1897',
        'Punch', 'S686', 'S12K', 'Micro UZI', 'Grenade', 'Mini 14', 'SKS',
        'Tommy Gun', 'Kar98k', 'Win94', 'P92', 'P1911', 'Hit by Car'
      ],
      "textposition":"inside",
      "domain": {"x": [.52, 1]},
      "name": "Miramar Squad Rest",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    }],
  "layout": {
        "title":"Miramar Squad Queue Ways of Death",
        "annotations": [
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Top5",
                "x": 0.20,
                "y": 0.5
            },
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Rest",
                "x": 0.8,
                "y": 0.5
            }
        ]
    }
}

```


```python
iplot(fig3, filename='donut2')
```


<div id="1ab101dd-d712-4d6f-95bf-1a0bfd2b73f3" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("1ab101dd-d712-4d6f-95bf-1a0bfd2b73f3", [{"domain": {"x": [0, 0.48]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["Down and Out", "M416", "SCAR-L", "AKM", "M16A4", "UMP9", "Kar98k", "SKS", "Mini 14", "Grenade", "Bluezone", "S1897", "Punch", "Micro UZI", "S686", "S12K", "Win94", "Tommy Gun", "Pickup Truck", "M24"], "name": "Miramar Squad Top 5", "values": [5666, 4744, 3734, 3204, 3085, 2064, 1233, 966, 923, 914, 854, 774, 747, 717, 676, 455, 395, 393, 334, 326], "type": "pie", "uid": "b062cb02-e42d-4a4b-a3bb-6fdee26de95b"}, {"domain": {"x": [0.52, 1]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["Down and Out", "M416", "AKM", "M16A4", "SCAR-L", "UMP9", "S1897", "Punch", "S686", "S12K", "Micro UZI", "Grenade", "Mini 14", "SKS", "Tommy Gun", "Kar98k", "Win94", "P92", "P1911", "Hit by Car"], "name": "Miramar Squad Rest", "textposition": "inside", "values": [247614, 150344, 128970, 127097, 125073, 96352, 69577, 50693, 41379, 39280, 34309, 32156, 30254, 30113, 22974, 22120, 21640, 18709, 16579, 12409], "type": "pie", "uid": "05f302db-e40b-4b79-a424-452de87486d8"}], {"annotations": [{"font": {"size": 20}, "showarrow": false, "text": "Top5", "x": 0.2, "y": 0.5}, {"font": {"size": 20}, "showarrow": false, "text": "Rest", "x": 0.8, "y": 0.5}], "title": "Miramar Squad Queue Ways of Death"}, {"showLink": true, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly"})});</script><script type="text/javascript">window.addEventListener("resize", function(){window._Plotly.Plots.resize(document.getElementById("1ab101dd-d712-4d6f-95bf-1a0bfd2b73f3"));});</script>



```python
# Erangle Solo donut chart

fig4 = {
  "data": [
    {
      "values": [217515, 161618, 118504, 102224,  67875,  50639,  43826,  36998,
         18517,  16278,  13629,  13391,  12592,  12232,  12162,  11961,
         11146,   7454,   7157,   7070],
      "labels": [
'M416', 'SCAR-L', 'M16A4', 'AKM', 'Kar98k', 'Mini 14', 'UMP9', 'SKS',
        'S1897', 'Grenade', 'M24', 'Hit by Car', 'S12K', 'Groza', 'S686',
        'Micro UZI', 'AWM', 'M249', 'Tommy Gun', 'Punch'
      ],
      "domain": {"x": [0, .48]},
      "name": "Erangle Solo Top 5",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    },
    {
      "values": [437134, 422328, 379210, 368276, 252111, 219649, 133063, 127039,
        124171, 117021, 109851,  93205,  89103,  77145,  73646,  72934,
         63461,  48887,  28010,  25945],
      "labels": [
      'M416', 'M16A4', 'AKM', 'SCAR-L', 'UMP9', 'S1897', 'S686', 'Punch',
        'Mini 14', 'S12K', 'Micro UZI', 'Kar98k', 'SKS', 'Hit by Car', 'P92',
        'P1911', 'Tommy Gun', 'Grenade', 'Vector', 'Falling'
      ],
      "textposition":"inside",
      "domain": {"x": [.52, 1]},
      "name": "Erangle Solo Rest",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    }],
  "layout": {
        "title":"Erangle Solo Queue Ways of Death",
        "annotations": [
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Top5",
                "x": 0.20,
                "y": 0.5
            },
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Rest",
                "x": 0.8,
                "y": 0.5
            }
        ]
    }
}

```


```python
iplot(fig4, filename='donut3')
```


<div id="1abcc935-fa4d-4ec8-8434-b36fa8acedea" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("1abcc935-fa4d-4ec8-8434-b36fa8acedea", [{"domain": {"x": [0, 0.48]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["M416", "SCAR-L", "M16A4", "AKM", "Kar98k", "Mini 14", "UMP9", "SKS", "S1897", "Grenade", "M24", "Hit by Car", "S12K", "Groza", "S686", "Micro UZI", "AWM", "M249", "Tommy Gun", "Punch"], "name": "Erangle Solo Top 5", "values": [217515, 161618, 118504, 102224, 67875, 50639, 43826, 36998, 18517, 16278, 13629, 13391, 12592, 12232, 12162, 11961, 11146, 7454, 7157, 7070], "type": "pie", "uid": "1e87c5ea-b8ba-469c-8f62-aabaea72a224"}, {"domain": {"x": [0.52, 1]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["M416", "M16A4", "AKM", "SCAR-L", "UMP9", "S1897", "S686", "Punch", "Mini 14", "S12K", "Micro UZI", "Kar98k", "SKS", "Hit by Car", "P92", "P1911", "Tommy Gun", "Grenade", "Vector", "Falling"], "name": "Erangle Solo Rest", "textposition": "inside", "values": [437134, 422328, 379210, 368276, 252111, 219649, 133063, 127039, 124171, 117021, 109851, 93205, 89103, 77145, 73646, 72934, 63461, 48887, 28010, 25945], "type": "pie", "uid": "48fd3345-91b8-4ab7-9457-16ff4eef6ae4"}], {"annotations": [{"font": {"size": 20}, "showarrow": false, "text": "Top5", "x": 0.2, "y": 0.5}, {"font": {"size": 20}, "showarrow": false, "text": "Rest", "x": 0.8, "y": 0.5}], "title": "Erangle Solo Queue Ways of Death"}, {"showLink": true, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly"})});</script><script type="text/javascript">window.addEventListener("resize", function(){window._Plotly.Plots.resize(document.getElementById("1abcc935-fa4d-4ec8-8434-b36fa8acedea"));});</script>



```python
# Erangle Duo donut chart

fig5 = {
  "data": [
    {
      "values": [474015, 307733, 238174, 187236, 161880,  92873,  88778,  77702,
         61099,  35630,  34870,  21588,  21396,  19871,  17698,  15594,
         15573,  15085,  13026,  11121],
      "labels": [
'Down and Out', 'M416', 'SCAR-L', 'M16A4', 'AKM', 'Kar98k', 'Mini 14',
        'UMP9', 'SKS', 'Grenade', 'S1897', 'S686', 'S12K', 'Micro UZI', 'Punch',
        'Bluezone', 'Groza', 'M24', 'Tommy Gun', 'AWM'
      ],
      "domain": {"x": [0, .48]},
      "name": "Erangle Duo Top 5",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    },
    {
      "values": [884693, 437930, 437327, 407342, 377983, 292610, 264868, 152123,
        149785, 133135, 129616, 114622,  91949,  91272,  78790,  74532,
         70345,  66375,  37798,  32399],
      "labels": [
      'Down and Out', 'M416', 'M16A4', 'AKM', 'SCAR-L', 'UMP9', 'S1897',
        'Punch', 'S686', 'S12K', 'Mini 14', 'Micro UZI', 'SKS', 'Grenade',
        'Kar98k', 'Tommy Gun', 'P1911', 'P92', 'Hit by Car', 'Vector'
      ],
      "textposition":"inside",
      "domain": {"x": [.52, 1]},
      "name": "Erangle Duo Rest",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    }],
  "layout": {
        "title":"Erangle Duo Queue Ways of Death",
        "annotations": [
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Top5",
                "x": 0.20,
                "y": 0.5
            },
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Rest",
                "x": 0.8,
                "y": 0.5
            }
        ]
    }
}

```


```python
iplot(fig5, filename='donut4')
```


<div id="a97cf298-77c5-4a56-a51d-c2564e723556" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("a97cf298-77c5-4a56-a51d-c2564e723556", [{"domain": {"x": [0, 0.48]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["Down and Out", "M416", "SCAR-L", "M16A4", "AKM", "Kar98k", "Mini 14", "UMP9", "SKS", "Grenade", "S1897", "S686", "S12K", "Micro UZI", "Punch", "Bluezone", "Groza", "M24", "Tommy Gun", "AWM"], "name": "Erangle Duo Top 5", "values": [474015, 307733, 238174, 187236, 161880, 92873, 88778, 77702, 61099, 35630, 34870, 21588, 21396, 19871, 17698, 15594, 15573, 15085, 13026, 11121], "type": "pie", "uid": "d907e771-7e4c-4d9a-ae71-c66c0db9b485"}, {"domain": {"x": [0.52, 1]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["Down and Out", "M416", "M16A4", "AKM", "SCAR-L", "UMP9", "S1897", "Punch", "S686", "S12K", "Mini 14", "Micro UZI", "SKS", "Grenade", "Kar98k", "Tommy Gun", "P1911", "P92", "Hit by Car", "Vector"], "name": "Erangle Duo Rest", "textposition": "inside", "values": [884693, 437930, 437327, 407342, 377983, 292610, 264868, 152123, 149785, 133135, 129616, 114622, 91949, 91272, 78790, 74532, 70345, 66375, 37798, 32399], "type": "pie", "uid": "d9a99300-5f4c-44a9-837f-0c27981f3890"}], {"annotations": [{"font": {"size": 20}, "showarrow": false, "text": "Top5", "x": 0.2, "y": 0.5}, {"font": {"size": 20}, "showarrow": false, "text": "Rest", "x": 0.8, "y": 0.5}], "title": "Erangle Duo Queue Ways of Death"}, {"showLink": true, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly"})});</script><script type="text/javascript">window.addEventListener("resize", function(){window._Plotly.Plots.resize(document.getElementById("a97cf298-77c5-4a56-a51d-c2564e723556"));});</script>



```python
# Erangle Squad donut chart

fig6 = {
  "data": [
    {
      "values": [213359, 140152, 102074,  66375,  65734,  34213,  33353,  27503,
         23762,  13583,  12578,  12542,   9987,   8264,   7932,   7921,
          5608,   5308,   5226,   4954],
      "labels": [
'Down and Out', 'M416', 'SCAR-L', 'AKM', 'M16A4', 'UMP9', 'Kar98k',
        'SKS', 'Mini 14', 'Grenade', 'Bluezone', 'S1897', 'Punch', 'Micro UZI',
        'S686', 'S12K', 'Win94', 'Tommy Gun', 'Pickup Truck', 'M24'
      ],
      "domain": {"x": [0, .48]},
      "name": "Erangle Squad Top 5",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    },
    {
      "values": [247614, 150344, 128970, 127097, 125073,  96352,  69577,  50693,
         41379,  39280,  34309,  32156,  30254,  30113,  22974,  22120,
         21640,  18709,  16579,  12409],
      "labels": [
      'Down and Out', 'M416', 'AKM', 'M16A4', 'SCAR-L', 'UMP9', 'S1897',
        'Punch', 'S686', 'S12K', 'Micro UZI', 'Grenade', 'Mini 14', 'SKS',
        'Tommy Gun', 'Kar98k', 'Win94', 'P92', 'P1911', 'Hit by Car'
      ],
      "textposition":"inside",
      "domain": {"x": [.52, 1]},
      "name": "Erangle Squad Rest",
      "hoverinfo":"label+percent+name",
      "hole": .4,
      "type": "pie"
    }],
  "layout": {
        "title":"Erangle Squad Queue Ways of Death",
        "annotations": [
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Top5",
                "x": 0.20,
                "y": 0.5
            },
            {
                "font": {
                    "size": 20
                },
                "showarrow": False,
                "text": "Rest",
                "x": 0.8,
                "y": 0.5
            }
        ]
    }
}

```


```python
iplot(fig6, filename='donut5')
```


<div id="fed7f836-e3ac-4de5-aeba-a46d951215a8" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("fed7f836-e3ac-4de5-aeba-a46d951215a8", [{"domain": {"x": [0, 0.48]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["Down and Out", "M416", "SCAR-L", "AKM", "M16A4", "UMP9", "Kar98k", "SKS", "Mini 14", "Grenade", "Bluezone", "S1897", "Punch", "Micro UZI", "S686", "S12K", "Win94", "Tommy Gun", "Pickup Truck", "M24"], "name": "Erangle Squad Top 5", "values": [213359, 140152, 102074, 66375, 65734, 34213, 33353, 27503, 23762, 13583, 12578, 12542, 9987, 8264, 7932, 7921, 5608, 5308, 5226, 4954], "type": "pie", "uid": "be583feb-2625-4269-887e-0e592aa17a76"}, {"domain": {"x": [0.52, 1]}, "hole": 0.4, "hoverinfo": "label+percent+name", "labels": ["Down and Out", "M416", "AKM", "M16A4", "SCAR-L", "UMP9", "S1897", "Punch", "S686", "S12K", "Micro UZI", "Grenade", "Mini 14", "SKS", "Tommy Gun", "Kar98k", "Win94", "P92", "P1911", "Hit by Car"], "name": "Erangle Squad Rest", "textposition": "inside", "values": [247614, 150344, 128970, 127097, 125073, 96352, 69577, 50693, 41379, 39280, 34309, 32156, 30254, 30113, 22974, 22120, 21640, 18709, 16579, 12409], "type": "pie", "uid": "bbfda5ad-864f-43e5-a942-e257cb795d00"}], {"annotations": [{"font": {"size": 20}, "showarrow": false, "text": "Top5", "x": 0.2, "y": 0.5}, {"font": {"size": 20}, "showarrow": false, "text": "Rest", "x": 0.8, "y": 0.5}], "title": "Erangle Squad Queue Ways of Death"}, {"showLink": true, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly"})});</script><script type="text/javascript">window.addEventListener("resize", function(){window._Plotly.Plots.resize(document.getElementById("fed7f836-e3ac-4de5-aeba-a46d951215a8"));});</script>



```python
import gc
gc.collect()
```




    73842




```python
import imageio as imageio
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns 
%matplotlib inline
from scipy.misc.pilutil import imread
```


```python
# use second place and to allocate placement
df_second_er = deaths_solo_er[(deaths_solo_er['victim_placement'] == 2)].dropna()
df_second_mr = deaths_solo_mr[(deaths_solo_mr['victim_placement'] == 2)].dropna()
print('%i Erangel matches where 2nd place didn''t die to bluezone' % len(df_second_er))
print('%i Miramar matches where 2nd place didn''t die to bluezone' % len(df_second_mr))
```

    47594 Erangel matches where 2nd place didnt die to bluezone
    9449 Miramar matches where 2nd place didnt die to bluezone
    


```python
plot_data_er = np.vstack([df_second_er[['victim_position_x', 'victim_position_y']].values, 
                          df_second_er[['killer_position_x', 'killer_position_y']].values])
plot_data_mr = np.vstack([df_second_mr[['victim_position_x', 'victim_position_y']].values, 
                          df_second_mr[['killer_position_x', 'killer_position_y']].values])
# placement into map size
plot_data_er = plot_data_er*4096/800000
plot_data_mr = plot_data_mr*1000/800000
```


```python
from scipy.ndimage.filters import gaussian_filter
import matplotlib.cm as cm
from matplotlib.colors import Normalize

#define heat map
def heatmap(x, y, s, bins=100):
    heatmap, xedges, yedges = np.histogram2d(x, y, bins=bins)
    heatmap = gaussian_filter(heatmap, sigma=s)

    extent = [xedges[0], xedges[-1], yedges[0], yedges[-1]]
    return heatmap.T, extent
```


```python
bg = imageio.imread('erangel.jpg')
hmap, extent = heatmap(plot_data_er[:,0], plot_data_er[:,1], 1.5)
alphas = np.clip(Normalize(0, hmap.max(), clip=True)(hmap)*1.5, 0.0, 1.)
colors = Normalize(0, hmap.max(), clip=True)(hmap)
colors = cm.Reds(colors)
colors[..., -1] = alphas
```


```python
fig, ax = plt.subplots(figsize=(24,24))
ax.set_xlim(0, 4096); ax.set_ylim(0, 4096)
ax.imshow(bg)
ax.imshow(colors, extent=extent, origin='lower', cmap=cm.Reds, alpha=0.9)
plt.gca().invert_yaxis()
plt.savefig('era.png')
```


![png](output_89_0.png)



```python
bg = imageio.imread('miramar.jpg')
hmap, extent = heatmap(plot_data_mr[:,0], plot_data_mr[:,1], 1.5)
alphas = np.clip(Normalize(0, hmap.max(), clip=True)(hmap)*1.5, 0.0, 1.)
colors = Normalize(0, hmap.max(), clip=True)(hmap)
colors = cm.Blues(colors)
colors[..., -1] = alphas
```


```python
fig, ax = plt.subplots(figsize=(24,24))     
ax.set_xlim(0, 1000); ax.set_ylim(0, 1000)
ax.imshow(bg)
ax.imshow(colors, extent=extent, origin='lower', cmap=cm.Blues, alpha=0.9)
#plt.scatter(plot_data_mr[:,0], plot_data_mr[:,1])
plt.gca().invert_yaxis()
plt.savefig('mir.png')
```


![png](output_91_0.png)


# Heat map to see where the last circle could be
### We use the second place player's death position to define where the last circle could be.
### We only use solo matches data here, because in duo and squad there are multiple second place death position

### Because the picture is too large and cannot fit properly in the screen so we uploaded them
### Original erangel map : http://personal.psu.edu/hkh5094/DS330/Finalproject/erangel.jpg
### Original miramar map: http://personal.psu.edu/hkh5094/DS330/Finalproject/miramar.jpg
### erangel heat map: http://personal.psu.edu/hkh5094/DS330/Finalproject/era.png
### miramar heat map: http://personal.psu.edu/hkh5094/DS330/Finalproject/mir.png


```python

```
