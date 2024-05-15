select * from subscriptions;
select * from plans;

/**1. How many customers has Foodie-Fi ever had?**/

select count(distinct customer_id) as Number_of_customer from subscriptions;

/**What is the monthly distribution of trial plan start_date values for our dataset - use the start  of the month as the group by value**/

select  date_format(start_date ,'%y-%m-01') as start_of_month, count(*) as trail_count from subscriptions where plan_id=0
group by  date_format(start_date ,'%y-%m-01') 
order by start_of_month;

/**What plan start_date values occur after the year 2020 for our dataset? Show the breakdown  by count of events for each plan_name**/
select  p.plan_id,p.plan_id,count(*) as count_events from subscriptions s
 inner join plans p on p.plan_id=s.plan_id where start_date>'2020-12-31' group by p.plan_name,p.plan_id order by p.plan_id;
/** What is the customer count and percentage of customers who have churned the rounded to 1 decimal place?**/

select * from subscriptions;
select count(*) as total_churned ,round( count(*)*100/
(select count(distinct customer_id)from subscriptions),1) as percentage from subscriptions
where plan_id=4; 
 
 /** How many customers have churned straight after their initial free trial -
 what percentage is  this rounded to the nearest whole number**/
 with cte_churn as(
 select *,
 lag(plan_id,1) over(partition by customer_id order by plan_id)as previous_plan from subscriptions
 )
 select count(previous_plan) as count_churn,
 round(count(*)*100/(select count(distinct customer_id)from subscriptions),0) as Percentage_churn from cte_churn
 where plan_id=4 and previous_plan=0;
 
 
 /** What is the number and percentage of customer plans after their initial free trial?**/
 with cte_lead_plan  as(
 select *,
 lead(plan_id,1) over(partition by customer_id order by plan_id)as lead_plan from subscriptions
 )
 select lead_plan,count(lead_plan) as count_lead, 
 round(count(*)*100/(select count(distinct customer_id)from subscriptions),1) 
 as Percentage_lead from cte_lead_plan
 where lead_plan is not null and plan_id=0 group by lead_plan order by lead_plan;
  /** 7. What is the customer count and percentage breakdown of all 5 plan_name values at 2020- 12-31**/
 SELECT 
   p.plan_name,
    COUNT(DISTINCT s.customer_id) AS customer_count,
    (COUNT(DISTINCT s.customer_id)* 100 / NULLIF((SELECT COUNT(DISTINCT customer_id)
    FROM subscriptions WHERE start_date <= '2020-12-31'), 0))  AS percentage
FROM 
    subscriptions s
JOIN 
    plans p ON s.plan_id = p.plan_id
WHERE 
    s.start_date <= '2020-12-31'
GROUP BY 
    p.plan_name;
/**8. How many customers have upgraded to an annual plan in 2020?**/
select * from plans;
 select* from subscriptions;
 select plan_id,count(distinct customer_id) NO_of_customer_upgraded from subscriptions where 
  start_date >= '2020-01-01' AND start_date < '2021-01-01' And plan_id=3;
 
/** How many days on average does it take for a 
customer to an annual plan from the day they joined Foodie-Fi?**/
select * from subscriptions;
with Annualplans as(
select customer_id , min(start_date) as annual_start_date from subscriptions where plan_id=3 group by customer_id
),
trialPlans as(
select customer_id , min(start_date) as trial_start_date from subscriptions where plan_id=0 group by customer_id
)
select AVG(DATEDIFF(Annualplans.annual_start_date, trialPlans.trial_start_date)) 
AS average_days_to_annual_plan
FROM AnnualPlans
 join trialPlans on AnnualPlans.customer_id=trialPlans.customer_id;
 
/** 10. Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60  days etc)**/
 
 WITH AnnualPlans AS (
    SELECT customer_id, MIN(start_date) AS annual_start_date
    FROM subscriptions
    WHERE plan_id = 3
    GROUP BY customer_id
),
TrialPlans AS (
    SELECT customer_id, MIN(start_date) AS trial_start_date
    FROM subscriptions
    WHERE plan_id = 0
    GROUP BY customer_id
),
DaysDifference AS (
    SELECT
        AnnualPlans.customer_id,
        DATEDIFF(AnnualPlans.annual_start_date, TrialPlans.trial_start_date) AS days_difference
    FROM
        AnnualPlans
    JOIN
        TrialPlans ON AnnualPlans.customer_id = TrialPlans.customer_id
)
SELECT
    SUM(CASE WHEN days_difference BETWEEN 0 AND 30 THEN 1 ELSE 0 END) AS "0-30 days",
    SUM(CASE WHEN days_difference BETWEEN 31 AND 60 THEN 1 ELSE 0 END) AS "31-60 days",
    SUM(CASE WHEN days_difference BETWEEN 61 AND 90 THEN 1 ELSE 0 END) AS "61-90 days",
    SUM(CASE WHEN days_difference BETWEEN 91 AND 120 THEN 1 ELSE 0 END) AS "91-120 days",
    SUM(CASE WHEN days_difference BETWEEN 121 AND 150 THEN 1 ELSE 0 END) AS "121-150 days",
    SUM(CASE WHEN days_difference > 150 THEN 1 ELSE 0 END) AS ">150 days"
FROM
    DaysDifference;

 /**How many customers downgraded from a pro monthly to a basic monthly plan in 2020?**/
  select plan_id,count(distinct customer_id) NO_of_customer_downgraded from subscriptions where 
  start_date >= '2020-01-01' AND start_date < '2021-01-01' And plan_id=3;
/** How many customers downgraded from a pro monthly to a basic monthly plan in 2020?**/
select * from plans;
 select COUNT(DISTINCT customer_id) AS num_downgrades
from subscriptions
where plan_id =  2
and start_date >= '2020-01-01'
and start_date < '2021-01-01'
and customer_id in (
    select customer_id
    from subscriptions
    where plan_id = 1
    and start_date >= '2020-01-01'
    and start_date < '2021-01-01'
);

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 












