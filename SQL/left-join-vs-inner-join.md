# Left Join vs Inner join

Given two tables, where on has a foreign key to the other, left join and inner join behave quit different.

Given the following tables:

OrderId* | CustomerId (FK) | OrderDate

CustomerId* | CustomerName | DateOfBirth

ItemId* | OrderId (FK) | ItemName | ItemDescription


Typically an Order will always be connected to a customer, but might not always have any items connected. This highlights the difference between inner and left joins.

```sql
SELECT o.* 
       c.*,
       i.*  
FROM Order o
INNER JOIN Customer c on c.CustomerId = o.CustomerId
LEFT JOIN Item i on i.OrderId = i.OrderId
```

This query will only return orders with a connected customer, while the item columns are "optional", meaning that orders without connected items will be included in the result.

Left join can be described as an "optional" join while inner join requires a match. 

![inner-join](img_innerjoin.gif) 
![left-join](img_leftjoin.gif) 
