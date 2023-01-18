# 📈 CASE STUDY 2 - (Investigating metric spike)
## 😉 Queries
## 1. User Engagement: To measure the activeness of a user. Measuring if the user finds quality in a product/service. task: Calculate the weekly user engagement?
```
SELECT EXTRACT(week FROM occurred_at) as weeknum,
COUNT(DISTINCT user_id)
FROM tutorial.yammer_events
GROUP BY 1
```
## 2. User Growth: Amount of users growing over time for a product. task: Calculate the user growth for product?
```
SELECT year, monthnum, num_active_users,
    SUM(num_active _users)OVER(ORDER BY monthnum ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS user_growth 
    FROM SELECT EXTRACT(year FROM activated_at) AS year,
                EXTRACT(MONTH FROM activated_at) AS monthnum,
                COUNT(DISTIN CT user_id) AS num_active_users
         FROM tutoria l.yammer_users
         WHERE state = 'active' GROUP BY year,monthnumGROUP BY year,monthnum 
         ORDER BY yearORDER BY year )a )a 
         WHERE year = 2013WHERE year = 2013
```

## 3. Weekly Retention: Users getting retained weekly after signing-up for a product. task: Calculate the weekly retention of users-sign up cohort?
```
SELECT COUNT(user_id) AS users,
       SUM(CASE WHEN retention_week = 1 THEN 1 ELSE 0 END ) AS week_1,
       SUM(CASE WHEN retention_week = 2 THEN 1 ELSE 0 END ) AS week_2,
       SUM(CASE WHEN retention_week = 3 THEN 1 ELSE 0 END ) AS week_3,
       SUM(CASE WHEN retention_week = 4 THEN 1 ELSE 0 END ) AS week_4,
       FROM ( SELECT a.us er_id a.signup_week , b.engagement_week,
       b.engagement_week a.signup_week AS retention_week FROM 
(SELECT DISTINCT user_id, EXTRACT(WEEK FROM occurred_at) AS signup_week
FROM tutorial.yammer_events
       WHERE event_type = 'signup_flow' AND event_name = 'complete_signup'
       AND EXTRACT(WEEK FROM occurred_at) = 18 )a
LEFT JOIN (SELECT DISTINCT user_id,DISTINCT user_id,
           EXTRACT(WEEK FROM occurred_at) AS engagement_weekEXTRACT(WEEK FROM occurred_at) AS engagement_week
       FROM tutorial.yammer_events
       WHERE event_type = 'engagement'
       )b  ON a.user_id = b.user_id )
       ORDER BY a.user_id )a
```

## 4. Weekly Engagement: To measure the activeness of a user. Measuring if the user finds quality in a product/service weekly. task: Calculate the weekly engagement per device?
```
SELECT EXTRACT(YEAR FROM occurred_at) AS year,
       EXTRACT(WEEK FROM occurred_at) AS week,
       device, COUNT(DISTINCT user_id) AS num_users
FROM tutorial.yammer_events
WHERE event_type = 'engagement'
GROUP BY year,week,device
ORDER BY week,num_users DESC
```

## 5. Email Engagement: Users engaging with the email service. task: Calculate the email engagement metrics?
```
SELECT 100.0*SUM(CASE WHEN email_category = 'email_open' THEN 1 ELSE 0 END)/SUM(CASE WHEN email_category = 'email_sent' THEN 1 ELSE 0 END) AS email_open_rate,
100.0*SUM(CASE WHEN email_category = 'email_clicked' THEN 1 ELSE 0 END)/SUM(CASE WHEN em ail_category = 'email_open' THEN 1 ELSE 0 END) AS email_click_rate
FROM SELECT 
     CASE
         WHEN action IN ('sent_weekly_digest','sent_reengagement_email')THEN 'email_sent'
         WHEN action IN ('email_open') THEN 'email_open'
         WHEN action IN ('email_clickthrough') THEN 'email_clicked'
         END AS email_category
         FROM tutorial.yammer_emails
)a
```

