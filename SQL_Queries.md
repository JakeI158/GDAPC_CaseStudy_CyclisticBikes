# SQL Queries Used for Analysis
Mean ride lengths above 20 minutes (per month, excluding avg's for each day):
```SQL
SELECT months, max_ride_length, mean_ride_length, mode_day_of_week, count_rides_casual, count_rides_member, total_rides
  FROM full_year
 WHERE mean_ride_length > 20.00
   AND mean_ride_length != 0.00;
```
<br />

Mean ride lengths 20 minutes or lower (per month, excluding avg's for each day):
```SQL
SELECT months, max_ride_length, mean_ride_length, mode_day_of_week, count_rides_casual, count_rides_member, total_rides
  FROM full_year
 WHERE mean_ride_length <= 20.00
   AND mean_ride_length != 0.00;
```
<br />

Finding the most popular day of the week for all users (1-Sunday, 7-Saturday, etc.):
```SQL
SELECT TOP 1 mode_day_of_week AS most_popular_day
  FROM full_year
 GROUP BY mode_day_of_week
 ORDER BY COUNT(*) DESC;
```
<br />

Determining which months had a weekday (not weekend) as the most popular day:
```SQL
SELECT months, mode_day_of_week, total_rides
  FROM full_year
 WHERE mode_day_of_week != 1
   AND mode_day_of_week != 7;
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
