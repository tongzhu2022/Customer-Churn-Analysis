# Maven-Churn-Analysis

In this project I performed the churn analysis using SQL (mySQL), you can find the dataset from [Telecom Customer Churn](https://www.mavenanalytics.io/data-playground?search=churn) from Maven

- I started with checking duplicated `customer_id` from the dataset:<br>
  <img width="374" alt="image" src="https://user-images.githubusercontent.com/127678136/230798400-36790af9-53b0-4ae5-8a99-d52b9582c107.png"><br>
  It turns out that there is no duplicated `customer_id` in this dataset<br><br>
  
- How much revenue did Maven lost due to customer churn?<br>
  <img width="939" alt="image" src="https://user-images.githubusercontent.com/127678136/230798622-bd143ad2-4658-4e22-8af1-8ad24b233e8c.png"><br>
  <img width="504" alt="image" src="https://user-images.githubusercontent.com/127678136/230799125-4944d58b-18c0-4e9f-b609-f37351c00d46.png"><br>
  Maven has experienced a loss of 1869 customers, resulting in a loss of over 3 million dollars (17.2% of its total revenue)<br><br>


- What is the typical tenure for churned customers?<br>
  <img width="897" alt="image" src="https://user-images.githubusercontent.com/127678136/230799319-45f6685c-19d8-4ea3-8d14-315a0e6fd252.png"><br>
  <img width="240" alt="image" src="https://user-images.githubusercontent.com/127678136/230799339-f6346860-43c2-4c8b-b2c9-e1840fdc0f55.png"><br>
  The majority of the customers who churned from Maven (31.9%), had only been with the company for a duration of three months or less. Additionally, the second largest group of customers who churned (28.8%), were loyal customers who had been with Maven for over two years.<br><br>

- Which cities had the highest churn rate?<br>
  <img width="740" alt="image" src="https://user-images.githubusercontent.com/127678136/230807704-55b1b8b7-7cb7-4718-b606-7dbd2cfed2c1.png"><br>
  <img width="277" alt="image" src="https://user-images.githubusercontent.com/127678136/230807818-be341f2f-c39e-4ac1-bebd-e8b899f588ac.png"><br>
  San Diego had the highest churn rate (64.9%), indicating that more than half of the customers have left Maven<br><br>

  





