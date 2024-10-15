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
