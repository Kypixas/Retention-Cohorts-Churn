# Weekly Subscription Retention Cohorts Analysis

## Overview
This project focuses on analyzing **subscription churn and retention** from a weekly standpoint. By performing **cohort analysis**, we track how many subscribers who started their subscription in a particular week remain active for the next six weeks. The analysis helps to identify trends in subscription retention more quickly than monthly-based reports, offering timely insights into subscriber behavior.

### Objective:
- Track how many subscribers who started their subscription in a given week (cohort) remain active in the following 6 weeks.
- Provide weekly retention data for each subscription cohort starting from **2021-02-07** (Week 0).
- Analyze the data using cohort analysis to evaluate retention and churn trends.

## Tools Used
- **SQL**: To query and extract data from the BigQuery dataset (`turing_data_analytics.subscriptions`).
- **Google Spreadsheets**: For visualizing the cohort analysis results and creating a retention cohort table.

## Data Query
The data is extracted from the `turing_data_analytics.subscriptions` table using the following SQL query to capture weekly retention cohorts:

```sql
WITH
  date_range AS (
    SELECT
      DATE_TRUNC(subscription_start, WEEK(Sunday)) AS cohort_week,
      user_pseudo_id,
      subscription_start,
      subscription_end
    FROM
      `tc-da-1.turing_data_analytics.subscriptions`
  )
    SELECT
      cohort_week,
      COUNT(user_pseudo_id) AS WEEK0,
      SUM(CASE 
            WHEN subscription_end IS NULL OR subscription_end > DATE_ADD(cohort_week, INTERVAL 1 WEEK) 
            THEN 1 
            ELSE 0 
          END) AS WEEK1,
      SUM(CASE 
            WHEN subscription_end IS NULL OR subscription_end > DATE_ADD(cohort_week, INTERVAL 2 WEEK)
            THEN 1 
            ELSE 0 
          END) AS WEEK2,
          SUM(CASE 
            WHEN subscription_end IS NULL OR subscription_end > DATE_ADD(cohort_week, INTERVAL 3 WEEK)
            THEN 1 
            ELSE 0 
          END) AS WEEK3,
          SUM(CASE 
            WHEN subscription_end IS NULL OR subscription_end > DATE_ADD(cohort_week, INTERVAL 4 WEEK)
            THEN 1 
            ELSE 0  
          END) AS WEEK4,
          SUM(CASE 
            WHEN subscription_end IS NULL OR subscription_end > DATE_ADD(cohort_week, INTERVAL 5 WEEK)
            THEN 1 
            ELSE 0 
          END) AS WEEK5,
          SUM(CASE 
            WHEN subscription_end IS NULL OR subscription_end > DATE_ADD(cohort_week, INTERVAL 6 WEEK)
            THEN 1 
            ELSE 0 
          END) AS WEEK6
    FROM
      date_range
    GROUP BY
      cohort_week
    ORDER BY
      cohort_week;
  
```
You can view the SQL query and results on Google Spreadsheets using the following link:

Tool Used for Data Extraction: [Big Query](https://console.cloud.google.com/bigquery?sq=147855269776:2d90f9e43c2141bdacd6ecd73f602fa1)

Tools Used for Visualization: [ Google Spreadsheets](https://docs.google.com/spreadsheets/d/1R8ElaVXHzVxggwAm4bdVq9SKnh9smtWcBZe8eJQbIs0/edit?usp=sharing)

![image](https://github.com/user-attachments/assets/773b5a43-d0f8-4d57-9593-1fc53ffbf26b)

# Conclusions
### Initial Churn:
-    The most substantial churn occurs within the first week of subscription, highlighting the importance of onboarding and engagement strategies to retain new subscribers.
### Long-term Retention:
- Cohorts that started later in the year (e.g., December 2020) show improved retention, suggesting that changes in product offerings or marketing strategies may have positively impacted subscriber loyalty.

### Actionable Insights:
-  To reduce churn, focusing on improving the customer experience during the initial subscription phase is crucial. This could involve enhanced onboarding processes, personalized communication, and tailored engagement strategies.
