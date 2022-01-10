# R (RStudio) Analysis and Visualizations Documentation
*Note: a file will be uploaded to the project to showcase the outputs of the code below*

Loading the necessary packages
```R
library(tidyverse)
library(skimr)
library(janitor)
```
<br />

Loading the data (this is the csv version of the "fullyear-tripdata" spreadsheet)
```R
full_year <- read_csv("fullyear-tripdata-R-v3.csv")
```
<br />

Quick review of the data
```R
head(full_year)
colnames(full_year)
skim_without_charts(full_year)
glimpse(full_year)
```
<br />

Making sure that the column names are unique and consistent
```R
clean_names(full_year)
```
<br />

Renaming the first column for better clarity
```R
full_year %>%
  rename(months = ...1)
```
<br />

Saving the updated full_year as a new data frame
```R
full_year_clean <- full_year %>%
  rename(months = ...1)
```
<br />

Taking a quick look at the new data frame
```R
View(full_year_clean)
```
<br />

Looking at a specific set of columns
```R
full_year_clean %>%
  select(months, mode_day_of_week, mean_ride_length, count_rides_casual, count_rides_member, total_rides)
```
<br />

Finding the months with mean ride lengths that are longer than 20 minutes
```R
full_year_clean %>%
  select(months, mode_day_of_week, mean_ride_length, count_rides_casual, count_rides_member, total_rides) %>%
  filter(mean_ride_length > 20.00) %>%
  filter(mean_ride_length != 0.00)
```
<br />

Finding the months with mean ride lengths that are 20 minutes or shorter
```R
full_year_clean %>%
  select(months, mode_day_of_week, mean_ride_length, count_rides_casual, count_rides_member, total_rides) %>%
  filter(mean_ride_length <= 20.00) %>%
  filter(mean_ride_length != 0.00)
```
<br />

Finding the most popular day of the week for all users
```R
full_year_clean %>%
  select(mode_day_of_week) %>%
  count(mode_day_of_week) %>%
  group_by(mode_day_of_week) %>%
  arrange(-n)
```
<br />

Determining which months had a weekday (not weekend) as the most popular day
```R
full_year_clean %>%
  select(months, mode_day_of_week, total_rides) %>%
  filter(mode_day_of_week != 1) %>% 
  filter(mode_day_of_week != 7)
```
<br />

Who took more rides on weekends? Casual riders or members?
```R
full_year_clean %>%
  select(months, mode_day_of_week, count_rides_casual, count_rides_member, total_rides) %>%
  group_by(mode_day_of_week) %>%
  filter(mode_day_of_week == 1 || mode_day_of_week == 7) %>%
  summarize(sum_casual = sum(count_rides_casual), sum_member = sum(count_rides_member), sum_total = sum(total_rides))
```
<br />

Who took more rides during the week? Casual riders or members? (Days 2-6)
```R
full_year_clean %>%
  select(months, mode_day_of_week, count_rides_casual, count_rides_member, total_rides) %>%
  group_by(mode_day_of_week) %>%
  filter(mode_day_of_week != 1 && mode_day_of_week != 7) %>%
  summarize(sum_casual = sum(count_rides_casual), sum_member = sum(count_rides_member), sum_total = sum(total_rides))
```
<br />

Creating the data visualizations
```R
library(scales)
```
<br />

Bar chart for total riders per month
```R
full_year_clean %>%
  mutate(months = factor(months, levels = c(months))) %>%
  ggplot(aes(x = months, y = total_rides, fill = months)) +
    geom_bar(stat = "identity") +
    theme(legend.position = "none") +
    theme(axis.text.y = element_text(angle = 45)) +
    scale_y_continuous(labels = comma)
```
<br />

Bar chart for mean ride length per month
```R
full_year_clean %>%
  mutate(months = factor(months, levels = c(months))) %>%
  ggplot(aes(x = months, y = mean_ride_length, fill = months)) +
  geom_bar(stat = "identity") +
  theme(legend.position = "none") +
  theme(axis.text.y = element_text(angle = 45))
```
