## Normal Mode 

1. How many users are there?
Query -  SELECT COUNT(\*) FROM users
Answer - There are 50 users in the database

2. What are the 5 most expensive items?
Query - SELECT \* FROM items ORDER BY PRICE DESC LIMIT 5;
Answer - The five most expensive items are:

           id|title|category|description|price
        *  25|Small Cotton Gloves|Automotive, Shoes & Beauty|Multi-layered modular service-desk|9984
        *  83|Small Wooden Computer|Health|Re-engineered fault-tolerant adapter|9859
        * 100|Awesome Granite Pants|Toys & Books|Upgradable 24/7 access|9790
        *  40|Sleek Wooden Hat|Music & Baby|Quality-focused heuristic info-mediaries|9390
        *  60|Ergonomic Steel Car|Books & Outdoors|Enterprise-wide secondary firmware|9341

3. What’s the cheapest book? (Does that change for “category is exactly ‘book’” versus “category contains ‘book’”?)
Query -  SELECT \* FROM items WHERE LOWER(category) LIKE '%book%' ORDER BY PRICE ASC LIMIT 1;
Answer - The cheapest book is:
          id|title|category|description|price
        
        * 76|Ergonomic Granite Chair|Books|De-engineered bi-directional portal|1496

4. Who lives at “6439 Zetta Hills, Willmouth, WY”? Do they have another address?

	Query ---> 1) SELECT \* FROM users WHERE id = (SELECT user\_id FROM addresses WHERE street = '6439 Zetta Hills');<br>
	Answer ---> The person with User\_id 40 lives at 6439 Zetta Hills, Willmouth, WY and they also live at 'Wolff Forges, Lake Bryon, CA 31587'

5. Correct Virginie Mitchell’s address to “New York, NY, 10108”.

	Query ---> UPDATE addresses SET city = "New York", state = "NY", zip = 10108 WHERE user\_id IN (SELECT id FROM users WHERE first\_name = "Virginie" AND last\_name = "Mitchell");<br>
Answer ---> (Done)

6. How much would it cost to buy one of each tool?

	Query ---> SELECT SUM(price) FROM items WHERE LOWER(category) LIKE '%tool%';<br>
	Answer ---> The sum of buying one of each tool is 46,477

7. How many total items did we sell?

	Query ---> SELECT SUM(quantity) FROM orders;<br>
	Answer ---> We sold 2,125 items.

8. How much was spent on books?

	Query ---> SELECT SUM(price \* quantity) FROM orders INNER JOIN items ON orders.item\_id = items.id WHERE LOWER(category) LIKE '%book%';<br>
	Answer ---> The sum of all books sold where the category of the item contains the word 'books' is 
         1,081,352

9. Simulate buying an item by inserting a User for yourself and an Order for that User.

	Query --->  INSERT INTO users VALUES (51, 'Mike', 'Hughes','mike@hughes.com');
         INSERT INTO orders VALUES (378, 51, 56, 6, (STRFTIME('%Y-%m-%d %H:%M:%f', 'NOW')));<br>
	Answer ---> (done)




## Hard Mode

1. What item was ordered most often? Grossed the most money?

	Query ---> 1) SELECT count(\*), item\_id, title, category, description FROM orders INNER JOIN items ON orders.item\_id = items.id GROUP BY item\_id ORDER BY count(\*) DESC;
	       <br><br>  2) SELECT (count(\*) \* price), count(\*), price, item\_id, title, category, description FROM orders INNER JOIN items ON orders.item\_id = items.id GROUP BY item\_id ORDER BY (count(\*) \* price) DESC;
	
	Answer ---> 1) There is a tie for the most orders between - Ergonomic Concrete Gloves, Practical Rubber Computer & Incredible Granite Car, with each having 9 orders.
	
	2) The item that grossed the most money is Practical Rubber Computer which brought in a gross total of 67,806.

2. What user spent the most?

	Query ---> SELECT user\_id, first\_name, last\_name, sum(quantity \* price) from orders inner join users on orders.user\_id = users.id inner join items on orders.item\_id = items.id GROUP BY user\_id ORDER BY SUM(quantity \* price) DESC LIMIT 1;<br>
Answer ---> The user Hassan Runte spent the most. He spent 639,386

3. What were the top 3 highest grossing categories?

	Query ---> SELECT SUM(quantity \* price), category FROM items INNER JOIN orders ON items.id = orders.item\_id GROUP BY category ORDER BY SUM(price \* quantity) DESC LIMIT 3;<br>
	Answer ---> The top 3 highes grossing categories are: 
	
		1) 525240|Music, Sports & Clothing
		2) 449496|Beauty, Toys & Sports
		3) 448410|Sports

