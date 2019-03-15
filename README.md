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
```
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

<b>Violating the 1st normalform</b>
In some cases depending on the businesslogic the first NF. might be violated by the customername column and the address column due to the fact that they contain more than one value (e.i. customername consists of both a first and a last name)

2.NF
(the table is not in NF1)
The second NF is violated in the following columns:
custCity and custZip, basically all the columns containing information about the rep 

3NF.
(the table is not in NF2)
This is the same problem as above, there are several columns that are not solely dependent on the primary key (MÅSKE SKAL DER FORKLARES NOGET MERE HER)


----------------------------------------------------------------------------------------------------------------
# cleanup
If you did indeed you the above script to setup the docker container, you can delete the entire thing with the following command
```
sudo docker rm -f mysql01
```
