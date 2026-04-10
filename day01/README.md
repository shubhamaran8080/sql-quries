# Day 01 - SQL Practice

## Database Schema

![Database Schema](./database_schema.png)

### Tables Overview

**Salespeople Table**
| SNUM | SNAME | CITY | COMM |
|------|-------|------|------|
| 1001 | Peel | London | .12 |
| 1002 | Serres | San Jose | .13 |
| 1004 | Motika | London | .11 |
| 1007 | Rifkin | Barcelona | .15 |
| 1003 | AxelRod | New York | .10 |
| 1005 | Fran | London | .26 |

**Customers Table**
| CNUM | CNAME | CITY | RATING | SNUM |
|------|-------|------|--------|------|
| 2001 | Hoffman | London | 100 | 1001 |
| 2002 | Giovanni | Rome | 200 | 1003 |
| 2003 | Liu | San Jose | 200 | 1002 |
| 2004 | Grass | Berlin | 300 | 1002 |
| 2006 | Clemens | London | 100 | 1001 |
| 2008 | Cisneros | San Jose | 300 | 1007 |
| 2007 | Pereira | Rome | 100 | 1004 |

**Orders Table**
| ONUM | AMT | ODATE | CNUM | SNUM |
|------|-----|-------|------|------|
| 3001 | 18.69 | 10/03/96 | 2008 | 1007 |
| 3003 | 767.19 | 10/03/96 | 2001 | 1001 |
| 3002 | 1900.10 | 10/03/96 | 2007 | 1004 |
| 3005 | 5160.45 | 10/03/96 | 2003 | 1002 |
| 3006 | 1098.16 | 10/03/96 | 2008 | 1007 |
| 3009 | 1713.23 | 10/04/96 | 2002 | 1003 |
| 3007 | 75.75 | 10/04/96 | 2002 | 1003 |
| 3008 | 4723.00 | 10/05/96 | 2006 | 1001 |
| 3010 | 1309.95 | 10/06/96 | 2004 | 1002 |
| 3011 | 9891.88 | 10/06/96 | 2006 | 1001 |

## SQL Queries

1. List all the columns of the Salespeople table.

select*from salespeople;
+------+---------+-----------+------+
| SNUM | SNAME   | CITY      | COMM |
+------+---------+-----------+------+
| 1001 | Peel    | London    | 0.12 |
| 1002 | Serres  | San Jose  | 0.13 |
| 1003 | AxelRod | New York  | 0.10 |
| 1004 | Motika  | London    | 0.11 |
| 1005 | Fran    | London    | 0.26 |
| 1007 | Rifkin  | Barcelona | 0.15 |




2.List all customers with a rating of 100.


select * from customers where rating = 100;
+------+---------+--------+--------+------+
| CNUM | CNAME   | CITY   | RATING | SNUM |
+------+---------+--------+--------+------+
| 2001 | Hoffman | London |    100 | 1001 |
| 2006 | Clemens | London |    100 | 1001 |
| 2007 | Pereira | Rome   |    100 | 1004 |
+------+---------+--------+--------+------+

3.Find all records in the Customer table with NULL values in the city column.


select * from customers where city is null;
Empty set (0.00 sec)



4. Find the largest order taken by each salesperson on each date.


select snum , odate, max(amt) from orders group by snum, odate;
+------+------------+----------+
| snum | odate      | max(amt) |
+------+------------+----------+
| 1007 | 1996-10-03 |  1098.16 |
| 1004 | 1996-10-03 |  1900.10 |
| 1001 | 1996-10-03 |   767.19 |
| 1002 | 1996-10-03 |  5160.45 |
| 1003 | 1996-10-04 |  1713.23 |
| 1001 | 1996-10-05 |  4723.00 |
| 1002 | 1996-10-06 |  1309.95 |
| 1001 | 1996-10-06 |  9891.88 |
+------+------------+----------+

5. Arrange the Orders table by descending customer number.
select amt, cnum from orders order by cnum desc;


SELECT amt, cnum FROM orders ORDER BY cnum DESC;

+------+------+
| amt  | cnum |
+------+------+
|  200 | 1005 |
|  300 | 1003 |
|  700 | 1002 |
|  500 | 1001 |
+------+------+
4 rows in set (0.00 sec)





6. Find which salespeople currently have orders in the Orders table.

select distinct snum from orders;
+------+
| snum |
+------+
| 1001 |
| 1002 |
| 1003 |
| 1004 |
| 1007 |
+------+
extra


7. List names of all customers matched with the salespeople serving them.


select distinct c.cname, s.sname from customers c join salespeople s on c.snum = s.snum;
+----------+---------+
| cname    | sname   |
+----------+---------+
| Hoffman  | Peel    |
| Giovanni | AxelRod |
| Liu      | Serres  |
| Grass    | Serres  |
| Clemens  | Peel    |
| Pereira  | Motika  |
| Cisneros | Rifkin  |
+----------+---------+

8. Find the names and numbers of all salespeople who had more than one customer.



SELECT s.snum, s.sname
FROM customers c
JOIN salespeople s
ON c.snum = s.snum
GROUP BY s.snum, s.sname
HAVING COUNT(c.cnum) > 1;



+------+--------+
| snum | sname  |
+------+--------+
| 1001 | Peel   |
| 1002 | Serres |
+------+--------+



9. Count the orders of each of the salespeople and output the results in descending order.



 select snum, count(8) as total_orders from orders group by snum order by total_orders desc;
+------+--------------+
| snum | total_orders |
+------+--------------+
| 1001 |            3 |
| 1002 |            2 |
| 1003 |            2 |
| 1007 |            2 |
| 1004 |            1 |
+------+--------------+
5 rows in set (0.00 sec)


10. List the Customer table if and only if one or more of the customers in the Customer table are
located in San Jose.



select * from customers
 where exists
(select 1 from customers where city = 'san jose');

| CNUM | CNAME    | CITY     | RATING | SNUM |
+------+----------+----------+--------+------+
| 2001 | Hoffman  | London   |    100 | 1001 |
| 2002 | Giovanni | Rome     |    200 | 1003 |
| 2003 | Liu      | San Jose |    200 | 1002 |
| 2004 | Grass    | Berlin   |    300 | 1002 |
| 2006 | Clemens  | London   |    100 | 1001 |
| 2007 | Pereira  | Rome     |    100 | 1004 |
| 2008 | Cisneros | San Jose |    300 | 1007 |
+------+----------+----------+--------+------+



11. Match salespeople to customers according to what city they lived in.


select s.sname, c.cname , c.city from customers c join salespeople s on c.city = s.city;
+--------+----------+----------+
| sname  | cname    | city     |
+--------+----------+----------+
| Fran   | Hoffman  | London   |
| Motika | Hoffman  | London   |
| Peel   | Hoffman  | London   |
| Serres | Liu      | San Jose |
| Fran   | Clemens  | London   |
| Motika | Clemens  | London   |
| Peel   | Clemens  | London   |
| Serres | Cisneros | San Jose |
+--------+----------+----------+

11. Match salespeople to customers according to what city they lived in.


select s.sname, c.cname , c.city from customers c join salespeople s on c.city = s.city;
+--------+----------+----------+
| sname  | cname    | city     |
+--------+----------+----------+
| Fran   | Hoffman  | London   |
| Motika | Hoffman  | London   |
| Peel   | Hoffman  | London   |
| Serres | Liu      | San Jose |
| Fran   | Clemens  | London   |
| Motika | Clemens  | London   |
| Peel   | Clemens  | London   |
| Serres | Cisneros | San Jose |
+--------+----------+----------+



12. Find the largest order taken by each salesperson.

select s.snum, s.sname, max(o.amt) as max_orders from orders o join salespeople s on s.snum = o.snum group by s.snum , s.sname;
+------+---------+------------+
| snum | sname   | max_orders |
+------+---------+------------+
| 1007 | Rifkin  |    1098.16 |
| 1004 | Motika  |    1900.10 |
| 1001 | Peel    |    9891.88 |
| 1002 | Serres  |    5160.45 |
| 1003 | AxelRod |    1713.23 |
+------+---------+------------+




```sql





