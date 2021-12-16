

# SQL PROJECT 

## SQL Queries 


The objective of this project was to analyze player retention across the last year of our video game. We were interested in what percentage of players were retained across each day of the year, as well as exploring any other interesting insights related to player retainment.

The metric we decided to use to determine retention was that a player played a game 30 days after joining the game.

### Query 1: Retention Table

The first step was to determine which players were considered retained. This was done utilizing a SQL query containing a CASE statement. If a player had played a game 30 or more days after joining they were assigned “1”. If they had not played a game after 30 days of joining they were assigned a “0”. Once I had this table, I was able to SUM the number of players retained and group it out by day of the year. With the number of players joined and the number of those players who were retained, I divided the two columns to arrive at a retention percentage. The table was then grouped by day joined and ordered 1-365. An important part of this query was a WHERE statement to remove any players who joined after day 335. Since some of these players wouldn’t have the chance to be considered retained according to our metric, I felt that they were distorting some of the visualizations. Perhaps if more data is collected in January of 2022, this question can be revisited to get a more accurate representation of the end of the year. 

### Query 2: Player Spending

One particular interest of mine was player spending. I wanted to see if retained players were spending more money on in-game items than their unretained counterparts. Building off of query 1, I joined my “retention table” to the purchase_info and item_info table. This allowed me to aggregate and pull the columns I needed from all four tables. I ended up with three columns: player_renention with two rows, one for retained and one for non-retained players,  the total spent in millions, and average spending per player in each grouping. 


### Query 3: Age Groups 

For my final query, I wanted to see how the retention percentage looked across the player ages. Like my previous query, I began with my “retention table”. I nested it in an outer query that would pull out the age column for my final table. After that, I followed a similar methodology as in my first table to achieve a retainment percentage. Although this time I grouped by age to aggregate the data. 


 ## Insights/Recommendations

I feel that I was able to uncover some interesting and tangible insights from the data we were given. I showed that player retention is a good marker for player spending as well as an even retention rate across the ages of our players.

I was quite surprised that non-retained players were spending more per player on average than retained players. One possible reason for this could be the phenomenon of “Pay to Win” gaming. It could be that in-game items are creating a gaming experience that becomes stale too quickly. This could become a deterrent as well for players who can’t afford the items to be competitive. It might be worth investigating the in-game items to see if they need to be adjusted at all. 

We also see a noticeable drop-off with players, both retained and non-retained, over the age of 30 and under the age of 15. Perhaps our match times are too long? This is why we are not seeing many players over 30 who might not have the time to commit to the current match time. Or, are the items available for purchase giving players too much of an advantage? Is this why we are seeing a lack of users under the age of 15 who might not have a disposable income.


With an overall retention average of 71% across the year, I would consider the first year successful. Although,  I feel a decision needs to be made. Do we want to grow our player base in those lower/upper age demographics? Or, continue to foster player retention in the groups that showed healthy numbers. If the company is solely concerned with increasing its profit, it seems would be more effective to pursue strategies to attract more new players rather than strategies to retain players. This is due to the fact retained players aren’t spending more on an individual level. I  believe as long as we stay curious, and listen to what our users and data are communicating. We will be able to create an incredible gaming experience for our players. 




