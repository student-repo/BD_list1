
1) select ProductName, CategoryName from products inner join categories on products.CategoryID = categories.CategoryID
where ProductName like '%p' && CategoryName like 'c%';


2) select ProductName, UnitPrice from products where UnitPrice=20 || UnitPrice=50 || UnitPrice=NULL;


3) select ProductID, ProductName, CategoryName, UnitPrice from products inner join categories on
products.CategoryID=categories.CategoryID order by CategoryName DESC, UnitPrice;


4) select Region, count(*) from suppliers group by Region;


5) select ProductID from order_details order by Quantity*UnitPrice limit 6;


6) select ProductID from(select ProductID, avg(Quantity) as a from order_details group by ProductID having a>21) as t;


7) select FirstName, OrderID from employees inner join orders orders on employees.EmployeeID=orders.EmployeeID where orders.OrderDate < DATE("1998-01-23");


8) select ContactName, OrderDate from customers left outer join orders on customers.CustomerID=orders.CustomerID;


9)


10) select ProductName, OrderDate from products inner join order_details on products.ProductID=order_details.ProductID inner
join orders on order_details.OrderID=orders.OrderID;


11)


12) select customers.CustomerID, ProductName, order_details.UnitPrice*order_details.Quantity from customers inner join orders on
customers.CustomerID=orders.CustomerID inner join
 order_details on orders.OrderID=order_details.OrderID inner join products on order_details.ProductID=products.ProductID ;


or select CustomerID, group_concat(ProductName SEPARATOR ', ') from orders inner join order_details on orders.OrderID=order_details.OrderID inner join
products on products.ProductID=order_details.ProductID group by CustomerID;

or select orders.CustomerID, group_concat(ProductName separator '\n'), foo from products inner join order_details on products.ProductID=order_details.ProductID inner join orders on order_details.OrderID=orders.OrderID inner join (select CustomerID, sum(Quantity*Unitprice) as foo from orders inner join order_details on orders.OrderID=order_details.OrderID group by customerID) as k on k.CustomerID=orders.CustomerID  group by CustomerID order by CustomerID;


13) select ProductID, min(Quantity) from order_details group by ProductID;


14) select customers.CustomerID, OrderDate from customers inner join orders on customers.CustomerID=orders.CustomerID where orders.OrderDate not in
(select OrderDate from customers inner join orders on customers.CustomerID=orders.CustomerID where OrderDate = date('1997-05-15'));


15) create temporary table if not exists temporary_customers (select * from customers where CustomerID like 'T%' or CustomerID like 't%');


16) create temporary table if not exists foo4 (select * from order_details);
     DELETE FROM order_details WHERE order_details.ID IN (select ID from foo4 where OrderID in (select OrderID from orders where
     OrderDate = date('1998-04-14') or
     OrderDate = date('1999-07-17')));


18) create temporary table if not exists foo7 (select * from products);
    update products set UnitPrice=UnitPrice+2 where ProductID IN (select ProductID from foo7 inner join  suppliers on
    foo7.SupplierID=suppliers.SupplierID where suppliers.Country = 'USA');


    much better:
    update products set UnitPrice=UnitPrice+2 where products.SupplierID in (select SupplierID from suppliers where Country="USA");


19) update products inner join (select ProductID, sum(Quantity) as total from order_details group by ProductID) as t on
products.ProductID=t.ProductID set products.TotalSales=t.total;


20) alter table products drop column TotalSales;






1) select customers.CustomerID, OrderDate from customers, orders where customers.CustomerID=orders.CustomerID and orders.OrderDate not in (select OrderDate from customers, orders where  customers.CustomerID=orders.CustomerID and OrderDate = date('1997-05-15'));
2) select CustomerID, CompanyName sum(Quantity*UnitPrice) from customers inner join orders on customers.CustomerID=orders.CustomerID inner join order_details on order_details.OrderID=orders.OrderID group by CustomerID;
3) select * from customers where City like 'C%' or City like 'c%';


1) Pobierz listę wszystkich firm, które nie złożyły zamówienia 15 maja 1997. Nie wolno użyć instrukcji JOIN.
2) Wypisz identyfikator i nazwę klienta(firmy) oraz zamówione przez niego towary i łączną ich cenę(z podziałem na towar i uwzględnieniem zniżki) dla wszystkich klientów, którzy złożyli jakiekolwiek zamówienie. Odfiltruj (tzn. pozostaw tylko pozostałe rekordy) te rekordy, w których łączna cena za produkt po uwzględnieniu zniżek nie przekracza 200.
3) Wybierz wszystkich klientów, pochodzących z miasta zaczynającego się na literę 'C'.
