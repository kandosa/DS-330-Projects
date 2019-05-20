
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
