1. 	SELECT FirstName|| " " ||LastName AS FullName, CustomerId, Country
	FROM Customer
	Where Country != "USA"

2. 	SELECT FirstName|| " " ||LastName AS FullName, CustomerId, Country
	FROM Customer
	Where Country = "Brazil"

3. 	SELECT FirstName|| " " ||LastName AS FullName, InvoiceId, InvoiceDate, 	BillingCountry
	FROM Invoice
	INNER JOIN Customer // joining the 2 tables
	Where Country = "Brazil"

4. 	SELECT FirstName|| " " ||LastName AS FullName, Title
	FROM Employee
	Where Title = "Sales Support Agent"

5. 	SELECT DISTINCT BillingCountry
	FROM Invoice
	ORDER By BillingCountry

6.	SELECT FirstName|| " " ||LastName AS FullName, InvoiceId, Title
	FROM Employee
	INNER JOIN Invoice
	Where Title = "Sales Support Agent"

7. SELECT Total, c.FirstName || " " || c.LastName AS FullName, c.Country,   e.FirstName || " " || e.LastName AS employeeName
	FROM Invoice i
	INNER JOIN Customer  c ON c.CustomerId = i.CustomerId
    INNER JOIN Employee e ON e.EmployeeId = c.SupportRepId

 8. How many Invoices were there in 2009 and 2011? What are the respective total sales for each of those years?(include both the answers and the queries used to find the answers)

	SELECT InvoiceDate, sum(Total)
	FROM Invoice i
	Where InvoiceDate >= '2009-01-01' AND InvoiceDate < '2010-01-01' 

9. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.

	SELECT COUNT(InvoiceId)
	FROM InvoiceLine il
	WHERE InvoiceId = 37

10. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: GROUP BY
	
	SELECT InvoiceId, count(Quantity) 
	FROM InvoiceLine 
	GROUP BY InvoiceId

11. Provide a query that includes the track name with each invoice line item.
	
	SELECT t.Name, il.InvoiceLineId
	FROM InvoiceLine il
	INNER JOIN Track t  ON  t.TrackId = il.TrackId


12. Provide a query that includes the purchased track name AND artist name with each invoice line item.
	
	SELECT InvoiceLineId, Track.Name AS TrackName, Artist.Name AS ArtistName
	FROM Track
	INNER JOIN InvoiceLine ON InvoiceLine.TrackId = Track.TrackId
	INNER JOIN Album ON Album.AlbumId = Track.AlbumId
	INNER JOIN Artist ON Artist.ArtistId = Album.AlbumId

13. Provide a query that shows the # of invoices per country. HINT: GROUP BY

	SELECT Count(InvoiceId), BillingCountry
	FROM Invoice
	GROUP BY BillingCountry

14. Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resulant table.

	SELECT pl.Name, COUNT(pt.TrackId)
	FROM Playlist pl
	INNER JOIN PlaylistTrack pt  ON  pt.PlaylistId = pl.PlaylistId
    GROUP BY pl.Name

15. Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.
	SELECT Album.Title AS AlbumTitle, Track.Name AS TrackName, MediaType.Name AS MediaType, Genre.Name AS Genre
	FROM Track
	INNER JOIN Album ON Album.AlbumId = Track.AlbumId
	INNER JOIN MediaType ON MediaType.MediaTypeId = Track.MediaTypeId
	INNER JOIN Genre ON Genre.GenreId = Track.GenreId

16. Provide a query that shows all Invoices but includes the # of invoice line items.

17. Provide a query that shows total sales made by each sales agent.
	SELECT e.FirstName, e.LastName AS FullName, c.CustomerId, i.CustomerId, c.SupportRepId, e.EmployeeId, c.supportRepId, sum(i.Total)
    FROM Customer c
    INNER JOIN Employee e ON e.EmployeeId = c.supportRepId
    INNER JOIN Invoice i ON i.CustomerId = c.CustomerID
	GROUP BY FullName

18. Which sales agent made the most in sales in 2009? HINT: MAX

	SELECT e.FirstName || " " || e.LastName AS FullName, i.InvoiceDate, sum(i.Total) AS totalSales
    FROM Customer c
    INNER JOIN Employee e ON e.EmployeeId = c.supportRepId
    INNER JOIN Invoice i ON i.CustomerId = c.CustomerID
    WHERE InvoiceDate LIKE '%2009%'
	GROUP BY FullName
	ORDER BY totalSales
	DESC LIMIT 1;

19. Which sales agent made the most in sales over all?

	SELECT e.FirstName || " " || e.LastName AS FullName, i.InvoiceDate, sum(i.Total) AS totalSales
    FROM Customer c
    INNER JOIN Employee e ON e.EmployeeId = c.supportRepId
    INNER JOIN Invoice i ON i.CustomerId = c.CustomerId
	ORDER BY totalSales
    LIMIT 1;

20. Provide a query that shows the # of customers assigned to each sales agent.

	SELECT e.FirstName || " " || e.LastName AS FullName, SUM(c.CustomerId)
    FROM Customer c
    INNER JOIN Employee e ON e.EmployeeId = c.supportRepId
    GROUP BY e.EmployeeId

21. Provide a query that shows the total sales per country. Which country's customers spent the most?

	SELECT i.BillingCountry, i.Total
	FROM Invoice i
	GROUP BY i.BillingCountry
	ORDER BY i.Total
	DESC

22. Provide a query that shows the most purchased track of 2013.

	SELECT SUM(InvoiceLine.Quantity) AS NumberOfTracksSold, Track.Name, Invoice.InvoiceDate AS DateSold
	FROM InvoiceLine
	INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
	INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
	WHERE Invoice.InvoiceDate LIKE '%2013%'
	GROUP BY Track.Name
	ORDER BY NumberOfTracksSold DESC

23. Provide a query that shows the top 5 most purchased tracks over all.
	
	SELECT SUM(InvoiceLine.Quantity) AS NumberOfTracksSold, Track.Name, Invoice.InvoiceDate AS DateSold
	FROM InvoiceLine
	INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
	INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
	GROUP BY Track.Name
	ORDER BY NumberOfTracksSold DESC
	LIMIT 5

24. Provide a query that shows the top 3 best selling artists.
	
	SELECT SUM(InvoiceLine.Quantity) AS NumberOfTracksSold, Track.Name, Invoice.InvoiceDate AS DateSold, Artist.Name AS ArtistName
	FROM InvoiceLine
	INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
	INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
	INNER JOIN Album ON Album.AlbumId = Track.AlbumId
	INNER JOIN Artist ON Artist.ArtistId = Album.ArtistId
	GROUP BY Track.Name
	ORDER BY NumberOfTracksSold DESC
	LIMIT 3

25. sProvide a query that shows the most purchased Media Type.
	
	SELECT SUM(Invoice.Total) AS TotalSales, Track.Name, MediaType.Name AS MediaType
	FROM InvoiceLine
	INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
	INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
	INNER JOIN MediaType ON MediaType.MediaTypeId = Track.MediaTypeId
	GROUP BY Track.Name
	ORDER BY TotalSales DESC
	LIMIT 1