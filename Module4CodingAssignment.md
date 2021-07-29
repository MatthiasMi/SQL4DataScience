# Looking at [ER-Diagram of Chinook Database](ChinookDatabaseER.png), the following solves tasks using
```sql
LIMIT 1 OFFSET 5 -- non-standard for 1 + 5 = 6th entry only
```

## 1. Pull a list of customer ids with the customer’s full name, and address, along with combining their city and country together. Be sure to make a space in between these two and make it UPPER CASE (e.g. LOS ANGELES USA). What is the city and country result for CustomerID 16?

```sql
SELECT CustomerId, Address,
FirstName || " " || LastName AS FullName,
UPPER(City || " " || Country) AS CityCountry
FROM Customers
--WHERE CustomerId = 16
; -- MOUNTAIN VIEW USA
```


## 2. Create a new employee user id by combining the first 4 letter of the employee’s first name with the first 2 letters of the employee’s last name. Make the new field lower case and pull each individual step to show your work. What is the final result for Robert King?

```sql
SELECT FirstName, LastName,
LOWER(SUBSTR(FirstName,1,4)) AS fn,
LOWER(SUBSTR(LastName ,1,2)) AS ln,
LOWER(SUBSTR(FirstName,1,4)) || LOWER(SUBSTR(LastName,1,2)) AS userId
-- fn || ln AS userId -- nope!
FROM Employees
--WHERE FirstName = "Robert" AND LastName = "King"
; -- robeki
```


## 3. Show a list of employees who have worked for the company for 15 or more years using the current date function. Sort by lastname ascending. What is the lastname of the last person on the list returned?

```sql
SELECT FirstName, LastName, HireDate,
(STRFTIME('%Y', 'now') - STRFTIME('%Y', HireDate)) - (STRFTIME('%m-%d', 'now') < STRFTIME('%m-%d', HireDate)) AS YearsWorked
FROM Employees
WHERE YearsWorked >= 15
ORDER BY LastName ASC
; -- Peacock
```


## 4. Profiling the Customers table, answer the following question. Column IN (FirstName, PostalCode, Company, Fax, Phone, Address)

```sql
SELECT COUNT(*)
FROM Customers
WHERE [column] IS NULL
; -- Company, Fax, Phone, Postal Code
```


## 5. Find the cities with the most customers and rank in descending order.

```sql
SELECT City, COUNT(*)
FROM Customers
GROUP BY City
ORDER BY COUNT(*) DESC
; -- London, Mountain View, São Paulo
```


## Create a new customer invoice id by combining a customer’s invoice id with their first and last name while ordering your query in the following order: firstname, lastname, and invoiceID. Select all of the correct "AstridGruber" entries that are returned in your results below.

```sql
SELECT C.FirstName, C.LastName, I.InvoiceId,
C.FirstName || C.LastName || I.InvoiceID AS NewId
FROM Customers C INNER JOIN Invoices I
ON C.CustomerId = I.CustomerID
WHERE NewId LIKE 'AstridGruber%'
; -- AstridGruber273, AstridGruber296, AstridGruber370
```