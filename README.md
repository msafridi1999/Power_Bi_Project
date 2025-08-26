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

# DAX (Measure)
## 1. Rank of stadium =
IF(RANKX(ALL(all_season_summary[venue_name]),CALCULATE(COUNT(all_season_summary[venue_name])))<11,CALCULATE(COUNT(all_season_summary[venue_name])), BLANK())

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

