## Exercise answers

### Exercise 1

1. How many rows are returned when you do a `left join` on `journeys` with `stations` and filter out the rows where `zone` is null?
```sql
select count(*)
from journeys as j
left join stations as s
on j.start_station = s.station
where zone is not null;
```

2. What is the above query equivalent to?
```sql
# an inner join
select count(*)
from journeys as j
inner join stations as s
on j.start_station = s.station;
```

3. How many stations on the tube network have I not started a journey at?
```sql
select *
from journeys as j
right join stations as s
on j.start_station = s.station
where start_station is null;
```

4. How many times have I started a journey in zone 2 and ended it in zone 1?  What were my most common journeys out of those?
```sql
select count(*)
from journeys as j
join stations as s1
on j.start_station = s1.station
join stations as s2
on j.end_station = s2.station
where s1.zone = '2'
and s2.zone = '1';

select start_station, end_station, count(*)
from journeys as j
join stations as s1
on j.start_station = s1.station
join stations as s2
on j.end_station = s2.station
where s1.zone = '2'
and s2.zone = '1'
group by start_station, end_station
order by count(*) desc;
```

5. What were my most common journeys in terms of zone to zone?
```sql
select s1.zone, s2.zone, count(*)
from journeys as j
join stations as s1
on j.start_station = s1.station
join stations as s2
on j.end_station = s2.station
group by s1.zone, s2.zone
order by count(*) desc;
```

### Exercise 2

1. Which start station has the longest name?
```sql
select start_station, character_length(start_station)
from journeys
where start_station not like '%top%'
and start_station not like '%bus %'
and start_station not like '%entered%'
order by character_length(start_station) desc;
```

2. What's the most common first word of a station name?
```sql
select substring_index(station, ' ', 1), count(*)
from stations
group by substring_index(station, ' ', 1)
order by count(*) desc;
```

3. Get all journeys where the end station is a station which I've ended at at least 30 times.
```sql
select *
from journeys
where end_station in (
    select end_station
    from journeys
    where end_station is not null
    group by end_station
    having count(*) >= 30
);
```
