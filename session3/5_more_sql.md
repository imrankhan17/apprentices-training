## Intermediate SQL continued

### Different types of joins

Last time we looked at using table joins.  There are several types of joins in SQL.  The default join type is the `inner join`:
```sql
select count(*)
from journeys as j
join stations as s
on j.start_station = s.station;

select count(*)
from journeys as j
inner join stations as s
on j.start_station = s.station;
```

Other types of joins include the `left join` and `right join`.  In a left join, the rows in the left hand table will remain there even if there isn't a match with the right hand table and vice versa for a right join.

```sql
# left join
select *
from journeys as j
left join stations as s
on j.start_station = s.station
order by start_time;

select count(*)
from journeys as j
left join stations as s
on j.start_station = s.station
order by start_time;

select count(*)
from journeys;

# right join
select *
from journeys as j
right join stations as s
on j.start_station = s.station
order by lon desc;
```

We can join multiple tables in the same query and the same table multiple times:
```sql
select *
from journeys as j
join stations as s1
on j.start_station = s1.station
join stations as s2
on j.end_station = s2.station;
```

We can apply filters to both the joined tables:
```sql
select *
from journeys as j
join stations as s1
on j.start_station = s1.station
join stations as s2
on j.end_station = s2.station
where s1.zone = '1'
and s2.zone = '2';

select start_station, end_station, count(*)
from journeys as j
join stations as s1
on j.start_station = s1.station
join stations as s2
on j.end_station = s2.station
where s1.zone = '1'
and s2.zone = '2'
group by start_station, end_station
order by count(*) desc;
```

### Exercise 1
1. How many rows are returned when you do a `left join` on `journeys` with `stations` and filter out the rows where `zone` is null?
2. What is the above query equivalent to?
3. How many stations on the tube network have I not started a journey at?
4. How many times have I started a journey in zone 2 and ended it in zone 1?  What were my most common journeys out of those?
5. What were my most common journeys in terms of zone to zone?

```sql
select start_station, end_station,
       3959 * acos(cos(radians(s1.lat)) * cos(radians(s2.lat))
            * cos(radians(s2.lon) - radians(s1.lon))
            + sin(radians(s1.lat)) * sin(radians(s2.lat))) as distance
from journeys as j
join stations as s1
on j.start_station = s1.station
join stations as s2
on j.end_station = s2.station
order by distance desc;

select start_station, end_station, start_time, end_time,
       timestampdiff(minute, start_time, end_time) as duration,
       3959 * acos(cos(radians(s1.lat)) * cos(radians(s2.lat))
            * cos(radians(s2.lon) - radians(s1.lon))
            + sin(radians(s1.lat)) * sin(radians(s2.lat))) as distance,
       3959 * acos(cos(radians(s1.lat)) * cos(radians(s2.lat))
            * cos(radians(s2.lon) - radians(s1.lon))
            + sin(radians(s1.lat)) * sin(radians(s2.lat))) / (timestampdiff(minute, start_time, end_time) / 60) as average_speed
from journeys as j
join stations as s1
on j.start_station = s1.station
join stations as s2
on j.end_station = s2.station
order by average_speed desc;
```

### Unions

It is possible to append tables one on top of each other using the `union all` keyword.  This increases the number of rows (as opposed to joins which increase the number of columns).
```sql
select start_station
from journeys
union all
select end_station
from journeys;

select count(*) * 2
from journeys;
```

The `union` keyword (as opposed to `union all`) will append the columns and return only distinct values:
```sql
select start_station as station
from journeys
union
select end_station as station
from journeys;

select start_station as station
from journeys
where start_station not like '%top%'
and start_station not like '%bus %'
union
select end_station as station
from journeys;
```

The `intersect` keyword is used to return results that are present in both queries:
```sql
select distinct start_station as station
from journeys
intersect
select distinct end_station as station
from journeys;
```

### String functions

SQL has a wide range of [functions](https://mariadb.com/kb/en/library/string-functions/) to allow you to manipulate string columns.

```sql
select start_station, character_length(start_station)
from journeys;

select start_station, end_station, concat(start_station, end_station)
from journeys;

select start_station, end_station, concat_ws(' - ',start_station, end_station)
from journeys;

select start_station, lower(start_station), upper(start_station)
from journeys;

select start_station, reverse(start_station)
from journeys;

select start_station, substring(start_station, 3)
from journeys;

select start_station, substring_index(start_station, ' ', 1), substring_index(start_station, ' ', -1)
from journeys;

select charge, format(charge, 2), round(charge, 0)
from journeys;

select end_station, ifnull(end_station, 'no end station')
from journeys;
```

### Subqueries

Subqueries can be used for more advanced filtering and joining.  Essentially you are using the results of one query and feeding them into another.  
```sql
select start_station, count(*)
from journeys
group by start_station
having count(*) >= 20;

select *
from journeys
where start_station in ('Earls Court', 'Edgware Road', ... )
```

```sql
select *
from journeys
where start_station in (

select start_station
from journeys
group by start_station
having count(*) >= 20);
```

Subqueries can have all the usual SQL clauses and functions.
```sql
select *
from journeys
where end_station not in (

select distinct start_station
from journeys);
```

### Exercise 2
1. Which start station has the longest name?
2. What's the most common first word of a station name?
3. Get all journeys where the end station is a station which I've ended at at least 30 times.

**Answers for all exercises are available [here](6_answers.md).**

**Even more advanced SQL:**
* Window functions
* DDL - creating tables, inserting data etc.
* Using SQL with Python
