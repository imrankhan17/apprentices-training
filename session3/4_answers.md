## Exercise answers

### Exercise 1

1. Identify all `start_station` values with information about top-ups.
```sql
select distinct start_station
from journeys
where start_station like '%top%';
```

2. What were my most common top-up amounts?
```sql
select charge, count(*)
from journeys
where start_station like '%top%'
group by charge
order by count(*) desc;
```

3. Identify all journeys which are neither bus journeys nor top-ups.
```sql
select distinct start_station
from journeys
where start_station not like '%top%'
and start_station not like '%bus %';
```

4. How many entries took place between the months of June and August?
```sql
select count(*)
from journeys
where month(start_time) between 6 and 8;
```

5. Use an `if` statement to identify how many times I've had a positive and negative balance.
```sql
select if(balance < 0, 'negative', 'positive') as balance_status, count(*)
from journeys
group by if(balance < 0, 'negative', 'positive');
```

### Exercise 2

1. How many journeys have I ended in zone 4?
```sql
select count(*)
from journeys as j
join stations as s
on j.end_station = s.station
where zone = '4';
```
2. How many journeys have I started in zone 1 (including stations in multiple zones)?
```sql
select count(*)
from journeys as j
join stations as s
on j.start_station = s.station
where zone like '%1%';
```
3. What's the northern/southern/eastern/western most stations I've started a journey from?
```sql
# northern
select start_station
from journeys as j
join stations as s
on j.start_station = s.station
order by lat desc
limit 1;

# southern
select start_station
from journeys as j
join stations as s
on j.start_station = s.station
order by lat asc
limit 1;

# eastern
select start_station
from journeys as j
join stations as s
on j.start_station = s.station
order by lon desc
limit 1;

# western
select start_station
from journeys as j
join stations as s
on j.start_station = s.station
order by lon asc
limit 1;
```
