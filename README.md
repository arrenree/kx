# KX Academy Notes
<a name="top"></a>

1. [qSQL](#qsql)
2. [Joins](#joins)
3. [Data Structures](#data)
4. [Functions](#functions)
5. [Loading Data](#load)
6. [Atoms & Primitives](#atoms)
7. [Lists](#Lists)

<hr>

<a name="qsql"></a>
### 🔴 [1.0] qSQL
[Top](#top)

<hr>

🔵 [1.1] Loading CSV Files (with headers)

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

🔵 [1.2] Data Exploration

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

🔵 [1.3] qSQL

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

🔵 [1.4] Grouping with By

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

🔵 [1.5] Using fby to avoid nested queries

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

🔵 [1.6] Updating Existing Data

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

🔵 [1.7] Temporal Arithmetic

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

<hr>

<a name="joins"></a>
### 🔴 [2.0] Joins
[Top](#top)

```q
/ a join combines data from 2 tables, or from a table and dict
/ some joins are keyed; cols in first arg are matched with key cols of second arg
/ some joins are as-of; time column in first arg specifies corresponding intervals in a time col of second arg
```

```q
/ 1. Display max and min temp each week from weather csv

select max maxtemp, min mintemp by 7 xbar date from weather

date      | maxtemp mintemp
----------| ---------------
2008.12.27| 34      15     
2009.01.03| 43      25     
2009.01.10| 41      9      
2009.01.17| 47      6      
2009.01.24| 46      13     
2009.01.31| 27      20
```

🔵 [2.1] Left Join

```q
/ 1. From the trips csv file, retrieve all trips in January, name this jan09

jan09: select from trips where date within 2009.01.01 2009.01.31

/ retrieves all trips within january
```

```q
/ 2. Count the number of trips per day from Jan09, call this jan09C

jan09C: select trips: count i by date from jan09

date      | trips 
----------| ------
2009.01.01| 327625
2009.01.02| 376708
2009.01.03| 432710
2009.01.04| 367525
2009.01.05| 370901
2009.01.06| 427394
2009.01.07| 371043
2009.01.08| 477502

/ note the above table is a keyed table
```

```q
/ How to Key tables:

`date xkey weather  / keying on date
1!weather           / keying first column
3! weather          / keying first 3 columns
0!                  / unkey table (remove all keys)

/ lj operator requires RIGHT table to be keyed

```

```q
/ 3. select date and precipitation from weather table
/ key the result on date
/ join to the jan09C table

select date, precip from weather / retreieves date and precip columns

`date xkey select date, precip from weather / keys date column from result

jan09W:jan09C lj `date xkey select date, precip from weather  / joins jan09C with resulting table

date      | trips  precip
----------| -------------
2009.01.01| 327625 0     
2009.01.02| 376708       
2009.01.03| 432710       
2009.01.04| 367525 0     
2009.01.05| 370901       
2009.01.06| 427394 0.08  
2009.01.07| 371043 1.19
```

```q
/ 4. Join the number of trips with avg temp from weather per day for month of jan

select date, avgtemp from weather 
/ retrieves date avgtemp from weather

`date xkey select date, avgtemp from weather
/ keys the date column

jan09C lj `date xkey select date, avgtemp from weather

date      | trips  avgtemp
----------| --------------
2009.01.01| 327625 20.5   
2009.01.02| 376708 28.5   
2009.01.03| 432710 33.5   
2009.01.04| 367525 33.5   
2009.01.05| 370901 40.5   
2009.01.06| 427394 34.5   
2009.01.07| 371043 34.5   

/ joins the number of trips from jan09C to the avg temp from weather
```

🔵 [2.2] As-of Join

```q
/ As-of joins is a powerful time series joins within q
/ aj[matching columns; t1; t2]

/ 1. Create a time table with 3 passengers and 30 minute times called timetab
 
timetab:([] passengers:1 2 3; event_time:2009.01.06D03:30:00+00:30*til 3)

passengers event_time                   
----------------------------------------
1          2009.01.06D03:30:00.000000000
2          2009.01.06D04:00:00.000000000
3          2009.01.06D04:30:00.000000000

/ note this table has 2 columns, passengers and event_time
```

```q
/ reminder - jan09 is a huge table
/ can extract column names using meta

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
```

```q
/ 2. Using aj, look up table jan09 to find what was the LAST trip taken at each of the times above with those pasengers
/ use pickup_time as event_time reference

aj[`passengers`event_time;timetab;select passengers, event_time:pickup_time, vendor, pickup_time from jan09]

/ aj[matching columns; t1; t2]
/ matching columns = passengers, event_time (appear on both tables)
/ t1 = timetab (table you created above)
/ t2 = retrieve passengers, event_time:pickup_time, vendor, and pickup_time from jan09
/ notice you rename pickup_time from jan09 as event_time to match t1

passengers event_time                    vendor pickup_time                  
-----------------------------------------------------------------------------
1          2009.01.06D03:30:00.000000000 VTS    2009.01.06D03:30:00.000000000
2          2009.01.06D04:00:00.000000000 VTS    2009.01.06D04:00:00.000000000
3          2009.01.06D04:30:00.000000000 CMT    2009.01.06D04:29:22.000000000
4          2009.01.06D05:00:00.000000000 CMT    2009.01.06D04:59:54.000000000
5          2009.01.06D05:30:00.000000000 VTS    2009.01.06D05:30:00.000000000
6          2009.01.06D06:00:00.000000000 VTS    2009.01.06D05:59:00.000000000

/ the result is the record for each vendor with time_event < to the time we specifieid
/ an aj join will always select the last record before the specified time
```

```q
/ 3. Find latest trips as of 09:30 on Jan 31 for each vendor

/ 3a. First, create timetab timeseries with vendors = VTS, DDS, CMT
/ and pickup times on 2009.01.31 @ 09:30 

timetab:([] vendor: `VTS`DDS`CMT; pickup_time:3#2009.01.31D09:30:00)
timetab

vendor pickup_time                  
------------------------------------
VTS    2009.01.31D09:30:00.000000000
DDS    2009.01.31D09:30:00.000000000
CMT    2009.01.31D09:30:00.000000000

/ 3b. Then use aj join to join this table with jan09
/ using vendor and pickup time as matching columns

aj[`vendor`pickup_time;timetab;jan09]

/ aj[matching columns; t1; t2]
/ vendor, pickup_time = matching columns (on both tables)
/ timetab = t1
/ jan09 = t2

vendor pickup_time                   date       month   dropoff_time         ..
-----------------------------------------------------------------------------..
VTS    2009.01.31D09:30:00.000000000 2009.01.31 2009.01 2009.01.31D09:41:00.0..
DDS    2009.01.31D09:30:00.000000000 2009.01.31 2009.01 2009.01.31D09:35:17.0..
CMT    2009.01.31D09:30:00.000000000 2009.01.31 2009.01 2009.01.31D09:38:56.0..

/ result is a joined table whereby it pulls in the latest result from jan09
/ based on the timeseries from timetab
```
<hr>

<a name="data"></a>
### 🔴 [3.0] Data Structures
[Top](#top)


🔵 [3.1] Lists

```q
/ you can simply retrieve a column from a table as a list

/ 1. Retrieve fares from trips on 2009.01.01 for vendor VTS
/ assign the result to variable vtsfares

vtsfare: select fare from trips where date = 2009.01.01, vendor = `VTS

fare
----
5.7 
4.9 
4.9 
4.5 
4.9 
15.3
3.7 
8.5 
13.3
11.3

/ retrieved single column (fare) from table trips
```

```q
/ since tables in kdb are column oriented, columns can be extracted by indexing

/ 2. Retrieve fares as a list from table vtsfare using indexing
/ assign the results to variable fares

fares: vtsfares`fare
5.7 4.9 4.9 4.5 4.9 15.3 3.7 8.5

/ indexing method to retrieve a list from a table
/ syntax is tablename`col_name
```

```q
/ 3. Check the datatype for fares

type fares
9h

/ 9h means simple list
/ simple list = list with all same datatypes
/ positive number means list (neg means atom)
```

```q
/ 4. Create a general list with sym VTS and float 23.45
/ general list = different datatypes (aka mixed list)

general: (`VTS;23.45)
```

```q
/ 5. Check the datatype for general

type general
0h

/ general lists always have type zero
```

🔵 [3.2] Casting

```q
/ casting is to convert one type to another

/ 1. show 3 ways to convert ints 1 2 to floats

`float$ 1 2 / converts using symbol name
"f"$ 1 2 / converts using character letter
9h$ 1 2 / converts using short value
1 2 f
```

```q
/ 2. Create an empty, general list

()

/ this creates an empty, general list
```

```q
/ 3. Cast the empty list to longs

`long$()
```

```q
/ 4. Retrieve fares as a list

fares
5.7 4.9 4.9 4.5 4.9 15.3 3.7...

/ can simply retrieve data as a list
/ by calling its varaible name (fares)
/ recall earlier you did this
/ fares: vtsfares`fare
```
```q
/ 5. Retrieve the 10 first rows of data from fares

10 sublist fares
5.7 4.9 4.9 4.5 4.9 15.3 3.7 8.5 13.3 11.3

/ sublist will retrieve the x number of items from a list
```
```q
/ 6. Retrieve the last 10 items from fares

-10 sublist fares
12.9 4.9 44.5 5.3 19.3 9.3 8.9 45 9.7 4.5

/ negative sublist will retrieve the LAST 10 numbs
```

```q
/ 7. Retrieve the second 10 elements in list

-10 sublist 20 sublist fares
22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1 5.7 8.5

/ so 20 sublist = retrieves 20 items
/ then -10 retrieves the LAST 10 items from the list of 20

/ alternative syntax:

10 10 sublist fares
```
```q
/ 8. What is the diff btwn sublist and take #

/ sublist only retrieves up to the list amount (does not loop)
/ while take keeps looping

a: 1 2 3 4 5

10 sublist a
1 2 3 4 5

/ 10 is longer than list (5), so stops at 5 items

10 # a
1 2 3 4 5 1 2 3 4 5

/ take loops back around the list
```
```q
/ 9. Sort fares by ascending order
/ then retrieve 10 items

10 sublist asc fares
2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5
```
```q
/ 10. Retrieve the 10 largest numbers from fares

-10 sublist asc fares
104 104 108.5 112.5 120 122 128 134.3 190 200

/ asc sorts from low to high
/ so taking the last 10 = 10 largest from list
```

🔵 [3.3] Obtaining Random Data

```q
/ 1. Create sortedFares, which is fares from low to high

sortedFares: asc fares

/ 2. Retrieve 10 random records from sortedFares
/ and assign as sampleFares

10?sortedFares
9.7 9.3 4.1 6.5 6.5 5.7 5.7 8.9 45 7.3
```

```q
/ 3. Retrieve the 10th element from sampleFares

sampleFares[9]
11.3

/ indexing starts at 0, so 10th element = 9
```

```q
/ 4. Retrieve the first 10 elements of fares using til

fares[til 10]

/ til 10 = 0 - 9
/ so you will index a list of 0-9
```

```q
/ 5. Retrieve the 11th to 20th element from fares using til

fares[10 + til 10]

/ til 10 = 0 1 2 3 4 5
/ 10 + til 10 = 10 + every element in list
/ so becomes 10, 11, 12, etc.
```
```q
/ 6. Assume a random list of 10 numbers
/ Use indexing to find the middle value in fares

a: 10?100
12 10 1 90 73 90 43 84 63

/ generate a random list of 10 numbers

a:asc a 

/ sort list from small to big

a [`long$(count a)%2]
73

/ count a = total number of elements %2
/ then cast as a long
/ doesn't work if you try to cast a float
```
```q
/ 7. What's the difference between 1 sublist and first?

1 sublist fares
,2.5

/ returns a list

first sublist
2.5

/ returns an atom
```
```q
/ 8. Use enlist to return a list containing 499

enlist 499
, 499

/ creates a list of a single element
```
```q
/ 9. Create a list by joining an empty list with 499

(), 499
, 499

/ can join a blank list with an atom
/ to create a single item list
```

🔵 [3.4] @ Operator

```q
/ 1. Generate a random list of 20 items from 0-99

a: 20? 100

/ 2. Retrieve elements 0 2 4 6 8 from a using indexing

a[2*til 5]
a[0 2 4 6 8]
93 38 88 68 2

/ til 5 = 0 1 2 3 4 
/ 2 * 0 2 4 6 8
```

```q
/ 3. Retrieve the same, using the @ operator

@[a; (2*til 5)]
93 38 88 68 2

/ syntax is @ [list; indexing]
```

```q
/ 4. Create b, list from 1 to 10

b: 1 + til 10
1 2 3 4 5 6 7 8 9 10
```

```q
/ 4b. Retrieve elements 0, 2, 4, 6, 8 from b using @ operator

@[b;(2*til 5)]
1 3 5 7 9

/ 2 * til 5 = 0 2 4 6 8
/ so from b, retrieves element using indexing
```

```q
/ 4c. Retrieve list b, but replace elements 0, 2, 4, 6, 8 with 100

@[b;(2* til 5); : ;100]
100 2 100 4 100 6 100 8 100 10

/ retrieves list b (1 2 3 ...10)
/ but replaces those indexed elements with 100
/ the : replaces the indexed result with 100
```

```q
/ 4d. Retrieve list b, but subtract 1 from elements 0, 2, 4, 6, 8

@[b;(2* til 5); - ; 1]
0 2 2 4 4 6 6 8 8 10

/ so b = 1 2 3 4 5 6 7 8 9 10
/ 1 - 1 = 0
/ 3 -2 = 2
/ etc

/ same works with + instead of -
/ note these changes are NOT persistent
/ if you want to save the changes, use backtick

@[`b;(2* til 5); - ; 1]
```
```q
/ 5. cast list b to floats
/ then replace index elements 0, 2, 4, 6, 8 with nulls (0Nf)

b: `float$b / cast list b as floats

@[`b;(2*til 5);:;0Nf]
0n 2 0n 4 0n 6 0n 8 0n 10f

/ replaced index elements with 0N = nulls
/ note backtick b saves the result
```
```q
/ 6. Check if list b has any nulls

any null b
1b

/ 1b = true
```
```q
/ 7. Replace any nulls in b with average of list b

@[b; where null b;:;avg b]
6 2 6 4 6 6 6 8 6 10f

/ note you can use "where null b"
/ aka, where there are nulls in b
/ replace with avg of b
/ can perform operations on replacement!
```

🔵 [3.5] @ Dictionaries

```q
/ dictionaries are first class objects in q
/ also known as hashmaps in other languages
/ use ! operator to create
```

```q
/ 1. Create a dictionary with syms a, b with 0 and 1

d: `a`b ! 0 1
a| 0
b| 1
```

```q
/ 2. update the value of a with 2

d[`a]: 2

a| 2
b| 1
```
```q
/ 3. add `c with value 3 to your dictionary

d[`c]:3

a| 2
b| 1
c| 3

/ indexing a new key will append it + value
```
```q
/ 4. create d1:

d1: `a`b`c`d ! 5 6 7 8
a| 5
b| 6
c| 7
d| 8
```
```q
/ 5. join d and d1 together, adding values for common keys

d + d1

a| 7
b| 7
c| 10
d| 8

/ matches on key, then adds value together
/ a = 2 + 5 = 7
/ b = 1 + 6 = 7
/ c = 3 + 7 = 10
/ d = null + 8 = 8
```
```q
/ 6. join d and d1 together, update values for matched keys, insert values for new keys

d, d1

a| 5
b| 6
c| 7
d| 8

/ match on a, replaces with 5 (from d1)
/ match on b, replaces with 6 (from d1)
/ match on c, replaces with 7 (from d1)
/ no match on d, inserts new record (from d1)
```

🔵 [3.6] @ Tables

```q
/ 1. Create a table from a list of like dictionaries

`a`b! 0 1

Key |	Value
----------
a   |	  0
b	  |   1

/ simple dictionary with keys a, b

/ if you create a list of dict with same keys:

(`a`b! 0 1; `a`b! 2 3)

a | b
------
0 | 1
2 | 3

/ notice the keys a and b become column headers
/ and the corresponding values fall into table
/ so a table is a flipped dictionary
```

```q
/ 2. Create a table from flipping a dictionary

flip `a`b! (0 2; 1 3)

a | b
------
0 | 1
2 | 3

```

```q
/ 3. Create dict with keys a, b, c and assign each key a list of 3 random ints

dict: `a`b`c! (3?10i; 3?10i; 3?10i)

a| 1 4 0
b| 0 8 5
c| 9 7 2

/ 4. Add new key d, with double values of key a

dict[`d]: 2*dict[`a]

a| 1 4 0
b| 0 8 5
c| 9 7 2
d| 2 8 0

/ when you index a new key for dict
/ it adds the key
/ dict[`a] will retrieve the VALUES for key a
/ and multiply by 2 for each
```

```q
/ 4. make a table from dict

tab: flip dict

a b c d
-------
1 0 9 2
4 8 7 8
0 5 2 0
```

```q
/ 5. Make a new table by joining the table to itself

tab2: tab, tab

a b c d
-------
1 0 9 2
4 8 7 8
0 5 2 0
1 0 9 2
4 8 7 8
0 5 2 0

```

```q
/ 6. Key column b in new table, rename as keytab

keytab: `b xkey tab2

/ syntax is col_name xkey table_name
```

```q
/ 7. Compare the types of dict, tab, and keytab

type each (dict;tab;keytab)
99 98 99h

/ 99 = dict
/ 98 = table
/ 99h = keyed table
```

<a name="qsql"></a>
### 🔴 [4.0] Functions
[Top](#top)

```q
/ functions are a list of expressions
/ separated by semicolons and encased by curly brackets

/ to "call" a function = run it with your inputs/arguments
/ functions are called with arguments in square brackets [ ]

max [10 11 12] / functional notation
12

/ with unary functions (single argument), we can omit the brackets
max 10 11 12 / infix notation
12
```

```q
/ 2. Create a binary function called speed

speed: {[miles;hours] mph: miles%hours; kph: 1.609*mph; :kph; }

/ binary function = 2 arguments
/ miles and hours are the arguments
/ you can define variables within function like mph
/ kph is a local variable we defined inside func
/ we can explictly return it using :
/ requires semi colons between expressions
```

```q
/ 3. Call function speed with args 15 and 0.5

speed[15;0.5]
48.27

```

```q
/ 4. If there is no explicit return from a function
/ its result is the last expression
/ re-write above with no return

speed: {[miles;hours] 1.609*miles%hours}
speed[15;0.5]
48.27

```

🔵 [4.2] Calling Functions with List of Arguments

```q
/ 4. You can call a function with a list of arguments
/ recall your arguments are mile and hours

speed[15 20 30; 0.5 1.0 1.5]
48.27 32.18 32.18

/ this is important!
/ you called a LIST of miles (15 20 30)
/ and a LIST of hours (0.5 1.0 1.5)
```

🔵 [4.3] Explicit and Implicit Parameters

```q
/ in the above speed function, miles and hours are explicit arguments
/ these are also known as explicit parameters

/ if function has less than 3 arguments
/ their names can be ommited
/ and x, y, and z are used as implicit arguments

/ 1. Re-write speed using implicit arguments

speed:{1.609*x%y}

/ replaced miles and hours with x and y
```

```q
/ 2. Call speed with 2 distances and a single duration

speed[15 30; 0.5]
48.27 96.54

```

```q
/ 3. Create a function that will find the area of a rectangle with length 7.93 and width 1.87 using implicit parameters

func:{x*y}
func[7.93;1.87]
```

🔵 [4.4] Call Functions from qSQL

```q
/ 1. from smalltrips, retrieve all trips in January

jan09:select from smalltrips where date within 2009.01.01 2009.01.31

/ filters dates in jan
```

```q
/ 2. Retrieve speed, distance, and duration for VTS
/ jan09 is a table, and in addition to retrieving data from columns
/ you can call functions in sQSL queries

select spd:speed[distance;duration % 0D01:00], distance, duration from jan09 where vendor=`VTS

spd      | distance |      duration             
-------------------------------------------
 48.5918 |   1.51   |  0D00:03:00.000000000 
-17.3772 |   1.26   | -0D00:07:00.000000000
14.28792 |   0.74   |  0D00:05:00.000000000 

/ spd is a FUNCTION (speed) and the arguments are column inputs
/ note you are also changing duration from timespan to hours
/ can use meta to confirm datatype of duration

```q
/ 3. Retrieve the avg speed by vendor
/ avg speed = total 

select avgspeed:speed[sum distance;sum[duration]%0D01:00] by vendor from jan09

vendor| avgspeed
------| --------
CMT   | 24.43787
DDS   | 22.21592
VTS   | 22.33302

/ avg speed calc as total distance * total duration
/ need to convert duration to hours (and float)
```

```q
/ 4. Create func called tipOverDistance that divides argument x by argument y

tod:{x%y}

/ 5. Create func called createtable that retrieves vendor, distance, tip, and adds new col from result of tipoverdistance applied to cols tip and distance where the distance is creater than 1 mile

createtable: {select vendor, distance, tip, tipPerDist:tod[tip;distance] from jan09 where distance > 1.0}

/ so you create this function called createtable
/ which is essentially a table
/ to display the output (table) of your function

/ just do createtable[]

createtable[]

vendor |	distance |	tip |	tipPerDist
-----------------------------------
CMT    |	   1.3   |	0.0 |  	 0.0
CMT	   |    5.5   |	0.0	|    0.0
CMT	   |    2.1	  | 0.0	|    0.0
CMT	   |    3.7  	| 0.0	|    0.0
```

```q
/ 6. Find the avg tip per mile per vendor from createtable

select avg tipPerDist, avg distance, avg tip by vendor from createTable[]

/ you are simply retrieving the averages of the columns from createtable[]
```

🔵 [4.5] Iterators

```q
/ an iterator is an operator that modifies how a function is applied
```

```q
/ 1. say we want to add 1 2 + 3 4 5

1 2 + 3 4 5
error

/ length error
/ the add operator iterates implictly, but expects its arguments to be atoms or have matching lengths
/ in this instance, we can use each right or each left
```

🔵 [4.6] Each Left

```q
/ syntax:
x f\: y

/ equivalent:
f[ ; y] each x

/ will function EACH LEFT ARG
/ to entire RIGHT
/ the top of the \ is pointing LEFT

1 2 +\: 3 4 5

4 5 6
5 6 7

/ so adds 1 to 3 4 5
/ then adds 2 to 3 4 5
```

🔵 [4.7] Each Right

```q
/ syntax:
x f/: y

/ equilvalent:
f[x; ] each y

/ will function EACH RIGHT arg
/ to entire LEFT
/ top of the / points RIGHT

1 2 +/: 3 4 5
4 5
5 6
6 7

/ adds 3 to 1 2
/ adds 4 to 1 2
/ adds 5 to 1 2
```

🔵 [4.8] Each

```q
/ 1. Create list l as a list of chars

l: ("the";"quick";"brown";"fox")
```
```q
/ 2. Count the number of elements in l

count l
4

/ there are 4 elements in the list
```

```q
/ 3. How many elements are within each list?

count each l
3 5 5 3

/ counts the number of chars within each list
```

🔵 [4.9] Each Both

```q
/ each both adds every element of x to every element of y
/ x,'y

/ 4. Create two lists x: 1 2 3 4, y: 10 20 30 40 and join them item by item, returning a pair of lists (type 0h

x: 1 2 3 4
y: 10 20 30 4 

x ,'y
(1 10;2 20;3 30;4 4)

/ each both joins every element of x to every element of y
/ returning a pair of lists
```

🔵 [4.10] Accumulating iterators

```q
/ map iterators apply a function across arguments
/ accumulator iterators are applied repeatedly to results of successive evaluations

/ scan (\) - shows all results. scan battlefield, so perched high in a defensive position \.

/ over (/) - only shows final result. Jump over the wall. so / easier to jump over. think "it's over" so only shows last result
```

```q
n: 1 3 6 9

+/[n] / sum over
19 

+\[n] / sum scan
1 4 10 19
```

```q
/ different syntax formats:

function/[data]
(function/) data


/ Add these numbers, fold '+' over the vector
/ fold is sometimes called reduce or inject
(+/) 1 2 3 4
10

sum 1 2 3 4 / Another way to sum the values, using a built-in function
10
```

```q
/ cumulative sums (running sums)

(+\)1 2 3 4 / Cumulative sums, using scan
1 3 6 10

sums 1 2 3 4 / Same, using the built-in function
1 3 6 10
```
```q
/ 1. create function add, which adds x+x, then iterate this list across 3 6 8

add:{x+x}
add each 3 6 8
6 12 16

/ you can use each to iterate function across multiple arguments
```

```q
/ 2. Create a new function, add2:{x+y}, and iterate this list of integers across it, (3 6 8;4 7 9) so that the first value of each list is added together

add2:{x+y}
add2'[3 6 8; 4 7 9]
7 13 17

/ alternative syntax
'[add2][3 6 8; 4 7 9]

/ so you use 'each
/ to feed EACH element of 2 lists into add2 function
```
```q
/ 3. Multiple each value in this list, 3 5 4 2, against the value 11. Use the over iterator

11 */ 3 5 4 2
1320

/ 3b. create a function that performs the same

{x*y}/[11;3 5 4 2]

/ so your "function" is x*y
/ and you're feeding in x = 11 and y = 3 5 4 2
/ the over iterator performs the function through entire arg list
```

🔵 [4.11] Fibonacci Case Study

```q
/ 4. Create a func that generates the first 10 elements of the fibo sequence
/ each num is sum of preceding 2 numbers
/ starts with 0 1

/ assume x is list of numbers

sum -2#x
/ take last 2 elements of x, add them together

x, sum -2#x
/ joins the result of sum of last 2 elements with list x

{x,sum -2#x} 0 1
/ Create function, test by using list of 0 and 1 as arg
/ so x = 0 1 (together)
/ RIGHT TO LEFT
/ takes list of 0 1, adds last 2 elements = 1
/ then takes x (0 1) and joins 1
/ hence 0 1 1

fib:{x, sum -2#x}
/ names function (remove testing)
 
fib[0 1 1 2 3 5]
0 1 1 2 3 5 8

/ test your function by feeding fibo sequence
/ so fib = calculates the NEXT number in sequence

/ use the "do" form of over
/ to repeat the function 10x
 
fib/[10;0 1]
0 1 1 2 3 5 8 13 21 34 55 89

/ so this says, repeat 10x
/ and start x as 0 1
```

<a name="atoms"></a>
### 🔴 [6.0] Atoms & Primitives
[Top](#top)

🔵 [6.1] Atoms

```q
/ an atom is a irreducible datatype in kdb/q
/ default number = long

/ if we want to create an atom of a datatype that isn't the default,
/ we can specify the type with a CHAR trailing type indicator (will be some letter)

type 30
-7

/ so default number = long = 7

/ 1b. now make 30 a real datatype using e

type 30e
-8h

/ now it comes a "real" = type 8

/ 1c. now make it a short

type 30h
-5h

/ now it comes a "short" = type 5
```

🔵 [6.2] Vectors/Lists

```q
/ a vector is a list of atoms

1 2 3
/ this is a list of longs
```

```q
/ a string is a list of chars

"thisisastring"

/ this is a string aka list of chars
/ encased with double parathesis
```

🔵 [6.3] Temporal Datatypes

```q
/ TIME datatype

type 09:30:00.000 
-19h / time datatype

/ SECOND datatype
type 09:30:00 
-18h / second datatype

/ MINUTE datatype
type 09:30
-17h / minute datatype
```

```q
/ KDB understands arithmetic between these types
/ and highest level of granularity is preserved

09:30:00.000 + 00:12
09:42.00.000

/ preseves time datatype

09:30:00 + 00:12
09:42.00

/ preserves second datatype
```

```q
/ DATES in KDB have format yyyy.mm.dd

type 2020.01.01
-14h

/ date datatype
```

```q
/ a TIMESTAMP is combination of time + date
/ the current timestamp is:

.z.p
2024-01-12T06:41:58.594829p
```

```q
/ 1. What is the current timestamp?

.z.p
2024-01-12T05:28:02.671597p

/ 1b. What datatype is this?

type .z.p
-12h

/ neg means atom
/ 12 h = timestamp
```

```q
/ 2. Show a null long

show nulllong:0Nj
0N

/ 2b. Show a null float

show nullfloat: 0Nf
0n
```

🔵 [6.4] Dataype Nulls and Infinite Values

```q
/ each datatype has an associated null and infinite value

type 0N / null long
type 0w / infinite float
type 0Nf / null float
```

```q
/ can use null keyword to identify null values

null 0N 2 1 0N 3
10010b

/ returns a list of booleans
```

```q
/ return null type of minute

0nu

type 0nu
-17h

/ -17 = minute datatype

/ use null keyword to check

null 0nu
1b

/ true
```

🔵 [6.5] Boolean Comparisons & Built in Functions

```q
/ 1. Given the list 2 4 1 2 3, return a boolean list
/ if the element contains a value of 2
/ expected outcome: 10010b

2 = 2 4 1 2 3
10010b

/ by using the comparison operator
/ will compare left element to every element in right list
/ and return a list of booleans
```

```q
/ 2. Calculate the standard deviation of the series 1 2 3 4 5

dev 1 2 3 4 5
1.414

/ use dev for standard deviation
```

```q
/ 3. Find the difference in values in the series 2 8 4 9

deltas 2 8 4 9
2 6 -4 5
```

🔵 [6.6] Primitives

```q
/ 1. There are 140 calories in candy. Daily calorie intake guidance is 2000
/ how many bags should we eat a day?

2000 % 140
14.2857143

/ 2. How do you round to nearest bag?
/ hint - use the floor keyword

floor 2000 % 140
14

/ floor rounds down to the nearest whole number

/ 3. you can also use div the keyword

2000 div 140
14
```

```q
/ 2. how do you check if 1 is in list 1 2 3 4?

1 in 1 2 3 4
1b

/ is LEFT in RIGHT list?
```

```q
/ 3. how do you check if 1 2 are in list 2 3 4?

1 2 in 2 3 4
01b

/ are (these LEFT items) in (this RIGHT list)
```

```q
/ 4. Check what day of the week today is?

.z.d mod 7
6i

/ in KDB, the week "starts" on a saturday
/ so sat = 0, sun = 1, mon =2....fri = 6
/ so friday!

/ worth remembering that weekends are 0 1 (sat n sun)
```

🔵 [6.7] Function Notation

```q
/ Function - a sequence of expressions separated by semicolons
/ and surrounded by left and right braces

/ there are various ways to apply a function to its arguments

f[x]         / bracket notation
f x          / prefix
x + y        / infix
f\           / postfix
```

```q
/ Functional notation uses keyword functions
/ and applies to arguments on right

neg -10 -2
-10 -2

/ neg = keyword function
/ applies neg function to all args on right

/ alternative syntax

neg[10 2]
-10 -2
```
```q
/ 1. Check if 1 is in 2 3 using functional notation

1 in 2 3
0b
/ false

/ alternative syntax

in[1;2 3]
0b

```

```q
/ Variable Assignment

/ in KDB, once you assign a variable, you can continue using it

pi: 22%7
radius: 5
area: pi * radius * radius
area
78.57

/ once you assign 22%7 to pi, you can re-use the variable pi
```

🔵 [6.8] Functional Notation & Projecting in-built Primitives

```q
/ in-built kdb functions which take multiple inputs
/ can be bound to a given input, creating a projection

add2: 2+ / we dont define second input
add2: +[2; ] / functional notation - leave second parameter blank
add2: +[2] / alternative syntax; same thing

add2 4
6

/ takes 2 from original definition
/ and feeds in 4 from second input
```

```q
/ 1. Create an addition function using the + operator

add:+
add[5;5]
10
```

```q
/ 2. Create an equality function using the = built-in primitive

=[2;3]
0b

/ = will compare the 2 elements for equality
```

```q
/ 3. Create an addition function that already has one input defined (for ex, 4)

add4: 4+
add4 3
7

/ you can save built in primitives (+ or -) as a variable (add4)
/ then apply the variable to another input

/ 3b. Create an addition function using the [ ] syntax

add5: +[5]
add5 2
7

/ 3c. Here's an alternative syntax

add6: +[ ;6]
add6 3
9

/ 4c. This also works:

add7: +[7; ]
add7 1
8
```

```q
/ 4. Create a function to see if 2 arguments have same value and type

equality:~
equality[3;3]
1b

/ ~ match compares both value + type
/ since its a built-in primitive, you can use it compare 2 arguments
```

```q
/ 5. Create a func to return a booleans indicating whether an arg is positive
/ hint - you want to "bind" the second argument as 0
/ and feed in a list of values as your first argument

ispositive: >[ ; 0]
ispositive -4 2 8
011b

/ so you feed in list of elements (-4 2 8) as x
/ and your function checks if x if > 0 (your second arg)
```

```q
/ 6. Create a function that takes 2 away from a list of inputs

minus2: -[ ; 2]
minus2 10 9 8
8 7 6

/ first argument (x) = list of inputs (10 9 8)
/ second argument (y) = 2
```

```q
/ 7. Create a function that subtracts a list of inputs from 10

sub10:-[10; ]
sub10 1 2 3
9 8 7

/ so x stays constant at 10
/ and subjects a list of inputs y
```

```q
/ 8. Create a funct that takes a sole number and doubles it

double:2*
double 2 3 4
4 6 8
```

<a name="Lists"></a>
### 🔴 [7.0] Lists
[Top](#top)

```q
/ a dictionary is a collection of lists
/ a table is a list of dictionaries

/ what is the diff between "a" and "abc"

/ "a" is a char
/ "abc" is a string aka char vector aka list of chars

/ there are 2 types of lists: simple and general lists
```

```q
/ note in kdb, default number type are LONGs
/ if you want a list of ints, need to specify

ints:1 2 3i

/ create a list of 10 ints

ints2: `int$til 10

/ til 10 generates list of 10 longs
/ then cast to `int
```

🔵 [7.1] General Lists

```q
/ a general list is where not all elements are same type
/ or a list of lists
/ need to use brackets () and semicolon ; to delimit the diff items
/ general list has type 0h

person: (`john;32;`USA)
/ a general list of a sym, long, and sym

type person
0h

/ type will check the type of entire list
/ aka general list = 0h

/ to check the type of each item within the list, use EACH

type each person
-11 -7 -11h

/ sym, long, sym
```
🔵 [7.2] ? Operator

```q
/ ? can either be used as a random generator
/ or find

/ if atom ? atom = randomly generate x items frm list y
/ if list ? atom = find y element from list x
```

```q
/ if atom ? atom = random generator
/ general x random elements from y list

?[5;10]
4 8 2 4 3
/ generate a list of 5 random longs between 0-9

/ alternative syntax

5?10
4 8 2 4 3
```

```q
/ canont generate more items than in list

11 ? 10
error

/ cannot generate 11 items from 10 elements
```

```q
/ can generate random data from a list

? [5; 1 2 3]
1 3 1 3 3

/ generate 5 elements from list 1 2 3
/ notice elements will repeat

/ alternative syntax

5 ? 1 2 3
1 3 1 3 3

/same thing
```
```q
/ 1. Generate 5 dates from the 3 dates leading up to 2023.01.01

5 ? 2011.01.01-til 3
2011.01.01 2010.12.30 2011.01.01 2010.12.31 2010.12.30
```
```q
/ 2. Generate 100 random numbers form 10-20
100 ? 10 + til 11

/ 10 + til 11 = 10...20
```

```q
/ ? as FIND operator

/ if list ? atom = FIND y atom in list x

1 2 3 4 5 ? 5
?[1 2 3 4 5; 5]
4

/ 5 first appears in index position 4
```
```q
/ 1. Find the first occurence of 5 and 1

1 2 3 4 5 ? 5 1
4 0
```





🔵 [7.3] Joining Lists and Indexing to Retrieve

```q
/ we can join lists using ,

1 2 , 3 4 5
1 2 3 4 5
```

```q
list: 10 20 30 40 50
list 0
10

/ retrieve index element 0 from list

/ alternative syntax
/ written explicitly (functionally)

list[0]
10
```
```q
/ indexing out of bounds

list[11]
0N

/ does not throw out of bounds exception
/ returns a null value of same type
/ 0N = because list is longs

z:`ibm`msft`goog
z[10]
`

/ null sym

y: 1 2 3e
y[10]
0Ne

/ null real
```
🔵 [7.4] Indexing Nested Lists / Matrix

```q
/ 1. Create a matrix with 9 elements using CUT
/ where you multiple base element by 3

m: 3 cut 3 * til 9
0  3  6 
9  12 15
18 21 24

/ 3 * til 9 = multiples 3 with every element from 0-8
/ CUT keyword cuts up list by left hand arg (3)
```
```q
/ 2. Retrieve the first row

m[0]
0 3 6

/ index 0 retrieves first ROW
```
```q
/ 3. Retrieve 3 6 from first row

m[0][1 2]
3 6

/ index 0 = first row
/ then retrieve index position 1 and 2
```
```q
/ 4. What is the difference between m[0;1] and m 0 1

m[0;1]
3

/ retrieve index row 0 = first row
/ then retrieve index element 1 = second element = 3

m 0 1
0  3  6 
9  12 15

/ retrieves entire rows 0 and 1
```
```q
/ 5. Retrieve the first item from all lists

m[ ; 0]
0 9 18

/ dont specify first arg (row)
/ and only retrieves second arg (column)
/ goes [ROW, COLUMN]
```

🔵 [7.5] List Manipulation

```q
# take

k: 1 2 3 4 5

2 # k
1 2

/ # take first 2 elements of list k

-2 # k
4 5 

/ # take last 2 elements of list k
```
```q
_ drop

1_k
2 3 4 5

/ drops first element of k

-1_k
1 2 3 4

/ drops last element of k
```
```q
sublist

/ sublist = first item = index position start
/ second item = number of items to retrieve

2 3 sublist 1 2 3 4 5
3 4 5

/ start from index position 2 (which is 3)
/ retrieve 3 elements (3, 4, 5)

0 6 sublist "KDB is fun"
"KDB is"

/ starts index 0
/ retrieves 6 elements
```
```q
/ 1. What's the diff between take and sublist?

8# til 5
0 1 2 3 4 0 1 2

8 sublist til 5
0 1 2 3 4

/ take "loops around" and repeats
/ while sublist DOES NOT loop nor repeats
```
```q
k: 1 2 3 4 5 6

/ 1. retrieve the last 2 items from k

-2 # k
5 6

/ 2. retrieve the second from last item of k

(-2#k) 0
5

/ first retrieve last 2 items of k
/ then use indexing to retrieve 0 index position
```
```q
k: 1 2 3 4 5

/ 3. Retrieve 2 and 3 from k

1 2 sublist k
2 3

/ 4. Retrieve index position 1 and 2 from k

k[1 2]
2 3
```


