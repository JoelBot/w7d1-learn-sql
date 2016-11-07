Questions:

How many users are there?:
- select count(*) from users;

What are the 5 most expensive items?:
25	Small Cotton Gloves	9984
83	Small Wooden Computer	9859
100	Awesome Granite Pants	9790
40	Sleek Wooden Hat	9390
60	Ergonomic Steel Car	9341
- select items.id, items.title, items.price from items order by price desc limit 5;

What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?): 76	Ergonomic Granite Chair	1496. yep, we get two more results with contains.
- select items.id, items.title, items.price, items.category from items where category is 'Books' order by price limit 1;

Who lives at "6439 Zetta Hills, Willmouth, WY"? Corrine	Little
- select a.user_id, u.first_name, u.last_name, a.street, a.state, a.zip from addresses a INNER JOIN users u on a.user_id=u.id where a.street like "%6439 Zetta Hills%";
Do they have another address? yep, 2 addresses
- ;select a.user_id, u.first_name, u.last_name, a.street, a.state, a.zip from addresses a INNER JOIN users u on a.user_id=u.id where u.id is 40;

Correct Virginie Mitchell's address to "New York, NY, 10108".
- update addresses SET city = 'New York', state = 'NY', zip = 10108 where user_id=39 and city='Roxanehaven';

How much would it cost to buy one of each tool? 467488
- select sum(price) from items;

How many total items did we sell? 2125
-select sum(quantity) from orders;

How much was spent on books? Books	420566
- select i.category, sum(o.quantity*i.price) from orders o INNER JOIN items i on o.item_id=i.id where i.category = 'Books';

Simulate buying an item by inserting a User for yourself and an Order for that User. Ok
- insert into users (first_name, last_name, email) values ('Joel', 'Lonneker', 'Joel.Lonneker@gmail.com');
- insert into orders (user_id, item_id, quantity, created_at) values (51, 5, 42, datetime());
- select o.id, o.user_id, i.title, i.price, o.quantity*i.price from orders o INNER JOIN items i on o.item_id=i.id where o.user_id = 51;
id          user_id     title                     price       o.quantity*i.price
----------  ----------  ------------------------  ----------  ------------------
378         51          Ergonomic Plastic Gloves  3953        166026             

Adventure Mode

What item was ordered most often? (as in frequency of order, not quantity)
id          item_id     title                      price       count(o.item_id)
----------  ----------  -------------------------  ----------  ----------------
340         10          Ergonomic Concrete Gloves  6114        9               
364         46          Practical Rubber Computer  7534        9               
334         65          Incredible Granite Car     7295        9               
346         68          Ergonomic Granite Compute  3249        8               
273         13          Small Plastic Pants        7476        7               
- select o.id, o.item_id, i.title, i.price, count(o.item_id) from orders o INNER JOIN items i on o.item_id=i.id group by o.item_id order by count(o.item_id) desc limit 5;

Which item grossed the most money?  
id          item_id     title                   price       sum(o.quantity)  sum(o.quantity)*i.price
----------  ----------  ----------------------  ----------  ---------------  -----------------------
334         65          Incredible Granite Car  7295        72               525240                 
363         41          Practical Granite Car   8324        54               449496                 
359         90          Practical Wooden Shoes  8895        50               444750                 
364         46          Practical Rubber Compu  7534        53               399302                 
341         45          Ergonomic Cotton Chair  7479        44               329076   
- select o.id, o.item_id, i.title, i.price, sum(o.quantity), sum(o.quantity)*i.price from orders o INNER JOIN items i on o.item_id=i.id group by i.id order by sum(o.quantity)*i.price desc limit 5;

What user spent the most?

What were the top 3 highest grossing categories?
