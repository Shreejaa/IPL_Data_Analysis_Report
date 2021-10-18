# INDIAN PREMIER LEAGUE TEAM ANALYSIS 

---

Shreejaa Talla<br>
stalla@student.gsu.edu<br>
Department of computer science<br>
Georgia State University<br>

---
[Analysis Report](IPL_Analysis_Report.html)
### Index
1. [Introduction](#intro)
2. [Install and Import required libraries](#import)
3. [Data](#data)
4. [Tools used](#tools)
5. [Challenges](#ch)
6. [References](#r)

<a id="intro"></a>
## Introduction: 
- Every year starting from 2008, India has been conducting the Indian Premier League(IPL), where it picks up all the players from different countries and selects about fifteen players per team. There are about 12 active teams in the IPL which are Chennai Super Kings(CSK), Sunrisers Hyderabad(SRH), Delhi Champions(DC), Royal Challenges Bangalore(RCB), Kochi Tuskers(Kochi), Kings XI Punjab(KXIP), Kolkata Knight Riders(KKR), Gujarat Lions(GL), Rajasthan Royals(RR), Rising Pune Supergiant(RPS), Mumbai Indians(MI) and Sahara Pune Warriors(PWI).
- Before the matches begin, Main Cricket Tournament leaders work on analyzing teams, and based on that; they pick up players for each group. Thus, every year few players keep shuffling.
- The cricket ambassadors examine each team based on the data formed by all the previous matches. Depicting different visualization based on the data analysis on each IPL team in two categories: batting and bowling can solve this problem by analyzing data to find the best players and matches played by each team.
- This analysis are prepared using **panel** for creating a dashboard and plots are designed using **matplotlib and seaborn**. Two custom designs are build on top of scatter plot and networkx graph.

## Compilation of this project

1. Please run each and every cell step by step.
2. The end result is displayed in the dashboard type1 and dashboard type2, once the cell is run and the dashboard is generated inline, the interactive component of those dashboards can be used to test the interactivity. The dash might take 1-2 mins to load the data based on dropdown values.
3. .HTML and .pdf results are given in the folder obtained by dashboard type3 which are not interactive as they dont have a conection to server.

<a id="import"></a>
## Install and Import required libraries

Install the following libraries in conda command prompt on windows:
1. conda install -c conda-forge panel
2. conda install -c conda-forge pydot
3. conda install -c conda-forge networkx 


```python
import pandas as pd
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plt
import networkx as nx
import pydot
from networkx.drawing.nx_pydot import graphviz_layout
from pandas import ExcelWriter
from pandas import ExcelFile
import matplotlib.pyplot as plt
from matplotlib.offsetbox import OffsetImage, AnnotationBbox
from matplotlib.animation import FuncAnimation
import panel as pn
import panel.widgets as widget
import random
import warnings

warnings.filterwarnings('ignore')
```

<a id="data"></a>
## Data

### Matches Data 

This data consists of 45 columns and 824 rows, it describes about each indian premiere league matches, year in which it is conducted, test score of two teams, venue of the match, match description, toss decision, innings, umpire details and winning team details.


```python
matches = pd.read_csv("data/all_season_summary.csv")
matches
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
      <th>season</th>
      <th>id</th>
      <th>name</th>
      <th>short_name</th>
      <th>description</th>
      <th>home_team</th>
      <th>away_team</th>
      <th>toss_won</th>
      <th>decision</th>
      <th>1st_inning_score</th>
      <th>...</th>
      <th>home_playx1</th>
      <th>away_playx1</th>
      <th>away_key_batsman</th>
      <th>away_key_bowler</th>
      <th>match_days</th>
      <th>umpire1</th>
      <th>umpire2</th>
      <th>tv_umpire</th>
      <th>referee</th>
      <th>reserve_umpire</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020</td>
      <td>1216492</td>
      <td>Mumbai Indians v Chennai Super Kings</td>
      <td>MI v CSK</td>
      <td>1st Match (N), Indian Premier League at Abu Dh...</td>
      <td>MI</td>
      <td>CSK</td>
      <td>CSK</td>
      <td>BOWL FIRST</td>
      <td>162/9</td>
      <td>...</td>
      <td>Rohit Sharma (BT),Quinton de Kock (WK),Suryaku...</td>
      <td>Murali Vijay (BT),Shane Watson (AR),Faf du Ple...</td>
      <td>Ambati Rayudu,Faf du Plessis</td>
      <td>Lungi Ngidi,Deepak Chahar</td>
      <td>19 September 2020 - night match (20-over match)</td>
      <td>Chris Gaffaney</td>
      <td>Virender Sharma</td>
      <td>Sundaram Ravi</td>
      <td>Manu Nayyar</td>
      <td>Ulhas Gandhe</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020</td>
      <td>1216493</td>
      <td>Delhi Capitals v Kings XI Punjab</td>
      <td>DC v KXIP</td>
      <td>2nd Match (N), Indian Premier League at Dubai ...</td>
      <td>DC</td>
      <td>KXIP</td>
      <td>KXIP</td>
      <td>BOWL FIRST</td>
      <td>157/8</td>
      <td>...</td>
      <td>Prithvi Shaw (BT),Shikhar Dhawan (BT),Shimron ...</td>
      <td>KL Rahul (WK),Mayank Agarwal (BT),Karun Nair (...</td>
      <td>Mayank Agarwal,KL Rahul</td>
      <td>Mohammed Shami,Sheldon Cottrell</td>
      <td>20 September 2020 - night match (20-over match)</td>
      <td>Anil Chaudhary</td>
      <td>Nitin Menon</td>
      <td>Paul Reiffel</td>
      <td>Javagal Srinath</td>
      <td>Yeshwant Barde</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020</td>
      <td>1216534</td>
      <td>Sunrisers Hyderabad v Royal Challengers Bangalore</td>
      <td>SRH v RCB</td>
      <td>3rd Match (N), Indian Premier League at Dubai ...</td>
      <td>SRH</td>
      <td>RCB</td>
      <td>SRH</td>
      <td>BOWL FIRST</td>
      <td>163/5</td>
      <td>...</td>
      <td>David Warner (BT),Jonny Bairstow (WK),Manish P...</td>
      <td>Devdutt Padikkal (BT),Aaron Finch (BT),Virat K...</td>
      <td>Devdutt Padikkal,AB de Villiers</td>
      <td>Yuzvendra Chahal,Shivam Dube</td>
      <td>21 September 2020 - night match (20-over match)</td>
      <td>Anil Dandekar</td>
      <td>Nitin Menon</td>
      <td>Anil Chaudhary</td>
      <td>Prakash Bhatt</td>
      <td>Yeshwant Barde</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020</td>
      <td>1216496</td>
      <td>Rajasthan Royals v Chennai Super Kings</td>
      <td>RR v CSK</td>
      <td>4th Match (N), Indian Premier League at Sharja...</td>
      <td>RR</td>
      <td>CSK</td>
      <td>CSK</td>
      <td>BOWL FIRST</td>
      <td>216/7</td>
      <td>...</td>
      <td>Yashasvi Jaiswal (BT),Steven Smith (BT),Sanju ...</td>
      <td>Murali Vijay (BT),Shane Watson (AR),Faf du Ple...</td>
      <td>Faf du Plessis,Shane Watson</td>
      <td>Sam Curran,Deepak Chahar</td>
      <td>22 September 2020 - night match (20-over match)</td>
      <td>Chettithody Shamshuddin</td>
      <td>Vineet Kulkarni</td>
      <td>KN Ananthapadmanabhan</td>
      <td>Vengalil Narayan Kutty</td>
      <td>Krishnamachari Srinivasan</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020</td>
      <td>1216508</td>
      <td>Kolkata Knight Riders v Mumbai Indians</td>
      <td>KKR v MI</td>
      <td>5th Match (N), Indian Premier League at Abu Dh...</td>
      <td>KKR</td>
      <td>MI</td>
      <td>KKR</td>
      <td>BOWL FIRST</td>
      <td>195/5</td>
      <td>...</td>
      <td>Shubman Gill (BT),Sunil Narine (AR),Dinesh Kar...</td>
      <td>Quinton de Kock (WK),Rohit Sharma (BT),Suryaku...</td>
      <td>Rohit Sharma,Suryakumar Yadav</td>
      <td>James Pattinson,Rahul Chahar</td>
      <td>23 September 2020 - night match (20-over match)</td>
      <td>Chris Gaffaney</td>
      <td>Sundaram Ravi</td>
      <td>Virender Sharma</td>
      <td>Manu Nayyar</td>
      <td>Pashchim Pathak</td>
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
      <th>819</th>
      <td>2008</td>
      <td>336012</td>
      <td>Royal Challengers Bangalore v Mumbai Indians</td>
      <td>RCB v MI</td>
      <td>55th match (D/N), Indian Premier League at Ben...</td>
      <td>RCB</td>
      <td>MI</td>
      <td>MI</td>
      <td>BOWL FIRST</td>
      <td>122/9</td>
      <td>...</td>
      <td>Mark Boucher (BT),Shreevats Goswami (WK),Misba...</td>
      <td>Sanath Jayasuriya (AR),Sachin Tendulkar (BT),R...</td>
      <td>Sanath Jayasuriya,Sachin Tendulkar</td>
      <td>Dilhara Fernando,Dwayne Smith</td>
      <td>28 May 2008 - day/night match (20-over match)</td>
      <td>Billy Bowden</td>
      <td>Arani Jayaprakash</td>
      <td>Billy Doctrove</td>
      <td>Sir Clive Lloyd</td>
      <td>None</td>
    </tr>
    <tr>
      <th>820</th>
      <td>2008</td>
      <td>336019</td>
      <td>Kings XI Punjab v Rajasthan Royals</td>
      <td>KXIP v RR</td>
      <td>56th match (N), Indian Premier League at Mohal...</td>
      <td>KXIP</td>
      <td>RR</td>
      <td>RR</td>
      <td>BOWL FIRST</td>
      <td>221/3</td>
      <td>...</td>
      <td>Shaun Marsh (BT),James Hopes (AR),Yuvraj Singh...</td>
      <td>Mohammad Kaif (BT),Niraj Patel (UKN),Younis Kh...</td>
      <td>Niraj Patel,Yusuf Pathan</td>
      <td>Shane Watson,Yusuf Pathan</td>
      <td>28 May 2008 - night match (20-over match)</td>
      <td>Krishna Hariharan</td>
      <td>Steve Davis</td>
      <td>Daryl Harper</td>
      <td>Srinivas Venkataraghavan</td>
      <td>MS Mahal</td>
    </tr>
    <tr>
      <th>821</th>
      <td>2008</td>
      <td>336038</td>
      <td>Delhi Daredevils v Rajasthan Royals</td>
      <td>DC v RR</td>
      <td>1st Semi-Final (N), Indian Premier League at M...</td>
      <td>DC</td>
      <td>RR</td>
      <td>DC</td>
      <td>BOWL FIRST</td>
      <td>192/9</td>
      <td>...</td>
      <td>Gautam Gambhir (BT),Virender Sehwag (BT),Shikh...</td>
      <td>Graeme Smith (BT),Swapnil Asnodkar (BT),Sohail...</td>
      <td>Shane Watson,Yusuf Pathan</td>
      <td>Shane Watson,Munaf Patel</td>
      <td>30 May 2008 - night match (20-over match)</td>
      <td>Billy Bowden</td>
      <td>Rudi Koertzen</td>
      <td>Billy Doctrove</td>
      <td>Javagal Srinath</td>
      <td>None</td>
    </tr>
    <tr>
      <th>822</th>
      <td>2008</td>
      <td>336039</td>
      <td>Chennai Super Kings v Kings XI Punjab</td>
      <td>CSK v KXIP</td>
      <td>2nd Semi-Final (N), Indian Premier League at M...</td>
      <td>CSK</td>
      <td>KXIP</td>
      <td>KXIP</td>
      <td>BAT FIRST</td>
      <td>112/8</td>
      <td>...</td>
      <td>Parthiv Patel (WK),Vidyut Sivaramakrishnan (UK...</td>
      <td>Shaun Marsh (BT),James Hopes (AR),Kumar Sangak...</td>
      <td>Ramesh Powar,Wilkin Mota</td>
      <td>Irfan Pathan,Vikram Singh</td>
      <td>31 May 2008 - night match (20-over match)</td>
      <td>Asad Rauf</td>
      <td>Daryl Harper</td>
      <td>Krishna Hariharan</td>
      <td>Srinivas Venkataraghavan</td>
      <td>None</td>
    </tr>
    <tr>
      <th>823</th>
      <td>2008</td>
      <td>336040</td>
      <td>Chennai Super Kings v Rajasthan Royals</td>
      <td>CSK v RR</td>
      <td>Final (N), Indian Premier League at Mumbai, Ju...</td>
      <td>CSK</td>
      <td>RR</td>
      <td>RR</td>
      <td>BOWL FIRST</td>
      <td>163/5</td>
      <td>...</td>
      <td>Parthiv Patel (WK),Vidyut Sivaramakrishnan (UK...</td>
      <td>Niraj Patel (UKN),Swapnil Asnodkar (BT),Kamran...</td>
      <td>Yusuf Pathan,Swapnil Asnodkar</td>
      <td>Yusuf Pathan,Shane Watson</td>
      <td>1 June 2008 - night match (20-over match)</td>
      <td>Billy Bowden</td>
      <td>Rudi Koertzen</td>
      <td>Daryl Harper</td>
      <td>Javagal Srinath</td>
      <td>MR Singh</td>
    </tr>
  </tbody>
</table>
<p>824 rows × 45 columns</p>
</div>



## Batting summary data

All innings of matches are present in this data with respect to batting information such as runs, running over, strike rate, number fours, number of sixes, captain, batsmen name, venue and commentary.


```python
batting = pd.read_csv("data/all_season_batting_card.csv")
batting
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
      <th>season</th>
      <th>match_id</th>
      <th>match_name</th>
      <th>home_team</th>
      <th>away_team</th>
      <th>venue</th>
      <th>city</th>
      <th>country</th>
      <th>current_innings</th>
      <th>innings_id</th>
      <th>...</th>
      <th>fours</th>
      <th>sixes</th>
      <th>strikeRate</th>
      <th>captain</th>
      <th>isNotOut</th>
      <th>runningScore</th>
      <th>runningOver</th>
      <th>shortText</th>
      <th>commentary</th>
      <th>link</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020</td>
      <td>1216492</td>
      <td>MI v CSK</td>
      <td>MI</td>
      <td>CSK</td>
      <td>Sheikh Zayed Stadium, Abu Dhabi</td>
      <td>Abu Dhabi</td>
      <td>United Arab Emirates</td>
      <td>MI</td>
      <td>1</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>120.00</td>
      <td>True</td>
      <td>False</td>
      <td>{'wickets': 1, 'runs': 46}</td>
      <td>4.4</td>
      <td>c Curran b Chawla</td>
      <td>&lt;b&gt;chipped straight to mid-off!&lt;/b&gt; Full on of...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020</td>
      <td>1216492</td>
      <td>MI v CSK</td>
      <td>MI</td>
      <td>CSK</td>
      <td>Sheikh Zayed Stadium, Abu Dhabi</td>
      <td>Abu Dhabi</td>
      <td>United Arab Emirates</td>
      <td>MI</td>
      <td>1</td>
      <td>...</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>165.00</td>
      <td>False</td>
      <td>False</td>
      <td>{'wickets': 2, 'runs': 48}</td>
      <td>5.1</td>
      <td>c Watson b Curran</td>
      <td>&lt;b&gt;straight to midwicket!&lt;/b&gt; Slow does it! Sp...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020</td>
      <td>1216492</td>
      <td>MI v CSK</td>
      <td>MI</td>
      <td>CSK</td>
      <td>Sheikh Zayed Stadium, Abu Dhabi</td>
      <td>Abu Dhabi</td>
      <td>United Arab Emirates</td>
      <td>MI</td>
      <td>1</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>106.25</td>
      <td>False</td>
      <td>False</td>
      <td>{'wickets': 3, 'runs': 92}</td>
      <td>10.6</td>
      <td>c Curran b Chahar</td>
      <td>&lt;b&gt;taken at long-on!&lt;/b&gt; Curran involved again...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020</td>
      <td>1216492</td>
      <td>MI v CSK</td>
      <td>MI</td>
      <td>CSK</td>
      <td>Sheikh Zayed Stadium, Abu Dhabi</td>
      <td>Abu Dhabi</td>
      <td>United Arab Emirates</td>
      <td>MI</td>
      <td>1</td>
      <td>...</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>135.48</td>
      <td>False</td>
      <td>False</td>
      <td>{'wickets': 4, 'runs': 121}</td>
      <td>14.1</td>
      <td>c du Plessis b Jadeja</td>
      <td>&lt;b&gt;taken at long-on.&lt;/b&gt; Faf with the lob back...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020</td>
      <td>1216492</td>
      <td>MI v CSK</td>
      <td>MI</td>
      <td>CSK</td>
      <td>Sheikh Zayed Stadium, Abu Dhabi</td>
      <td>Abu Dhabi</td>
      <td>United Arab Emirates</td>
      <td>MI</td>
      <td>1</td>
      <td>...</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>140.00</td>
      <td>False</td>
      <td>False</td>
      <td>{'wickets': 5, 'runs': 124}</td>
      <td>14.5</td>
      <td>c du Plessis b Jadeja</td>
      <td>&lt;b&gt;Faf you stunner!&lt;/b&gt; Wow! What a fielder. T...</td>
      <td>NaN</td>
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
      <th>12424</th>
      <td>2008</td>
      <td>336040</td>
      <td>CSK v RR</td>
      <td>CSK</td>
      <td>RR</td>
      <td>Dr DY Patil Sports Academy, Mumbai</td>
      <td>Mumbai</td>
      <td>India</td>
      <td>RR</td>
      <td>2</td>
      <td>...</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>143.58</td>
      <td>False</td>
      <td>False</td>
      <td>{'wickets': 7, 'runs': 143}</td>
      <td>17.4</td>
      <td>run out (Raina)</td>
      <td>Raina you beauty! A direct hit runs out Yusuf!...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12425</th>
      <td>2008</td>
      <td>336040</td>
      <td>CSK v RR</td>
      <td>CSK</td>
      <td>RR</td>
      <td>Dr DY Patil Sports Academy, Mumbai</td>
      <td>Mumbai</td>
      <td>India</td>
      <td>RR</td>
      <td>2</td>
      <td>...</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>133.33</td>
      <td>False</td>
      <td>False</td>
      <td>{'wickets': 5, 'runs': 139}</td>
      <td>16.6</td>
      <td>c Dhoni b Muralitharan</td>
      <td>caught! Another twist! Kaif steps out of his c...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12426</th>
      <td>2008</td>
      <td>336040</td>
      <td>CSK v RR</td>
      <td>CSK</td>
      <td>RR</td>
      <td>Dr DY Patil Sports Academy, Mumbai</td>
      <td>Mumbai</td>
      <td>India</td>
      <td>RR</td>
      <td>2</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>False</td>
      <td>False</td>
      <td>{'wickets': 6, 'runs': 139}</td>
      <td>17.1</td>
      <td>c Kapugedera b Morkel</td>
      <td>what has he done? Jadeja backs away towards le...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12427</th>
      <td>2008</td>
      <td>336040</td>
      <td>CSK v RR</td>
      <td>CSK</td>
      <td>RR</td>
      <td>Dr DY Patil Sports Academy, Mumbai</td>
      <td>Mumbai</td>
      <td>India</td>
      <td>RR</td>
      <td>2</td>
      <td>...</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>100.00</td>
      <td>True</td>
      <td>True</td>
      <td>{}</td>
      <td>NaN</td>
      <td>not out</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12428</th>
      <td>2008</td>
      <td>336040</td>
      <td>CSK v RR</td>
      <td>CSK</td>
      <td>RR</td>
      <td>Dr DY Patil Sports Academy, Mumbai</td>
      <td>Mumbai</td>
      <td>India</td>
      <td>RR</td>
      <td>2</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>128.57</td>
      <td>False</td>
      <td>True</td>
      <td>{}</td>
      <td>NaN</td>
      <td>not out</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>12429 rows × 25 columns</p>
</div>



## Bowling summary data

Each over and ball by ball data is captured in this table, it consist of bowling team information such as economy rate, differnt types of conceds, maidens, no balls and dots. This data also provides the information of each player through all the matches.


```python
bowling = pd.read_csv("data/all_season_bowling_card.csv")
bowling
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
      <th>season</th>
      <th>match_id</th>
      <th>match_name</th>
      <th>home_team</th>
      <th>away_team</th>
      <th>bowling_team</th>
      <th>venue</th>
      <th>city</th>
      <th>country</th>
      <th>innings_id</th>
      <th>...</th>
      <th>conceded</th>
      <th>wickets</th>
      <th>economyRate</th>
      <th>dots</th>
      <th>foursConceded</th>
      <th>sixesConceded</th>
      <th>wides</th>
      <th>noballs</th>
      <th>captain</th>
      <th>href</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020</td>
      <td>1216492</td>
      <td>MI v CSK</td>
      <td>MI</td>
      <td>CSK</td>
      <td>CSK</td>
      <td>Sheikh Zayed Stadium, Abu Dhabi</td>
      <td>Abu Dhabi</td>
      <td>United Arab Emirates</td>
      <td>1</td>
      <td>...</td>
      <td>32</td>
      <td>2</td>
      <td>8.0</td>
      <td>7</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>False</td>
      <td>https://www.espncricinfo.com/ci/content/player...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020</td>
      <td>1216492</td>
      <td>MI v CSK</td>
      <td>MI</td>
      <td>CSK</td>
      <td>CSK</td>
      <td>Sheikh Zayed Stadium, Abu Dhabi</td>
      <td>Abu Dhabi</td>
      <td>United Arab Emirates</td>
      <td>1</td>
      <td>...</td>
      <td>28</td>
      <td>1</td>
      <td>7.0</td>
      <td>9</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>False</td>
      <td>https://www.espncricinfo.com/ci/content/player...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020</td>
      <td>1216492</td>
      <td>MI v CSK</td>
      <td>MI</td>
      <td>CSK</td>
      <td>CSK</td>
      <td>Sheikh Zayed Stadium, Abu Dhabi</td>
      <td>Abu Dhabi</td>
      <td>United Arab Emirates</td>
      <td>1</td>
      <td>...</td>
      <td>38</td>
      <td>3</td>
      <td>9.5</td>
      <td>8</td>
      <td>6</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>False</td>
      <td>https://www.espncricinfo.com/ci/content/player...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020</td>
      <td>1216492</td>
      <td>MI v CSK</td>
      <td>MI</td>
      <td>CSK</td>
      <td>CSK</td>
      <td>Sheikh Zayed Stadium, Abu Dhabi</td>
      <td>Abu Dhabi</td>
      <td>United Arab Emirates</td>
      <td>1</td>
      <td>...</td>
      <td>21</td>
      <td>1</td>
      <td>5.25</td>
      <td>11</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>False</td>
      <td>https://www.espncricinfo.com/ci/content/player...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020</td>
      <td>1216492</td>
      <td>MI v CSK</td>
      <td>MI</td>
      <td>CSK</td>
      <td>CSK</td>
      <td>Sheikh Zayed Stadium, Abu Dhabi</td>
      <td>Abu Dhabi</td>
      <td>United Arab Emirates</td>
      <td>1</td>
      <td>...</td>
      <td>42</td>
      <td>2</td>
      <td>10.5</td>
      <td>5</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>False</td>
      <td>https://www.espncricinfo.com/ci/content/player...</td>
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
      <th>9662</th>
      <td>2008</td>
      <td>336040</td>
      <td>CSK v RR</td>
      <td>CSK</td>
      <td>RR</td>
      <td>CSK</td>
      <td>Dr DY Patil Sports Academy, Mumbai</td>
      <td>Mumbai</td>
      <td>India</td>
      <td>2</td>
      <td>...</td>
      <td>21</td>
      <td>0</td>
      <td>5.25</td>
      <td>13</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>False</td>
      <td>https://www.espncricinfo.com/ci/content/player...</td>
    </tr>
    <tr>
      <th>9663</th>
      <td>2008</td>
      <td>336040</td>
      <td>CSK v RR</td>
      <td>CSK</td>
      <td>RR</td>
      <td>CSK</td>
      <td>Dr DY Patil Sports Academy, Mumbai</td>
      <td>Mumbai</td>
      <td>India</td>
      <td>2</td>
      <td>...</td>
      <td>30</td>
      <td>1</td>
      <td>7.5</td>
      <td>8</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>False</td>
      <td>https://www.espncricinfo.com/ci/content/player...</td>
    </tr>
    <tr>
      <th>9664</th>
      <td>2008</td>
      <td>336040</td>
      <td>CSK v RR</td>
      <td>CSK</td>
      <td>RR</td>
      <td>CSK</td>
      <td>Dr DY Patil Sports Academy, Mumbai</td>
      <td>Mumbai</td>
      <td>India</td>
      <td>2</td>
      <td>...</td>
      <td>25</td>
      <td>2</td>
      <td>6.25</td>
      <td>11</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>False</td>
      <td>https://www.espncricinfo.com/ci/content/player...</td>
    </tr>
    <tr>
      <th>9665</th>
      <td>2008</td>
      <td>336040</td>
      <td>CSK v RR</td>
      <td>CSK</td>
      <td>RR</td>
      <td>CSK</td>
      <td>Dr DY Patil Sports Academy, Mumbai</td>
      <td>Mumbai</td>
      <td>India</td>
      <td>2</td>
      <td>...</td>
      <td>42</td>
      <td>0</td>
      <td>10.5</td>
      <td>7</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>False</td>
      <td>https://www.espncricinfo.com/ci/content/player...</td>
    </tr>
    <tr>
      <th>9666</th>
      <td>2008</td>
      <td>336040</td>
      <td>CSK v RR</td>
      <td>CSK</td>
      <td>RR</td>
      <td>CSK</td>
      <td>Dr DY Patil Sports Academy, Mumbai</td>
      <td>Mumbai</td>
      <td>India</td>
      <td>2</td>
      <td>...</td>
      <td>39</td>
      <td>2</td>
      <td>9.75</td>
      <td>8</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>False</td>
      <td>https://www.espncricinfo.com/ci/content/player...</td>
    </tr>
  </tbody>
</table>
<p>9667 rows × 24 columns</p>
</div>



## Challenges

1. The first challenge was to create a custom scatter plot, and with interactivity and resizing the images for a change in drop down values.
2. To Build the dashboard based on seaborn and matplotlib, which has only one source to create and was pretty tough[3].
3. Only axes graphs were allowed with in the dashboard as it display atmost one figure at a time. Hence, all the graphs are combined into a figure and displayed due to which all faced grid plots are ignored.

<a id="r"></a>
## References:

1. https://stackoverflow.com/questions/29586520/can-one-get-hierarchical-graphs-from-networkx-with-python-3
2. https://stackoverflow.com/questions/22566284/matplotlib-how-to-plot-images-instead-of-points
3. https://panel.holoviz.org/reference/widgets/
4. https://coderzcolumn.com/tutorials/data-science/how-to-create-dashboard-using-python-matplotlib-panel
