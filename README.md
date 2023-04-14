# Maven Churn Analysis

In this project I performed the churn analysis using SQL (mySQL), you can find the dataset from [Telecom Customer Churn](https://www.mavenanalytics.io/data-playground?search=churn)<br><br>

- I started with checking duplicated `customer_id` from the dataset
```sql
SELECT customer_id, COUNT(*) as count
FROM telecom.telecom_customer_churn
GROUP BY customer_id
HAVING COUNT(customer_id) > 1;
```
It turns out that there is no duplicated `customer_id` in this dataset<br><br>
  
- How much revenue did Maven lost due to churned customers?
```sql
SELECT customer_status,
       COUNT(customer_id) AS customer_count,
       ROUND(SUM(total_revenue), 1) AS total_revenue,
       ROUND((SUM(total_revenue)*100) / SUM(SUM(total_revenue)) OVER(), 1) AS revenue_percentage
FROM telecom.telecom_customer_churn
GROUP BY customer_status;
```
<img width="515" alt="image" src="https://user-images.githubusercontent.com/127678136/232131736-cb6f2f1f-b75d-49e9-817a-ca4c15434333.png"><br>
Maven has experienced a loss of 1869 customers, resulting in a loss of over 3 million dollars (17.2% of its total revenue)<br><br>

- What is the typical tenure for churned customers?
```sql
SELECT CASE WHEN tenure_in_months <= 3 THEN '3 months'
	    WHEN tenure_in_months <= 6 THEN '6 months'
	    WHEN tenure_in_months <= 12 THEN '1 year'
            WHEN tenure_in_months <= 24 THEN '2 years'
            ELSE '2 years +' END AS tenure_length,
       ROUND(COUNT(customer_id)*100 / SUM(COUNT(customer_id)) OVER(), 1) AS churn_percentage
FROM telecom.telecom_customer_churn
WHERE customer_status = 'Churned'
GROUP BY tenure_length
ORDER BY churn_percentage DESC;
```
<img width="240" alt="image" src="https://user-images.githubusercontent.com/127678136/230799339-f6346860-43c2-4c8b-b2c9-e1840fdc0f55.png"><br>
The majority of the customers who churned from Maven (31.9%), had only been with the company for a duration of three months or less. Additionally, the second largest group of customers who churned (28.8%), were loyal customers who had been with Maven for over two years.<br><br>

- Which cities had the highest churn rate?
```sql
SELECT city,
       COUNT(CASE WHEN customer_status = 'Churned' THEN customer_id
	     ELSE NULL END) AS churned,
       ROUND(COUNT(CASE WHEN customer_status = 'Churned' THEN customer_id
	     ELSE NULL END)*100 / COUNT(customer_id), 1) AS churn_rate
FROM telecom.telecom_customer_churn
GROUP BY city
HAVING churned > 10
ORDER BY churn_rate DESC
LIMIT 5;
```
<img width="277" alt="image" src="https://user-images.githubusercontent.com/127678136/230807818-be341f2f-c39e-4ac1-bebd-e8b899f588ac.png"><br>
San Diego had the highest churn rate (64.9%), indicating that more than half of the customers have left Maven<br><br>
  
- Breakdown of customers who have churned
```sql
SELECT churn_category,
       ROUND(SUM(total_revenue),1) AS churn_revenue,
       ROUND(COUNT(customer_id)*100 / SUM(COUNT(customer_id)) OVER(), 1)AS churn_percentage
FROM telecom.telecom_customer_churn
WHERE customer_status = 'Churned'
GROUP BY churn_category
ORDER BY churn_revenue DESC;
```
<img width="374" alt="image" src="https://user-images.githubusercontent.com/127678136/230809120-686bbad4-9a44-492e-8139-a61d8cb6d9d6.png"><br>
About 45% of the customers listed 'Competitor' as their reason for leaving Maven<br><br>
  
- Detailed reason for churn
```sql
SELECT churn_category,
       ROUND(SUM(total_revenue),1) AS churn_revenue,
       ROUND(COUNT(customer_id)*100 / SUM(COUNT(customer_id)) OVER(), 1)AS churn_percentage
FROM telecom.telecom_customer_churn
WHERE customer_status = 'Churned'
GROUP BY churn_category
ORDER BY churn_revenue DESC;
```
<img width="484" alt="image" src="https://user-images.githubusercontent.com/127678136/230817025-5d4a5686-8736-4d46-a60d-f5679414589e.png"><br>
Both 'Competitor made better offer' and 'Competitor had better device' were identified as top reasons for customer churn<br><br>
  
- What offers were made to customers who have churned
```sql
SELECT offer,
       ROUND(COUNT(customer_id)*100 / SUM(COUNT(customer_id)) OVER(), 1) AS churn_percentage
FROM telecom.telecom_customer_churn
WHERE customer_status = 'Churned'
GROUP BY offer
ORDER BY churn_percentage DESC;
```
<img width="196" alt="image" src="https://user-images.githubusercontent.com/127678136/230814506-a4cab9a9-7830-49d9-ae0e-d6bf8aef85f1.png"><br>
Most of the churned customers (56.2%) did not receive any offer<br><br>
  
- What internet did the churned customers have?
```sql
SELECT internet_type,
       COUNT(customer_id) AS churned,
       ROUND(COUNT(customer_id)*100 / SUM(COUNT(customer_id)) OVER(), 1) AS churn_percentage
FROM telecom.telecom_customer_churn
WHERE customer_status = 'Churned'
GROUP BY internet_type
ORDER BY churn_percentage DESC;
```
<img width="313" alt="image" src="https://user-images.githubusercontent.com/127678136/230815132-bbd59ed4-0f0c-489b-8e69-df4bd3656a94.png"><br>
66.1% of the churned customers were using Fiber Optic internet<br><br>
  
- What internet type did 'Competitor' churners have?
```sql
SELECT internet_type,
       churn_category,
       COUNT(customer_id) AS churned,
       ROUND(COUNT(customer_id)*100 / SUM(COUNT(customer_id)) OVER(), 1) AS churn_percentage
FROM telecom.telecom_customer_churn
WHERE customer_status = 'Churned' AND churn_category = 'Competitor'
GROUP BY internet_type
ORDER BY churn_percentage DESC;
```
<img width="419" alt="image" src="https://user-images.githubusercontent.com/127678136/230815892-93f6f0c6-8bfc-412e-b8a8-e587033c8332.png"><br>
69.8% of the customers left for competitor's Fiber Optic, indicating that there might be a quality issue with Maven's Fiber Optic internet<br><br>
  
- What type of contract were churned customers on?
```sql
SELECT contract,
       COUNT(customer_id) AS churned,
       ROUND(COUNT(customer_id)*100 / SUM(COUNT(customer_id)) OVER(), 1) AS churn_percentage
FROM telecom.telecom_customer_churn
WHERE customer_status = 'Churned'
GROUP BY contract
ORDER BY churn_percentage DESC;
```
<img width="325" alt="image" src="https://user-images.githubusercontent.com/127678136/230816100-a812f89a-53ee-46f7-88bc-e28481c532bf.png"><br>
Customers on a short contract(Month-to-Month) are more likely to churn<br><br>
  
- Did churned customers have premium tech support?
```sql
SELECT premium_tech_support,
	   COUNT(customer_id) AS churned,
       ROUND(COUNT(customer_id)*100 / SUM(COUNT(customer_id)) OVER(), 1) AS churn_percentage
FROM telecom.telecom_customer_churn
WHERE customer_status = 'Churned'
GROUP BY premium_tech_support
ORDER BY churn_percentage DESC;
```
<img width="378" alt="image" src="https://user-images.githubusercontent.com/127678136/230816517-f0fffe6d-11fe-4dd1-b0e4-a03083eb21de.png"><br>
77.4% churned customers does not have access to premium tech support. Improving this service could potentially make a significant difference in reducing customer churn<br><br>









  

  


  


  

  





