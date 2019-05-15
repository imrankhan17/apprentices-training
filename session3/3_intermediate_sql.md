## More advanced SQL

### Aliases

We can give names or rename the columns that we have in our `select` statement using the `as` keyword:
```sql
select charge as fare
from journeys;

select start_station, count(*) as number_of_entries, avg(charge) as average_charge, max(charge) as `highest charge`
from journeys
group by start_station;
```

### Advanced filtering

Previously we used the `where` keyword to filter the table based on certain exact conditions.  At times we would want a more "fuzzy" filter where the condition is less exact.  For this we use the `like` keyword:

```sql
select distinct start_station
from journeys
where start_station like 'c%';

select distinct start_station
from journeys
where start_station like '%road%';

select distinct start_station
from journeys
where start_station like '%a';
```

Let's identify all the bus journeys:
```sql
select distinct start_station
from journeys
where start_station like '%bus%';

select distinct start_station
from journeys
where start_station like '%bus %';

select distinct start_station
from journeys
where start_station not like '%bus %';
```

We can filter numerical values with the `between` keyword:
```sql
select *
from journeys
where charge <= 2
and charge >= 1;

select *
from journeys
where charge between 1 and 2;
```

To filter the results of `group by` queries we use the `having` keyword:
```sql
select start_station, count(*)
from journeys
group by start_station
having count(*) >= 10;
```

The `having` clause acts on aggregations only and always goes after the `group by` clause.

### If statements

We can manipulate columns values base on conditions using the `if` keyword:
```sql
select charge, if(charge < 1.5, 'cheap', 'expensive')
from journeys;

select charge, if(charge < 1.5, 'cheap', 'expensive') as charge_status
from journeys;

select if(charge < 1.5, 'cheap', 'expensive') as charge_status, count(*)
from journeys
group by if(charge < 1.5, 'cheap', 'expensive');
```

### Exercise 1
1. Identify all `start_station` values with information about top-ups.
2. What were my most common top-up amounts?
3. Identify all journeys which are neither bus journeys nor top-ups.
4. How many entries took place between the months of June and August?
5. Use an `if` statement to identify how many times I've had a positive and negative balance.

### Joins

To combine and analyse data stored in different tables we use joins.  Joins rely on a key - this is a common column appearing in both tables.  We use the `join` key and aliases for the tables:
```sql
select *
from stations;

select *
from journeys as j
join stations as s
on j.start_station = s.station;

select start_station, end_station, zone
from journeys as j
join stations as s
on j.start_station = s.station;

select *
from journeys as j
join stations as s
on j.start_station = s.station
where zone = 1;

select zone, count(*)
from journeys as j
join stations as s
on j.start_station = s.station
group by zone;
```

### Exercise 2
1. How many journeys have I ended in zone 4?
2. How many journeys have I started in zone 1 (including stations in multiple zones)?
3. What's the northern/southern/eastern/western most stations I've started a journey from?

**Answers for all exercises are [here](4_answers.md).**

