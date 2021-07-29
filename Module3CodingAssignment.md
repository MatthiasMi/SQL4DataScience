# Looking at [ER-Diagram of Chinook Database](ChinookDatabaseER.png), the following solves:


## 1. Using a subquery, find the names of all the tracks for the album "Californication". What is the title of the 8th track?

```sql
SELECT t.Name
FROM Tracks AS t
LEFT JOIN Albums AS a
ON t.AlbumId = a.AlbumId
WHERE a.Title = "Californication"
LIMIT 1 OFFSET 7 -- non-standard for 1 + 7 = 8th entry only
; -- Porcelain
```


## 2. Find the total number of invoices for each customer along with the customer's full name, city and email. After running the query described, what is the email address of the 5th person, František Wichterlová?

```sql
SELECT a.FirstName, a.LastName, a.City, a.Email, count(a.InvoiceId) AS total_invoices
FROM
(SELECT c.FirstName, c.LastName, c.City, c.Email, i.InvoiceId
FROM Customers AS c
LEFT JOIN Invoices AS i
ON c.CustomerId = i.CustomerId) AS a
WHERE a.FirstName = "František" AND a.LastName = "Wichterlová" 
; -- frantisekw@jetbrains.com
```


## 3. Retrieve the track name, album, artistID, and trackID for all the albums. What is the song title of trackID 12 FROM the "For Those About to Rock We Salute You" album?

```sql
SELECT t.Name AS trackName, a.Title, a.ArtistId, t.TrackId
FROM Tracks AS t
LEFT JOIN Albums AS a
ON t.AlbumId = a.AlbumId
WHERE t.TrackId = 12 AND a.Title LIKE "For Those About to Rock We Salute You"
; -- Breaking The Rules
```


## 4. Retrieve a list with the managers last name, and the last name of the employees who report to him or her. After running the query described, who are the reports for the manager named Mitchell (SELECT all that apply)?

```sql
SELECT e.LastName AS EmployeeLastName, e.ReportsTo, m.LastName AS ManagerLastName
FROM Employees AS e
LEFT JOIN Employees AS m
ON e.ReportsTo = m.EmployeeId
WHERE m.LastName LIKE "Mitchell"
; -- King, Callahan
```


## 5. Find the name and ID of the artists who do not have albums. After running the query described, two of the records returned have the same last name.

```sql
SELECT Name AS Artist, ar.ArtistId, al.Title AS Album
FROM Artists AS ar
LEFT JOIN Albums AS al
ON ar.ArtistId = al.ArtistId
WHERE Album IS NULL
LIMIT 2 OFFSET 2 -- detected visually
; -- Gilberto
```


## 6. Use a UNION to create a list of all the employee's and customer's first names and last names ordered by the last name in descending order. After running the query described, determine what is the last name of the 6th record?

```sql
SELECT FirstName, LastName FROM Employees
UNION
SELECT FirstName, LastName FROM Customers
ORDER BY LastName DESC
LIMIT 1 OFFSET 5 -- non-standard for 1 + 5 = 6th entry only
; -- Taylor
```


## 7. See if there are any customers who have a different city listed in their billing city versus their customer city.

```sql
SELECT c.FirstName, c.City, i.BillingCity
FROM Customers AS c
LEFT JOIN Invoices AS i
ON c.CustomerId = i.CustomerId
WHERE i.BillingCity <> c.City
; -- No customers have a different city listed in their billing city versus customer city.
```