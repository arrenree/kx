# KX Academy Notes
<a name="top"></a>

1. [qSQL](#qsql)
2. [Joins](#joins)
3. [Data Structures](#data)
4. [Functions](#functions)
5. [Loading Data](#load)
6. [Atoms & Primitives](#atoms)
7. [Lists](#Lists)
8. [String Manipulation](#string)
9. [Casting](#casting)
10. [Functions](#func)
11. [Iterators](#iterators)
12. [Function Control](#funccontrol)
13. [Dictionaries](#dict)
14. [Tables](#tables)


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

<a name="functions"></a>
### 🔴 [4.0] Functions
[Top](#top)


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

🔵 [7.6] Where Operator in Lists

```q
where 000111b
3 4 5

/ where operator in list returns indices where booleans are TRUE (1b)
```
```q
/ WHERE is useful to find positions where certain criteria is met

k: 10?50
/ generate 10 random numbers from 0-49

k > 30 
0001000010b

/ using comparison operator, returns list of booleans
/ where element fits criteria

where k > 30
3 8

/ index positions 3 and 8 are TRUE

/ can then use list of indices to index BACK into original list
/ and return VALUES that meet the criteria

k[where[k>30]]
40 43 37

/ so this is actually pretty important
/ you first use comparison operator to find TRUE values in criteria
/ which is any number > 30
/ then you ask WHERE the index positions are
/ then you INDEX those index positions back into list
/ to retrieve the actual values
```
```q
/ 4. return items in k that are divisible by 3

k: 1 2 3 4 5 6 7

k mod 3
1 2 0 1 2 0 1

/ mod = remainder after dividing 
/ third element = 3%3, no remainder, hence 0

where 0=k mod 3
2 5

/ WHERE = find which index positions =0
/ when i try to divide by 3
/ notice syntax. HAVE to put 0= in front
/ this gives you the INDEX POSITION

/ to retrieve the actual value, need to plug it
/ back into the original list

k where 0=k mod 3
3 6

/ boom.
```
```q
/ 5. Given list 1 1 2 3 1 1 2:

/ 5a. Find index position of first 3

1 1 2 3 1 1 2 ? 3
3

/ list ? atom = FIND index pos of y atom in x list

/ 5b. all index occurences of 3

where 1 1 2 3 1 1 2 = 3
,3

/ 3 only shows up once in index position 3

/ 5c. type of each return value. what do you notice

type where 1 1 2 3 1 1 2 = 3
7h

/ notice WHERE returns a list, even if its only one atom
/ notice its NOT negative
```

🔵 [7.7] Built in Functions helpful in Lists

```q
/ these keywords return a singular/atomic value

count / Obtain the count of a list
first / Extract the first element of a list
last / Extract the last element of the list
max / Find the maximum value in a list
min / Find the minimum value in a list
avg / Find the mean of a list
dev / Standard deviation of a list
var / Variance of a list
sum / Total summation of a list
```

```q
/ these keywords return a list/vector of values

sums / Running total of a list
maxs / Running maximums through a list
mins / Running minimums through a list
avgs / Running average through a list
null / Returns a boolean list with 1b where any value is null
distinct / Returns the unique items in the list
group - Returns a dictionary where each distinct item in the list is mapped to the indexes of occurrence
```

```q
L:30 0N 10 20 50 30 60 30 40 50

maxs L
30 30 30 30 50 50 60 60 60 60

/ keeps returning the max value vs corresponding element in list

mins L
30 30 10 10 10 10 10 10 10 10

/ keeps returning the min value vs corresponding element in list

avgs L 
30 30 20 20 27.5 28 33.33333 32.85714 33.75 35.55556

/ returns running average of list

sums L
30 30 40 60 110 140 200 230 270 320

/ returns running sum of list
```
```q
/ 1. create string t6: ("Welcome";"to";"KDB")
/ return the word with the most letters

t6: ("Welcome";"to";"KDB")

count each t6
7 2 3

/ how long is each word?
/ so we know the first element in list is longest

m: max count each t6
7

/ of this 3 element list (7 2 3)
/ which number is the biggest?

t6 where m = max m:count each t6
/ NGL dont really understand the syntax here
```
```q
/ 2. how do you cap the results from a list query?

/ first create list x

x: 10?50
x[where x>30]:30

/ this caps the elements of x at 30
/ but NGL i dont understand the syntax
```
```q
/ 3. given list a: 10 20 1 15 102 3 8 40 3e,
/ update the value of every second item to be null

/ so assuming a list of 0, 1, 2, 3, 4, 5, etc
/ want to change 1, 3, 5 = NULL

/ first need to retrieve list of odd indices

count a
9

/ 9 total elements in a

div[count a;2]
4

/ 9%5 = 4.5 but div is int division

til div[count a;2]
0 1 2 3

2 *til div[count a;2]
0 2 4 6

odd: 1+2 *til div[count a;2]
1 3 5 7

/ retrieves ODD index positions
a[odd]: 0Ne
10 0N 1 0N 102 0N 8 0N 3e

/ update every index position of odd (1 3 5 7)
/ to null
/ also updates null to e = real null
```

🔵 [7.8] Amendment using @

```q
/ 1. using @ to retrieve items from list

k: 1+til 10
/ list of 1-10

@[k; 1 2]
2 3

/ retrieves elements from index position 1 2
```
```q
/ @ primitive can take on more arguments to extend behavior
/ third arg = functions we wish to perf

@[k; ::; neg]
-1 -2 -3 -4 -5 -6 -7 -8 -9 -10

/ so the :: in the second argument just means
/ we aren't selected any indexes in particular
/ ie, ignore it
/ then third arg = make everything negative
```

```q
/ using @ operator, add 2 to every element in list

@[k; ::; + ; 2] 
3 4 5 6 7 8 9 10 11 12

/ taking entire list of k
/ ignoring second arg
/ add 2 to every element
```

```q
/ by specifying the assignment operator : as the 3rd arg
/ we can update our list values
/ try updating list l where anything above 10 = null

@[L;where L>10;:;0N]
9 2 0N 0N 1 0N 0N 4 3 2

/ makes everything above 10 = null
/ note the @ operator is only in place
/ does NOT update original list
/ you'll need to re-assign output back to list if you want to save

L: @[L;where L>10;:;0N]

/ this will save your output back to L
```

```q
/ 3. add 300 to every item in list k that is within range 10-15

k: 19 12 16 18 1 11 18 4 3 2

where k within 10 15
1 5

/ where returns index positions of k
/ that are within 10 - 15

@[k;where k within 10 15; +; 300]
19 312 16 18 1 311 18 4 3 2

/ added 300 to index positions 1 and 5
```

🔵 [7.8] Amendment using . (dot notation)

```q
m: 3 cut 1 + til 9

1 2 3
4 5 6
7 8 9

/ index into the second row, third item

m . 1 2
6

/ weird syntax...but i guess it makes sense

m . (::;0)
1 4 7

/ retrieves first column
/ strange syntax, but :: just means ignore
/ so ignore row, and retrieve 0 column
```
```q
.[m;0 1]
2

/ this is the same as m[0;1]
/ index 0 = row 1
/ index 1 = second element
```

```q
/ update 2 to 500

.[m; 0 1; : ; 500]

1 500  3
4   5  6
7   8  9

/ from m, first row, second column
/ assign :
/ to 500
```
🔵 [7.9] ^ Operator

```q
/ ^ fill operator (or coalesce)
/ can replace a null with value

g: 1 2 0N 0N 3 4 0N
0^g
1 2 0 0 3 4 0

/ syntax is REPLACEMENT^list_name
/ in this case replaced nulls with 0
```
```q
/ another way is to replace nulls with the average

avg[g]^g
1 2 2.5 2.5 3 4 2.5

/ calculates the avg of g
/ fills the nulls with calculated avg
```
```q
/ say you have 2 lists:

j: 1 2 3 4 5 / reguar list
k: 10 0N 30 0N 50 / list with nulls

j^k
10 2 30 4 50

/ replaced nulls in k with corresponding value
/ in list j
```

🔵 [7.10] Fills Operator

```q
/ Fills replaces a null with the previous non null value

g:1 2 0N 0N 3 4 0N
fills g
1 2 2 2 3 4 4

/ replaced 0N with 2
/ then the last 0N with 4

/ this is useful when dealing with time associated data
/ as we often assume the prevailing value is the one to use
/ for reference in the absence of a new value
```
```q
/ how would you fill nulls with the reverse value?

g: 1 2 0N 0N 3 4 0N

reverse g
0N 4 3 0N 0N 2 1

/ first reverse the entire list

fills reverse g
0N 4 3 3 3 2 1

/ then fill nulls with prevailing value

reverse fills reverse g
1 2 3 3 3 4 0N

/ then reverse it again to flip it back to original order
```

🔵 [7.11] Syms vs Strings

```q
/ strings are commonly used for textual data
/ and preferred datatype for ID columns

/ syms are commonly used for stock tickers in tables
```

🔵 [7.12] Lists Problem Set

```q
/ 1. create list of 20 elements called list1

list1: til 20
```

```q
/ 2. use ? to create 10 elements from 10-40

list2: 10?10_til 41

/ til 41 = 0 t0 40
/ then drop the first 10 elements

/ alternative syntax

10 + 10?31

/ so you are retrieving 10 elements from 0-30
/ then adding 10 to the outcome
```

```q
/ 3. create list3 which is list1 joined with list2

list3: list1, list2

/ easy, join lists using comma ,
```

```q
/ 4. create list4, which is first 2 elements of list1
/ joined to everything except first element of list2

list4: (2#list1), 1_list2

/ take first 2 from list1
/ drop first element from list2
```
```q
/ 5. Generate a "rack" of timeseries from 10:00am
/ to 2pm using 30 minute intervals

/ a. how many total hours?

14-10
4

/ from 10am to 2pm
/ so 4 hours

/ b. how many time intervals? (remember 30 mins)

2*(14-10)
8

/ so 8 total intervals

/ c. you need to create a "timeseries"
/ so need to create "list" of 8 elements
/ can achieve this by using til 1

til 1 + 2*(14-10)
0 1 2 3 4 5 7 8

/ created timeseries of 8 intervals

/ d. Create a series of 30 minute windows

00:30:00* til 1+2*(14-10)
00:00:00v;
00:30:00v;
01:00:00v;
01:30:00v;
02:00:00v;
02:30:00v;
03:00:00v;
03:30:00v;
04:00:00v)

/ created timeseries of 30 minutes
/ up to 8 intervals

/ e. Start time is 10:00, so add 10:00

10:00:00 + 00:30:00* til 1+2*(14-10)
10:00:00v;
10:30:00v;
11:00:00v;
11:30:00v;
12:00:00v;
12:30:00v;
13:00:00v;
13:30:00v;
14:00:00v)

/ boom.
```
```q
/ 6. Create a new variable newlist3 by
/ extracting the 3rd element from list1

newlist3:list1[2]

/ retrieves index position 2 = 3rd element
```
```q
/ 7. given list k, calc the number of elements > 5

k : 3 4 5 6 7 8 9

sum k > 5

/ k > 5 returns a list of booleans
/ 1  = true
/ so adding those up lets you know total number
/ of elements > 5
```
```q
/ 8. Extract the elements in k greater than 5

k where k > 5
6 7 8 9

/ k > 5 = returns boolean list
/ where = returns index position where true
/ then feed it back into K as index retrieve
/ to retrieve the values based on those index positions
```
```q
/ 9. update the elements > 5 to be null long

k[where k>5]: 0N
3 4 5 0N 0N 0N 0N

/ use indexing to replace values in list
/ [where k>5] returns index position
```
```q
/ 10. Create p: 5 random shorts from 0-20
/ and create v: 10 random shorts from 0-20

p: 5?21h
v: 10?21h

/ type h = short
```
```q
/ 11. Compute intersection of p and v

p inter v
4 9 11 11h

/ use inter to calculate the intersection of 2 lists
```
```q
/ 12. Return the distinct elements in p in ascending order

distinct asc p

/ distinct = only retrieves distinct values
/ asc = sorts by ascending
```
```q
/ 13. find the index positions where elements overlap in p and v

where p in v
0 2 3 4

/ checks if elements in p appear in v
/ where = return sindex position
```
```q
/ 14. Retrieve the elements in p NOT in v

p except v

/ alternative syntax

p where not p in v

/ not p in v = elements that DONT appear
/ where = index position
/ feed back into p
```

<a name="string"></a>
### 🔴 [8.0] String Manipulation
[Top](#top)

```q
/ a string is a list of chars
/ you can create a string by using the "string" operator

string 20
"20"

string 6*5
"30"

string .z.d
"2024.01.14"
```

Display Output

```q
/ if we dont want to return our final evaluation
/ we use a ; at the end of expression

"hello"
"farewell";
"world"

"hello"
"world"

/ the "farewell" doesn't get evaluated due ;
```
```q
/ to print to consol, use 0N!

0N!"it's time";
"it's time"

/ this prints to the consol
/ need the ; at end otherwise will run twice
```
Vectors from Scalar (vs) 
Scalars from Vectors (sv)

```q
/ vs can be used to split up string
/ based on existing delimiter

vs[";";"a=10;b=20;c=IBM"]
"a=10"
"b=20"
"c=IBM"

/ result is list of strings
/ first arg = delimiter = ;
/ splits string into 3

/ alternative syntax:

";" vs "a=10;b=20;c=IBM"  
"a=10"
"b=20"
"c=IBM"
```
```q
/ 1. split the string on ";*"

a:vs[";*";"a=10;*b=20;*c=IBM"]
"a=10"
"b=20"
"c=IBM"

/ split on ;*
/ result is a list of strings
```
sv function

```q
/ sv function used to create a bigger string
/ from a list of smaller strings
/ so opposite of vs

sv["|";("a=10";"b=20";"c=IBM")]
"a=10|b=20|c=IBM"

/ so taking the list of strings:
/ ("a=10";"b=20";"c=IBM")
/ create a bigger string, using | to connect
```
```q
/ 1. Use vs to write each word of
/ "Its about time!" on seperate line

vs[" ";"Its about time!"]
"Its"
"about"
"time!"

/ uses the space as delimiter
```
```q
/ 2. Given the list of strings
/ ("AAPL";"TD12kdi12";"34.21"), combined these
/ together to create a single pipe ("|") delimited string

"|" sv ("AAPL";"TD12kdi12";"34.21")
"AAPL|TD12kdi12|34.21"

/ alternative syntax for sv
/ uses | as delimiter to join the list of strings
```
Trimming Strings

```q
/ ltrim removes leading or left whitespace from strings
/ rtrim moves trailing or right whitespace from strings
/ trim will remove both leading and trailing whitespace from strings

ltrim "             abc   "
"abc   "

/ removed leading whitespace

rtrim "  abc              "
"  abc"

/ removed trailing whitespace

trim  "         abc       "
"abc"

/ removed both leading and trailing whitespace
```
Add Padding

```q
/ use $ operator to add padding
/ aka "pad"

10$"example"
"example   "

/ added 10 spaces to trailing whitespace

-10$"example"
"   example"

/ neg = adds 10 spaces to prevailing whitespace
```

Lower/Upper

```q
lower `SMALL
`small

upper`big
`BIG
```
Like Comparison for Strings

```q
/ The keyword like is used to:
/ 1. Compare strings with other strings
/ 2. Compare strings with symbols

y:"IBM.OQ"

/ y is a simple string

like[y;"IBM*"]
1b

/ Does the input string begin in “IBM” - followed by anything
/ the * means followed by anything
/ since y = "IBM.OQ"

/ alternative syntax
y like "IBM*"
```
```q
/ does the string end in .OQ?

y:"IBM.OQ"
like[y;"*.OQ"]  
1b

/ does the input string end in “.OQ”
/ yes, it does
/ the * before means wildcard
```
```q
/ can use ? as a single wild card

y:"IBM.OQ"
like[y;"I?M*"]
1b

/ the "?" acts like a single wild card
/ does the string begin wtih I_M
/ followed by something?
```

```q
like["this";"[tT]his"]     
1b

/ the 1st character can either be “t” or “T”
/ uses [ ] to check

like["his";"[tT]his"]
0b
```
String Search (and replace)

```q
ss[x;y]

s:"toronto ontario"
s ss "ont"

3 8

/ searches string for "ont"
/ shows up twice
/ 3 8 are index positions
```
```q
s:"toronto ontario"
s ss "[ir]o"
2 13

/ mix with regex search
```

```q
s ss "t?r"
0 10

/ match with tor and tar
```

 ```q
/ Find index pos of all o's in string

s:"toronto ontario"
ss[s;o]
1 3 6 8 14

/ from string s
/ search for o
```

String Search Replace

```q
s:"toronto ontario"
ssr[s;"ont";"x"] 
"torxo xario"

/ for string s
/ search for "ont"
/ replace with x

/ note you cannot use * to match ss and ssr
```
```q
/ replace all "o's" with "s"

s:"toronto ontario"
ssr[s;"o";"s"]
"tsrsnts sntaris"
```

String Manipulation Exercises

```q
/ 1. Return tomorrow's date as a string

string .z.d  + 1
```

```q
/ 2. Show the various ways to get "o" from "hello"

"hello" 4
"hello" [4]
last "hello" / use "last" to retrieve last char in string

/ ss method
"hello" ss "o" / find index position of "o"
4
"hello" "hello" ss "o" / then feed back into "hello"
"o"

/ equality method
where "hello" = "o" / find index position of "o"
4
"hello" where "hello" = "o" / then feed back into "hello"
"o"
```

```q
/ 3. Find the index positions of blank spaces in "kdb plus is fun"

/ method 1
"kdb plus is fun" ss " "
3 8 11 / uses ss to retrieve the blank index positions

/ method 2 (null method)
/ a null character is " "

null "kdb plus is fun"
000100001001000b / returns boolean string of nulls = 1

where null "kdb plus is fun"
3 8 11

/ where returns index position where null = 1
```

```q
/ 4. Find the index positions of non-blank chars
/ in "kdb plus is fun"

where not null "kdb plus is fun"
0 1 2 4 5 6 7 9 10 12 13 14

/ where null = when nulls = 1
/ so where NOT null = non blanks = null = 0
```

```q
/ 5. Using the string "kdb plus is fun"
/ replace the blank spaces with "_"

ssr["kdb plus is fun";" ";"_"]
"kdb_plus_is_fun"
```

```q
/ 6. Return a 4 item list of strings "kdb plus is fun"

vs[" ";"kdb plus is fun"]
("kdb";"plus";"is";"fun")

/ vector string splitting on " "
```

```q
/ 7. Capitalize on the first letter of every word
/ and replace the _ with spaces in "kdb_plus_is_fun"

/ a. first, find the index position of spaces ("_")
where "kdb_plus_is_fun" = "_"
3 8 11
/ index position of _

/ b. second, find the index position 1 char after the _
1+ where "kdb_plus_is_fun"
4 9 12
/ index position 1 char AFTER the _

/ c. third, find index position of first char
0, 1+ where "kdb_plus_is_fun"
0 4 9 12
/ first index pos + index pos after the _

/ d. use @ operator to capitalize the first chars
@["kdb_plus_is_fun"; 0, 1+ where "kdb_plus_is_fun" = "_"; upper]
"Kdb_Plus_Is_Fun"
/ @ operator; first arg = original string
/ second arg = index positions to target
/ last arg = upper function

/ e. save outcome as new variable

a: @["kdb_plus_is_fun"; 0, 1+ where "kdb_plus_is_fun" = "_"; upper]

/ f. then simply use ssr on this newly saved variable

ssr[a; "_"; " "]
"Kdb Plus Is Fun"
```

```q
/ 8. Remove excess spaces at the end, but keep the space before: "   Abc         "

rtrim "   Abc         "
"   Abc"
```

```q
/ 9. Create the string "NEWRY" from the string "     newry    " using string manipulation

upper trim "     newry    "
NEWRY

/ first trim, then apply the UPPER function
```

```q
/ 10. Using an in-built operator, find out if the following strings are all capitalized:

"a"
"aaB"
"BBB"
"ABC_123"

"a"~upper "a"
0b / false

"aaB"~upper "aaB"
0b / false

"BBB"~upper "BBB"
1b / true

"ABC_123"~upper "ABC_123"
1b / true
```

```q
/ 11. Using an in-built operator, find out if the following strings STARTS with "abc"

"abcdef"
"bcdabcef"
"kdb"

"abcdef" like "abc"
1b

"bcdabcef" like "abc"
0b

"kdb" like "abc"
0b
```

```q
/ 12. Using an in-built operator, find out if the following strings CONTAIN the pattern "abc"

"abcdef"
"bcdabcef"
"kdb"

"abcdef" like "*abc*"
1b / note has wildcard * on either side

"bcdabcef" like "*abc*"
1b

"kdb" like "*abc*"
0b
```

```q
/ 13. Using an in-built operator, find out if the following strings contain numbers:

"abc1def"
"bcdabcef"
"kdb4.0"

"abc1def" like "*[0-9]*"
1b

"bcdabcef" like "*[0-9]*"
0b

"kdb4.0" like "*[0-9]*"
1b

/ checks if string contains any numbers [0-9]
/ the wildcard * before or after the number char

/ string comparison way

.Q.n / utility function that stores all numbers
"0123456789"

any "abc1def" in .Q.n
1b

```

```q
/ 14. Check if string starts with a capital letter and finish with a number

"Abc1def" like "[A-Z]*[0-9]"
0b / so the * acts as a wildcard for any middle chars
```

```q
/ 15. Given s: ("wK<<<De<<< arB33e &:n";"Ia<<< quSe<<<sGREATt!")
/ apply the following manipulations:
/ (hint - above is a list of strings aka nested string)

/ 15b. Replace all occurrences of "&:" with "o"

ssr[ ;"&:"; "o"] each s
"wK<<<De<<< arB33e on"
"Ia<<< quSe<<<sGREATt!"

/ leave first arg BLANK
/ since this is a nested list
/ instead, use EACH adverb
/ to apply ssr to each element in nested list

/ 15c. Remove all occurences of "<<<"

ssr[ ;"<<<"; ""] each s
"wKDe arB33e on"
"Ia quSesGREATt!"

/ use SSR to replace <<< with "" (nothing aka delete)
/ leave first arg BLANK (since nested list)
/ and use EACH to apply ssr to each nested element

/ 15d. Remove all capital letters
/ (hint - NOT make everything lowercase, but REMOVE the lower case ones)

.Q.A / stores the capital letters
"ABCDEFGHIJKLMNOPQRSTUVWXYZ"

except[ ;.Q.A] each s
"we ar33e on"
"a quest!"

/ use the EXCEPT keyword
/ to retrieve all chars (ex numbers)
/ for EACH of the nested elements

/ 15e. Remove all numbers

.Q.n / stores all numbers
"0123456789"

except[ ;.Q.n] each s
"we are on"
"a quest!"

/ uses EXCEPT function
/ to retireve all chars (ex numbers)
/ for EACH nested elements

/ 15f. Turn the nested list into a single string

s:("wK<<<De<<< arB33e &:n";"Ia<<< quSe<<<sGREATt!")
" " sv s

"we are on a quest!"

/ uses first arg " "
/ as delimiter
/ essentially help you combine the nested list
/ into 1 bigger list
```

```q
/ 16. Split the string "aa/bb/cc/dd" where the "/" occurs, to produce a 4 item list 

vs["/" vs "aa/bb/cc/dd"]
"aa"
"bb"
"cc"
"dd"

/ vs breaks down a string into smaller strings
/ first arg = delimiter

/ alternative syntax:

("/" vs "aa/bb/cc/dd")
```

<a name="casting"></a>
### 🔴 [9.0] Casting
[Top](#top)

```q
/ typically the more granular type prevails if you add 2 types together

/ time + timespan

09:00:00 + 2D00:00:00
2D09:00:00.000000000

/ becomes datetime, since datetime is more granular
```

```q
/ Show 3 ways to convert longs to floats

x: 10 20 30

`float$x / 1 symbol name - functional form $[`float;x]
"f"$x    / 2 character value = functional form $["f";x]
9h$x     / 3 short value - functional form $[9h;x]
```
temporal datatypes

```q
/ temporal datatypes

`long$.z.d / number of dates from 1 jan 2000
8781

`long$09:30:00.000 / number of miliseconds from midnight
34200000

`long$.z.d + 09:30:00.000 / number of miliseconds from midnight 1 jan 2020
758712600000000000
```

```q
/ casting time to minute rounds down to nearest minute
`minute$17:55.01 / rounds down to nearest minute
17:55u

`minute$17:55.59
17:55u

/ there's no hour type in kdb, but rounds to nearest int
`hh$17:55:59 / no hour type, so it casts to an int
17i

/ i = int datatype
```

```q
/ 1. Cast 16:30:00 to an int

"i"$16:30:00
59400i / trailing i = int
```
```q
/ 2. Cast 16:30:00 to a float

"f"$16:30:00
59400f / trailing f = float

```

```q
/ 3. Cast 16:30:00 to a minute

`minute$16:30:00
16:30
```

```q
/ 4. Cast 16:30:00 to a second

`second$16:30:00
16:30:00 
```

```q
/ 5. Cast the mixed list to a string
/ a:(1000;2000i;2020.01.01;.z.t)

string A
"1000"
"2000"
"2020.01.01"
"07:34:54.218"
```

```q
/ to cast a string to a float, need to use capital "F"

"F"$"4222.001"
4222.001
```

```q
/ 6. Cast a string to a long

"J" $ "1000"
1000

/ numbers are longs in kdb
```

```q
/ 7. Cast a string to a real

"E" $ "3283.192"
3283.192e

/ reals are trailing e
```

```q
/ 8. Cast a string to a time

"T" $ "09.10:15.888"
09:10:15.888
```

```q
/ 9. Cast a string "2020.01.01" to a date

"D" $ "2020.001.01"
2020.01.01
```

Casting to a sym

```q
/ there is only ONE datatype that can be cast to a sym
/ and that is a STRING
/ need to first cast to a STRING
/ then cast to a sym
```

```q
/ 1. Cast today's date to a sym

`$ string .z.d
`2024.01.16
```

```q
/ 2. Cast 3.14 to a sym

`$ string 3.14
`3.14
```

```q
/ 3. Cast `IBM`GOOG`MSFT to a string

string `IBM`GOOG`MSFT
("IBM";"GOOG";"MSFT")
```

```q
/ 4. Cast `0`1`2`3 to a string
"J" $ string `0`1`2`3
0 1 2 3
```

```q
/ 5. Extract month, week, and year from date 2015.06.03

`month$2015.06.03
2015.06m / trailing m = month

`week$ 2015.06.03
2015.06.01 / week

`year$2015.06.03
2015.i

/ no "year" datatype, so when casting to year, it becomes an int
```

```q
/ 6. Get the current minute, second, hour

`uu$.z.T / cast to minute
0i

`ss$.z.T / cast to second
59i

`hh$.z.T / cast to hour
10i

/ note, need capital T for current time zone
```

```q
/ 7. Split the string "casting all day every day" to a list of symbols: `casting`all`day`every`day

" " vs "casting all day every day"
"casting"
"all"
"day"
"every"
"day"

/ first use vs function to separate string into a list of strings
/ concatenates on the " " blank

then cast this list of strings to a sym

`$" " vs "casting all day every day"
`casting`all`day`every`day

/ done by casting to a backtick
```

```q
/ 8. Cast each item in the general list (0i; 45f; .z.p; 1b)
/ to a list of symbols: `0`45`2020.07.17D14:01:22.808442000`1


`$string (0i; 45f; .z.p; 1b)
`0`45`2024.01.18D06:59:25.008369419`1

/ ONLY strings can be cast to a sym
/ so need to first cast mixed list to a string
/ then cast string to sym
```

```q
/ 9. Given the mixed list, cast the second element to a sym:
/ strings:("string1"; "symbol"; "string2"; "string3")

/ a. use indexing to retrieve second element

strings[1]
"symbol"

/ b. cast it to a sym, 

`$strings[1]
`symbol

/ c. then "reassign it" as the second variable

strings[1]: `$strings[1]

/ d. now when you retrieve the mixed list:

strings
"string1"
`symbol
"string2"
"string3
```
```q
/ dates and times are represented as integers "under the hood"
/ which means they have designated 0 or initial values

/ 10. What are the starting values of the following types:

/ Byte
"x"$0
0x00

/ Date
"d"$0
2000.01.01

/ Time
"t"$0
00:00:00.000

/ Timestamp
"p"$0 //timestamp combines date and time 
2000.01.01D00:00:00.000000000
```

<a name="func"></a>
### 🔴 [10.0] Functions
[Top](#top)

What are Functions

```q
/ Function are a sequence of expressions which are evaluated
/ function valence = the number of input parameters (can be 0 or up to 8)
/ function logic = code you write to make the function do what you want
/ function return = output returned by function (can also be nothing)
```

Function Valence

```q
/ Function valence = number of arguments or parameters

/ 0 parameters (Niladi)

.Q.gf[]
0

/ 1 parameter (Monadic)
+[3;]

/ another example of a monadic Function (1 arg)

neg 1 2 3
-1 -2 -3

/ 2 parameters (Dyadic)
+[3;4]

+[3;4;1]
error

/ error because binary operation aka dyadic function
/ single parameter = monadic function
```

Calling functions

```q
/ when you call/apply a function, it causes expressions to be evaluated in sequence
/ substituting the value of each argument for corresponding input parameters
/ to "call" a function = run it with your inputs/arguments
```

```q
/ 1. Call built in MAX function with 10 11 12

max [10 11 12]
12

/ this is known as functional notation
/ With unary functions (single argument), we can omit the brackets

max 10 11 12
12

/ this is called infix notation
```

Different Ways to Pass parameters to Functions

```q
/ different ways to pass parameters to a function:

3 % 5       / infix notation 
%[3;5]      / functional notation
%[ ;5] 3    / eliding the first argument to pass the second 
%[3][5]     / projecting over the first argument and then calling with the second explicity 
%[3] 5      / projecting over the first argument and then applying implicity to the RHS value of 5
```

```q
/ another example of diff ways to call a function
/ suppose you want to call using inputs 3 and 5

divide:{[numerator;denominator] numerator % denominator} 

divide[3;5]    / functionally calling
divide[3][5]
divide[3] 5 
divide[;5] 3
divide[;5][3]

/ the above all work!

3 divide 5     
error

/ infix calling throws an error
/ this syntax is wrong
```

```q
/ Single input function with 4 expressions:

sumStats:{[l]
    countL:count l; 
    minL:min l; 
    maxL:max l; 
    avgL:avg l; 
    :(countL;minL;maxL;avgL) / using force return 
 }
```

Function Problem Set 1

```q
/ 1. Create a binary function called speed

speed: {[miles;hours] mph: miles%hours; kph: 1.609*mph; :kph; }

/ binary function = 2 arguments
/ miles and hours are the arguments
/ you can define variables within function like mph
/ kph is a local variable we defined inside func
/ we can explictly return it using :
/ requires semi colons between expressions
```

```q
/ 2. Call function speed with args 15 and 0.5

speed[15;0.5]
48.27
```

```q
/ 3. If there is NO explicit return from a function,
/ its result is the last expression
/ re-write above with no return

speed: {[miles; hours] 1.609 * miles % hours}
speed[15; 0.5]
48.27
```

Calling Functions with List of Arguments

```q
/ 4. You can call a function with a LIST OF ARGUMENTS
/ recall your arguments are mile and hours

speed: {[miles; hours] 1.609 * miles % hours}
speed[15 20 30; 0.5 1.0 1.5]
48.27 32.18 32.18

/ this is important!
/ you called a LIST of miles (15 20 30)
/ and a LIST of hours (0.5 1.0 1.5)
```

Function Problem Set 2

```q
/ 1. Create a function called range that takes 1 parameter but doesn't do anything

range: {[1] }

/ function runs, but doesn't return anything
```

```q
/ 2. Create a function that converts farenheit to celcius
/ arg = farenheit
/ formula is F-32 * 5/9

FtoC: {[F] offset: F-32; offset*5/9}
FtoC[70]
21.11

/ arg = F
/ can have multiple expressions (in this case, 2)
```

```q
/ 3. Use semicolon to STOP output of above function

FtoC: { [F] offset: F-32; offset*5/9;}

/ since we added the extra ; at the end
/ function will run, but not return any results
```

```q
/ 4. Use colon : to force return first expression

FtoC: { [F] :offset: F-32; offset*5/9;}
FtoC[70]
38

/ the : forces return before the second expression
/ the ; at the end of the last expression halts return
```

```q
/ 5. Create a function called range that accepts 1 input
/ and calculates the range of numbers in a list
/ for ex, calling range on -10 20 30 40 50 should return 60

range:{ [x] max [x] - min x}

/ max retrieves the max number in a list
/ min retrieves min nuimber in list
/ difference is the range
```

```q
/ 6. Save the above function to a variable, then force return that variable

range:{ [x] a: max [x] - min x; :a }

/ so saved first expression to variable a
/ second expression gets printed
```

```q
/ 7. What is the difference between these three:

neg [1 2 3]
neg 1 2 3
neg [1;2;3]

/ first 2 are lists; would apply neg operator on the vector
/ 3rd one = error cuz semi colon delimits the function parameters, passing 3 input parameters
```

Explicit vs Implicit Parameters/Arguments

```q
/ Explicit arguments are defined within square brackets in a function [ ]

f: { [a; b; c] a + b + c}

/ a, b, c are EXPLICIT parameters
```

```q
/ Implicit arguments don't need to be defined
/ if function has less than 3 arguments, their names can be ommited
/ x, y, and z are used as implicit arguments

f: { x + y + z }

/ if you are only using 3 parameters, don't need to define them
```

Functions Problem Set 3

```q
/ 1. Re-write the below function using implicit arguments

original function:
speed: {[miles; hours] 1.609 * miles % hours}

/ implicit version:
speed:{1.609 * x % y}

/ replaced miles and hours with x and y
```

```q
/ 2. Call function speed with 2 distances and a single duration

speed[15 30; 0.5]
48.27 96.54

/ can pass through multiple args for x
```

```q
/ 3. Create a function that will find the area of a rectangle
/ with length 7.93 and width 1.87 using implicit parameters

func:{x*y}
func[7.93;1.87]

```

Function Problem Set 4

```q
/ 1. Create a monadic function to find all odd numbers in a list
/ then multiply them by 10

/ mod = whatever is leftover after division
/ so 3 mod 2 = 1
/ if mod = 0 = even
/ if mod = non zero = odd

x mod 2 / x = list
1 2 3 4 5 mod 2
1 0 1 0 1

where 1=1 2 3 4 5 mod 2
0 2 4 / find where 

/ retrieve index position where mod = 1
/ aka where there is LEFTOVER after division
/ aka ODD numbers

1 2 3 4 5 where 1=1 2 3 4 5 mod 2
1 3 5

/ if you feed back index positions back to orig list
/ retrieves values where true
/ aka, ODD

/ can save the output to variable called odd
odd: 1 2 3 4 5 where 1=1 2 3 4 5 mod 2

/ then form second expression (*10)
odd: 1 2 3 4 5 where 1=1 2 3 4 5 mod 2; odd * 10
10 30 50

/ re-write as a function

f: { [x] odd: x where 1=x mod 2; odd*10}
f[1 2 3 4 5]
10 30 50
```

```q
/ 2. Re-write above function, but include the even numbers
/ but dont multiply them

f: { [x] odd: where 1=x mod 2; @[x;odd;*;10]}
f[1 2 3 4]
10 2 30 4

/ note - where will retrieve index position
/ we took out the feeding back into original list
/ because we want the index position
```

Function Scope (Local vs Global Variables)

```q
/ Function scope refers to the "view" of the variables
/ that are accessible to a function during it's execution.

/ local scope = local variables
/ local variables = variables passed WITHIN function

/ When a function calls for a variable, it first "looks" locally,
/ and then if the variable can't be found in local scope, the function "looks" globally.
/ If the variable can't be found in either scope, then the function will error.

f: {[localv] }

/ localv is a local variable that only exists within the function
```

```q
/ 1. Calc the circumference of a circle

c: {[r] pi: 3.14; 2*pi*r}

/ pi is a local defined variable
```

Defining local variables as global using ::

```q
/ use :: to define local variables as global

b:6
/ set b:6 as GLOBAL variable
/ notice its NOT within a function

f:{b::7; x*b}
f[10]
70

/ inside the function, b::7 is a local variable, but is being set
/ as global variable
/ so when you call the function with 10
/ b = 7, hence 7 *10 = 70
```

Local vs Global Variable Problem Set

```q
/ 1. Example 1

b:6
/ set b:6 as GLOBAL variable
                   
f:{b:42; b::x; b}

/ set b:42 as LOCAL variable.
/ this overrides 6 as variable

/ what happens when you call f[98]

f[98]
42

/ since you call function
/ function will take LOCAL variable
/ aka b: 42

/ what happens you simply call b (outside of function)

b
6
/ global variable is still 6
```

```q
/ For the above example, use SET to define the global variable
/ from inside the function call instead of using ::

b:6                  
/ resetting b:6 globally

f:{b:42; `b set x; b}
/ setting b:42 locally, setting b set x globally and doesn't overwrite the local variable

f[98]
42

/ when you call function, b is still local variable; so 42
```

Projections

```q
/ A projection is created by passing a function less than its total req arguments
/ used when we want to keep one or more arguments CONSTANT

/ lets create an addition projection

add: 2+

/ normally, addition requires 2 inputs
/ but we are only using 1 here
/ so binds first input to 2

add 1
3

/ since we bound the first input to 2
/ the new output = 3
```

Projection Problem Set 5

```
/ 1. To create a projection where the second argument is held constant, simply omit the first argument

half:%[ ;2]
half 3
1.5

/ % requires 2 inputs
/ left first arg empty
/ so binds second arg as 2
/ then use that as projection to calc new function
```

```q
/ 2. Create a projection called modSeven that calculates the input modulo 7

modSeven:mod[ ;7]
modSeven 15
1

/ mod = whatever leftover after division
/ aka remainder after % 7
/ mod takes 2 arguments
/ left first arg blank
/ binds second arg = 7
```

```q
/ 3. Using the projection modSeven, apply it to the series 0-10

modSeven til 11
0 1 2 3 4 5 6 0 1 2 3

/ first arg for modSeven = blank
/ your list of 0-10 can feed in

```

Functions Problem Set 6

```q
/ 1. Create a monadic function that returns a boolean indicating
/ whether argument is a whole number

/ a whole number is the following types

short (-5h)
int (-6h)
long (-7h)

whole:{ type [x] in -5 -6 -7h}
whole[4h]
1b

whole[4.2]
0b

/ use in keyword to check if type = whole numbers
```

```q
/ 2. Create a dyadic function that takes list and a long as 2 arguments
/ and returns items that are wholly divisible by long

divide:{ [x;y] x where 0=x mod y}
divide[2 3 4 5;5]
5

/ use mod to check if remainders exist after division
```

```q
/ 3. Create a function that rounds down number to nearest whole number

rounddown:{floor x}
rounddown[7.8]
7

/ use FLOOR keyword to round down to whole number
```

```q
/ 4. Create a monadic function that accepts a list and replaces last item with 99

replace:{ @[x;count [x]-1;:;99]}
replace[1 2 3 4]

/ use @ function for amending lists
/ first arg = x = list (input)
/ second arg = index position
/ third arg = action = assign :
/ last arg = new value 99
```

```q
/ 5. Create a monadic function that accepts a list and returns list with
/ second item doubled

double:{ @[x;1;2*]}
double[1 2 3 4]
1 4 3 4

/ use @ apply function to amend list
/ first arg = x = list (input)
/ second arg = index position
/ third arg = action (2*)

/ alternative syntax

double:{ @[x;1;{2*x}]}

/ notice you use brackets { } inside the apply [ ]
```

```q
/ 6. Create a monadic function that accepts an M x N matrix
/ and returns a N x M matrix

transpose:{flip x}
transpose(1 2 3 4; 10 20 30 40; 100 200 300 400)
1 10 100
2 20 200
3 30 300
4 40 400

/ you simply flip the matrix
/ note when calling function with matrix, use parenthesis ( )  instead of [ ]
```

```q
/ 7. Create a triadic function that accepts the following arguments:
/ arg1: a delimiter string, aka "|" or ","
/ arg2: datatype to be cast aka "SJF" would cast to sym, long, float
/ arg 3: a string ("JPM, 100, 4.5, test string")

/ and returns a list cast to desired type

f:{[x;y;z] y$x vs z}
f[",";"SJF*"; "JPM, 100, 4.5; test string"]
`JPM
100
4.5
"test string"

/ so z = string
/ vs function breaks down a string via the delimiter
/ so breaks down z string
/ then casts to y datatype
```

```q
/ 8. Create a dyadic function that accepts a first and last name (as syms)
/ and returns a single sym including a space between the names

string `john`allen / first convert list of syms to list of strings
("john";"allen")

" " sv ("john";"allen") / use sv to create bigger string, using " "  to connect
"john allen"

`$"john allen" / then cast longer string to a sym
`john allen

f:{`$" " sv (x;y)}
f[`john;`allen]
`john allen

```

```q
/ 9. Create a monadic function that takes a list of numbers and converts all to type longs

f:{`long$x}
f 4.1 4.3 4.5
4 4 5

/ use `long to cast to longs
```

Call Functions from qSQL 7

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


<a name="iterators"></a>
### 🔴 [11.0] Iterators
[Top](#top)

```q
/ iterators replaces the use of loops commonly seen in coding
/ modify's a function to iterate across every item in a list
```

Each

```q
/ used to apply a function to each item in a list
/ useful in a nested list

a: (1 2 3; 10 20i; 30 40 50f; 60)
count a
4 / four items in list

count each a
3 2 3 1 / how many elements are in each list
```

```q
/ each will not work with multivalent functions
/ only works for functions with 1 argument

/ the IN keyword takes 2 inputs:

3 in 1 2
0b/ false
```

```q
/ 1. Check if 3 is in (1; 1 2; 3 4)

3 in each (1; 1 2; 3 4)
error
/ can't use each in a multivalent function (in requires 2 arguments)

/ solution: project function "in" to a single input function
/ by fixing the first parameter

isthreein: 3 in
/ create projection of in
/ fixed first parameter as 3

isthreein 2 3 4 5
1b

isthreein each (1; 1 2; 3 4)
0011b
/ now it works!
```

```q
/ 2. Find the first element of a

a: (1 2 3; 10 20i; 30 40 50f; 60)
first a
1 2 3

/ use FIRST function to retrieve first element in mixed list
```

```q
/ 3. Find the first element of each item in a (indexing wont work)

a: (1 2 3; 10 20i; 30 40 50f; 60)
first each a
1 10i 30f 60

/ 4. why can't you use indexing?

a[ ;0] / syntax for retrieving index position 0
error

/ doesn't work because not every element within a is a list
/ last element is an atom (60)
/ you cannot index into an atomic value!
```

```q
/ 5. using keyword WITHIN, create a projection to test if 5 is
/ within each of the following ranges: (3 6; 4 8; 10 15)

is5within: 5 within
is5within each (3 6; 4 8; 10 15)
110b

/ first create projection of 5 within
/ then use function + each on nested list

/ alternative syntax

within[5] each (3 6; 4 8; 10 15)

/ this also works
```

each both

```q
/ each both ' uses items from 2 lists in pairwise fashion
/ can also be a list and an atom

/ take each both (#') example

1 2 3 4 #' ("abcd"; "abcd";"abcd";"abcd")
("a";"ab";"abc";"abcd")

/ take 1 items from first element
/ take 2 items from second elements
/ take 3 items from third element
```

```q
/ atom #' list
/ take first 3 items from each element in list

3#'("abcd"; "abcd";"abcd";"abcd")
("abc";"abc";"abc";"abc")

/ behaves similar to 2 + 1 2 3
/ the atom (2) is applied to every element of list

/ alternative syntax

#[3] each ("abcd"; "abcd";"abcd";"abcd")
("abc";"abc";"abc";"abc")

/ same thing
/ but instead of ' you use EACH keyword
```

```q
/ returning to our in example before, can avoid projection by using '

/ projection method using IN

isthreein: 3 in / create projection of in
isthreein each (1; 1 2; 3 4)
0011b

/ each both keyword

3 in '(1; 1 2; 3 4)
001b

/ by using ' each both, can avoid function projection

/ equivalent to doing this:
3 3 3 in '(1; 1 2; 3 4)
001b

/ the atom (3) gets applied to every element of right side list
```

each left \:

```q

```

each right /:

```q
/ top of arrow points to RIGHT
/ takes entire left and performs to EACH element in RIGHT

2 3 +/: 3 4 5

5 6
6 7
7 8

/ adds 2 3 to each element of 3 4 5
```

```q
/ join each right (,/:)

2 3 ,/: 3 4 5
2 3 3
2 3 4
2 3 5

/ joins 2 3 to each item to the right
```

```q
/ multiply each right (*/:)

2 3 */: 3 4 5
6 9
8 12
10 15

/ multiplies 2 3 to each item to the right
```

each left \:

```q
/ takes entire right to EACH item in left

2 3 +\: 3 4 5
5 6 8
6 7 8

/ takes 3 4 5 + each item of left
```

in 

```
/ check if 3 is in each of the nested list (1;1 4; 2 3; 3 5 6)

3 in'(1; 1 4; 2 3; 3 5 6)
0011b

/ checks if 3 is in each of the nested lists
```

```q
/ check if 3 and 4 are in each of the nested list (1; 1 4; 2 3; 3 5 6)

3 4 in/: (1; 1 4; 2 3; 3 5 6)
00b
01b
10b
10b

/ combines IN with each right
/ check if 3 4 are in each of the right (nested list)
```

Like each left (string iterators)

```q
/ LIKE is used to compare strings with other strings/symbols

like["IBM.OQ";"IBM*]
1b

/ or alternative syntax
"IBM.OQ" like "IBM*"

/ does the input string begin with "IBM" - followed by anything (the *)
```

```q
/ 1. using LIKE + each left, check if "kdb+" appears in any of the elements in nested list

a:("kdb+ is fast"; "kdb+ is cool"; "q is easy")
a like \: "*kdb+*"
110b

/ the * before and after = wildcard (can be anything)
/ like keyword checks if target string appears in right hand string
```

string pattern checks

```q
ids:("A123";"A234";"B123";"B234")
patterns:("A";"*123*")

/ so the patterns you are after is string A followed by wildcard
/ and wildcard followed by 123

/ check if any of the patterns appear in any of the id nested strings

patternCheck: ids like/: patterns
1100b
1010b

/ equilvalent of:
("A";"*123*") like/: ("A123";"A234";"B123";"B234")

/ so checks entire left to EACH of the right nested strings
```

```q
/ did any of the ids match the patterns?

any patternCheck?
1110b
/ 1 = true = patterns matched

/ use where to discover index position
/ then feed back into original id list
/ to retrieve the items that match

ids where any patternCheck
"A123"
"A234"
"B123"

/ so these are the values in id that match your pattern
```

```q
/ checking for multiple string patterns in a table is very common
/ syntax is: any stringCol like/: pattern

```
matrix

```q
/ 1. create a multiplication matrix using list 0 1 2 3 4
/ entry should look like i * j

row: til 5
0 1 2 3 4

row*/:row / multiple entire left row by each of right row

0 0 0 0 0
0 1 2 3 4
0 2 4 6 8
0 3 6 9 12
0 4 8 12 16

/ 0 1 2 3 4 * 0
/ 0 1 2 3 4 * 1
/ 0 1 2 3 4 * 2
/ 0 1 2 3 4 * 3
/ 0 1 2 3 4 * 4
```
each prior

```q
/ each prior ': or keyword prior applies the function to adjacent pair of items in list

0 +': 1 2 3
1 3 5

so think of it as:

/ 0 + index position 0 (0 + 1)
/ index position 0 + index position 1 (1 + 2)
/ index position 1 + index position 2 (2 + 3)

/ alternative syntax

+':[1 2 3]
prior[+;1 2 3]
```
```q
/ its common to use subtract - with this iterator
/ since time series analysis usually helpful to know how something changed

0 -': 1 2 3 4 6 8 10
1 1 1 1 2 2 2

/ subtracts index position with its prior index position 
```

Deltas

```q
/ this has become so common that kdb has a keyword called DELTAS

deltas 1 2 3 4 6 8 10
1 1 1 1 2 2 2

/ so deltas = 0 -':
```
```q
/ 1. Create function called myMax that will return max of 2 inputs
/ use each prior (':) to return the pairwise rolling maximum across list

myMax:{max x,y}
myMax': [20 30 2 3 20 40 70]
20 30 30 3 20 40 70

/ so mymax takes just takes the max between 2 inputs (x and y)
/ by using each prior, it compares each element at index position
/ vs the previous element at the previous index position
```

Scan \

```q
/ scan returns the intermediate values associated with each execution
/ so returns every single iteration
/ think of it as a man standing behind a fort, "scanning" the battlefield
```

```q
/ plus scan example

(+\) 1 2 3 4 5
1 3 6 10 15

/ (1)
/ 1 + (2) = 3
/ 3 + (3) = 6
/ 6 + (4) = 10
/ 10 + (5) = 15

/ alternative syntax

(+) scan 1 2 3 4 5
1 3 6 10 15

/ this is actually the same as SUMS

sums 1 2 3 4 5
1 3 6 10 15
```

```q
/ append one element of a list to the next to a list in each iteration

(,\)(1 + 0 1 2 3 4)

,1         / 1+0 = 1, appends 1
1 2        / 1+1 = 2, appends 2
1 2 3      / 1+2 = 3, appends 3
1 2 3 4    / 1+3 = 4, appends 4
1 2 3 4 5  / 1+4 = 5, appends 5
```

Compound Interest Example

```
/ 1. given 2 inputs: savings & interest, find a function that tells us annual return

f: { [savings;interest] rate: 1 + 0.01*interest; savings*rate}
f[10000;1.2] / 10,000 in savings, 1.2% int rate
10120f / savings after 1 year

/ now lets assume the interest rates over the past 5 years:

interestRates: 1 1 1.25 2 1.6 1.5

/ 2. Calculate the savings based on these new interest rates

(f\) 10000,interestRates
10000 10100 10201 10328.51 10535.08 10703.64 10864.2

/ SCAN allows you to iterate your function across a list
/ so taking your formula, and using SCAN (f\)
/ using 10,000 as your first argument
/ and a list (of int rates) as your second arg
```

Over

```q
/ over / iterates similar to scan, but only returns the final result
/ think of when it's "over" you only care about the last result

(+/) 1 2 3 4 5
15

/ the same as sum

sum 1 2 3 4 5
15
```

```q
/ 1. Use drop to remove first element from x and last 2 elements of y

x: 10 30 20 40
y: 13 34 25 46

1 -2 _' (x;y)
30 20 40 / dropped first element 10
13 34    / dropped last 2 elements 25 and 46

/ so the way i see this is,
/ left side = elements 1 and 2
/ drop each
/ right side = lists x and y
```

```q
/ 2. Create a function using each both (') that combines 2 corresponding strings

combinestring:{x,'y}
/ output will pair every x with every y

/ call with 2 list of strings:

combinestring[("1";"2";"3");("a";"b";"c")]
("1a";"2b";"3c")
```

```q
/ 3. Create a function using each right that joins a given string to the front of a list of strings

prefixstring:{x,/:y}

/ each right means takes entire x (left) to EACH of y (right)

prefixstring[("1";"2";"3");("a";"b";"c")]
("123a";"123b";"123c")
```

<a name="funccontrol"></a>
### 🔴 [11.0] Function Control
[Top](#top)

how to return a custom error from a function

```q
{[] a:10; b:`this; a+b} []
type error

/ you can't add a long with a sym
```

```q
/ to raise our own custom error message
/ we use signal '

{[] a: 10; b:`this; '"wrong datatype"; a+b} []
wrong datatype
error

/ now when it returns an error, you get your custom message!
/ note your custom msg has to go before your expression
```

if conditional

```q
/ checks to see if given condition true
/ if so, executes all subsequent statements
/ if false, skips all subsequent statements, and moves to next q expression
```
example 1

```q
a: 10
if[1b; a:a+1; a]
11

/ if condition = 1b (true)
/ so proceeds to execute a:a+1 (10+1)

b: 20
if[0b; b:b+1; b]
20

/ since first condition = 0b (false)
/ skips following statement
/ returns b
```

```q
/ 2. Define variable [r] as 100 and [ans] to be an empty string
/ write an if statement to say if r is greater than 85, then add 10 to r
/ and change [ans] to say "high"

r: 100
ans:"" / ans is an empty string

if[r > 85; r+:10; ans:"high"]
r
110

ans
"high"
```

if-else statements

```q
/ designated with a $
/ if statements don't return a value
/ if-else statements will return a value

$[condition;ifTrueStatement; ifFalseStatement]
```

```q
$[1b; "true"; "false"]
true

/ since condition is true (1b), executes first statement
```

```q
/ create a function that accepts a sym as an arg
/ and returns a capital string if caps_on is true
/ otherwise return the sym as a string

caps_on:0b         / declare global variable as false

toString:{if[not 10h=type x; x:string x]; $[caps_on; upper x; x]}
toString`hey
"hey"

/ so first part is an [if-statement]
/ if x is not a string,
/ then convert x to a string
/ if it is a string, then ignore and move onto next statement

/ $ if condition is true (it's not), then capitalize x
/ since its not true, just return x
```

```q
/ 3. Create a function that accepts 1 argument as a number
/ and checks if it is within 20 and 30
/ and outputs "inside range" or "outside range"

f:{ [val] r:$[val within 20 30 ; "within"; "outside"]; r," range"}
f[50]
"outside range"

/ val = your arg/input
/ if-else statement = either within or outside
/ finally output = r (within or outside as a string) and joins another string "range"
 ```

```q
/ 4. Create a function that accepts 1 argument balance
/ and if its less than 10, return 1b, but also export to consol "under 10.0"
/ and if its more than 10, only print to consol "above 10.0"

balance: 9:32
befrugal: $[balance < 10;
             [2 "under 10.0"; 1b]; 
             [1 "above 10.0"; 0b;]]
befrugal
befrugal[11]
0b

/ so this starts with a simple if-else condition
/ the 2 = exports to CONSOL
/ 11 > 10, so no output (due to the ; suppresses the 0b)
/ but in the consol, it will print "above 10.0"
```

```q
/ comparison tests can be applied to any datatype

0p
2000.01.01T00:00:00.000000p

/ p is the datatype for timestamp
/ so 0p is the start of all time, ie, jan 1, 2000 @ midnight

time:0p
$[time;`after_midnight;`midnight]
`midnight

/ 0p = midnight
/ so first condition is true
```

```q
/ 4. Create function priceCheck that accepts a sym as an input
/ and if the sym is mars, return 2.5
/ otherwise return 0
/ and prints to standard error (consol) "who cares"

priceCheck:{$[x~`mars; 2.5; [-2 "who cares"; 0]]}
priceCheck `mars
2.5

/ since your input was `mars, returns 2.5

priceCheck `snickers
0
/ also prints "who cares" to the consol
```

Multiple If-Else Statements

```q

thresholds: 160 170

tempMonitor:{ [sensorTemp; thresholds]         / sensor = your input
               $[sensorTemp > thresholds[1];   / if input greater than index position 1 of threshold 170
                   `"temp too hot! stop!";     / if true, return this custom error message 
                 sensorTemp < thresholds[0];   / if first condition false, move onto this condition. is input less than 160?
                   -2 "temp too low";          / if true, print to consol this message   
                   -1"Temp within range"];     / else, print this message to consol
               sensorTemp}                     / show your input

tempMonitor[162;thresholds]
162
/ also prints to consol "Temp within range"

tempMonitor[150;thresholds]
150
/ also prints to consol "temp too low"

tempMonitor[180;thresholds]
"temp too hot! stop!"
error
```

```q
/ Let's say you have 2 separate thresholds:
/ wool threshold = 160 170
/ viscose threshold = 150 180

/ and 3 possible outcomes:
/ "temp too low", "temp too high", or "just right"

/ create a core function, then projections of this function
/ for wool and wiscose

/ 1. temp thresholds
woolThresh: 160 170
viscoseThresh: 150 180

/ 2. temp actions
tooLow:{-2 "temp too low"; min (0,x)}
tooHigh:{`"temp too high"};
justRight:(::) 

/ tooLow - min chooses the smaller number btwn 0 and your input
/ returns 0, and prints "too low" to consol

/ tooHigh - if too high, error message will read "too high", then STOP executing
/ note the ; at the end

/ justRight - :: means functional null - do nothing

/ 3. Core function logic
tempMonitor: { [sensorTemp; thresh]
                function:$[sensorTemp > thresh[1];
                      tooHigh;
                   sensorTemp < thresh[0];
                      tooLow;
                      justRight];
                 function[sensorTemp]}

/ tempMonitor is a function that accepts 2 args
/ your sensor input, and the previously established thresholds
/ function is a variable that saves your if-else conditional
/ if input is > index position 1 (max threshold), action tooHigh
/ else if input < index position 0 (low threshold), action tooLow
/ else action justRight(functional null - do nothing)
/ then run this if/else condition using your input

/ 4. Projection for each material

woolMonitor: tempMonitor[ ;woolThresh];
viscoseMonitor: tempMonitor[ ;viscoseThresh];

/ binds the second argument to your thresholds set above
/ and allows your first arg to be your temp input

/ 5. Calling the function

woolMonitor[162]
162 / justRight; does nothing

woolMonitor[150]
0 / returns min of (0;150). Prints "temp too low" to consol
```

```q
/ Create dyadic function that accepts either syms or strings
/ and compares the [type] of first arg to a [sym],
/ and if first arg = [sym], print second arg as a
/ [lower case string]
/ else, return value of second arg as a string

/ so in other words:
/ if [sym, string] = return lowercase string
/ if [string, sym] = return "sym" (as a string)
/ so no matter what, return y as a string

/ part 1

/ sym = type 11h

type x~11h / comparison boolean true or false
abs type x~11h / want to compare both atoms and lists

f:{ [x;y] $[(abs type x)~11h; lower; ::]

/ so if first arg = sym, we want to return the function LOWER
/ else, ignore (do nothing)

/ part 2

/ if second arg = string (or char 10h)
/ simply return the string
/ else string it, and return it

$[10h = type y; y; string y]

/ so the first part will LOWER the entire second part
/ if x = sym
/ otherwise, it'll just return a string y

/ part 3: combine the 2 parts

f: { [x;y] $[abs type x ~ 11h; lower; ::] $[10h= type y; y; string y] }

/ note NO SEMICOLON after first if/else conditional
/ since you are using the LOWER FUNCTION
/ to apply to second if/else conditional
/ if you use ; then LOWER FUNCTION doesn't get applied 

/ part 4: call function to test out

f["string";`sym]
"sym"

f[`sym;`SYM]
"sym"

f[`sym;"STRING"]
"string"
```

Vector Conditional Evaluation

```q
syntax: ?[x;y;z]

/ !note vector conditional uses ? instead of $!
/ x = boolean vector
/ y and z = return the same type

/ vector conditionals provide a return value
/ but cannot be extended to else-if clauses
```

```q
?[10001b; 1 2 3 4 5; 10 20 30 40 50]
1 20 30 40 5

/ so takes first statement of booleans
/ and if true, returns corresponding first statement
/ if false, returns corresponding second statement
```

```q
/ if y = atomic, then will be repeated where necessary

?[10001b; 1; 10 20 30 40 50]
1 20 30 40 1

/ so 1 just repeats
```

```q
/ Create a dyadic function that accepts a list (vector)
/ but caps the  NUMBER of items in vector based on your second arg

vector: 10?20
/ so vector is a random list of 10 numbers

f:{[vec; cap] ?[(count vec) > cap; cap; vec]}
f[vector;5]
5

/ caps list to 5 elements
```

```q
/ Now create a dyadic function that accepts a list (vector)
/ but caps the VALUE of each element in vector based on second arg

vector: 10?20
/ so vector is a random list of 10 numbers

f:{[vec; cap] ?[vec> cap; cap; vec]}
f[vector;5]
5 3 2 3 1 2 3 4 2

/ caps the VALUE of each element to 5
```

```q
/ create a list of 3 sides (`buy or `sell)
/ create a list of 3 bids
/ create a list of 3 asks

/ create an if/else conditional such that if
/ side = buy, return the ask price
/ else, return the bid price

side:`buy`buy`sell
bid: 10 11 12
ask: 11 12 13

?[side=`buy;ask;bid]
11 12 12
```

```q
/ Create list to be 1 2 3 0N 5
/ use vector conditional to fill null with 10

k: 1 2 3 0N 5

?[null k;10; k]
1 2 3 10 5

/ so using VECTOR CONDITIONAL
/ if value = null (boolean check), replace with 10
/ otherwise return the value (of k)
```

2 things KDB rarely ever uses

```q
/ DO operator allows us to repeat an execution multiple times
/ useful for checking timing of queries
/ no return value
```

```q
/ WHILE operator allows expression to be evaluated while condition is true
/ no return value
/ typically not used in KDB because can use vector conditioning

/ create a function that calcs the fibo sequence
/ and accepts arg = number of elements returned
/ using the WHILE loop

f:{[x] r: 1 1; while[x-:1; r,:sum -2#r];r}
f[10]
1 1 2 3 5 8 13 21 34 55 89

/ r = starting value of fibo sequence
/ while

/ for index position x: x-1
/ join r with th sum of the last 2 elements of r
```

Function Control Exercises

```q
/ 1. Create variable v which is a random number btwn 1 and 100
/ write an if statement that doubles v if it is less than 50

v:rand 100
54

if[v<50;v*:2]
show v
54

/ use RAND operator to generate random number from 0-99
/ for IF CONDITIONAL, checks if first statement is true
/ then evaluates expression
/ if false, then stops
```

```q
/ 2. Create a func that uses if conditional to check if
/ a number is >= 0.9. if so, print warning message
/ to std error that says "warning!" and continues with
/ func to return today's date

f:{ if[x>= 0.9; -2 "warning!]; .z.d}
f[0.9]
2024.01.28d

/ returns today's date
/ and prints "warning" to consol
/ note, use -2 to print to std error

/ note if false, still returns today's date
/ just doesn't print "warning!" to consol
```

 ```
/ 3. Create func that uses if conditional to check if input is a date
/ if not, use [error signal] to return `not a date (note sym)
/ if it is a date, add 1 to the date to return tom's date

f:{ if[not -14h=type x; '`$"not a date"]; x + 1}
/ first checks if x = date (14h)
/ then cast your string to a sym
/ then use ' to use this as your error message

f .z.t
error: not a date

f .z.d
2024.01.29d
```

```q
/ 4. Create a monadic func that takes whole number
/ and returns `odd if input is an odd number
/ and `even if input is an even number

f:{$[0=x mod 2; `even; `odd]}
f[10]
`even
```

```q
/ 5. Create a monadic func that returns a boolean stating
/ if input is a float

f:{$[-9h=type x; 1b; 0b]}
f[10.5]
1b

/ note, need to use -9 since atomic
/ can use -9 or -9h
```

```q
/ 6. Create a monadic func that takes a sym as an input
/ if sym = a, return 1
/ if sym = b, return 2
/ if sym = c, return 3
/ else, return 0

abc:{$[x~`a;1;x~`b;2;x~`c;3;0]}

/ if x = `a, return 1
/ if x = `b, return 2
/ if x = `c, return 3
/ else, return 0

/ so note if/else conditional $ can chain logic
/ note use ~ to check equality + type
```

```q
/ 7. Create monadic func that checks if input is a char or string,
/ return the arg
/ otherwise return the arg as a string


f:{ $[10h=abs type x;x; string x]}
f[`test]
"test"

/ note - have to put 10h FIRST
/ doesnt work otherway around for some reason
/ can use 10h or 10
/ use abs to check for both char + string
```

```q
/ 8. Create a func that takes a list of pos + neg numbers
/ and replaces neg numbers with 0

f:{?[x<0; 0; x]}
f[1 2 -3 4]
1 2 0 4

/ need to use comparison operator for vector conditional
```

```q
/ 9. Create a func that takes a list of numbers
/ and replaces numbers greater than 100
/ with half their value

f:{?[x>100; x%2; x]}
f[10 200 400]
10 100 200
```

```q
/ 10. Create a monadic function that take a list of orderIds
/ and use the [LAST character] to return `Buy (B) or `Sell (S)

/ input example: (`9280408_B`71100_B)
/ output example:`Buy`Buy

f:{?["B"=last each string x;`Buy;`Sell]}
f[(`9280408_B`71100_B)]
`Buy`Buy

/ so x = list of syms
/ first convert x from sym to string
/ then using indexing, obtain the LAST element of EACH
/ nested entry (since input = list of strings)
/ if this = "B", then return `buy
```

```q
/ 11. What is wrong with this function:

add1:{[a] {a+1}[a]}

/ a is a local variable
/ so the inner function of {a+1} doesnt know what a is

/ can fix by doing this:

add1:{[a] {[a] a+1} [a]} / declare explicit variable

add1[2]
3
```

```q

/ 12. Create a monadic function that takes an integer and returns the symbol:
/ `fizz if number divisible by 3
/ `buzz if the number divisible by 5
/ `fizzbuzz if the number divisible by both 3 and 5
/ otherwise just return the number itself

/ pass numbers 1-100 to this function

f:{[x] $[0=x mod 3; `fizz; 0=x mod 5; `buzz; x]}

/ so this takes care of outcomes for `fizz and `buzz
/ but you still need to figure out `fizzbuzz (both 3 and 5)
/ need to insert ANOTHER if/else conditional in the middle

f:{[x] $[0=x mod 3; $[0=x mod 5; `fizzbuzz; `fizz]; 0=x mod 5; `buzz; x]}

/ syntax is:

IF[condition1; IF[condition2a; ifTrue; ifFalse]; condition2b; ifTrue; ifFalse]

/ if condition1 is true, move onto condition2a. if true, means divisible by both 3 and 5, hence `fizzbuzz
/ if condition1 is true, move onto condition2a. if false, means only divisible by 3, hence `fizz
/ if condition1 is false, skip middle condition2a, move onto condition2b

/ now to CALL the function as a list:

a: 1+ til 100
/ a = a list from 1 to 100

f[a]
error
/ this doesn't work since you are trying to feed in a list

/ need to do this

f each a
1 2 `fizz 4 `buzz `fizz 7 8
```

```q
/ 13. Create a monadic function that takes a list of string values in distances in both miles and km
/ extract the distance value and any value that is in miles, convert to km (multiply miles by 1.609)
/ return a numerical list (all in km)

/ input: ("1 mile"; "5 km"; "3 miles")
/ output 1.609 5 4.827

/ part 1. create boolean "LIKE" for miles
b: x like "*mile*"

/ so if x contains the string "mile" will be stored as b

/ part 2. extract numerical value from mile string
/ the number is the first part of the string ("1 mile")
/ vs is used to split up strings

" " vs/: ("1 mile"; "3 mile")
(("1";"mile");("3";"mile")) / string successfully split based on space " " delimiter

/ so need to use each RIGHT to apply vs through every element in list of strings
/ if only 1 element simply this:

" " vs "1 mile"
("1";"mile")


/ part 3. convert first element of each string to a float (the number)

v: "F"$first each " " vs/: ("1 mile"; "3 mile")
1 3F

/ so take the FIRST element of EACH nested string
/ and cast to a float
/ and save as variable v

/ part 4. Core Logic (if miles, convert to km * 1.609)

?[b; 1.609*v; v]

/ if string contains mile string (b)
/ extract the number (v) and multiply by 1.609
/ otherwise, simply return the number (v)

/ part 5. put it all together

f:{b: x like "*mile*"; v: "F"$first each " " vs/: x; ?[b; 1.609*v; v]}
f ("1 mile"; "5 km"; "3 mile")
1.609 5 4.827


/ remember, when using vector conditional, need to use ? instead of $
```

<a name="dict"></a>
### 🔴 [13.0] Dictionaries
[Top](#top)

```q
/ dictionaries are first order datatypes in q
/ associated type of 99h

/ since dictionaries are first order types, can apply functions to them

d: `a`b ! 1 2

a | 1
b | 2

d + 3

a | 4
b | 5

d * 1 2 / pairwise vector multiplication

a | 1
b | 4
```

```q
/ Create a dictionary from 2 lists

names: `john`steve`rachel
ages: 20 21 22

dict: names ! ages

john   | 20
steve  | 21
rachel | 22
```

```q
/ Creating a dictionary from nested lists

(`arthur`dent; `allen`sherman) ! 10 20

arthur dent   | 10
allen sherman | 20
```

```q
/ Create an empty dictionary:

a: () ! ()

```

```q
/ Create a single element dictionary with key jan and value 1

(enlist `jan) ! enlist 1

jan | 1

/ note you don't need brackets for value
/ need to use enlist since single item
```

```q
/ assume this dictionary:

dict: `john`steve`rachel ! 20 21 22

john   | 20
steve  | 21
rachel | 22

/ we can specify specific KEYs, and the corresponding VALUES are returned:

dict `john
dict[`john]
20

```

```q
/ Pass a list of keys to retrieve multiple corresponding values

L: `john`steve`rachel

dict[L]
20 21 22

/ can pass multiple keys
/ to retrieve multiple values
```

Using ? Find on Dictionaries

```q
10 20 30 40 ? 20
1

/ from list, FIND the index location of 20

/ given the dict below, retrieve the key from range 20

dict:
john   | 20
steve  | 21
rachel | 22

dict?20
`john
```

```q
/ Create dict with entries for `john`steve`rachel with ranges jan, sep, oct (as strings)

dict: `john`steve`rach ! ("jan";"sep";"oct")
john  | "jan"
steve | "sep"
rach  | "oct"

/ 1. what is steve's birth month?

dict[`steve]
"sep"

/ 2. Who's birthday was in Oct?

dict?"oct"
`Steve

/ so remember, if you are looking for VALUES, can use dict[key]
/ if you are looking for KEYS, need to use dict ? value
```

```q
/ assume the dict:

dict: `john`allen`sherman ! 10 10 10

john    | 10 
allen   | 10
sherman | 10

/ 1. update john's value to 20

dict[`john]: 20

/ 2. subtract 1 from john's value

dict[`john]: dict[`john]-1
john | 9

/ note you have to re-save the output as the key
/ so you retrieve the VALUE from dict[`john], then -1 (9)
/ then save this new value to dict[`john]

/ alternative syntax:

dict[`john]-:1

/ similar to a:a-1
/ a-:1
```
Removing Entries

```q
/ use drop _ to remove entries in dictionaries

dict: `tom`brian`steve`sarah`jane! (18 19 20 21 22)

/ 1. drop the first 2 entries from dict

2 _ dict

steve	| 20
sarah	| 21
jane	 | 22

/ notice the whitespace before and after the _
```

```q
/ 2. drop tom and jane from dict

`tom`jane _ dict

brian |	19
steve	| 20
sarah	| 21
```

take # from dictionaries

```q
/ can use # take to retrieve subset of diction

/ 1. retrieve only the values for tom and jane

`tom`jane # dict

tom  | 18
jane | 22 
```

```q
/ 2. What happens when you attempt to retrieve a key that isnt defined?

`casper`tom # dict
casper |
tom    | 18    

/ returns a null
```

```q
/ 3. Retrieve casper and tom from dict. use 100 for any missing values

100^`casper`tom # dict
casper | 100
tom    | 18

/ use carrot ^ to fill missing values
```

appending new values to dict

```q
/ assignment method

dict[`olivia]: 100

/ indexing a key that doesn't exist appends it

/ can add in bulk
dict[`sally`joe]: 23 24
```

joining 2 dict

```q
/ use join , to join 2 dicts

d1: `AAPL`IBM`KX! 10 20 30
d2: `FD`MSFT ! 40 50

d1, d2

AAPL	| 10
IBM	 | 20
KX	  | 30
FD	  | 40
MSFT	| 50
```

```q
/ find the common keys between d1 and d2

d1: `AAPL`IBM`KX! 10 20 30
d2: `KX`MSFT ! 40 50

key [d1] inter key d2

/ use inter to find common keys
/ note you have to bracket d1
```

coalesce ^

```q
/ coalesce ^ employs upsert semantics to merge 2 dict

d1: `a`b`c!10 20 30
d2: `b`c`d!200 0N 400

d1^d2

a	| 10
b	| 200
c	| 30
d	| 400

/ so rather than keeping null from d2, retains value from d1
/ if match on key, takes VALUE from d2
/ ONLY retains d1 value if d2 value = null
```

```q
/ for a list of incoming bids, if the first bid is null, replace this null with 0.0

bids: 0n 1 2 3f

if[null first bids; bids[0]:0.0]
bids
0 1 2 3f
/ if first element in [bids] is null, then upsert index pos 0 with 0.0
```

Associations

```q
/ associations provide easy way to extend this solution to handle more than 1 datatype
/ the following provides initial values for simple lists of
/ type char symbol int and float

/ assume this dict:

dict: 10 11 6 9h ! ("f"; `first; 0i; 0.0)

10 | "f"
11 | `first
6  | 0i
9  | 0f

/ assume data is a list of incoming data
/ at runtime, data will be one of these 4 datatypes
/ null replacement becomes

/ data is a list of syms.
/ first element is null =`
data:``a`c`h

if[null first data; data[0]: dict type data]
data
`first`a`c`h

/ if first element of data is null
/ replace index pos 0 of data with
/ the datatype of data (sym) corresponding value in dict (`first)
```

```q
/ Create a dict of error messages

code:`c`t`o!`corruptdata`typemismatch`outofrange

c |	corruptdata
t	| typemismatch
o	| outofrange

/ lets say you have a list of errors that occur within a program
/ you can decode this to make it more readable

errors:`c`o`t`o`o`c
code[errors]
`corruptdata`outofrange`typemismatch`outofrange`outofrange`corruptdata

/ converts the error codes into more readable format
/ by substituiting keys with their associated values
```
Dictionaries and code readability

```q
/ when dict gets too large they can be difficult to read
/ can use alternative syntax for easier reading
/ dict can be generated in more clear and readable format
/ use the . operator

.[!; flip ((`is;("caught";"in a landslide"));
           (`this;`no`escape`from);
           (`real;2 3))]
----------------------------------
is	  | ("caught";"in a landslide")
this	| `no`escape`from
real	| 2 3

/ so note how the dict is easier to read
```
```q
/ You can combine multiple dictionaries into a nested dictionary

/ Assume the following dicts:

nums: 1 2 3 ! `one`two`three

1 | `one
2 | `two
3 | `three

times: 12:00 00:00 ! `noon`midnight

12:00 | `noon
00:00 | `midnight

dates: 2020.04.03 2020.04.02!`today`yesterday

2020.04.03 | `today
2020.04.02 | `yesterday


/ you can COMBINE these dictionaries into a single dictionary!

dict: `nums`times`dates! (nums; times; dates)

Key	  | Value
------------------------------------
nums	 | 1 2 3!`one`two`three
times	| (12:00u;00:00u)!`noon`midnight
dates	| (2020.04.03d;2020.04.02d)!`today`yesterday

/ so you set the KEYS as syms of the NAMES of dict
/ then you set the VALUES as the dictionaries themselves
```
```q
/ 2. From dict, retrieve the word `two

dict[`nums; 2]
`two

/ index nums dict
/ then indexing the key 2
/ to retrieve value `two
```
```q
/ 3. Using reverse lookup, retrieve yesterday's date from dict

dict[`dates]?`yesterday
2020.04.02

/ so dict[key] ? value
/ reverse because you "find" value to retrieve key
```

```q
/ 4. Assume dict airport:

airport:`JFK`DUB`LHR ! ("John Kennedy"; "Dublin"; "London")

Key	Value
-------------------
JFK |	John Kennedy
DUB	| Dublin
LHR	| London

/ return the list of airport names corresponding to DUB, LHR, SAN

airport[`DUB`LHR`SAN]
("Dublin";"London";"")

/ Since no SAN, it returns ""
```

```q
/ below are two dictionaries: dictContinents and dict

dictContinents: 1 2 3 4 ! ("eu";"sa";"us";"as")
dictContinents
1 |	eu
2	| sa
3	| us
4	| as

dict:`Italy`Spain`Norway`Brazil`US`Yemen`Mexico`Albania`Japan`AU ! 1 1 1 2 3 4 2 1 4 5

Italy   |	1
Spain	  | 1
Norway	 | 1
Brazil	 | 2
US	     | 3
Yemen	  | 4
Mexico	 | 2
Albania	| 1
Japan	  | 4
AU	     | 5

/ 1. Using the 2 dictionaries, retrieve the relevant values based on the associated numbers
/ 1 = eu, 2 = sa, etc.

newdict: key [dict] ! dictContinents value dict

Italy   |	eu
Spain	  | eu
Norway	 | eu
Brazil	 | sa
US	     | us
Yemen	  | as
Mexico	 | sa
Albania	| eu
Japan	  | as
AU	     |

/ so value dict = retrieves values form dict = 1 1 1 2 3 4 2 1 4 5
/ then you use those as KEYS, and index retrieve from dictContinents
/ so dictContinents[1] = "eu" (for example)
/ so it looks like DictContinents[1 1 1 2 3 4 2 1 4 5]
/ and that gets you a list of "eu";"eu";"eu";"sa";"us";"as";"sa";"eu";"as";""

/ 2. Replace any missing values in newdict with "na"

newdict[where 0=count each newdict]: enlist"na"

Italy   |	eu
Spain	  | eu
Norway	 | eu
Brazil	 | sa
US	     | us
Yemen	  | as
Mexico	 | sa
Albania	| eu
Japan	  | as
AU	     | na

/ your KEY becomes count each entry of newdict
/ and if any values = 0 characters
/ replace with "na"
```

Dictionary Functions

```q
/ 1. Create a dyadic func that accepts keys and values and returns a dict

f:{[x;y] x ! y}
f["kdb";1 2 3]

k | 1 
d | 2
b | 3
```

```q
/ 2. Create a func that accepts a dict as an argument and returns the key and count of each value

dict:{[x] count each x}

/ now assume this wacky dictionary

`k`d`b!("four";`a`b`c; enlist 7)

k	| four
d	| `a`b`c
b	| ",7"

/ try feeding this dict into f and see what you get

dict`k`d`b!("four";`a`b`c; enlist 7)
k | 4
d | 3
b | 1
```

 <a name="tables"></a>
### 🔴 [14.0] Tables
[Top](#top)

```q
example: ([] a: 1 2 3; b: `mini`example`table)
keyedExample: ([b:`mini`keyboard`example] a: 1 2 3)
complexTab:([] c:(1 2; 3 4 ; 5 6); d: `more`complex`table

/ 1. what tables do we have in our workspace?

tables[]
`example`complexTab`keyedExample

/ 2. How many records are in example?

count example
3

/ 3. What column names are in table example?

cols example
`a`b
```

```q
/ 4. What type is a table? What type is a keyed table?

type example
98h / table

type keyedExample
99h / a keyed table is a dictionary!
```

Unkeyed Tables

```q
/ an unkeyed table is a list of dictionaries
/ each column in the table is treated as a vector

example
a  b
------
1  `mini
2  `example
3  `table 

/ 5. Use indexing to retrieve values in column b

example[`b]
`mini`example`table

/ alternative syntax

example`b
`mini`example`table

/ 6. Use index at depth to retrieve first 2 items from b

example[`b; 0 1]
`mini`example
```

Keyed Tables

```q
/ a keyed table is a special form of dictionary where both keys and values are both tables

keyedExample
`b`	       | a
-------------
`mini`	    | 1
`keyboard`	| 2
`example`	 | 3


/ first column b is a keyed column

/ 1. Retrieve the list of keys from keyedExample

key keyedExample

b
----
mini
keyed
example

/ use KEY operator

/ 2. Retrieve list of values from keyedExample

value keyedExample
a
--
1
2
3
```

```q
/ You can make a keyed table (table to table dictionary)

example
a  b
------------
1  `mini
2  `example
3  `table

complexTab
c   | d
------------
1 2 |	more
3 4	| complex
5 6	| table

/ 3. by explicitly using the ! keyword with 2 unkeyed tables of equal length

example!complexTab

`a	b`       |	c	d
--------------------
`1	mini`	   | 1 2	more
`2	example` |	3 4	complex
`3	table`	  | 5 6	table

/ result is a keyed table

```

Keyed Table Operations

```q
/ 4. What happens when you use FIRST on a keyed table?

keyedExample
b	       | a
-------------
mini	    | 1
keyboard	| 2
example	 | 3

first keyedExample
a | 1

/ returns dict association of our keyed tables "value"

/ same thing as:

first value keyedExample
a | 1
```

```q
/ 5. what happens when you try to index retrieve on a keyed table?

keyedExample[`b]
a |

/ retrieving columns by name does not work on keyed tables!
```

```q
/ 6. Retrieve key column from keyedExample

key keyedExample
b
----
mini
keyed
example

/ 7. use key value `mini to retrieve associated 'value table' values

keyedExample[`mini]
a | 1

keyedExample[`example]
a | 3
```

Adding Keys

```q
/ you can dynamically amend a table to a keyed table by using xkey

newkey:`a xkey example

`a`   | b
---------------
`1`   | mini
`2`   | example
`3`   | table

/ column a is now keyed
```
```q
/ 1. why is this an error?

newkey[`mini]
error

/ can't use VALUES for index retrieve

/ need to do this:

newkey?`mini

key | value
----------
a   | 1

```

```q
/ 2. remove the key from newkey

() xkey newkey

a | b
-------
1	| mini
2	| example
3	| table
```

```q
/ 3. what's another way to add/remove keyed columns in a table?

0! newkey
/ this unkeys the table

1! newkey
/ this makes the first column keyed

```

```q
/ 4. What happens when you try to create a table with 1 row?

t:([] sym:`jpm; size: 100; price: 100.0)
error

/ need to use enlist!!

t:([] sym: enlist`jpm; size: enlist 100; price: enlist 100.0)

sym | size | price
-------------------
jpm | 100  | 100
```

```q
/ 5. create an empty table with columns sym, size, price

t:([] sym:(); size:(); price:())

/ 6. specify the types associated with each column:
/ sym = syms, size = longs, price= floats

t:([] sym:`$(); size: `long$(); price: `float$())

/ restrict the datatypes by "casting" empty lists
```





