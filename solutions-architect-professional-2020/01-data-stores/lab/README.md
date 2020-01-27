## Challenge

1. Identify an open source data repository for air quality readings
2. Setup a way to query that data via AWS services and tools
3. What City had the highest average ozone (O3) reading on October 9 2018


```sql
SELECT AVG(value) as air, city FROM "default"."openaq"
where from_iso8601_timestamp(date.utc) between timestamp '2018-10-09 00:00:00.000' AND timestamp '2018-10-10 00:00:00.000' and parameter = 'o3' and country = 'US'
group by city
order by air desc
limit 5;
```