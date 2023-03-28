##Customer

|Customer_Id| Customer_Name|
|-|-
|1| John|
|2| Smith|
|3| Ricky|
|4| Walsh|
|5| Stefen|
|6| Fleming|
|7| Thomson|
|8| David|

```
create table customer(
    customer_id int primary key auto_increment,
    customer_name varchar(50)
);
```
```
insert into customer(customer_name) values("John");
insert into customer(customer_name) values("Smith");
insert into customer(customer_name) values("Ricky");
insert into customer(customer_name) values("Walsh");
insert into customer(customer_name) values("Stefen");
insert into customer(customer_name) values("Fleming");
insert into customer(customer_name) values("Thomson");
insert into customer(customer_name) values("David");
```

##Product


|Product_Id| Product_Name| Product_Price |
|-|-|-|
|1| Television| 19000 | 
|2| DVD| 3600 |
|3| Washing Machine|  7600 |
|4| Computer| 35900 | 
|5| Ipod| 3210 |
|6| Panasonic Phone|  2100 |
|7| Chair| 360 |
|8| Table| 490 |
|9| Sound System|  12050 |
|10| Home Theatre|  19350 |

```
create table product(
    product_id int primary key auto_increment,
    product_name varchar(100),
    product_price int
);
```
```
insert into product(product_name,product_price) values("Television",1900);
insert into product(product_name,product_price) values("DVD",3600);
insert into product(product_name,product_price) values("Washing Machine",7600);
insert into product(product_name,product_price) values("Computer",35900);
insert into product(product_name,product_price) values("Ipod",3210);
insert into product(product_name,product_price) values("Panasonic Phone",2100);
insert into product(product_name,product_price) values("Chair",360);
insert into product(product_name,product_price) values("Table",490);
insert into product(product_name,product_price) values("Sound System",12050);
insert into product(product_name,product_price) values("Home Theatre",19350);
```

##Order

|Order_Id| Customer_Id| Ordered_Date |
|-|-|-
|1| 4| 10-Jan-05|
|2| 2| 10-Feb-06|
|3| 3| 20-Mar-05|
|4| 3| 10-Mar-06|
|5| 1| 5-Apr-07|
|6| 7| 13-Dec-06|
|7| 6| 13-Mar-08|
|8| 6| 29-Nov-04|
|9| 5| 13-Jan-05|
|10| 1| 12-Dep-2007|

```
create table orders(
    order_id int primary key auto_increment,
    customer_id int,
    order_date date,
    foreign key(customer_id) references customer(customer_id)
);
```
```
insert into orders(customer_id,order_date) values(4,"2005-01-10");
insert into orders(customer_id,order_date) values(2,"2006-02-10");
insert into orders(customer_id,order_date) values(3,"2005-03-20");
insert into orders(customer_id,order_date) values(3,"2006-03-10");
insert into orders(customer_id,order_date) values(1,"2007-04-5");
insert into orders(customer_id,order_date) values(7,"2006-12-13");
insert into orders(customer_id,order_date) values(6,"2008-03-13");
insert into orders(customer_id,order_date) values(6,"2004-11-29");
insert into orders(customer_id,order_date) values(5,"2005-01-13");
insert into orders(customer_id,order_date) values(2,"2007-12-12");
```

##Order_Details

|Order_Detail_Id| Order_Id| Product_Id| Quantity|
|-|-|-|-|
|1| 1| 3| 1|
|2| 1| 2| 3|
|3| 2| 10| 2|
|4| 3| 7| 10|
|5| 3| 4| 2|
|6| 3| 5| 4|
|7| 4| 3| 1|
|8| 5| 1| 2|
|9| 5| 2| 1|
|10| 6| 5| 1|
|11| 7| 6| 1|
|12| 8| 10| 2|
|13| 8| 3| 1|
|14| 9| 10| 3|
|15| 10| 1| 1|

```
create table order_details(
    order_detail_id int primary key auto_increment,
    order_id int,
    product_id int,
    quantity int,
    unique key(order_id, product_id),
    foreign key(order_id) references orders(order_id),
    foreign key (product_id) references product(product_id)
);
```
```
insert into order_details(order_id,product_id,quantity) values(1,3,1);
insert into order_details(order_id,product_id,quantity) values(1,2,3);
insert into order_details(order_id,product_id,quantity) values(2,10,2);
insert into order_details(order_id,product_id,quantity) values(3,7,10);
insert into order_details(order_id,product_id,quantity) values(3,4,2);
insert into order_details(order_id,product_id,quantity) values(3,5,4);
insert into order_details(order_id,product_id,quantity) values(4,3,1);
insert into order_details(order_id,product_id,quantity) values(5,1,2);
insert into order_details(order_id,product_id,quantity) values(5,2,1);
insert into order_details(order_id,product_id,quantity) values(6,5,1);
insert into order_details(order_id,product_id,quantity) values(7,6,1);
insert into order_details(order_id,product_id,quantity) values(8,10,2);
insert into order_details(order_id,product_id,quantity) values(8,3,1);
insert into order_details(order_id,product_id,quantity) values(9,10,3);
insert into order_details(order_id,product_id,quantity) values(10,1,1);
```

---
##Queries


1. Fetch all the Customer Details along with the product names that the customer has ordered.
    ```
    select c.*, p.product_name 
    from customer c 
    join orders o on o.customer_id=c.customer_id
    join order_details d on d.order_id=o.order_id
    join product p on p.product_id=d.product_id;
    ```

2. Fetch Order_Id, Ordered_Date, Total Price of the order (product price*qty).
    ```
    select o.order_id, o.order_date, sum(p.product_price*d.quantity) as Price 
    from order_details d
    join orders o on  o.order_id=d.order_id
    join product p on p.product_id=d.product_id
    group by o.order_id; 
    ```

3. Fetch the Customer Name, who has not placed any order
    ```
    select customer_name from customer 
    where customer_id not in (select customer_id from orders);
    ```


4. Fetch the Product Details without any order(purchase)
    ```
    select * from product 
    where product_id not in (select product_id from order_details);
    ```


5. Fetch the Customer name along with the total Purchase Amount
    ```
    select c.customer_name, sum(p.product_price*d.quantity) as "Total Purchase amount"
    from order_details d
    join product p on d.product_id=p.product_id
    join orders o on o.order_id=d.order_id
    join customer c on c.customer_id=o.customer_id
    group by c.customer_name
    order by "totla purchase amount" desc;
    ```

6. Fetch the Customer details, who has placed the first and last order
    ```
    select * from customer 
    where customer_id in (
        select customer_id from orders where order_date in 
        (
            select max(order_date) from orders
            union
            select min(order_date) from orders 
            )
        );
    ```

7. Fetch the customer details , who has placed more number of orders
    ```
    select * from customer 
    where customer_id = 
        (select customer_id from orders 
        group by customer_id 
        order by count(customer_id) desc 
        limit 1);
    ```


8. Fetch the customer details, who has placed multiple orders in the same year
    ```
    select c.customer_name, year(o.order_date) as year, count(o.order_id) as NoOfOrders
    from customer c
    join orders o on o.customer_id=c.customer_id 
    group by year,c.customer_id having NoOfOrders>1 ;
    ```


9. Fetch the name of the month, in which more number of orders has been placed
    ```
    select DATE_FORMAT(order_date, "%M") as month, count(order_id) as totalOrders 
    from orders 
    group by month 
    order by totalOrders desc 
    limit 1 ;
    ```


10. Fetch the maximum priced Ordered Product
    ```
    select * from product 
    where product_id in (select distinct product_id from order_details) 
    order by product_price desc 
    limit 1;
    ```

