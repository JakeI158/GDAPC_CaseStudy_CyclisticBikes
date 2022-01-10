# SQL Queries Used for Analysis (Including Outputs)
Mean ride lengths above 20 minutes (per month, excluding avg's for each day):
```SQL
SELECT months, max_ride_length, mean_ride_length, mode_day_of_week, count_rides_casual, count_rides_member, total_rides
  FROM full_year
 WHERE mean_ride_length > 20.00
   AND mean_ride_length != 0.00;
```
```
months  max_ride_length  mean_ride_length  mode_day_of_week  count_rides_casual  count_rides_member  total_rides
21-Apr	47776.7          24.13             6                 136601              200629              337230
21-Aug  41629.17         21.63             1                 412671              391681              804352
21-Feb  30129.23         24.42             7                 10131               39491               49622
21-Jul  49107.15         24.22             7                 442056              380354              822410
21-Jun  55944.15         26.08             7                 370681              358914              729595
21-Mar  31681.65         22.87             7                 84033               144463              228496
21-May  53921.6          26.03             7                 256916              274717              531633
21-Sep  32858.53         20.52             7                 363890              392257              756147
```
<br />

Mean ride lengths 20 minutes or lower (per month, excluding avg's for each day):
```SQL
SELECT months, max_ride_length, mean_ride_length, mode_day_of_week, count_rides_casual, count_rides_member, total_rides
  FROM full_year
 WHERE mean_ride_length <= 20.00
   AND mean_ride_length != 0.00;
```
```
months  max_ride_length  mean_ride_length  mode_day_of_week  count_rides_casual  count_rides_member  total_rides
20-Dec  9740.98          15.97             4                 29997               101142              131139
21-Jan  19825.92         15.27             7                 18117               78717               96834
21-Nov  34997.72         14.82             3                 106929              253049              359978
21-Oct  40705.02         19.1              7                 257242              373984              631226
```
<br />

Finding the most popular day of the week for all users (1-Sunday, 7-Saturday, etc.):
```SQL
SELECT TOP 1 mode_day_of_week AS most_popular_day
  FROM full_year
 GROUP BY mode_day_of_week
 ORDER BY COUNT(*) DESC;
```
```
most_popular_day
7
```
<br />

Determining which months had a weekday (not weekend) as the most popular day:
```SQL
SELECT months, mode_day_of_week, total_rides
  FROM full_year
 WHERE mode_day_of_week != 1
   AND mode_day_of_week != 7;
```
```
months  mode_day_of_week  total_rides
20-Dec  4                 131139
21-Apr  6                 337230
21-Nov  3                 359978
```
<br />

Who took more rides on weekends? Casual riders or members?:
```SQL
SELECT mode_day_of_week AS day_of_week, SUM(count_rides_casual) AS casual, SUM(count_rides_member) AS member, SUM(total_rides) AS total_rides
  FROM full_year
 WHERE mode_day_of_week = 1
    OR mode_day_of_week = 7
 GROUP BY mode_day_of_week
 ORDER BY COUNT(*);
```
```
day_of_week  casual  member  total_rides
1            412671  391681  804352
7            1803066 2042897 3845963
```
<br />

Who took more rides during the week? Casual riders or members? (days 2-6):
```SQL
SELECT mode_day_of_week AS day_of_week, SUM(count_rides_casual) AS casual, SUM(count_rides_member) AS member, SUM(total_rides) AS total_rides
  FROM full_year
 WHERE mode_day_of_week != 1
   AND mode_day_of_week != 7
 GROUP BY mode_day_of_week
 ORDER BY COUNT(*);
```
```
day_of_week  casual  member  total_rides
3            106929  253049  359978
4            29997   101142  131139
6            136601  200629  337230
```
