## Exercise answers

### Exercise 1

1. How many rows have a non-null value for `end_station` in the `journeys` table?
```sql
select count(end_station)
from journeys;
```

2. How many null values are there in the `note` column?
```sql
select count(*) - count(note)
from journeys;
```

3. What's the average tube journey fare?  What's the highest and lowest?
```sql
select avg(charge), min(charge), max(charge)
from journeys;
```

4. What happens if we apply `max` or `min` on the tube station names?  What about the times?
```sql
# max/min on string columns considers alphabetical ordering
# max/min on datetime columns looks at most recent/latest tiemstamps
select max(start_station), min(start_station), max(start_time), min(start_time)
from journeys;
```

### Exercise 2

1. Order the journeys by `balance` highest to lowest.
```sql
select *
from journeys
order by balance desc;
```

2. How many times have I travelled from Earls Court to Temple?  What was the average fare?
```sql
select count(*), avg(charge)
from journeys
where start_station = 'Earls Court'
and end_station = 'Temple';
```

3. How many unique journeys have I taken?
```sql
select distinct start_station, end_station
from journeys;
```

4. What can you say about the journeys where the end station is missing?  What about when balance is missing?
```sql
select *
from journeys
where end_station is null;

select *
from journeys
where balance is null;
```

### Exercise 3

1. What were the shortest and longest journeys I took?
```sql
select start_time, end_time, start_station, end_station, timestampdiff(minute, start_time, end_time)
from journeys
order by timestampdiff(minute, start_time, end_time) desc;
```

2. What was the average journey time to go from Earls Court to Temple?
```sql
select avg(timestampdiff(minute, start_time, end_time))
from journeys
where start_station = 'Earls Court'
and end_station = 'Temple';
```

3. Which day of the week have I used the tube the most?
```sql
select dayname(start_time), count(*)
from journeys
group by dayname(start_time)
order by count(*) desc;
```

4. Which hour of the day have I used the tube the most?
```sql
select hour(start_time), count(*)
from journeys
group by hour(start_time)
order by count(*) asc;
```

5. On which single day did I have the most journeys?
```sql
select date(start_time), count(*)
from journeys
group by date(start_time)
order by count(*) desc;
```

6. How many journeys have I taken which started before 8AM?  After 11PM?
```sql
select count(*)
from journeys
where hour(start_time) < 8;

select count(*)
from journeys
where hour(start_time) >= 23;
```

7. Which journey was the most expensive in terms of charge per minute?
```sql
select start_time, end_time, start_station, end_station, charge, charge / timestampdiff(minute, start_time, end_time)
from journeys
order by charge / timestampdiff(minute, start_time, end_time) desc;
```

8. From which single station have I visited the most number of unique stations?
```sql
select start_station, count(distinct end_station)
from journeys
group by start_station
order by count(distinct end_station) desc;
```
