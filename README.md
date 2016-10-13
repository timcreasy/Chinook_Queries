# Chinook Queries


Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.
```sql
SELECT FirstName || " " || LastName AS "Full Name", CustomerId, Country FROM Customer
WHERE Country != "Canada";
```
Provide a query only showing the Customers from Brazil.
```sql
SELECT * FROM Customer
WHERE Country == "Brazil";
```
Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.
```sql
SELECT Customer.FirstName || " " || Customer.LastName AS "Full Name", Invoice.InvoiceId, Invoice.InvoiceDate, Invoice.BillingCountry FROM Customer
JOIN Invoice ON Customer.CustomerId == Invoice.CustomerId
WHERE Customer.Country == "Brazil";
```
Provide a query showing only the Employees who are Sales Agents.
```sql
SELECT * FROM Employee
WHERE Title == "Sales Support Agent";
```
Provide a query showing a unique/distinct list of billing countries from the Invoice table.
```sql
SELECT BillingCountry FROM Invoice
GROUP BY BillingCountry;
```
Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.
```sql
SELECT Employee.FirstName || " " || Employee.LastName AS "Full Name", Invoice.* FROM Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Customer.SupportRepId == Employee.EmployeeId
WHERE Employee.Title == "Sales Support Agent";
```
Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.
```sql
SELECT Customer.FirstName || " " || Customer.LastName, Invoice.Total, Customer.Country, Employee.FirstName || " " || Employee.LastName AS "Sales Agent" FROM Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Customer.SupportRepId == Employee.EmployeeId;
```
How many Invoices were there in 2009 and 2011? 
```sql
SELECT COUNT(*) FROM Invoice
WHERE strftime('%Y', InvoiceDate) = "2009" OR strftime('%Y', InvoiceDate) = "2011";
```
What are the respective total sales for each of those years?
```sql
SELECT SUM(Total) FROM Invoice
WHERE strftime('%Y', InvoiceDate) = "2009";

SELECT SUM(Total) FROM Invoice
WHERE strftime('%Y', InvoiceDate) = "2011";
```
Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.
```sql
SELECT COUNT(*) FROM InvoiceLine
WHERE InvoiceId = 37
```
Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: [GROUP BY](http://www.sqlite.org/lang_select.html#resultset)
```sql
SELECT COUNT(*), InvoiceId FROM InvoiceLine
GROUP BY InvoiceId
```
Provide a query that includes the purchased track name with each invoice line item.
```sql
SELECT Track.Name, InvoiceLine.InvoiceLineId FROM InvoiceLine
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
```
Provide a query that includes the purchased track name AND artist name with each invoice line item.
```sql
SELECT InvoiceLine.InvoiceLineId, Track.Name, Artist.Name FROM InvoiceLine
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN Artist ON Album.ArtistId = Artist.ArtistId
```
Provide a query that shows the # of invoices per country. HINT: [GROUP BY](http://www.sqlite.org/lang_select.html#resultset)
```sql
SELECT BillingCountry, COUNT(*) AS "Number of Invoices"  FROM Invoice
GROUP BY BillingCountry
```
Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resulant table.
```sql
SELECT Playlist.Name, COUNT(Playlist.Name) AS "Total" FROM Playlist
JOIN PlaylistTrack ON Playlist.PlaylistId = PlaylistTrack.PlaylistId
GROUP BY Playlist.Name
```
Provide a query that shows all the Tracks, but displays no IDs. The result should include the Album name, Media type and Genre.
```sql
SELECT Track.Name, Track.Composer, Track.Milliseconds, Track.UnitPrice, Album.Title, MediaType.Name, Genre.Name  FROM Track
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN MediaType ON Track.MediaTypeId = MediaType.MediaTypeId
JOIN Genre ON Track.GenreId = Genre.GenreId
```
Provide a query that shows all Invoices but includes the # of invoice line items.
```sql
SELECT Invoice.*, COUNT(InvoiceLine.InvoiceId) AS "Invoice Lines" FROM Invoice
JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId
GROUP BY InvoiceLine.InvoiceId
```
Provide a query that shows total sales made by each sales agent.
```sql
SELECT Employee.*, SUM(Invoice.Total) AS "Total Sales" FROM Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
GROUP BY Employee.EmployeeId
```
Which sales agent made the most in sales in 2009?
```sql
SELECT Employee.*, SUM(Invoice.Total) AS "Total Sales" FROM Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
WHERE strftime("%Y", Invoice.InvoiceDate) = "2009"
GROUP BY Employee.EmployeeId
ORDER BY "Total Sales" DESC LIMIT 1
```
1. Which sales agent made the most in sales over all?
1. Provide a query that shows the count of customers assigned to each sales agent.
1. Provide a query that shows the total sales per country.
1. Which country's customers spent the most?
1. Provide a query that shows the most purchased track of 2013.
1. Provide a query that shows the top 5 most purchased tracks over all.
1. Provide a query that shows the top 3 best selling artists.
1. Provide a query that shows the most purchased Media Type.
