# nba analysis

* download core data and preserve as static files on the file system (stats + nbac)
  * this contains raw data for boxscores and play by play
* calculate season data such as leader boards
  * this can be calculated on the fly, but nba api also provides csv format for this data, but this data is overwritten daily so if you miss it, then you will have a gap in data, i think this is easy enough to calculate.
  * where should this data be stored, in memory or persisted to a storage system?
  * need some library to parse play by play data, find trends and help with analysis of games
  * depends on what types of queries are required, i think it makes sense to first list the queries that are of interest
* 

## statistics

* leaderboards (points, rebounds, assists, steals, blocks, etc)
* true PER and PER
* percentage of shots vs overall number
* plus/minus
* good players vs bad players, historically define who are good players or manually mark
* player rankings, should count wins/record
* impactful players, harmful players, off the chart players
* player rankings within a team
* player data over last n games or range of games
* player per game

## player, team, and game analysis

based on boxscore, play by play information, leaderboards, standings, news articles, social media, and
historical data, provide game analysis.  a step further would be to analyze trends in the league, playoff
picture, mvp candidates, rookie of the year, defensive player of the year, etc.

## play by play

* 

## leaderboards

* parse nba api data, but this is replaced daily, would be hard to have point in time statistics, for example, who was the point leader yesterday, it's always a clean re-write of the data, does there need to be a snapshot information to have a point in time calculation, if so, this needs to be calculated daily or some periodic method and preserved.  if this is the case, perhaps a daily job to sync and calculate the snapshot and preserve it.  preservation should all be done to the file system or some blob storage, but access to the data should be fast via memory and some more organized, query-able data structure.
* perhaps all this should be stored to json and saved per day, if no games that day then no change.

```
{
  "id": 247699,
  "name": "Lebron James",
  "games": {
    135235: {
      "gameId": 135235,
      "date": "2023-03-02 17:35:30",
      "points": 23,
      "rebounds": 12,
    },
    135236: {
      "gameId": 135236,
      "date": "2023-03-04 21:23:50",
      "points": 15,
      "rebounds": 7,
    },
  }

}
```

calculate from beginning always?  accummulate?  or both?

```
{
  "id": 247699,
  "name": "Lebron James",
  "pointsPerGame": 32.7,
  "reboundsTotal": 12.3,
}
```
