# Shopify-Data-Science-Intern-Challenge

Please complete the following questions, and provide your thought process/work. You can attach your work in a text file, link, etc. on the application page. Please ensure answers are easily visible for reviewers!

Question 1: Given some sample data, write a program to answer the following: click here to access the required data set

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 


a.	Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 

Here, the arithmetic mean was used to determine the average order value. In general, the purpose of the arithmetic mean is to get a measure of centre of the data. One advantage of using the arithmetic mean is that its calculation involves each data point. However due to this, the arithmetic mean is very sensitive to outliers, meaning that if there is a data point with a very different value than the others, the mean of the data can change drastically. Upon sorting the data by order amount, we can see that there were 17 of the 5000 orders of quantity 2000 from the exact same person and at the exact same shop. This indicates that these points are outliers, which explains why the average order value is drastically higher than expected. 

.
b.	What metric would you report for this dataset?

We can still use the arithmetic mean, instead this time we would add a condition that the quantity of the order is within a certain threshold. Not including the 17 orders mentioned above, the maximum quantity per order is 8, so setting a threshold of 25 should be sufficient. If we set the threshold at 8, then if there was future data that had a quantity of 10, then that data point wouldn’t be included in the average. This is a problem because 10 is not necessarily an outlier, it is reasonably close to 8. Therefore, we want to set the threshold at a figure that any number higher could reasonably be considered as an outlier.


c.	What is its value?

The value of the arithmetic mean, not including any orders that had a total_items count of higher than 25, is $754.09.
Question 2: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.


a.	How many orders were shipped by Speedy Express in total?

SELECT * FROM [Orders]
WHERE ShipperID = 
	(SELECT ShipperID FROM [Shippers]
    	WHERE ShipperName = "Speedy Express")

Answer: 54 orders in total shipped by Speedy Express.
				
b.	What is the last name of the employee with the most orders?

SELECT LastName FROM [Employees] 
WHERE EmployeeID = 
(SELECT EmployeeID FROM
(SELECT EmployeeID,MAX(OrdersbyEmployee) FROM
(SELECT Employees.EmployeeID, COUNT(OrderID) AS "OrdersbyEmployee" FROM [Orders],[Employees]
WHERE Employees.EmployeeID = Orders.EmployeeID
GROUP BY Employees.EmployeeID)))
Answer: Peacock is the last name of the employee with the most orders

c.	What product was ordered the most by customers in Germany?

SELECT ProductID, MAX(CountofOrders) FROM 
(SELECT ProductID, COUNT(ProductID) AS "CountofOrders" FROM [OrderDetails] 
WHERE OrderID IN
(SELECT OrderID FROM 
(SELECT Orders.OrderID, Customers.CustomerID 
FROM [Orders],[Customers]
WHERE Customers.CustomerID = Orders.CustomerID AND Customers.Country = "Germany"))
GROUP BY ProductID)

Answer: The product with ProductID 31 was ordered most by customers in Germany
