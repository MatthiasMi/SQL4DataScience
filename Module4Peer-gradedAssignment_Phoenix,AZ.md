# [Yelp Dataset ER Diagram](YelpDatabaseSchema.png) based solution
## Review criteria
This is a 2-part assignment. In the first part, you are asked a series of questions that will help you profile and understand the data just like a data scientist would. For this first part of the assignment, you will be assessed both on the correctness of your findings, as well as the code you used to arrive at your answer. You will be graded on how easy your code is to read, so remember to use proper formatting and comments where necessary.

In the second part of the assignment, you are asked to come up with your own inferences and analysis of the data for a particular question you want to answer. You will be required to prepare the dataset for the analysis you choose to do. As with the first part, you will be graded, in part, on how easy your code is to read, so use proper formatting and comments to illustrate your intent as required.

For both parts of this assignment, use the following "worksheet." It provides all the questions you are being asked, and your job will be to transfer your answers and SQL coding into this worksheet where indicated so that your peers can review your work. You should be able to use any Text Editor (Windows Notepad, Apple TextEdit, Notepad ++, Sublime Text, etc.) to copy and paste your answers.


## Part 1: Yelp Dataset Profiling and Understanding

1. Profile the data by finding the total number of records for each of the tables below:
	
	```sql
	SELECT COUNT(*) FROM tab;
	```

	--0.	tab table = COUNT(*)
	i. 	Attribute table = 10000
	ii. 	Business table = 10000
	iii. 	Category table = 10000
	iv. 	Checkin table = 10000
	v. 	elite_years table = 10000 
	vi. 	friend table = 10000
	vii. 	hours table = 10000
	viii. 	photo table = 10000
	ix. 	review table = 10000
	x. 	tip table = 10000
	xi. 	user table = 10000


2. Find the total number of distinct records for each of the keys listed below: Note: Primary Keys are denoted in the ER-Diagram with a yellow key icon.	

	```sql
	SELECT COUNT(DISTINCT(key)) FROM tab;
	```

	--0.	tab = key: COUNT(DISTINCT(key))
	i. 	Business = id: 10000
	ii. 	Hours = business_id: 1562
	iii. 	Category = business_id: 2643
	iv. 	Attribute = business_id: 1115
	v. 	Review = id: 10000, business_id: 8090, user_id: 9581
	vi. 	Checkin = business_id: 493
	vii. 	Photo = id: 10000, business_id: 6493
	viii. 	Tip = user_id: 537, business_id: 3979
	ix. 	User = id: 10000
	x. 	Friend = user_id: 11
	xi. 	Elite_years = user_id: 2780


3. Are there any columns with null values in the Users table? Indicate "yes," or "no."

	Answer: "no"
	
	SQL code used to arrive at answer:

	```sql
	SELECT COUNT(*) FROM user WHERE
	  id IS NULL OR 
	  name IS NULL OR 
	  review_count IS NULL OR 
	  yelping_since IS NULL OR
	  useful IS NULL OR 
	  funny IS NULL OR 
	  cool IS NULL OR 
	  fans IS NULL OR 
	  average_stars IS NULL OR 
	  compliment_hot IS NULL OR 
	  compliment_more IS NULL OR 
	  compliment_profile IS NULL OR 
	  compliment_cute IS NULL OR 
	  compliment_list IS NULL OR 
	  compliment_note IS NULL OR 
	  compliment_plain IS NULL OR 
	  compliment_cool IS NULL OR 
	  compliment_funny IS NULL OR 
	  compliment_writer IS NULL OR 
	  compliment_photos IS NULL
	;
	```
	or
	```sql
	SELECT COUNT(*) FROM user WHERE
	COALESCE(id, name, review_count, yelping_since, useful,
	funny, cool, fans, average_stars, compliment_hot,
	compliment_more, compliment_profile, compliment_cute, compliment_list, 
	compliment_note, compliment_plain, compliment_cool,
	compliment_funny,compliment_writer, compliment_photos) IS NULL 
	;
	```

	
4. For each table and column listed below, display the smallest (minimum), largest (maximum), and average (mean) value for the following fields:

	```sql
	SELECT MIN(col), MAX(col), AVG(col) FROM tab;
	```

	i. Table: Review, Column: Stars
	
		min: 1		max: 5		avg: 3.7082
		
	
	ii. Table: Business, Column: Stars
	
		min: 1 		max: 5		avg: 3.6549
		
	
	iii. Table: Tip, Column: Likes
	
		min: 0		max: 2		avg: 0.0144
		
	
	iv. Table: Checkin, Column: Count
	
		min: 1		max: 53		avg: 1.9414
		
	
	v. Table: User, Column: Review_count
	
		min: 0		max: 2000		avg: 24.2995
		

5. List the cities with the most reviews in descending order:

	SQL code used to arrive at answer:

	```sql
	SELECT city, SUM(review_count) AS reviews
	FROM business
	GROUP BY city
	ORDER BY reviews DESC
	;
	```
	
	Copy and Paste the Result Below:
	
		+-----------------+---------+
		| city            | reviews |
		+-----------------+---------+
		| Las Vegas       |   82854 |
		| Phoenix         |   34503 |
		| Toronto         |   24113 |
		| Scottsdale      |   20614 |
		| Charlotte       |   12523 |
		| Henderson       |   10871 |
		| Tempe           |   10504 |
		| Pittsburgh      |    9798 |
		| Montréal        |    9448 |
		| Chandler        |    8112 |
		| Mesa            |    6875 |
		| Gilbert         |    6380 |
		| Cleveland       |    5593 |
		| Madison         |    5265 |
		| Glendale        |    4406 |
		| Mississauga     |    3814 |
		| Edinburgh       |    2792 |
		| Peoria          |    2624 |
		| North Las Vegas |    2438 |
		| Markham         |    2352 |
		| Champaign       |    2029 |
		| Stuttgart       |    1849 |
		| Surprise        |    1520 |
		| Lakewood        |    1465 |
		| Goodyear        |    1155 |
		+-----------------+---------+

	
6. Find the distribution of star ratings to the business in the following cities:

	i. Avon
	
		SQL code used to arrive at answer:
		
		```sql
		SELECT stars, SUM(review_count) AS count
		FROM business
		WHERE city == 'Avon'
		GROUP BY stars
		;
		```
		
		Copy and Paste the Resulting Table Below (2 columns - star rating and count):
	
			+-------+-------+
			| stars | count |
			+-------+-------+
			|   1.5 |    10 |
			|   2.5 |     6 |
			|   3.5 |    88 |
			|   4.0 |    21 |
			|   4.5 |    31 |
			|   5.0 |     3 |
			+-------+-------+	
	
	
	ii. Beachwood

		SQL code used to arrive at answer:

		```sql
		SELECT stars, SUM(review_count) AS count
		FROM business
		WHERE city == 'Beachwood'
		GROUP BY stars
		;
		```

		Copy and Paste the Resulting Table Below (2 columns - star rating and count):
		
			+-------+-------+
			| stars | count |
			+-------+-------+
			|   2.0 |     8 |
			|   2.5 |     3 |
			|   3.0 |    11 |
			|   3.5 |     6 |
			|   4.0 |    69 |
			|   4.5 |    17 |
			|   5.0 |    23 |
			+-------+-------+
		

7. Find the top 3 users based on their total number of reviews:
		
	SQL code used to arrive at answer:

		```sql
		SELECT id, name, review_count
		FROM user
		ORDER BY review_count DESC
		LIMIT 3
		;
		```

	Copy and Paste the Result Below:
		
		+------------------------+--------+--------------+
		| id                     | name   | review_count |
		+------------------------+--------+--------------+
		| -G7Zkl1wIWBBmD0KRy_sCw | Gerald |         2000 |
		| -3s52C4zL_DHRK0ULG6qtg | Sara   |         1629 |
		| -8lbUNlXVSoXqaRRiHiSNg | Yuri   |         1339 |
		+------------------------+--------+--------------+

8. Does posing more reviews correlate with more fans? 

		Yes, the data suggests the time since they have been yelping and more reviews correlate with a higher fan count. To say this hypothesis holds with significance more in-depth analysis is required.

	Please explain your findings and interpretation of the results:

		```sql
		SELECT id, name, review_count, yelping_since, fans
		FROM user
		ORDER BY fans DESC
		;
		```

		+------------------------+-----------+--------------+---------------------+------+
		| id                     | name      | review_count | yelping_since       | fans |
		+------------------------+-----------+--------------+---------------------+------+
		| -9I98YbNQnLdAmcYfb324Q | Amy       |          609 | 2007-07-19 00:00:00 |  503 |
		| -8EnCioUmDygAbsYZmTeRQ | Mimi      |          968 | 2011-03-30 00:00:00 |  497 |
		| --2vR0DIsmQ6WfcSzKWigw | Harald    |         1153 | 2012-11-27 00:00:00 |  311 |
		| -G7Zkl1wIWBBmD0KRy_sCw | Gerald    |         2000 | 2012-12-16 00:00:00 |  253 |
		| -0IiMAZI2SsQ7VmyzJjokQ | Christine |          930 | 2009-07-08 00:00:00 |  173 |
		| -g3XIcCb2b-BD0QBCcq2Sw | Lisa      |          813 | 2009-10-05 00:00:00 |  159 |
		| -9bbDysuiWeo2VShFJJtcw | Cat       |          377 | 2009-02-05 00:00:00 |  133 |
		| -FZBTkAZEXoP7CYvRV2ZwQ | William   |         1215 | 2015-02-19 00:00:00 |  126 |
		| -9da1xk7zgnnfO1uTVYGkA | Fran      |          862 | 2012-04-05 00:00:00 |  124 |
		| -lh59ko3dxChBSZ9U7LfUw | Lissa     |          834 | 2007-08-14 00:00:00 |  120 |
		| -B-QEUESGWHPE_889WJaeg | Mark      |          861 | 2009-05-31 00:00:00 |  115 |
		| -DmqnhW4Omr3YhmnigaqHg | Tiffany   |          408 | 2008-10-28 00:00:00 |  111 |
		| -cv9PPT7IHux7XUc9dOpkg | bernice   |          255 | 2007-08-29 00:00:00 |  105 |
		| -DFCC64NXgqrxlO8aLU5rg | Roanna    |         1039 | 2006-03-28 00:00:00 |  104 |
		| -IgKkE8JvYNWeGu8ze4P8Q | Angela    |          694 | 2010-10-01 00:00:00 |  101 |
		| -K2Tcgh2EKX6e6HqqIrBIQ | .Hon      |         1246 | 2006-07-19 00:00:00 |  101 |
		| -4viTt9UC44lWCFJwleMNQ | Ben       |          307 | 2007-03-10 00:00:00 |   96 |
		| -3i9bhfvrM3F1wsC9XIB8g | Linda     |          584 | 2005-08-07 00:00:00 |   89 |
		| -kLVfaJytOJY2-QdQoCcNQ | Christina |          842 | 2012-10-08 00:00:00 |   85 |
		| -ePh4Prox7ZXnEBNGKyUEA | Jessica   |          220 | 2009-01-12 00:00:00 |   84 |
		| -4BEUkLvHQntN6qPfKJP2w | Greg      |          408 | 2008-02-16 00:00:00 |   81 |
		| -C-l8EHSLXtZZVfUAUhsPA | Nieves    |          178 | 2013-07-08 00:00:00 |   80 |
		| -dw8f7FLaUmWR7bfJ_Yf0w | Sui       |          754 | 2009-09-07 00:00:00 |   78 |
		| -8lbUNlXVSoXqaRRiHiSNg | Yuri      |         1339 | 2008-01-03 00:00:00 |   76 |
		| -0zEEaDFIjABtPQni0XlHA | Nicole    |          161 | 2009-04-30 00:00:00 |   73 |
		+------------------------+-----------+--------------+---------------------+------+
		(Output limit exceeded, 25 of 10000 total rows shown)

	
9. Are there more reviews with the word "love" or with the word "hate" in them?

	Answer: love occurs 1780, while hate only 232 times.

	SQL code used to arrive at answer:
		```sql
		SELECT love, hate FROM
		(SELECT COUNT(*) AS love FROM review WHERE text LIKE "%love%")
		LEFT JOIN
		(SELECT COUNT(*) AS hate FROM review WHERE text LIKE "%hate%")
		;
		```
		or
		```sql
		SELECT count(id) AS Love, 
		(SELECT count(id) FROM review WHERE text LIKE '%hate%') AS Hate
		FROM review WHERE text like '%love%'
		;
		```
	
10. Find the top 10 users with the most fans:

	SQL code used to arrive at answer:

		```sql
		SELECT id, name, fans FROM user
		ORDER BY fans DESC
		LIMIT 10
		;
		```
	
	Copy and Paste the Result Below:

		+------------------------+-----------+------+
		| id                     | name      | fans |
		+------------------------+-----------+------+
		| -9I98YbNQnLdAmcYfb324Q | Amy       |  503 |
		| -8EnCioUmDygAbsYZmTeRQ | Mimi      |  497 |
		| --2vR0DIsmQ6WfcSzKWigw | Harald    |  311 |
		| -G7Zkl1wIWBBmD0KRy_sCw | Gerald    |  253 |
		| -0IiMAZI2SsQ7VmyzJjokQ | Christine |  173 |
		| -g3XIcCb2b-BD0QBCcq2Sw | Lisa      |  159 |
		| -9bbDysuiWeo2VShFJJtcw | Cat       |  133 |
		| -FZBTkAZEXoP7CYvRV2ZwQ | William   |  126 |
		| -9da1xk7zgnnfO1uTVYGkA | Fran      |  124 |
		| -lh59ko3dxChBSZ9U7LfUw | Lissa     |  120 |
		+------------------------+-----------+------+


## Part 2: Inferences and Analysis

1. Pick one city and category of your choice and group the businesses in that city or category by their overall star rating. Compare the businesses with 2-3 stars to the businesses with 4-5 stars and answer the following questions. Include your code.
	
	i. Do the two groups you chose to analyze have a different distribution of hours?
	
		The data suggests that the 4-5 star group has shorter hours compared with the 2-3 star group, possibly biased by the small sample size.
	
	ii.	Do the two groups you chose to analyze have a different number of reviews?
	
		Yes, they have a different number of reviews.
	
	iii.	Are you able to infer anything from the location data provided between these two
	groups? Explain.

		No, each business has a different zip-code.

	SQL code used for analysis:
	
		```sql
		SELECT B.name, B.review_count, H.hours, postal_code,
		CASE
			WHEN hours LIKE "%monday%" THEN 1
			WHEN hours LIKE "%tuesday%" THEN 2
			WHEN hours LIKE "%wednesday%" THEN 3
			WHEN hours LIKE "%thursday%" THEN 4
			WHEN hours LIKE "%friday%" THEN 5
			WHEN hours LIKE "%saturday%" THEN 6
			WHEN hours LIKE "%sunday%" THEN 7
		END AS bin,
		CASE -- introduce abbreviations
			WHEN B.stars BETWEEN 2 AND 3 THEN '2-3 stars'
			WHEN B.stars BETWEEN 4 AND 5 THEN '4-5 stars'
		END AS star_rating
		FROM business B INNER JOIN hours H
		ON B.id = H.business_id
		INNER JOIN category C
		ON C.business_id = B.id WHERE
        (B.city == 'Las Vegas' AND C.category LIKE 'shopping')
		AND
		(B.stars BETWEEN 2 AND 3 OR B.stars BETWEEN 4 AND 5)
		GROUP BY stars, bin
		ORDER BY bin, star_rating ASC
		```


2. Group business based on the ones that are open and the ones that are closed. What differences can you find between the ones that are still open and the ones that are closed? List at least two differences and the SQL code you used to arrive at your answer.
		
	i. Difference 1:
	
		Open businesses have more reviews than closed ones on average.
		
		Open:   AVG(review_count) = 31.8
		Closed: AVG(review_count) = 23.2
	
	ii.	Difference 2:
	
		Open businesses have a higher star rating than closed ones on average.
	
		Open:   AVG(stars) = 3.7
		Closed: AVG(stars) = 3.5
	
	SQL code used for analysis:

		```sql
		SELECT COUNT(DISTINCT(id)), AVG(review_count), SUM(review_count), AVG(stars), is_open
		FROM business
		GROUP BY is_open
		;
		```
	
	
3. For this last part of your analysis, you are going to choose the type of analysis you want to conduct on the Yelp dataset and are going to prepare the data for analysis.

Ideas for analysis include: Parsing out keywords and business attributes for sentiment analysis, clustering businesses to find commonalities or anomalies between them, predicting the overall star rating for a business, predicting the number of fans a user will have, and so on. These are just a few examples to get you started, so feel free to be creative and come up with your own problem you want to solve. Provide answers, in-line, to all of the following:
	
	i. Indicate the type of analysis you chose to do:
	
		Predicting whether a business will stay open or closed, excluding review texts.
	
	ii.	Write 1-2 brief paragraphs on the type of data you will need for your analysis and why you chose that data:
		
		Helping businesses in Phoenix (state "AZ") understand factors for their business staying open. While `is_open` determines which business is open or has closed permanently, more data is important: number of reviews, star rating of business, hours open, and adress. Categories and attributes can be used for manual post-processing.
		
	iii.	Output of your finished dataset:

	+---------+--------------+-------+------------------------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+---------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| is_open | review_count | stars | B.name || ' (' || B.postal_code || ', ' || B.address || ')'      | MON         | TUE         | WED         | THU         | FRI         | SAT         | SUN         | categories                                                                      | attributes                                                                                                                                                                                                                                                                                                                                                                                        |
+---------+--------------+-------+------------------------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+---------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|       0 |            7 |   4.5 | Charlie D's Catfish & Chicken (85034, 1153 E Jefferson St)       | 11:00-18:00 | 11:00-18:00 | 11:00-18:00 | 11:00-18:00 | 11:00-18:00 | 11:00-18:00 | 13:00-16:00 | American (Traditional),Soul Food,Restaurants,Seafood                            | GoodForMeal,Alcohol,Caters,HasTV,RestaurantsGoodForGroups,NoiseLevel,WiFi,RestaurantsAttire,RestaurantsReservations,OutdoorSeating,BusinessAcceptsCreditCards,RestaurantsPriceRange2,RestaurantsTableService,RestaurantsDelivery,Ambience,RestaurantsTakeOut,GoodForKids,WheelchairAccessible,BusinessParking                                                                                     |
|       1 |           15 |   3.5 | Standard Restaurant Supply (85008, 2922 E McDowell Rd)           | 8:00-18:00  | 8:00-18:00  | 8:00-18:00  | 8:00-18:00  | 8:00-18:00  | 9:00-17:00  | None        | Shopping,Wholesalers,Restaurant Supplies,Professional Services,Wholesale Stores | BusinessAcceptsCreditCards,RestaurantsPriceRange2,BusinessParking,BikeParking,WheelchairAccessible                                                                                                                                                                                                                                                                                                |
|       1 |           13 |   4.0 | Pinnacle Fencing Solutions (85060, )                             | 8:00-16:00  | 8:00-16:00  | 8:00-16:00  | 8:00-16:00  | 8:00-16:00  | None        | None        | Home Services,Contractors,Fences & Gates                                        | BusinessAcceptsCreditCards,ByAppointmentOnly                                                                                                                                                                                                                                                                                                                                                      |
|       1 |           63 |   3.5 | Five Guys (85008, 2641 N 44th St, Ste 100)                       | 10:00-22:00 | 10:00-22:00 | 10:00-22:00 | 10:00-22:00 | 10:00-22:00 | 10:00-22:00 | 10:00-22:00 | American (New),Burgers,Fast Food,Restaurants                                    | RestaurantsTableService,GoodForMeal,Alcohol,Caters,HasTV,RestaurantsGoodForGroups,NoiseLevel,WiFi,RestaurantsAttire,RestaurantsReservations,OutdoorSeating,BusinessAcceptsCreditCards,RestaurantsPriceRange2,BikeParking,RestaurantsDelivery,Ambience,RestaurantsTakeOut,GoodForKids,DriveThru,BusinessParking                                                                                    |
|       1 |           52 |   3.0 | Starbucks (85048, 4605 E Chandler Blvd, Ste A)                   | 5:00-20:00  | 5:00-20:00  | 5:00-20:00  | 5:00-20:30  | 5:00-20:00  | 5:00-20:00  | 5:00-20:00  | Coffee & Tea,Food                                                               | BusinessParking,Caters,WiFi,OutdoorSeating,BusinessAcceptsCreditCards,RestaurantsPriceRange2,BikeParking,RestaurantsTakeOut                                                                                                                                                                                                                                                                       |
|       1 |            8 |   2.0 | McDonald's (85004, 1850 S 7th St)                                | 5:00-23:00  | 5:00-23:00  | 5:00-23:00  | 5:00-23:00  | 5:00-0:00   | 5:00-0:00   | 5:00-23:00  | Burgers,Restaurants,Fast Food                                                   | RestaurantsTableService,GoodForMeal,Alcohol,Caters,HasTV,RestaurantsGoodForGroups,NoiseLevel,WiFi,RestaurantsAttire,RestaurantsReservations,OutdoorSeating,BusinessAcceptsCreditCards,RestaurantsPriceRange2,BikeParking,RestaurantsDelivery,Ambience,RestaurantsTakeOut,GoodForKids,DriveThru,BusinessParking                                                                                    |
|       1 |           60 |   3.0 | Gallagher's (85024, 751 E Union Hls Dr)                          | 11:00-0:00  | 11:00-0:00  | 11:00-0:00  | 11:00-2:00  | 11:00-2:00  | 9:00-2:00   | 9:00-0:00   | Sports Bars,Bars,Nightlife,American (Traditional),Restaurants                   | Alcohol,HasTV,NoiseLevel,RestaurantsAttire,BusinessAcceptsCreditCards,Music,Ambience,RestaurantsGoodForGroups,Caters,WiFi,RestaurantsReservations,RestaurantsTableService,RestaurantsTakeOut,GoodForKids,HappyHour,GoodForDancing,BikeParking,OutdoorSeating,RestaurantsPriceRange2,RestaurantsDelivery,BestNights,GoodForMeal,BusinessParking,CoatCheck,Smoking                                  |
|       1 |           19 |   5.0 | Back-Health Chiropractic (85016, 4425 N 24th St, Ste 125)        | 14:30-17:00 | 14:00-19:00 | 14:30-17:00 | 14:00-19:00 | 9:00-12:00  | None        | None        | Chiropractors,Health & Medical                                                  | AcceptsInsurance,ByAppointmentOnly,BusinessAcceptsCreditCards                                                                                                                                                                                                                                                                                                                                     |
|       1 |          431 |   4.0 | Bootleggers Modern American Smokehouse (85028, 3375 E Shea Blvd) | 11:00-22:00 | 11:00-22:00 | 11:00-22:00 | 11:00-22:00 | 11:00-22:00 | 11:00-22:00 | 11:00-22:00 | Nightlife,Bars,Food,Restaurants,Smokehouse,American (Traditional),Barbeque      | Alcohol,HasTV,NoiseLevel,RestaurantsAttire,BusinessAcceptsCreditCards,Music,Ambience,RestaurantsGoodForGroups,Caters,WiFi,RestaurantsReservations,BikeParking,RestaurantsTakeOut,GoodForKids,HappyHour,GoodForDancing,DogsAllowed,RestaurantsTableService,OutdoorSeating,RestaurantsPriceRange2,RestaurantsDelivery,BestNights,GoodForMeal,BusinessParking,CoatCheck,Smoking,WheelchairAccessible |
+---------+--------------+-------+------------------------------------------------------------------+-------------+-------------+-------------+-------------+-------------+-------------+-------------+---------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

	iv. Provide the SQL code you used to create your final dataset:

		```sql
		SELECT B.is_open, B.review_count, B.stars,
               B.name || ' (' || B.postal_code || ', ' || B.address || ')',
		MAX(CASE
		   WHEN H.hours LIKE "%mon%" THEN TRIM(H.hours,'%MondayTuesWednesThursFriSatSun|%') 
		   END) AS MON,
		MAX(CASE
		   WHEN H.hours LIKE "%tue%" THEN TRIM(H.hours,'%MondayTuesWednesThursFriSatSun|%') 
		   END) AS TUE,
		MAX(CASE
		   WHEN H.hours LIKE "%wed%" THEN TRIM(H.hours,'%MondayTuesWednesThursFriSatSun|%') 
		   END) AS WED,
		MAX(CASE
		   WHEN H.hours LIKE "%thu%" THEN TRIM(H.hours,'%MondayTuesWednesThursFriSatSun|%') 
		   END) AS THU,
		MAX(CASE
		   WHEN H.hours LIKE "%fri%" THEN TRIM(H.hours,'%MondayTuesWednesThursFriSatSun|%') 
		   END) AS FRI,
		MAX(CASE
		   WHEN H.hours LIKE "%sat%" THEN TRIM(H.hours,'%MondayTuesWednesThursFriSatSun|%') 
		   END) AS SAT,
		MAX(CASE
		   WHEN H.hours LIKE "%sun%" THEN TRIM(H.hours,'%MondayTuesWednesThursFriSatSun|%') 
		   END) AS SUN,
		GROUP_CONCAT(DISTINCT(C.category)) AS categories,
		GROUP_CONCAT(DISTINCT(A.name)) AS attributes
		FROM business B
		INNER JOIN hours H     ON B.id = H.business_id
		INNER JOIN category C  ON B.id = C.business_id
		INNER JOIN attribute A ON B.id = A.business_id
		WHERE B.state = "AZ" AND B.city = "Phoenix"
		GROUP BY B.id
		ORDER BY B.is_open
		```
