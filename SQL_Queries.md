# Queries 


## This query created the “player retention table”. This query will be used repeatedly as a foundation to build the next queries.



```sql
SELECT
 joined AS day_joined,
 COUNT(player_id) AS number_of_players_joined, --counts number of players
 SUM(player_retention) AS number_of_players_retained,--counts number of retained players
 ROUND(SUM(player_retention) / COUNT((player_id)),2) AS retention_percentage -- calculates retention %
FROM
 (SELECT
   p.player_id,
   p.joined,
   MAX(m.day) AS most_recent_match,
   CASE WHEN MAX(m.day) - p.joined >= 30 THEN 1 ELSE 0 END player_retention --CASE statement determines player retainment
 FROM
   `juno-sql-project.Game_Company_Data.match_info` m
   JOIN `juno-sql-project.Game_Company_Data.player_info` p
   ON m.player_id = p.player_id
 WHERE
   p.joined <= 335 --this WHERE function eliminates players who couldn't be considered retained or not
 GROUP BY
   p.player_id,
   p.joined)
GROUP BY
joined
ORDER BY -- orders table by day of the year
Joined
``` 
 
## 2(a). This query nests the first query inside of an outer query to determine player spending by retained and non-retained players: 

```sql
SELECT
   Player_Retention, -- to group retained and non-retained player 
   ROUND(SUM(i.price)/ 1000000,2) total_spent_in_millions, --- calculates total spent per group 
   ROUND(SUM(i.price) / COUNT(DISTINCT players),2) avg_spent_per_player --calculates average spend per player (retained/non-retained)
FROM (
   SELECT --- This is my previous query to get a table to calculate player retention 
     p.player_id AS players,
     p.joined,
     MAX(m.day) AS most_recent_match,
     CASE
       WHEN MAX(m.day) - p.joined >= 30 THEN 1
     ELSE 
     0
   END
     Player_Retention,
   FROM `juno-sql-project.Game_Company_Data.match_info` m
   JOIN `juno-sql-project.Game_Company_Data.player_info` p
        ON m.player_id = p.player_id
   WHERE p.joined <= 335
   GROUP BY
     p.player_id,
     p.joined) r --- Allis for retention table
    JOIN `juno-sql-project.Game_Company_Data.purchase_info` pu ---joining retention table to the purchase info and item info table to get spending info
        ON r.players = pu.player_id
    JOIN `juno-sql-project.Game_Company_Data.item_info` i
        ON pu.item_id = i.item_id
   GROUP BY
   	Player_Retention
 
```	
## 2(b). This is a query using a WITH clause instead of nested queries to get the same results as query 2(a). 
 
```sql
WITH retention AS (
 SELECT -- This temporary table determines if a player is retained or not
   p.player_id,
   p.joined,
   MAX(m.day) AS most_recent_match,
   CASE WHEN MAX(m.day) - p.joined >= 30 THEN 1 ELSE 0 END Player_Retention,
 FROM
   `juno-sql-project.Game_Company_Data.match_info` m
   JOIN `juno-sql-project.Game_Company_Data.player_info` p ON m.player_id = p.player_id
 WHERE
   p.joined <= 335
 GROUP BY
   p.player_id,
   p.joined
),
table_2 AS (
 SELECT --This temporary table determines the amounts players spend
   retention.player_id as players,
   retention.Player_Retention,
   SUM(i.price) total_spent
 FROM
   `juno-sql-project.Game_Company_Data.purchase_info` pu
   JOIN `juno-sql-project.Game_Company_Data.item_info` i ON pu.item_id = i.item_id
   JOIN retention ON retention.player_id = pu.player_id
 GROUP BY
   retention.player_id,
   retention.Player_Retention
)
SELECT --this query pulls what is needed from the first two tables to achieve the desired final result
 table_2.Player_Retention,
 ROUND(SUM(total_spent) / 1000000,2) total_spent_in_millions,
 ROUND(SUM(total_spent) / COUNT(players),2)
FROM
 table_2
GROUP BY
 table_2.Player_Retention
 ```
 
 
## 3. This query looks at what age groups are represented in the customer base. As well as calculating a retention rate across each age:

```sql
SELECT ---creates columns for: number of player, number of retained players, retention percentage 
   age,
   COUNT(player_id) AS number_of_players,
   SUM(player_retention) number_of_players_retained,
   ROUND(SUM(player_retention) / COUNT(player_id),3) retention_percentage
FROM
     (
       SELECT --table to calculate player retention 
         p.player_id,
         p.joined,
         MAX(m.day) AS most_recent_match,
         CASE WHEN MAX(m.day) - p.joined >= 30 THEN 1 ELSE 0 END player_retention,
         age
       FROM
         `juno-sql-project.Game_Company_Data.match_info` m
         JOIN `juno-sql-project.Game_Company_Data.player_info` p ON m.player_id = p.player_id
       WHERE
         p.joined <= 335
       GROUP BY
         p.player_id,
         p.joined,
         age
     )
GROUP BY
   age
 ORDER BY age
 ```
