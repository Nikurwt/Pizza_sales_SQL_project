# Pizza_sales_SQL_project

### Project Goal:
This project uses SQL as a means of dealing with pizza sales data that it receives from the source in the form of Excel files. It organizes the data into four tables: order details, orders, pizza types, and pizza details. Its aim is to give some valuable inputs on customer preferences and order trends, the given pricing of pizzas, and other sell-through data.Basically, the project would provide an opportunity for optimization in the business area while improving on the menu and other selling plans.

### Disclaimer

This project uses a "fictional dataset" based on the structure of Pizza sales. The data is completely synthetic and used solely for educational purposes. None of the information represents real Pizza shop or business operations.

### Data Overview:-

This project organizes pizza sales data across four interconnected tables:

### Order Details:

**order_details_id:** Unique identifier for each order detail.

**order_id:** References the order.

**pizza_id:** References the specific pizza ordered.

**quantity:** Number of pizzas ordered.

### Orders:

**order_id:** Unique identifier for each order.

**date:** Date of the order.

**time:** Time the order was placed.


### Pizza Types:

**pizza_type_id:** Unique identifier for each pizza type.

**name:** Name of the pizza (e.g., Margherita, Pepperoni)
.
**category:** Category of pizza (e.g., Classic, non-vegetarian).

**ingredients:** List of ingredients used in each pizza type.

### Pizza Details:

**pizza_id:** Unique identifier for each pizza.

**pizza_type_id:** References the pizza type.

**size:** Size of the pizza (e.g., small, medium, large).

**price:** Price of the pizza based on its size.

### Business problems and solutions

### 1. Retrieve the total number of orders placed.

```sql
SELECT 
    COUNT(order_id) AS total_orders
FROM
    orders;
```
**Objective-** The objective is to calculate the total number of orders placed in the system.

### 2. Calculate the total revenue generated from pizza sales.

```sql
SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price),
            2) AS Total_revenue
FROM
    order_details
        JOIN
    pizzas ON pizzas.pizza_id = order_details.pizza_id;
```
**Objective-** The aim is to compute the total revenue generated from pizza sales by multiplying the quantity sold with the price of the respective pizzas and rounded to two decimal places for the purpose of recording it in the financial books.

### 3. Identify the highest-priced pizza.

```sql
SELECT 
    pizzas.price, pizza_types.name
FROM
    pizzas
        JOIN
    pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
```
**Objective-** The aim is extracting the most costly pizza by combining pizza information and types of pizzas sorted in descending order of the cost. The query returned the name and the price of the priciest pizza.

### 4. Identify the most common pizza size ordered.

```sql
SELECT 
    pizzas.size,
    COUNT(order_details.order_details_id) AS order_count
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC;
```
**Objective-** The objective is to find the most frequent size ordered in pizzas, that is, get the number of orders of each different size. The query actually groups all the pizzas by size and then orders them in a descending manner with respect to the number of orders.

### 5. List the top 5 most ordered pizza types along with their quantities.

```sql
SELECT 
    pizza_types.name, SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity DESC
LIMIT 5;
```
**Objective-** We would like to find the top 5 most ordered types of pizza by adding together quantities ordered for each type. Query for pizza types, aggregated by name and ordered descending by total quantity sold.

### 6. Join the necessary tables to find the total quantity of each pizza category ordered.

```sql
SELECT 
    pizza_types.category,
    SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;
```
**Objective-** Join the proper tables to get the total number of pizzas ordered by each category, then the results are ordered by pizza category in descending order by total number of pizzas ordered.

### 7. Group the orders by date and calculate the average number of pizzas ordered per day.

```sql
SELECT 
    ROUND(AVG(quantity), 0) AS avg_pizza_order_per_day
FROM
    (SELECT 
        orders.order_date, SUM(order_details.quantity) AS quantity
    FROM
        orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date) AS order_quantity;
```
**Objective-** The objective is to group orders by date and calculate the average number of pizzas ordered per day. The query aggregates the total quantity of pizzas for each date and then computes the overall average.

### 8. Determine the top 3 most ordered pizza types based on revenue.

```sql
SELECT 
    pizza_types.name,
    SUM(order_details.quantity * pizzas.price) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
```
**Objective-** The objective is to identify the top 3 most ordered pizza types based on total revenue generated. The query calculates revenue by multiplying the quantity ordered by the price of each pizza type and sorts the results in descending order.


### 9. Calculate the percentage contribution of each pizza type to total revenue.

```sql
SELECT 
    pizza_types.category,
    ROUND(SUM(order_details.quantity * pizzas.price) / (SELECT 
                    ROUND(SUM(order_details.quantity * pizzas.price),
                                2) AS Total_revenue
                FROM
                    order_details
                        JOIN
                    pizzas ON pizzas.pizza_id = order_details.pizza_id) * 100,
            2) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY revenue DESC;
```
**Objective-** The objective is to calculate the percentage contribution of each pizza type to the total revenue generated from sales. The query computes the revenue for each category and divides it by the overall total revenue, presenting the result as a percentage.

### 10. Analyze the cumulative revenue generated over time.

```sql
SELECT order_date,
	SUM(revenue) OVER (ORDER BY order_date) AS cum_revenue
FROM
	(SELECT orders.order_date,
	SUM(order_details.quantity * pizzas.price) AS revenue
	FROM order_details JOIN pizzas
	ON order_details.pizza_id = pizzas.pizza_id 
	JOIN orders
	ON order_details.order_id = orders.order_id
GROUP BY orders.order_date) AS sales;
```
**Objective-** The objective is to analyze the cumulative revenue generated over time by calculating the total revenue for each order date. The query uses a window function to compute the running total of revenue, allowing for insights into revenue trends over time.

### 11. Determine the top 3 most ordered pizza types based on revenue for each pizza category.

```sql
SELECT name,revenue 
FROM
	(SELECT category, name, revenue,
	RANK() OVER(PARTITION BY category ORDER BY revenue DESC) AS rn
FROM
	(SELECT pizza_types.category , pizza_types.name,
	SUM((order_details.quantity) * pizzas.price) AS revenue
FROM
	pizza_types 
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
join order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category , pizza_types.name) AS a) AS b
WHERE rn <=3;
```
**Objective-** The objective is to determine the top 3 most ordered pizza types based on revenue for each pizza category. The query ranks pizza types within their respective categories and filters the results to show only the highest revenue-generating types.

## Conclusion

This project on pizza sales analysis using SQL presents itself well as a very powerful tool in extracting useful information from structured data. The project made use of four key tables: order details, orders, pizza types, and pizza details for complete sales performance analysis, customer preferences, and revenue generation. Some of the findings were determining the most popular pizza types and sizes, understanding revenue contributions by category, and tracking cumulative sales trends over time. In essence, this project shows how data analysis helps to inform better business decisions and optimize the food sector's operation strategies.
