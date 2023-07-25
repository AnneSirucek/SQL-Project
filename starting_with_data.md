1.	Following is a query that will find the unique visitors from referring sites that made a purchase.  This assumes that the channelGrouping column with the value ‘Referral’ means that it was a referral from another website.
   
SELECT DISTINCT ("fullVisitorId")
FROM all_sessions
WHERE "totalTransactionRevenue" IS NOT NULL
AND "channelGrouping" = 'Referral'

fullVisitorId
4614920625832713258
5635635846003176783
5795630000736964663
8379929056656388083
4934955623514361329
etc.

 2.   Following is a query that will find the total number of unique visitors from referring sites that made a purchase.
    
SELECT DISTINCT COUNT("fullVisitorId")
FROM all_sessions
WHERE "totalTransactionRevenue" IS NOT NULL
AND "channelGrouping" = 'Referral'

Count – 32

3.	Following is a query that will show if the full visitor Ids that made a purchase also visited the site at other times. This would make the assumption that the full visitor Id would be the same if a visitor was on the site multiple times.

SELECT "fullVisitorId", "totalTransactionRevenue", COUNT ("fullVisitorId") AS num_visit
FROM all_sessions
GROUP BY "fullVisitorId", "totalTransactionRevenue"
HAVING COUNT ("fullVisitorId") > 1
AND "totalTransactionRevenue" IS NOT NULL

fullVisitorId			totalTransactionRevenue	num_visit
3764227345226401562		124.00				2


4.	This query did not show any complete  duplicates in the all sessions table.

SELECT *, COUNT (*)
FROM all_sessions
GROUP BY "fullVisitorId", "channelGrouping", time, country, city, "totalTransactionRevenue", transactions, "timeOnSite", pageviews, "sessionQualityDim", date, "visitId", "type", "productQuantity","productPrice","productRevenue","productSKU","v2ProductName","v2ProductCategory","productVariant","currencyCode","itemQuantity","itemRevenue","transactionRevenue","transactionId","pageTitle","searchKeyword","pagePathLevel1","eCommerceAction_type","eCommerceAction_step","eCommerceAction_option"
HAVING COUNT (*) > 1

