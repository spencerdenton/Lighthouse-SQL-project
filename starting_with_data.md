Question 1: What percentage of visitors make a purchase?

SQL Queries:

WITH T AS 
(SELECT COUNT(DISTINCT full_visitor_id) AS total_visitors,
COUNT(transactions) AS total_purchases
FROM sessions
)
SELECT total_purchases::FLOAT / total_visitors::FLOAT * 100 AS percent_purchased FROM T

Answer: 

0.56% of visitors make a purchase. 

Question 2: What was the most common channels visitors came from. 

SQL Queries: 
SELECT channel_group, COUNT(channel_group) AS channel_num
FROM sessions
GROUP BY channel_group
ORDER BY channel_num DESC

Answer:

"Organic Search"	8347
"Direct"	2861
"Referral"	2488
"Paid Search"	488
"Affiliates"	247
"Display"	120
"(Other)"	5

Question 3: Is there any pattern between the amount of time on site and revenue per visit for people who made orders?

SQL Queries:

SELECT full_visitor_id, 
time_on_site, 
total_transaction_revenue, 
total_transaction_revenue / time_on_site AS rev_ratio
FROM sessions
WHERE time_on_site IS NOT NULL AND total_transaction_revenue IS NOT NULL
GROUP BY full_visitor_id, time_on_site, total_transaction_revenue
ORDER BY total_transaction_revenue DESC
ORDER BY rev_ratio DESC

Answer:

It is hard to determine an exact pattern as data set is small. Total revenue seems to be maximized by visits between 4 and 15 min. Revenue per second is generated when customers spend between one and just over five minutes on the site. 

Question 4: What are the top ordered products by category?

SQL Queries:

  SELECT 
        v2_product_category,
        SUM(sbs.total_ordered) as ordered_amt
    FROM sessions s
    INNER JOIN sales_by_sku sbs
        ON s.product_sku = sbs.product_sku
        WHERE v2_product_category != '(not set)'
    GROUP BY v2_product_category
    ORDER BY ordered_amt DESC
    limit 10

Answer:

    "Home/Shop by Brand/YouTube/"                    31352
    "Home/Nest/Nest-USA/"                            24110
    "Home/Drinkware/"                                21901
    "Home/Office/"                                    21027
    "Home/Shop by Brand/Google/"                    10879
    "Home/Bags/"                                    8629
    "Home/Office/Notebooks & Journals/"                8504
    "Home/Electronics/"                                6595
    "Home/Drinkware/Water Bottles and Tumblers/"    5556
    "Home/Apparel/Men's/Men's-T-Shirts/"            5337


Question 5: Who spent the most time on each page?

SQL Queries:

    SELECT     full_visitor_id,
        page_views,
        time_on_site,
        (time_on_site / page_views)::numeric (5,2) as avg_time_per_page
    FROM sessions
        WHERE page_views >0 AND time_on_site is not null
    ORDER BY avg_time_per_page DESC
    limit 10

Answer:

"full_visitor_id"        "page_views"    "time_on_site"    "avg_time_per_page"
"4344524303428134441"        2                1718                859.00
"3897651648778359490"        2                1710                855.00
"1869599090210681270"        2                1657                828.50
"0349198466881148447"        2                1650                825.00
"7011681108778678989"        2                1646                823.00
"0927682010281318434"        3                2467                822.33
"1309382816398247582"        2                1621                810.50
"8401779914861806064"        2                1607                803.50
"3211307282081510146"        2                1601                800.50
"6684417199282789854"        2                1596                798.00