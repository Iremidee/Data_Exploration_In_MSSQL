# Data Exploration In MSSQL (Adventure works 2019)

1. Retrieve information about the products with colour values except null, red, silver/black, white and list price between $75 and $750. 
Rename the column standardcost to Price. Also, sort the results in descending order by list price.

Select * from Production.Product
Select ProductID, Name, ProductNumber, MakeFlag, FinishedGoodsFlag, Color, SafetyStockLevel, ReorderPoint, StandardCost AS Price, ListPrice, Size, 
SizeUnitMeasureCode, WeightUnitMeasureCode, Weight, DaysToManufacture, ProductLine, Class, ProductSubcategoryID, ProductModelID, SellStartDate, SellEndDate, DiscontinuedDate
from Production.Product 
where ListPrice between 75 and 750 AND Color not in ('RED','SILVER/BLACK', 'WHITE') AND Color is not NULL order by ListPrice desc;

2. Find all the male employees born between 1962 to 1970 and with hire date greater than 2001 and female employees born between 1972 & 1975 and the hire date between 2001 & 2002.


Select [BusinessEntityID], [BirthDate], [Gender], [HireDate]
From [HumanResources].[Employee]
Where Gender= 'M' AND BirthDate Between '1962-01-01' AND '1970-12-31' AND HireDate > '2001-01-01'
OR Gender= 'F' AND BirthDate Between '1972-01-01' AND '1975-12-31' AND HireDate Between '2001-01-01' AND '2002-12-31';

3. Create a list of 10 most expensive products that have a product number beginning with'BK'. Include only the product ID, Name and colour.


Select Top 10 ProductID, Name, Color, ProductNumber, ListPrice
From Production.Product
Where SUBSTRING(ProductNumber,1,2) = 'BK' 
Order by ListPrice Desc;

4. Create a list of all contact persons, where the first 4 characters of the last name are the same as the first four character of the email address. 
Also, for all contacts whose first name and last name begins with the same characters, create a new column called full name combining first name and last name only. 
Also provide  the length of the new column full name.


Select * from Person.EmailAddress
Select Person.Person.BusinessEntityID, FirstName, LastName, CONCAT(FirstName, ' ', LastName) AS FullName, 
LEN (CONCAT(FirstName, ' ', LastName)) AS LengthName, EmailAddress
FROM Person.Person inner join Person.EmailAddress 
ON Person.Person.BusinessEntityID = Person.EmailAddress.BusinessEntityID
where SUBSTRING(LastName,1,4) = SUBSTRING(EmailAddress,1,4) AND SUBSTRING(FirstName,1,4) = SUBSTRING(LastName,1,4) 

5. Return all product subcategories that take an average of 3 days or longer to manufacture.


Select ProductSubcategoryID, AVG(DaysToManufacture)
From [Production].[Product] GROUP BY ProductSubcategoryID
 HAVING AVG(DaysToManufacture)>= 3;
 
6. How many Distinct Job title is present in the Employee table?


Select Count (Distinct JobTitle) from [HumanResources].[Employee];

7. Use employee table and calculate the ages of each employee at the time of hiring


Select BusinessEntityID, BirthDate, HireDate, DATEDIFF (YEAR, BirthDate, HireDate) AS Age From HumanResources.Employee;
