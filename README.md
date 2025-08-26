# IPL Dashboard

The IPL Dashboard is a comprehensive visualization tool designed to analyze and present insights into the Indian Premier League (IPL), one of the most popular professional cricket leagues in the world. The dashboard provides in-depth analyses of team performances, player statistics, and match trends using Power BI.


# About the IPL

The Indian Premier League (IPL), launched in 2008 by the Board of Control for Cricket in India (BCCI), is known for merging entertainment with cricket. It attracts top cricketing talent worldwide and features franchise-based teams representing various Indian cities.

# Key Highlights:
> Total Seasons: 17

> Total Teams Participating: 16

> Total Matches Played: 1106

> Average Runs Per Match (Season-wise): Trends vary from 140 to 180 runs.

# Features

The IPL Dashboard includes the following components:

# IPL Overview

KPIs: Total Seasons, Teams Participated, Total Matches Played

## Charts:

    > Rank of stadiums by venue

    > No‑balls & % wides by season

    > Average runs per match (season‑wise)

    > Final four / qualifiers table (Q_T_1 to Q_T_4)
    
# Team Profile

> Team selector (buttons/slicers for CSK, MI, RCB, SRH, etc.)

Tables:

    > Venues with total matches & win% at each ground

    > Season summary: matches played, won, lost, NRR, no‑result

> Top Performers: Top 5 run scorers & wicket takers (team filtered)

> Interactive chart: Runs / Wickets / Boundaries by over, with over‑range and season sliders

> Year filter card 

# Player Profile

> Slicers: Bowler, Batter, Season

> Head‑to‑head table: Balls, Runs, Wickets, Boundaries, Strike Rate (batter) & Economy (bowler)

> Leaderboards: Top 5 bowlers, batsmen, and hitters

> Trends: Total boundaries by season; run distribution (1/2/3/4/6) pie

# Dynamic Filters

> Season-wise and team-wise filters for customized insights.

> Comparison of players and teams based on runs, wickets, and other KPIs.

# Data Source

The data used for this dashboard is sourced from IPL matches and player statistics. Detailed datasets include information about:

> Match results.

> Player performances.

> Venues and umpires.

## DAX (Measure) for (IPL Overview page)
### 1) Rank of stadium(measure)
Rank of stadium = IF(RANKX(ALL(all_season_summary[venue_name]),CALCULATE(COUNT(all_season_summary[venue_name])))<11,CALCULATE(COUNT(all_season_summary[venue_name])), BLANK())

### 2) % of wide balls(measure)
% of wide balls = 
var totalwideballs = CALCULATE(SUM(all_season_bowling_card[wides]))
var totalballs = CALCULATE(SUM(all_season_bowling_card[total balls]))
return(
    DIVIDE(totalwideballs,totalballs)*100)

### 3) Average(measure)
Average = 
var sum1 = CALCULATE(SUM(all_season_summary[first_inning_cleaned]))
var sum2 = CALCULATE(SUM(all_season_summary[second_inning_cleaned]))
var tot = CALCULATE(COUNT(all_season_summary[id]))

return(
    (sum1+sum2)/(tot*2))

### 4) Rank of umpire(measure)
Rank of umpire = IF(RANKX(ALL(all_season_summary[tv_umpire]),CALCULATE(COUNT(all_season_summary[tv_umpire])))<11,CALCULATE(COUNT(all_season_summary[tv_umpire])),BLANK())

### 5) Season Table (measure)
   1) Q_T_1 = CALCULATE(VALUES(points_table[name]),points_table[rank]= 1,points_table[season]=SELECTEDVALUE(points_table[season]))
   2) Q_T_2 = CALCULATE(VALUES(points_table[name]),points_table[rank]= 2,points_table[season]=SELECTEDVALUE(points_table[season]))
   3) Q_T_3 = CALCULATE(VALUES(points_table[name]),points_table[rank]= 3,points_table[season]=SELECTEDVALUE(points_table[season]))
   4) Q_T_4 = CALCULATE(VALUES(points_table[name]),points_table[rank]= 4,points_table[season]=SELECTEDVALUE(points_table[season]))

## DAX (Measure) for (Team Profile page)
### 1) Total count of venue(measure)
Total count of venue = CALCULATE(COUNT(all_season_summary[venue_name]),
                                 FILTER(all_season_summary,all_season_summary[home_team]= SELECTEDVALUE(points_table[short_name])||all_season_summary[away_team] = SELECTEDVALUE(points_table[short_name])))  
                                 
### 2) Winning % per ground
Winning % per ground = ([Winning Ground]/[Total count of venue])*100

### 3) Winning Ground
Winning Ground = CALCULATE(COUNT(all_season_summary[venue_name]),
                                 FILTER(
                                      FILTER(all_season_summary,all_season_summary[home_team]= SELECTEDVALUE(points_table[short_name])||all_season_summary[away_team] = SELECTEDVALUE(points_table[short_name])),all_season_summary[winner]=SELECTEDVALUE(points_table[short_name])))    

### 4) Top 5 Run Score(measure)
Rank of batsman = IF(RANKX(ALL(all_season_batting_card[fullName]),[Player_runs])<=5, [Player_runs], BLANK())

### 5) Top 5 Wicket Takers(measure)
Rank of bowlers = IF(RANKX(ALL(all_season_bowling_card[fullName]), [Bowlers Wickets])<=5, [Bowlers Wickets],BLANK())

### 6) Dynamic(measure)
Dynamic = 
var low = MIN(selected_over_range[selected_over_range])
var upp = MAX(selected_over_range[selected_over_range])
var selected = SELECTEDVALUE(Selected_Field[Selected_Field Order])
var selected_l = MIN(Season[Season])
var selected_u = MAX(Season[Season])
return(
       IF(selected=0,
            CALCULATE(SUM('Team Profile'[Boundary]),
                FILTER('Team Profile','Team Profile'[over]>=low && 'Team Profile'[over]<=upp && 'Team Profile'[season]>=selected_l && 'Team Profile'[season]<=selected_u)),
    
       IF(selected=1,
            CALCULATE(SUM('Team Profile'[Runs]),
                FILTER('Team Profile','Team Profile'[over]>=low && 'Team Profile'[over]<=upp && 'Team Profile'[season]>=selected_l && 'Team Profile'[season]<=selected_u)),

         IF(selected=2,
            CALCULATE(SUM('Team Profile'[Wickets]),
                FILTER('Team Profile','Team Profile'[over]>=low && 'Team Profile'[over]<=upp && 'Team Profile'[season]>=selected_l && 'Team Profile'[season]<=selected_u))))))

### 7) Selected_Field
     1) Selected_Field = {
    ("Boundary", NAMEOF('Team Profile'[Boundary]), 0),
    ("over", NAMEOF('Team Profile'[over]), 1),
    ("Runs", NAMEOF('Team Profile'[Runs]), 2)
}

    2) selected_over_range = GENERATESERIES(1, 20, 1)

    3)Season = GENERATESERIES(2008, 2024, 1)

## DAX (Measure) for (Player Profile page)
### 1) Player Profile (measure)
     1) Player Profile balls =     CALCULATE(SUM(all_season_details[Balls]),all_season_details[bowler1_name]=SELECTEDVALUE(all_season_details[bowler1_name]),all_season_details[batsman1_name]=SELECTEDVALUE(all_season_details[batsman1_name]),all_season_details[season]=SELECTEDVALUE(all_season_details[season]))

      2) Player Profile Boundary = CALCULATE(SUM(all_season_details[Boundary]),all_season_details[bowler1_name]=SELECTEDVALUE(all_season_details[bowler1_name]),all_season_details[batsman1_name]=SELECTEDVALUE(all_season_details[batsman1_name]),all_season_details[season]=SELECTEDVALUE(all_season_details[season]))

      3) Player Profile Economy = 
var ballsss = CALCULATE(SUM(all_season_details[Balls]),all_season_details[bowler1_name]= SELECTEDVALUE(all_season_details[bowler1_name]),all_season_details[batsman1_name]=SELECTEDVALUE(all_season_details[batsman1_name]),all_season_details[season]=SELECTEDVALUE(all_season_details[season]))

var runsss = CALCULATE(SUM(all_season_details[runs]),all_season_details[bowler1_name]=SELECTEDVALUE(all_season_details[bowler1_name]),all_season_details[batsman1_name]=SELECTEDVALUE(all_season_details[batsman1_name]),all_season_details[season]=SELECTEDVALUE(all_season_details[season]))

return((runsss)/(ballsss/6))

    4) Player Profile Runs = CALCULATE(SUM(all_season_details[runs]),all_season_details[bowler1_name]=SELECTEDVALUE(all_season_details[bowler1_name]),all_season_details[batsman1_name]=SELECTEDVALUE(all_season_details[batsman1_name]),all_season_details[season]=SELECTEDVALUE(all_season_details[season]))

    5) Player Profile Strike Rate = 
var balls = CALCULATE(SUM(all_season_details[Balls]),all_season_details[bowler1_name]=SELECTEDVALUE(all_season_details[bowler1_name]),all_season_details[batsman1_name]=SELECTEDVALUE(all_season_details[batsman1_name]),all_season_details[season]=SELECTEDVALUE(all_season_details[season]))

var runs = CALCULATE(SUM(all_season_details[runs]),all_season_details[bowler1_name]=SELECTEDVALUE(all_season_details[bowler1_name]),all_season_details[batsman1_name]=SELECTEDVALUE(all_season_details[batsman1_name]),all_season_details[season]=SELECTEDVALUE(all_season_details[season]))

return((runs/balls)*100)

    6) Player Profile Wickets = CALCULATE(SUM(all_season_details[wickets]),all_season_details[bowler1_name]=SELECTEDVALUE(all_season_details[bowler1_name]),all_season_details[batsman1_name]=SELECTEDVALUE(all_season_details[batsman1_name]),all_season_details[season]=SELECTEDVALUE(all_season_details[season]))

### 2) Top 5 Bowlers
Top 5 bowlers = IF(RANKX(ALL(all_season_details[bowler1_name]),[Wickets_Player_Profile])<=5 ,[Wickets_Player_Profile],BLANK())

### 3) Top 5 batsman
Top 5 batsman = IF(RANKX(ALL(all_season_details[batsman1_name]),[Runs Player Profile])<=5,[Runs Player Profile],BLANK())

### 4) Top 5 hitters
Top 5 hitters = IF( RANKX(ALL(all_season_details[batsman1_name]),[Boundary Player Profile])<=5,[Boundary Player Profile],BLANK())

### 5) total boundary(measure of Tooltip)
total boundary = 
var hometeam = CALCULATE(SUM(all_season_summary[home_boundaries]))
var awayteam = CALCULATE(SUM(all_season_summary[away_boundaries]))

return(hometeam+awayteam)

# Project Structure

│ ├── IPL-Overview.png
│ ├── Team-Profile.png
│ ├── Player-Profile.png
│ └── Player-Extras.png
├── data/ # (optional) raw/clean data files or sample CSVs
├── docs/ # (optional) notes, DAX snippets, wireframes
├── IPL_Dashboard.pbix # Power BI report
└── README.md

# How to Use

> Download the Power BI file containing the dashboard.

> Open the file in Power BI Desktop.

> Use the interactive filters and visuals to explore insights:

> Select seasons, teams, or players to view specific details.

> Analyze trends using graphs and charts for deeper understanding.

# Key Insights

> Top-performing teams: Historical performances of teams like Mumbai Indians, Chennai Super Kings, and others.

> Venue statistics: Analysis of winning percentages across various stadiums like Eden Gardens, Wankhede, and more.

> Player highlights: Top batsmen and bowlers for each season.

> Trends: Patterns in runs scored, boundaries hit, and bowler contributions.

# Future Enhancements

> Integration with live IPL data for real-time updates.

> Advanced analytics for predictive insights.

> Enhanced visualizations using custom visuals and maps.

> Player comparison over multiple seasons.

# Contributions

Feel free to suggest improvements, raise issues, or contribute to this project by submitting pull requests.

# Acknowledgements

Inspiration from the cricket analytics community

Thanks to data providers and open‑source contributors

 # Contact

Md Shahid Afridi
Email: mdshahidafridi9060@gmail.com

# Tags

Power BI · Data Visualization · Cricket Analytics · IPL · Sports Analytics

