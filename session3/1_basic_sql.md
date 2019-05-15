## Getting started with SQL

Most companies will store their data in a database - this is a structured set of tables with column names (like Excel spreadsheets) that have a _schema_ (defined relationships between different tables via similar columns).

The way we interact with these tables is with a programming language called _SQL_.

With SQL we can do the following and more:
* view the contents of tables
* filter tables by conditions
* compute basic statistics e.g. counts, averages, arithmetic etc.
* sort tables
* combine tables with joins

### Writing queries

In our database (called `personal`) we have two tables called `journeys` and `stations`.

Let's view the contents of `journeys`.  In the console type:
```sql
select * from journeys
```
Then click the green play button to execute the query (or cmd+enter as a shortcut).

Across the bottom you should see some data appear with 7 columns showing details about tube journeys.  `<null>` just means the data is missing for the cell.

In SQL `select` is the most common keyword you will use.  `*` means return every column in the table.  The name of the table, `journeys` should appear after the `from` keyword.

In SQL you use a semi-colon `;` to separate different queries.  The following queries will be run one after the other:
```sql
select * from journeys;
select * from stations;
```

In the DataGrip IDE, you can execute a particular query by highlighting it or placing your cursor on it.

We can view certain columns by stating those explicity after the `select` (separated by commas), instead of using `*`:
```sql
select start_station from journeys;
```

```sql
select start_station, end_station, charge from journeys;
```

We can count the number of rows in the table:
```sql
select count(*) from journeys;
```

A side note: SQL doesn't really care about linebreaks or indents like Python does.  The case of keywords also doesn't matter.  The above query is the same as:
```sql
SELECT count(*)
FROM journeys;
```

You can comment in SQL using the `#` symbol as with Python.

The `count` function ignores null values.  Try running:
```sql
select count(*), count(end_station)
from journeys;
```

We can compute basic statistics:
```sql
select sum(charge)
from journeys;

select max(balance), min(balance), avg(balance)
from journeys;

select charge * 100
from journeys;

select charge + balance
from journeys;
```

### Exercise 1

1. How many rows have a non-null value for `end_station` in the `journeys` table?
2. How many null values are there in the `note` column?
3. What's the average tube journey fare?  What's the highest and lowest?
4. What happens if we apply `max` or `min` on the tube station names?  What about the times?

### Ordering

To order the table in numerical or alphabetical order, we use the `order by` keyword:
```sql
select *
from journeys
order by start_station;

select *
from journeys
order by start_station desc;
```

### Filtering

To filter a table to only return rows that meet a certain condition we use the `where` keyword:
```sql
select *
from journeys
where start_station = 'Marylebone';

select *
from journeys
where charge >= 2;

select *
from journeys
where end_station is null;
```

We can then apply the aggregations as above:
```sql
select count(*), avg(charge)
from journeys
where start_station = 'Marylebone';
```

We can apply multiple conditions using the `and` and `or` keywords:
```sql
select *
from journeys
where start_station = 'Marylebone'
and end_station = 'Earls Court';

select *
from journeys
where start_station = 'Marylebone'
or start_station = 'Paddington';
```

The last query would be really cumbersome if we wanted to filter for multiple stations.  Instead we can use the `in` keyword:
```sql
select *
from journeys
where start_station in ('Marylebone', 'Paddington');
```

Note: SQL uses round brackets for lists unlike Python.

We can combine ordering and filtering.  The `where` clause always goes before the `order by` clause.
```sql
select *
from journeys
where start_station = 'Paddington'
and end_station = 'Earls Court'
order by charge desc;
```

We can find unique values of columns using the `distinct` keyword:
```sql
select distinct(start_station)
from journeys;
```

### Exercise 2

1. Order the journeys by `balance` highest to lowest.
2. How many times have I travelled from Earls Court to Temple?  What was the average fare?
3. How many unique journeys have I taken?
4. What can you say about the journeys where the end station is missing?  What about when balance is missing?

### Aggregating by group

So far we've applied aggregation functions (counts, averages etc.) on the whole table and with filters.  We can combine both aspects by applying aggregate functions on every unique value of a particular column at a time using the `group by` keyword.

```sql
select start_station, count(*)
from journeys
group by start_station;
```

The above query will return the number of journeys for each unique start station.  All columns in the `select` statement should be aggregate functions apart from the column we are grouping by.

If we wanted to sort the results by the count we can add a `order by` clause:

```sql
select start_station, count(*)
from journeys
group by start_station
order by count(*) desc;
```

We can group by multiple columns:
```sql
select start_station, end_station, count(*), sum(charge)
from journeys
group by start_station, end_station
order by sum(charge) desc;
```

If we wanted to add filters, the `where` clause goes before the `group by` clause:
```sql
select start_station, end_station, count(*), sum(charge)
from journeys
where end_station is not null
group by start_station, end_station
order by sum(charge) desc;
```

### Date/time functions

SQL has a [wide range](https://mariadb.com/kb/en/library/date-time-functions/) of date/time functions.
```sql
select start_time, year(start_time), month(start_time), day(start_time), dayname(start_time), hour(start_time), minute(start_time)
from journeys;
```

We can calculate the number of journeys I've made by year:
```sql
select year(start_time), count(*)
from journeys
group by year(start_time);
```

We can calculate how long journeys [took](https://mariadb.com/kb/en/library/timestampdiff/):
```sql
select start_time, end_time, start_station, end_station, timestampdiff(minute, start_time, end_time)
from journeys;
```

We can compare times against the current time use `now()`, `current_date()`, `current_time()`:
```sql
select now(), current_date(), current_time()
from journeys;

select start_time, datediff(now(), start_time)
from journeys;
```

### Exercise 3
1. What were the shortest and longest journeys I took?
2. What was the average journey time to go from Earls Court to Temple?
3. Which day of the week have I used the tube the most?
4. Which hour of the day have I used the tube the most?
5. On which single day did I have the most journeys?
6. How many journeys have I taken which started before 8AM?  After 11PM?
7. Which journey was the most expensive in terms of charge per minute?
8. From which single station have I visited the most number of unique stations?

**Answers for all exercises are [here](2_answers.md).**

**Next week:**
* Column aliases with `as`
* Advanced filtering with `like` and `between`
* Filtering group by results with `having`
* Joining tables
* If statements
* Subqueries
