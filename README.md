# Assignment7Database
This is the 7th. assignment for PBA Database soft2019spring

-----------------------------------------------------------------------------------------------------------------------------------
# Setup
The following have been run with the setup found in this small script. [small setup script](https://github.com/cph-js284/MySqlSetup)
*please notice this script sets up both the classicmodels and coffee database, which is a bit overkill*

-----------------------------------------------------------------------------------------------------------------------------------
# Exercise 1
Mention which violation there are to:
  - First normal form (if any)
  - Second normal form (if any)
  - Third normal form (if any)

*The following answer is based on this query, given in the exercise<br>*
```sql
select customerNumber,
       customerName,
       concat(contactFirstName,' ', contactLastName) as contactName,
       customers.phone as contactPhone,
       customers.city as custCity,
       customers.postalCode as custZip,
       customers.country as custCountry,
       concat(firstName,' ',lastName) as repName,
       employees.email as repEmail,
       offices.phone as repPhone,
       offices.postalCode as repZip,
       offices.city as repCity,
       concat(offices.addressLine1, '\n', offices.addressLine2) as repAddress,
       offices.country as repCountry
from employees 
inner join customers on employees.employeeNumber = customers.salesRepEmployeeNumber
inner join offices on employees.officeCode = offices.officeCode
```

The resulting table, from running the query above, have the following violations:

<b>Violating the 1st normalform</b><br>
In some cases, depending on the businesslogic the first normalform, might be violated by the customername column and the address column due to the fact that they contain more than one value (e.i. customername consists of both a first and a last name)<br>
<br>
<b>Violating the 2nd normalform</b><br>
Given that the above violation of the 1st normalform stands, the 2nd normalform is violated by not being on the 1st normalform.<br>
<br>
<b>Violating the 3rd normalform</b><br>
Given that the above violation of the 2nd normalform stands, the 3rd normalform is violated by not being on the 2nd normalform.<br>
Furthermore there are several columns that are not solely dependent on the primary key, due to the fact that this was 3 seperate tables
that were joined. So basicly all the columns from the customer table and the column from the emploeey table are in violation.

----------------------------------------------------------------------------------------------------------------------------------
# Exercise 2

*Assume we did not include the customerNumber in the table. What could be a key, and do we get the same violations of the normal forms?*

It was considered creating a composite key of out the name and phonenumber to uniquely identify the customers, but in a (very)rare case you could end up with a different person(customer), with the same name acquiring the phonenumber after it had been given up(for some reason). This same argument can be used for all the address information aswell.
 
This ultimately leads to the conclusion that none of the columns- composite or otherwise can functions as a valid primarykey. 
Without a primarykey the table would therefore be violating the 1 st normalform and by extension the rest of the of the normalforms.

------------------------------------------------------------------------------------------------------------------------
# Exercise 3
*Assume the same table as in exercise 2.

    Write a safe update statement that change the repPhone column from oldNumber (say 12345678) to newNumber (say 87654321).
    Write an update of repEmail which do not update properly (do not update it everywhere it should)
*
A safe update statement:
```sql
update CustomerOverview set repPhone = 87654321 where repPhone = 12345678
```
The ‘safe’ part of this update is the fact that the ‘where’-clause is used

An unsafe update statement:
```sql
update CustomerOverview set repEmail = “new@email.address” where customerName = "Mini Wheels Co." and repName = "Leslie Jennings"
```
The ‘unsafe’ part of this update is the fact that both the repName and the customerName can change, this would lead to undesired updates.

------------------------------------------------------------------------------------------------------------------
# Exercise 4

*Assume we have an index on customerName, and assume a fan-out in the B+ tree of 4.
Draw a representation of of the B+ tree with index and leaf nodes, as well as the actual table data.*

Based on the following sql-statement:
```sql
with my_cust as
   (select customerNumber,
       customerName,
       customers.country as custCountry,
       offices.city      as repCity
    from employees
       inner join customers on employees.employeeNumber = customers.salesRepEmployeeNumber
       inner join offices on employees.officeCode = offices.officeCode)
select *
from my_cust
where repCity = 'Sydney'
```
<br>
The below drawing illustrates the b-tree index, with a fanout of 4.
The red line traversing the indices illustrates how the index 'Handji Gifts& Co' is found.
A comparison is made for each index untill it either finds a direct match or the index it is compared to contains a larger value.
In this case 'Handji Gifts& Co' is larger than the first 2 indices it is compared to, but smaller than the 3rd, so the "line" from the 3rd index is followed to a new block of indices, untill it finally reaches a match. At this point the values related to the index can be retrived.<br>

![whiteboard](https://github.com/cph-js284/Assignment7Database/blob/master/B-tree-index.png)

----------------------------------------------------------------------------------------------------------------

# cleanup
If you did indeed you the above script to setup the docker container, you can delete the entire thing with the following command
```
sudo docker rm -f mysql01
```
