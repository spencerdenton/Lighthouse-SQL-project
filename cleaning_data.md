What issues will you address by cleaning the data?
Duplicate entries 
Missing entries 
Update data type formats (dates, etc).
Delete columns with all NULL values 
Divide product price by 1,000,000 as way too many zeroes. 

ANALYTICS 
Revenue not showing from colums where units have been sold...
Unit price lsited as 0 for rows 243-246
Unit price divide by 1,000,000
DELETE Time on site as it is blank
visit_number seems to be cloned. Many similar columns

PRODUCTS TABLE
Sku format not matching on some and indented weird as in the opposite 
Sentiment score and sentiment gravity are NULL for row 2
Extra spaces in name column 

SALES REPORT
Ratio (set number odd digits to show so decimals are the same?)
Extra spaces in name column 
Sentiment Score and ratio have same number of decimal places. 3 to 3.0 to match. 0.7

SESSIONS
Time on site has some NULL values 12,13,14,18
Currency has some blanks - fill in with USD
Time is 0 on several rows

Queries:
Below, provide the SQL queries you used to clean your data.


ALTER TABLE analytics
ALTER COLUMN date TYPE DATE USING date::DATE

ALTER TABLE sessions 
ALTER COLUMN session_date TYPE VARCHAR USING session_date::VARCHAR
ALTER COLUMN session_date TYPE DATE USING session_date::DATE

UPDATE sessions
SET product_price = product_price/1000000

SELECT user_id FROM analytics WHERE user_id IS NOT null
ALTER TABLE analytics
DROP COLUMN user_id

SELECT social_engagement_type FROM analytics WHERE social_engagement_type != 'Not Socially Engaged'

UPDATE analytics
SET unit_price = unit_price/1000000

SELECT search_keyword FROM sessions WHERE search_keyword IS NOT null
ALTER TABLE sessions
DROP COLUMN search_keyword

SELECT item_quantity FROM sessions WHERE item_quantity IS NOT null
ALTER TABLE sessions
DROP COLUMN item_quantity 

SELECT product_refund_amount FROM sessions WHERE product_refund_amount IS NOT null
ALTER TABLE sessions
DROP COLUMN product_refund_amount

SELECT item_revenue FROM sessions WHERE item_revenue IS NOT null
ALTER TABLE sessions
DROP COLUMN item_revenue

UPDATE sessions
SET total_transaction_revenue = total_transaction_revenue/1000000

UPDATE sessions
SET city = NULL
WHERE city = 'not available in demo dataset' 

UPDATE sessions
SET city = NULL
WHERE city = '(not set)' 

SELECT visit_id, COUNT(visit_id) FROM sessions
GROUP BY visit_id
HAVING COUNT (visit_id)> 1

ALTER TABLE sessions
ADD COLUMN session_id SERIAL PRIMARY KEY

DELETE FROM
    sessions a
        USING sessions b
WHERE
    a.session_id < b.session_id
    AND a.visit_id = b.visit_id; 

UPDATE sales_report
SET name = TRIM(name)

ALTER TABLE analytics
ADD COLUMN analytics_pkey SERIAL PRIMARY KEY 

ALTER TABLE products
ADD PRIMARY KEY (product_sku) 
