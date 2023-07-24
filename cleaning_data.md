What issues will you address by cleaning the data?

1.	I used the query below created a view with the columns from the all_sessions table that I thought were necessary to answer the project questions. I also looked at deleting the columns in the original all_sessions table that didn’t appear to have values but most of them did contain some data or it looked like they could be populated with information at some point.

CREATE VIEW all_sessions_v2 AS
SELECT city, country, “totalTransactionRevenue”, transactions, date, “visitId”, “productQuantity”, “productPrice”, “productSKU”, “v2ProductName”, “v2ProductCategory”
FROM all_sessions
WHERE “totalTransactionRevenu” IS NOT NULL

Query result:

city	      country		    totalTrRev	trans	  date	    visitId	    prodQuan	prodPrice	  productSKU	etc…
"Palo Alto""United States"305000000	  1	      20161116  1479318391		        119000000	  "GGOENEBQ078999"	








Queries:
Below, provide the SQL queries you used to clean your data.
