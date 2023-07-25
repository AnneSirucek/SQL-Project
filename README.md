# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
The goal of this project is to try to understand what this data set represents and how it relates to itself. The second goal is to work through the project objectives and answer the questions.

## Process
The first step in this process was to look at the data in the tables and compare them to see how they relate to each other.  The product SKU connects all of the tables except for the analytics table.  The all sessions table and the analytics table can be connected using the full visitor id and the visit id. 

## Results
The data in the all sessions table was used to answer the project questions about sales, popular products and categories.  I calculated a product quantity using the total transaction revenue and the product price.  When I checked these calculated values with the product quantity values that existed in the table, only about half were a close match.  
The difference between the product revenue values that exist in the table and the total transaction revenues were also compared.  The product revenues were a close match when multiplying the product price  with the product quantity that exist in the table.  There is a significant difference between the product revenues and the total transaction revenues.   I suspect that there is something else included in the total transaction revenues other than shipping and tax costs.   I found one duplicate total transaction revenue but this did not show a relationship of what else could be included.  
The sales report table looks like it describes how people felt about the products they ordered.  The sentiment scores and the magnitude scores from the sales report match in the products table.  I could not determine what the ratio value in the sales report table was supposed to represent. The total ordered values are equal to each other in the sales by sku and the sales report tables but the ordered quantity in the products table  is a different value so it must represent something else.  I compared the total ordered values with the quantities ordered from the all sessions table as well but did not see a relationship. The products table appears to be the most complete list of the products available.
It was difficult to determine how the information in the analytics table related to the all sessions table. They have some matching values but it is still not clear how they are related, maybe the all sessions table is some how a smaller data subset of the analytics table. Another interesting point, is the visit id in the analytics table for the most part matches the visit start time of that table.


## Challenges 
The main challenge of this project is that we do not know the history of this data.  It is difficult to get a complete picture of what it represents without knowing the intention of what it was supposed to portray.  The data set contains a lot of incomplete and null values and there was little insight or direction on how to populate or correct the missing information.  An explanation of how and why this date set was collected  and the meaning of the values would be really helpful.  I have made my best guess to understand it and make some connections.

## Future Goals
The goal in the future would be to have more information about this data and a more complete dataset.  The data owner could provide a history of the data collection.  It would be easier to understand if we knew how and why it was collected in it’s current format.  It would also be helpful to know what the company goals are and what they are hoping to achieve with the data.  A more complete picture could help assure the correct information is recorded as new data is collected. 
