# Data Cleaning
Tools used throughout the project:
- Excel
- SQL (MS SQL Server)
- R (RStudio)
- Tableau (for visualization)

Excel:
- All times are formatted in HH:MM:SS
- Created some initial calculated columns:
  - ride_length (ended_at - started_at)
  - day_of_week (started_at, 1 = Sun, 7 = Sat)
- When calculating the mean ride length, there were some ride times that were negative (meaning that the data was entered incorrectly) which Excel cannot handle
  - This only applied to Dec 2020
  - Removed all rows with ride_length of ####### (about 6,322 rows)
  - Now all calculations are in line with the other months
- Column real_average (=AVERAGE(N:N)) represented the final mean ride length calculation after fixing the bad data in Dec 2020
- Calculated the max ride length in the month and the mean for each day of the week
- Created a pivot table to quickly view the number of rides casual riders vs members take in that month
- Pivot table to compare average ride length of casual riders vs members
- Pivot table to compare average ride length per day of casual riders vs members
- Added additional columns:
  - count_rides_casual (=COUNTIF(M:M, "casual"))
  - count_rides_member (=COUNTIF(M:M, "member"))
  - total_rides (=(COUNTA(A:A))-1)
  - avg_ride_casual (=AVERAGEIF(M:M, "casual", N:N))
  - avg_ride_member (=AVERAGEIF(M:M, "member", N:N))

- Created a fresh spreadsheet that combines all the calculations above to summarize the years' worth of data (named "fullyear-tripdata")
  - Columns:
    - max_ride_length
    - avg_ride_sun through avg_ride_sat (all 7 days)
    - mean_ride_length (taken from the real_average column in the individual months)
    - mode_day_of_week
    - count_rides_casual
    - count_rides_member
    - total_rides
    - avg_ride_casual
    - avg_ride_member
  - Rows:
    - Months: Dec-20 through Nov-21
  - Worksheets:
    - fullyear_raw
    - fullyear_color_v1
    - full_year_converted (times converted to minutes in decimal format for ease of use in SQL, R, and Tableau)
    - full_year_color_v2 (times converted to minutes in decimal format for ease of use in SQL, R, and Tableau)
