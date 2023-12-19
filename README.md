# kx


1. Loading CSV Files (with headers)

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

2. Data Exploration

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

3. qSQL

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

select date, month, vendor, passengers, fare, tip from smalltrips where date = min date, tip >20

