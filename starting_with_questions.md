Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT country, SUM(total_transaction_revenue::numeric(10,2)) as revenue
FROM sessions
WHERE transactions >0
GROUP BY country
ORDER BY revenue DESC

SELECT city, SUM(total_transaction_revenue::numeric(10,2)) as revenue
FROM sessions
WHERE transactions >0 AND city IS NOT NULL
GROUP BY city
ORDER BY revenue DESC

Answer:

"United States"	13030.17
"Israel"	602.00
"Australia"	358.00
"Canada"	150.15
"Switzerland"	16.99


"San Francisco"	1564.32
"Sunnyvale"	992.23
"Atlanta"	854.44
"Palo Alto"	608.00
"Tel Aviv-Yafo"	602.00
"New York"	598.35
"Mountain View"	483.36
"Los Angeles"	479.48
"Chicago"	449.52
"Sydney"	358.00
"Seattle"	358.00
"San Jose"	262.38
"Austin"	157.78
"Nashville"	157.00
"San Bruno"	103.77
"Toronto"	82.16
"Houston"	38.98
"Columbus"	21.99
"Zurich"	16.99


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

SELECT  country
        ,ROUND(AVG(product_quantity),2) AS average_product_ordered
FROM sessions
WHERE country IS NOT NULL AND product_quantity > 0
GROUP BY country
ORDER BY average_product_ordered DESC

SELECT  country
        ,city
        ,ROUND(AVG(product_quantity),2) AS average_product_ordered
FROM sessions
WHERE city IS NOT NULL AND product_quantity > 0
GROUP BY country
         ,city
ORDER BY average_product_ordered DESC

"Spain"	10.00
"United States"	4.10
"Colombia"	1.00
"Finland"	1.00
"France"	1.00
"Argentina"	1.00
"Ireland"	1.00
"Mexico"	1.00
"India"	1.00
"Canada"	1.00

Answer:

"Spain"	"Madrid"	10.00
"United States"	"Salem"	8.00
"United States"	"Atlanta"	4.00
"United States"	"Houston"	2.00
"United States"	"New York"	1.17
"United States"	"Dallas"	1.00
"United States"	"Detroit"	1.00
"United States"	"Los Angeles"	1.00
"United States"	"Mountain View"	1.00
"United States"	"Palo Alto"	1.00
"United States"	"San Francisco"	1.00
"United States"	"San Jose"	1.00
"United States"	"Seattle"	1.00
"India"	"Bengaluru"	1.00
"United States"	"Sunnyvale"	1.00
"Ireland"	"Dublin"	1.00
"United States"	"Ann Arbor"	1.00
"United States"	"Chicago"	1.00
"United States"	"Columbus"	1.00




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

SELECT v2_product_category, country, count (v2_product_category) as number_purchases
FROM sessions
WHERE transactions > 0
GROUP BY country, v2_product_category
ORDER BY number_purchases DESC

SELECT v2_product_category, city, count (v2_product_category) as number_purchases
FROM sessions
WHERE transactions >0 AND city IS NOT NULL
GROUP BY city, v2_product_category
ORDER BY number_purchases DESC


Answer:

NEST products are the most popular in the USA followed by apparel. No other countries has more than one transaction so no pattern can be determined.

NEST products are also the most popular in San Franscisco and Palo Alto with Apparel in Mountainview based on multiple transactions. Given these are only values of 3 and 2 though not much can be determined from such a small data set. 

**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

SELECT v2_product_name, country, count (v2_product_name) as number_purchases
FROM sessions
WHERE transactions > 0
GROUP BY v2_product_name, country
ORDER BY number_purchases DESC

Answer:

United States: Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel - 7
Canada: Google Men's  Zip Hoodie -1 and Google Men's 3/4 Sleeve Raglan Henley Grey - 1
Israel: Nest® Protect Smoke + CO Black Wired Alarm-USA - 1
Switzerland: YouTube Men's 3/4 Sleeve Henley - 1
Australia: Nest® Cam Indoor Security Camera - USA - 1

SELECT v2_product_name, city, count (v2_product_name) as number_purchases
FROM sessions
WHERE transactions > 0
GROUP BY v2_product_name, city
ORDER BY number_purchases DESC, city

Not enough data US is only place where the number one product sold more than one unit. NULL is the only value in city that has a value greater than one.

**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

WITH T AS 
(SELECT country, SUM(total_transaction_revenue::numeric(10,2)) as revenue
FROM sessions
WHERE transactions >0
GROUP BY country
ORDER BY revenue DESC
) 
SELECT t.revenue / SUM(t.revenue) OVER() *100 AS percent_rev FROM T

Answer:

"United States"	92.03845928357858943500
"Israel"	4.25222023110322511800
"Australia"	2.52872897464278171500
"Canada"	1.06058283671121138100
"Switzerland"	0.12000867396419235000





