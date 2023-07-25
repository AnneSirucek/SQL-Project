Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

This query shows the cities and countries with the highest number of revenue transactions on the site.

SELECT city, country, COUNT ("totalTransactionRevenue") AS number_transactions
FROM all_sessions_v2
GROUP BY city, country
ORDER BY number_transactions DESC
LIMIT 5;

city				            country			number_transactitons
"not available in demo dataset"	"United States"		25
"San Francisco"			        "United States"		12
"New York"			            "United States"		8
"Mountain View"		            "United States"		8
"Sunnyvale"			            "United States"		4

This query shows the cities and countries with the highest level of transaction revenues on the site.

SELECT city, country, MAX ("totalTransactionRevenue") AS transaction_revenues
FROM all_sessions_v2
GROUP BY city, country
ORDER BY transaction_revenues DESC
LIMIT 5;

City				            country			transaction_revenues
"not available in demo dataset"	"United States"		1015.48
"Atlanta"			            "United States"		742.48
"Sunnyvale"			            "United States"		649.24
"Tel Aviv-Yafo"			        "Israel"			602.00
"Los Angeles"			        "United States"		363.00





**Question 2: What is the average number of products ordered from visitors in each city and country?**

The product quantity column did not have very many values so I calculated a quantity by dividing the total transaction revenue by the product price.  I assume that there were some shipping costs or taxes included in the total cost but I thought this calculated number could be very close to the actual number of products ordered.  

Using this calculation the average number of products ordered from visitors in each city and country can be viewed with the query below. 

SELECT city, country, ROUND(SUM("totalTransactionRevenue"/"productPrice")/COUNT("visitId"),0) AS avg_quan
FROM all_sessions_v2	
GROUP BY city, country
ORDER BY avg_quan DESC
LIMIT 5;

city					country			avg_quan
"Sunnyvale"				"United States"		117
"Atlanta"				"United States"		36
"not available in demo dataset"		"United States"		25
"Chicago"				"United States"		17
"Tel Aviv-Yafo"				"Israel"			8




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

The following query shows the product categories of the products ordered from visitors in each city and country.

SELECT city, country, "v2ProductCategory", ROUND(SUM("totalTransactionRevenue"/"productPrice"),0)  AS product_quantity  
FROM all_sessions_v2	
GROUP BY city, country, "v2ProductCategory"
ORDER BY product_quantity DESC
LIMIT 5;
	
city				            country			    v2ProductName 			        product_quantity
"Sunnyvale"			            "United States"		"Housewares"				        464
"not available in demo dataset"	"United States"		"Bags"					            290
"not available in demo dataset"	"United States"		"Home/Accessories/Pet/"		        187
"not available in demo dataset"	"United States"		"Home/Office/Notebooks & Journals/"	77
"Atlanta"			            "United States"		"Bags"					            66








**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**

The top selling product is Lip Balm from Sunnyvale in the United States which was determined from using the query below.

SELECT city, country, "v2ProductName", ROUND(SUM("totalTransactionRevenue"/"productPrice"),0)  AS product_quantity  
FROM all_sessions_v2	
GROUP BY city, country, "v2ProductName"
ORDER BY product_quantity DESC
LIMIT 5;
	
city				            country			    v2ProductName 			        product_quantity
"Sunnyvale"			            "United States"		"SPF-15 Slim & Slender Lip Balm"	464
"not available in demo dataset"	"United States"		"Reusable Shopping Bag"		        290
"not available in demo dataset"	"United States"		"Crunch Noise Dog Toy"			    187
"not available in demo dataset"	"United States"		"Leatherette Journal"			    77
"Atlanta"			            "United States"		"Reusable Shopping Bag"		        66






**Question 5: Can we summarize the impact of revenue generated from each city/country?**

The following query calculates the total transaction revenue from each city and country.

SELECT city, country, SUM ("totalTransactionRevenue") AS transaction_revenues
FROM all_sessions_v2
GROUP BY city, country
ORDER BY transaction_revenues DESC
LIMIT 5;

City				            country			transaction_revenues
"not available in demo dataset"	"United States"		6092.56
"San Francisco"			        "United States"		1564.32
"Sunnyvale"			            "United States"		992.23
"Atlanta"			            "United States"		854.44
"Palo Alto"			            "United States"		608.00








