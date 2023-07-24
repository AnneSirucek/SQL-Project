What issues will you address by cleaning the data?

1.	I used the query below to create a view with the columns from the all_sessions table that I thought were necessary to answer the project questions. I also looked at deleting the columns in the original all_sessions table that didn’t appear to have values but most of them did contain some data or it looked like they could be populated with information at some point.

CREATE VIEW all_sessions_v2 AS
SELECT city, country, “totalTransactionRevenue”, transactions, date, “visitId”, “productQuantity”, “productPrice”, “productSKU”, “v2ProductName”, “v2ProductCategory”
FROM all_sessions
WHERE “totalTransactionRevenu” IS NOT NULL

Query result:

city	      country		    totalTrRev	trans	  date	    visitId	    prodQuan	prodPrice	  productSKU	etc…
"Palo Alto""United States"305000000	  1	      20161116  1479318391		        119000000	  "GGOENEBQ078999"	

2.	As per the project instructions, the unit cost was divided by 1,000,000.
   
SELECT "productPrice",  ROUND(("productPrice"/1000000), 2) AS adjusted_price
FROM all_sessions_v2
LIMIT 5;

Query Result:

productPrice	adjusted_price
119000000		  119.00
149000000		  149.00
19990000		  19.99
15190000		  15.19
11200000		  11.20

3.	The following query can be used to update the city column to change the value to null when the city name is not available.

SELECT city, country,
CASE 
	WHEN city = 'not available in demo dataset' THEN NULL
	WHEN city = '(not set)' THEN NULL
	ELSE city
END AS city
FROM all_sessions_v2

4.	The following query can be used to update  the country column to change the value to null when the country name is not available.

SELECT country,
CASE 
	WHEN country = '(not set)' THEN NULL
	ELSE country
END AS country
FROM all_sessions_v2

5.	The following query can be used to update the date format to make it easier to read.
   
TO_CHAR (date, YYYY-MM-DD)
FROM all_sessions_v2









