# KX Academy Notes
<hr>



# [1.0] qSQL 
<hr>

[1.1] Loading CSV Files (with headers)

```q
/ load weather.csv
/ load smalltrips.csv

/ 1a. Read the weather CSV file to see what datatypes are in file

read0 `:weather.csv

"date,maxtemp,mintemp,avgtemp,departuretemp,hdd,cdd,precip,newsnow,snowdepth";
"2009-01-01,26,15,20.5,-12.9,44,0,0.00,0.0,0";
"2009-01-02,34,23,28.5,-4.8,36,0,T,T,0";

/ use the read0 `: function to return the contents of csv file as a list of strings
/ gives you an idea of what datatypes are contained in file
```

```q
/ 2a. Determine the datatypes of table

"date,maxtemp,mintemp,avgtemp,departuretemp,hdd,cdd,precip,newsnow,snowdepth";
"2009-01-01,26,15,20.5,-12.9,44,0,0.00,0.0,0";
"2009-01-02,34,23,28.5,-4.8,36,0,T,T,0";

2009-01-01 / date d
26 / long j
15 / long j
20.5 /float f
-12.9 / float f
44 / long j
0 / long j
0.00 / float f
0.0 / float f
0 / long j
```

```q
/ 3a. Load the weather csv with headers using 0:

weather:("DJJFFJJFFJ"; enlist ",") 0: `:weather.csv

date       | maxtemp | mintemp | avgtemp | departuretemp | hdd | cdd | precip | newsnow | snowdepth
----------------------------------------------------------------------------------------------------
2009-01-01 |    26   | 	  15   |	 20.5  | 	    -12.9    |	44 |   0 | 	 0.0 	|    0.0  |	    0
2009-01-02 |    34   |	  23   |	 28.5  |	     -4.8	   |  36 |	 0 |			  |         |     0
2009-01-03 |    38   |	  29   |	 33.5  |	      0.4    |	31 |	 0 |			  |         |     0
2009-01-04 |    42   |	  25   |	 33.5  | 	      0.5    |	31 |	 0 |	 0.0	|    0.0  |    	0
2009-01-05 |    43   |	  38   |	 40.5  |	      7.6	   |  24 |	 0 |	 0.0	|         |     0
2009-01-06 |    38   | 	  31   |	 34.5  |	      1.7	   |  30 |   0 |	0.08	|	        |     0
2009-01-07 |    38   | 	  31   |	 34.5  |	      1.8	   |  30 |   0 | 1.19	  |    0.0	|     0

/ also assigned the file to variable name = weather
```

```q
/ 1b. Read the smalltrips CSV file to determine what datatypes are in file

read0 `:smalltrips.csv

date,month,vendor,pickup_time,dropoff_time,duration,passengers,distance,start_long,start_lat,end_long,end_lat,payment_type,fare,surcharge,tip,tolls,total";

2009-01-01 / date D
2009-01 / month M
CMT / char C
2009-01-01D00:00:00.000000000 / timestamp P
2009-01 01D00:04:12.000000000 / timestamp P
0D00:04:12.000000000 / timespan N
1 / long J
1.3 / float F
-73.96592 / float F
40.77124 / float F
-73.94961 / float F
40.77706 / float F
CASH / char C
5.8 / float F
0 / float F
0 / float F
0 / float F
5.8 / float F
```

```q
/ 2b. Load the smalltrips csv with headers using 0:

smalltrips:("DMSPPNJFFFFFSFFFFF"; enlist ",") 0: `:smalltrips.csv

date       |  Month  |vendor|      pickup_time       |       dropoff_time      |   duration	
---------------------------------------------------------------------------------------------
2009-01-01 | 2009-01 | CMT  | 2009-01-01T00:00:00.00 |	2009-01-01T00:04:12.00 | 00:04:12.00
2009-01-01 | 2009-01 | CMT  | 2009-01-01T00:00:00.00 |	2009-01-01T00:05:03.00 | 00:05:03.00
2009-01-01 | 2009-01 | CMT  | 2009-01-01T00:00:02.00 |	2009-01-01T00:05:40.00 | 00:05:38.00
2009-01-01 | 2009-01 | CMT  | 2009-01-01T00:00:04.00 |	2009-01-01T00:03:08.00 | 00:03:04.00
2009-01-01 | 2009-01 | CMT  | 2009-01-01T00:00:07.00 |	2009-01-01T00:19:01.00 | 00:18:54.00
```

```q
/ 3. Load the partitioned DB

/ a partitioned DB means when data is stored to disk, it is partitioned into different folders
/ for ex, days might be stored into a new date folder
/ this partitioning allows kdb to perform very fast queries as a full database scan is not rquired to retrieve data
``` 

[1.2] Data Exploration

```q
/ 1. Check to see what tables are currently in database

tables []

`smalltrips`weather
```
```q
/ 2. Check how many records are in the smalltrips table

count smalltrips
1000
```

```q
/ 3. Check the schema of smalltrips (column names, types, foreign keys, attributes)

meta smalltrips

c	           | t | f | a
-------------------------
date	       | d		
month	       | m		
vendor       | s		
pickup_time  | p       s		
dropoff_time | p		
duration	   | n		
passengers   | j		
distance	   | f		
start_long	 | f		
start_lat	   | f		
end_long	   | f		
end_lat	     | f		
payment_type | s		
fare	       | f		
surcharge	   | f		
tip	         | f		
tolls	       | f		
total	       | f		
```

[1.3] qSQL

```q
/ 1. Show the table for smalltrips

select from smalltrips

date       |  Month  |vendor|      pickup_time       |       dropoff_time      |   duration	
---------------------------------------------------------------------------------------------
2009-01-01 | 2009-01 | CMT  | 2009-01-01T00:00:00.00 |	2009-01-01T00:04:12.00 | 00:04:12.00
2009-01-01 | 2009-01 | CMT  | 2009-01-01T00:00:00.00 |	2009-01-01T00:05:03.00 | 00:05:03.00
```

```q
/ 2. Retrieve the vendor, pickup times, and fares from smalltrips

select vendor, pickup_time, fare from smalltrips

vendor	pickup_time	fare
----------------------------------
CMT	2009-01-01T00:00:00.000000	5.8
CMT	2009-01-01T00:00:00.000000	5.4
CMT	2009-01-01T00:00:02.000000	5.8
CMT	2009-01-01T00:00:04.000000	4.6

```
```q
/ 3. Retrieve the date, month, vendor, passengers, fare, and tip
/ on the earliest date where the tip is greater than 20

select date, month, vendor, passengers, fare, tip from smalltrips where date = min date, tip > 20

date       |   month | vendor | passengers | fare | tip  
--------------------------------------------------------
2009.01.01 | 2009.01 |  DDS   |     1      | 33.3 | 33.8 
2009.01.01 | 2009.01 |  CMT   |     2      | 77.4 | 30   
2009.01.01 | 2009.01 |  CMT   |     1      | 47   | 95.45
2009.01.01 | 2009.01 |  CMT   |     1      | 112  | 23.6 
2009.01.01 | 2009.01 |  CMT   |     1      | 41.4 | 40   
```

```q
/ 4. Retrieve duration, total (fare + tip), fare and tip from smalltrips

select duration, total: fare + tip, fare, tip from smalltrips

duration	   total	fare	tip
-------------------------------
00:04:12.00	  5.8	   5.8	0.0
00:05:03.00	  5.4	   5.4	0.0
00:05:38.00	  5.8	   5.8	0.0
```

```q
/ 5. Count the number of trips on the earliest date and for 2 pax

select count i from smalltrips where date = min date, passengers = 2
270
```

```q
/ 6. Retrieve the payment type and fare on the earliest date

select payment_type, fare from smalltrips where date = min date

payment_type fare
-----------------
CASH         5.8 
CASH         5.4 
CASH         5.8 

```

```q
/ 7. Retrieve trips where dates are within 2009.01.10 and 2009.01.31
/ and save in a variable called jan09

jan09: select from smalltrips where date within 2009.01.10 2009.01.31

/ 8. How many records are in the filtered table?

count jan09
10420159
```
```q

/ 9. Retrieve payment type, fare on the earliest date and save as res2

res2: select payment_type, fare from smalltrades where date = min date
```
```q
/ 10. Retrieve the sum of fare and tips from jan09

select sum fare, sum tips from jan09

fare         tip    
--------------------
9.840941e+07 5005536

/ other built in aggregators:
/ sum
/ avg
/ med
/ min
/ max
/ count
```

```q
/ 11. Retrieve the min and max trip from Jan09

select minTip:min tip, maxTip: max tip from jan09

minTip maxTip
-------------
0      100   
```

[1.4] Grouping with By

```q
/ qSQL lets you group and aggregate separately
/ easiest way to group similar values is using the by clause

/ 1. Retrieve fares by vendor from Jan09

select fare by vendor from jan09

vendor| fare
------| ----------------
CMT   | 6.2   12.6   7 
DDS   | 10.9   8.9   9.7 
VTS   | 11.3  15.7  18.1
```

```q
/ 2. Retrieve the total fare and tips by vendor from Jan09

select sum fare, sum tip by vendor from jan09

vendor| fare         tip     
------| ---------------------
CMT   | 4.430574e+07 2059982 
DDS   | 6120686      273323.2
VTS   | 4.798299e+07 2672231 
```

```q
/ 3. Get the number of records per day from Jan09

select count i by date from jan09

date      | x     
----------| ------
2009.01.10| 483350
2009.01.11| 405075
2009.01.12| 414642
2009.01.13| 442543
```

```q
/ 4. What was the biggest tip for each company?

select max tip by vendor from Jan09

vendor| tip  
------| -----
CMT   | 93.22
DDS   | 100  
VTS   | 100  
```
```q
/ 5. What was the highest tip and avg tip per payment type?

select maxTip:max tip, avgTip:avg tip by payment_type from jan09

payment_type| maxTip avgTip     
------------| ------------------
CASH        | 82     0.00077795 
CREDIT      | 100    2.145682   
Dispute     | 11.25  0.01481096 
No Charge   | 13.35  0.006573717
```

[1.5] Using fby to avoid nested queries

```q
/ nested queries are commonly required in SQL
/ where filter criteria require aggregation in the context of another column
/ for ex, getting all records where ride duration is less than the average
/ for that vendor
```

```q
/ 1. Get the avg duration per vendor and save as resby

resby: select avgDuration: avg duration by vendor from jan09

/ 2. join this to the jan09 table, and retrieve where duration less than avg duration

select from jan09 lj resby where duration < avgDuration

ate       month   vendor pickup_time                   dropoff_time         ..
-----------------------------------------------------------------------------..
2009.01.10 2009.01 VTS    2009.01.10D00:00:00.000000000 2009.01.10D00:08:00.0..
2009.01.10 2009.01 VTS    2009.01.10D00:00:00.000000000 2009.01.10D00:04:00.0..
```

```q
/ can be simplified using fby
/ syntax is (aggregation; data) fby group

/ 3. Find the max fare from jan09 where the duration is less than the avg duration by vendor

select max fare from jan09 where duration < (avg;duration) fby vendor

fare
----
200 
```

```
/ 4. Which vendor has the largest number of trips for trips shorter than the avg duration by vendor?

select count i by vendor from jan09 where duration < (avg;duration) fby vendor;

vendor| x      
------| -------
CMT   | 2883225
DDS   | 402781 
VTS   | 3592460

select vendor from res7b where x = max x

vendor
------
VTS   
```

[1.6] Updating Existing Data

```q
/ 1. Retrieve the max number of passengers by vendor

select max passenger by vendor from jan09

vendor| passengers
------| ----------
CMT   | 5         
DDS   | 6         
VTS   | 113  
```

```q
/ 2. update max number of passengers to 5 and save to jan09

jan09: update passengers: 5 from jan09 where passengers > 5

/ updated passengers to 5 where passengers > 5
```
```q

/ 3. Double check your work by running the above query again

select max passengers by vendor from jan09

vendor| passengers
------| ----------
CMT   | 5         
DDS   | 5         
VTS   | 5

/ note that it's now 5 passengers
```

```q
/ 4. Add a new column to jan09 that calculates the weighted average fare per passenger

jan09: update wavgfare: passengers wavg fare from jan09

/ updating a new column name will append it to the table
/ wavg = built in function that takes weighted average of x wavg y
```

```q
/ 5. How can you check if the new column is properly appended (if table too big)

meta jan09

c           | t f a
------------| -----
date        | d    
month       | m    
vendor      | s    
pickup_time | p    
dropoff_time| p    
duration    | n    
passengers  | i    
distance    | f    
start_long  | f    
start_lat   | f    
end_long    | f    
end_lat     | f    
payment_type| s    
fare        | f    
surcharge   | e    
tip         | f    
tolls       | f    
total       | f    
wAvgfare    | f

/ using meta, you can see the wavgfare column at the end
```
```q
/ 6. Count the number of lines in Jan09

count jan09
10420159
```
```q
/ 7. Delete the trips that didnt take place (ie, no duration)

jan09: delete from jan09 where duration = 00:00:00.000

```

```q
/ 8. Now double check your work (use count again)

count jan09
10357181

/ records removed
```

[1.7] Temporal Arithmetic

```q

/ 1. Retrieve the pickup time, cast as seconds, minutes, and hours

select pickup_time, pickup_time.second, pickup_time.minute, pickup_time.hh from jan09

pickup_time                   second   minute hh
------------------------------------------------
2009.01.10D00:00:00.000000000 00:00:00 00:00  0 
2009.01.10D00:00:00.000000000 00:00:00 00:00  0 
2009.01.10D00:00:00.000000000 00:00:00 00:00  0 
2009.01.10D00:00:00.000000000 00:00:00 00:00  0

/ .second cast as seconds
/ .minute cast as minutes
/ . hh cast as hours
```

```q
/ 2. retrieve the total fare + tip by pickup time in minutes

select total: sum fare + tip by pickup_time.minutes from jan09

minute| total   
------| --------
00:00 | 81036.05
00:01 | 80896.72
00:02 | 81054.25
00:03 | 79761.3
```

```q
/ 3. Aggregate the number of rides in 15 minute buckets on 2009.01.01

select count i by 60 xbar pickup_time.minute from jan09 where date = 2009.01.01

minute| x    
------| -----
00:00 | 28975
01:00 | 24007
02:00 | 20202
03:00 | 15472
04:00 | 9721 
05:00 | 4288 
06:00 | 4309 
07:00 | 6303 
08:00 | 9735

/ since you're "counting" the number of rides, need to use count i
/ 60 xbar pickup_time.minute = groups pickup_time col into 60 min buckets
```

```q
/ 4. Show the largest tip for each 15 minute timespan during january

select max tip by 15 xbar pickup_time.minute from jan09

minute| tip  
------| -----
00:00 | 90   
00:15 | 68   
00:30 | 62   
00:45 | 100  
01:00 | 73.3
```

```q
/ 5. Break the above down by vendor

select max tip by 15 xbar pickup_time.minute, vendor from jan09

minute vendor| tip  
-------------| -----
00:00  CMT   | 35   
00:00  DDS   | 23.2 
00:00  VTS   | 90   
00:15  CMT   | 55   
00:15  DDS   | 55   
00:15  VTS   | 68
```


