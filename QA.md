What are your risk areas? Identify and describe them.

1.	 I compared my calculated product quantity from the ‘starting with questions’ with the 18 product quantity values that were existing in the all sessions table.  The following query showed that approximately half of the calculated values are not close to the existing product quantity values in the table.
   
SELECT a1."productSKU", a1."productQuantity", ROUND((a2."totalTransactionRevenue"/a2."productPrice"),1) AS calculated_quantity
FROM all_sessions_v2 AS a1
INNER JOIN all_sessions_v2 AS a2
ON a1."visitId" = a2."visitId"
WHERE a1."productQuantity" IS NOT NULL
ORDER BY a1."productSKU"

Query Result:

productSKU		productQuantity	calculated_quantity
"GGOEGAAJ032617"	  1			      1.2
"GGOEGAAJ080616"	  1			      1.1
"GGOEGAAX0104"	    1			      1.4
"GGOEGAAX0794"	    1			      1.3
"GGOEGBJR018199"	  50			    290.1
"GGOEGBJR018199"	  4			      66.3
"GGOEGEVB070899"	  1			      16.8
"GGOEGEVJ023999"	  1			      3.0
"GGOEGHGR019499"	  1			      44.3
"GGOEGOCB017499"	  65			    77.3
"GGOENEBB078899"	  1		      	1.7
"GGOENEBB078899"  	1			      3.0
"GGOENEBJ079499"	  1			      2.6
"GGOENEBJ079499"	  1			      1.0
"GGOENEBJ079499"  	1			      0.6
"GGOENEBJ079499"	  1			      1.0
"GGOENEBQ078999"	  1			      1.7
"GGOENEBQ079199"  	1			      4.1

2.	 This query shows the difference between the total transaction revenue and the values that existed in the product revenue column of the all sessions table. 

SELECT "totalTransactionRevenue", "productRevenue", "productPrice", "productQuantity"
FROM all_sessions
WHERE "productRevenue" IS NOT NULL

Query Result :

transactionRevenue	productRevenue	productPrice	productQuantity
200.00		          	"120.00"		      119.00		  1
1699.70		            "58.65"			      55.99		    1
1015.48		            "176.40"		      3.50		    50
1005.50		            "60.36"			      59.99		    1

Based on this query, the product price multiplied by the product quantity is very close to the product revenue.  This result made me interested to know if there were additional purchases made in the total transaction revenues.

3.	 The following query checks for complete duplicates in the all_sessions view table that I had created. The query did not return any complete duplicate results.

SELECT *, COUNT (*)
FROM all_sessions_v2
GROUP BY city, country, "totalTransactionRevenue", transactions, date, "visitId", "productQuantity", "productPrice", "productSKU", "v2ProductName", "v2ProductCategory"
HAVING COUNT (*) >1

4.	The following query was used to check to see if there were duplicates with the same total transaction revenue that could help to explain the difference between the product revenue and transaction revenue totals in point 2 above.

SELECT date, "visitId", "totalTransactionRevenue", COUNT ("totalTransactionRevenue") AS num_dup
FROM all_sessions_v2
GROUP BY date, "visitId", "totalTransactionRevenue"

Query Result:

Date		  visitId		  TranRevenue	num_dup
20170320	1490046065	124.00		  2

SELECT *
FROM all_sessions_v2
WHERE "visitId" = 1490046065

Query Result:

TransRev	Quan	Date		VisitId		Prodprice	ProductSKU
124.00		1	20170320	1490046065	249.00		"GGOEGAAX0794”
124.00		1	20170320	1490046065	119.00		"GGOENEBB078899"	

There was one duplicate visit Id but it did not help to explain the difference in revenues in the columns.

5.	 The following query tries to establish the connection between the analytics table and the all_sessions table,  there are many full visitor Ids that match (234,508) but this query shows that there can be a difference in the visit Id in these matches (130,096).

SELECT a."visitId" AS all_visit, an."visitId" AS an_visit
FROM all_sessions AS a
JOIN analytics AS an
ON a."fullVisitorId" = an."fullvisitorId"
WHERE a."visitId" <> an."visitId"

Query Result:

All_visit		an_visit
1498333732	1498413140
1498333732	1498413140
1498333732	1498413140
1498333732	1498413140
1498333732	1498413140

6.	Another interesting point about the analytics database is that the visit id column matches the visit start time most of the time (38,610 records did not match out of 4,301,122) as per the following query.

SELECT "visitId", "visitStartTime"
FROM analytics
WHERE "visitId" <> "visitStartTime"

Query Result:

visitId		visitStartTime
1501013399	1501013425
1501013399	1501013425
1501013399	1501013425
1501013399	1501013425
1501013399	1501013425


7.	The following queries confirms that sentiment scores and magnitude scores values match in the products table and sales_report table.
   
SELECT "SKU", p."sentimentScore", p."sentimentMagnitude"
FROM products AS p
JOIN sales_report AS s
ON p."SKU" = s."productSKU"
WHERE p."sentimentMagnitude" <> s."sentimentMagnitude"

SELECT "SKU", p."sentimentScore", p."sentimentMagnitude"
FROM products AS p
JOIN sales_report AS s
ON p."SKU" = s."productSKU"
WHERE p."sentimentScore" <> s."sentimentScore"

8.	This query confirms that the total ordered in the sales report table and the sales by sku tables match.

SELECT s."productSKU", ss."total_ordered", s."total_ordered"
FROM sales_by_sku AS ss
JOIN sales_report AS s
ON ss."productSKU" = s."productSKU"
WHERE ss."total_ordered"<>s."total_ordered"

9.	The following query result shows that the ordered quantity in the products table and the total ordered in the sales_report represent different things.

SELECT "SKU", p."ordereedQuantity", s."total_ordered"
FROM products AS p
JOIN sales_report AS s
ON p."SKU" = s."productSKU"

Query Result:

SKU			      orderedQuantity	total_ordered
"GGOEGAAX0581"	0		        	0
"GGOEGAAX0596"	26		      	1
"GGOEGAAX0365"	65		      	0
"GGOEGAAX0325"	53			      6
"GGOEGAAX0296"	19		      	0

10.	 The following query shows a comparison between the quantity ordered in the all sessions table and the total ordered values in the sales by sku table.  There doesn’t appear to be a relationship between these values.

SELECT a."productSKU", a."productQuantity", s."total_ordered"
FROM all_sessions_v2 AS a
JOIN sales_by_sku AS s
ON a."productSKU" = s."productSKU"
WHERE "productQuantity" IS NOT NULL

Query Result:

productSKU		  productQuantity	total_ordered
"GGOENEBJ079499"	1			        94
"GGOENEBB078899"	1		        	102
"GGOEGOCB017499"	65		      	319
"GGOEGAAX0104"	  1			        15
"GGOENEBQ078999"	1			        112
"GGOENEBJ079499"	1			        94
"GGOENEBJ079499"	1			        94
"GGOENEBQ079199"	1			        42
"GGOENEBJ079499"	1			        94
"GGOEGHGR019499"	1			        26
"GGOENEBB078899"	1			        102



