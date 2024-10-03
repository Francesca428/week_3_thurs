# Road traffic accidents in Edinburgh

Data vis challenge! Your task is to recreate the graph below.

The data can be found in the data directory, and it’s called `accidents` It covers all recorded accidents in Edinburgh in 2018 and some of the variables were modified for the purposes of this assignment. 

![Road traffic accidents in Edinburgh](./edi-accidents-1.png "Road traffic accidents in Edinburgh")

<cite>
    This exercise has been adapted from Mine Çetinkaya-Rundel, CC-BY-NC.
</cite>

#Step 1: Read the data
#Step 2: Filter the info we want
#Step 3: Mutate to add a new variable (weekend or weekday)
#Step 4: Plot -> facit wrap

library(here)
library(tidyverse)
library(lubridate)

accidents <- read_rds(here("data","accidents.rds"))

accidents_filtered <- accidents %>%
  select(c(day_of_week, time, severity, date))
  
accidents_filtered

accidents_filtered %>%
  mutate(weekday = day_of_week == "Monday" | day_of_week == "Tuesday" | day_of_week == "Wednesday" | day_of_week == "Thursday" | day_of_week == "Friday") +
  mutate(weekday = paste(""))

accidents_day <- accidents_filtered %>%
  mutate(day_type = ifelse(day_of_week %in% c("Saturday", "Sunday"), "Weekend", "Weekday"))
accidents_day

accidents_filtered %>%
  mutate(DayType = ifelse(wday(date) %in% c(1, 7), "Weekend", "Weekday"))


accidents_plot <- accidents_day %>%
  ggplot(mapping = aes(x = time,
  color = severity,
  fill = severity)) +
  geom_density(alpha = 0.4) +
  facet_wrap(~day_type, ncol = 1)

accidents_plot

