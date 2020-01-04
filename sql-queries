
-- står at datasett oppdateres hver dag, men den siste datoen som ligger inne er 31. mai 2018

--1. What is the most popular start station and end station?
--avg number of trips starting per station per day, assume that only stations that have been open longer than 14 days are representative of demand
select 
    count(*)/active_days as avg_num_of_trips_per_day, 
    start_station_name
from `bigquery-public-data.new_york_citibike.citibike_trips` t1
inner join 
    (select 
        date_diff(cast(max(starttime) as date), cast(min(starttime) as date), day) as active_days, 
        max(starttime) as max_time,
        start_station_id
    from `bigquery-public-data.new_york_citibike.citibike_trips`
    where starttime is not null --exclude blank rows
    group by start_station_id) t2
on t1.start_station_id = t2.start_station_id
and active_days>14
where starttime is not null  --exclude blank rows
--and extract(year from max_time) = 2018 --if old stations are not relevant
group by start_station_name, active_days
order by avg_num_of_trips_per_day desc 
limit 10


--avg number of trips ending per station per day, assume that only stations that have been open longer than 14 days are representative of demand
select 
    count(*)/active_days as avg_num_of_trips_per_day, 
    end_station_name
from `bigquery-public-data.new_york_citibike.citibike_trips` t1
inner join 
    (select 
        date_diff(cast(max(stoptime) as date), cast(min(stoptime) as date), day) as active_days, 
        max(stoptime) as max_time,
        end_station_id
    from `bigquery-public-data.new_york_citibike.citibike_trips`
    where stoptime is not null --exclude blank rows
    group by end_station_id) t2
on t1.end_station_id = t2.end_station_id
and active_days>14 
where stoptime is not null  --exclude blank rows
--and extract(year from max_time) = 2018
group by end_station_name, active_days
order by avg_num_of_trips_per_day desc 
limit 10




--2. What is the gender distribution of Citibike users?
-- gender distribution, include comments to see development
-- difficult to say if the difference in male and female is due to fewer female users or female users taking fewer trips in avg 
select 
    count(*) as num_of_trips, 
--    extract(year from starttime) as year,
    gender
from `bigquery-public-data.new_york_citibike.citibike_trips` t1
where starttime is not null  --exclude blank rows
--group by gender, year
order by year desc


--3. What is the trip duration distribution of Citibike trips?
-- Here we could also check the development of duration distribution thorugh time 
-- Plot it as a histogram, alt one per year and compare the shapes
-- Trips between 6-7 minutes are the most popular
select 
    count(*) as number_of_trips, 
    case when floor(tripduration/60) >= 90 then 90 --cut the tail at 90 minutes
         else floor(tripduration/60) end 
         as tripduration_in_minutes
from `bigquery-public-data.new_york_citibike.citibike_trips`
where starttime is not null  --exclude blank rows
group by tripduration_in_minutes
order by tripduration_in_minutes 

--4. What is the most popular Citibike trip?
-- this gives the most popular trip in terms of number of trips. Could also look at the avg number of trips per day
--some of the most popular routes are mostly people with 24-hour og 7-day cards which I assume often are tourists
-- there are probably great seasonal variations due to tourism
select 
    count(*) as number_of_trips, 
    start_station_name,
    end_station_name,
    --usertype
from `bigquery-public-data.new_york_citibike.citibike_trips`
where starttime is not null--exclude blank rows
--and extract(year from starttime) in (2017,2018)  --
group by start_station_name, end_station_name--, usertype
order by number_of_trips desc
limit 10

--5. Were there new bike stations introduced or removed at any point in time? What makes you think it did or didn’t?
/* yes, there are both stations coming and going based on the usage. I assume that if there are no trips for more than 14 day
the station has been removed. Stations can be closed for festivals etc. therefor this limit*/








--most popular start stations 2018
select start_station_name,
  start_station_latitude,
  start_station_longitude,
  extract(year from starttime),
  COUNT(*) AS num_trips
from `bigquery-public-data.new_york_citibike.citibike_trips` 
where extract(year from starttime) = 2018
group by 
1,
2,
3,
4
ORDER BY
  num_trips DESC
Limit 10

--most popular end stations 2018
select end_station_name,
  end_station_latitude,
  end_station_longitude,
  extract(year from stoptime),
  COUNT(*) AS num_trips
from `bigquery-public-data.new_york_citibike.citibike_trips` 
where extract(year from stoptime) = 2018
group by 
1,
2,
3,
4
ORDER BY
  num_trips DESC
Limit 10


--Number of trips per start station per year
select 
    count(*) as num_of_trips, 
    start_station_name, 
    extract(year from starttime) as year, 
    extract(month from starttime) as month 
from `bigquery-public-data.new_york_citibike.citibike_trips`
group by extract(year from starttime), extract(month from starttime),start_station_name




--truncate data
select count(*) from `bigquery-public-data.new_york_citibike.citibike_trips`
where starttime is null --5 828 994

select count(*) from `bigquery-public-data.new_york_citibike.citibike_trips`
where starttime is not null --53 108 721