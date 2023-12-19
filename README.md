# kx


Loading CSV Files (with headers)

```q
/ load weather.csv
/ load smalltrips.csv

/ 1. Read the CSV file to see what datatypes are in file

read0 `:weather.csv

"date,maxtemp,mintemp,avgtemp,departuretemp,hdd,cdd,precip,newsnow,snowdepth";
"2009-01-01,26,15,20.5,-12.9,44,0,0.00,0.0,0";
"2009-01-02,34,23,28.5,-4.8,36,0,T,T,0";

/ use the read0 `: function to return the contents of csv file as a list of strings
/ gives you an idea of what datatypes are contained in file
```

```q
/ 2. Determine the datatypes of table

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
/ 3. Load the file with headers using 0:

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



