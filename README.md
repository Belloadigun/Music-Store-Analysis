# 🎵Music-Store-Analysis
This project involves querying a relational database to uncover insights into sales trends, customer behaviour, and product performance. It demonstrates the use of SQL for data extraction, transformation, and reporting.
![scott-kelley--vNLZFT8Kcg-unsplash](https://github.com/user-attachments/assets/dfd387cf-34aa-4c7f-84f1-077370ec1bfd)

# 🧠Introduction
This project is a hands-on analytical exploration of a fictional music store database using Structured Query Language (SQL). Through this analysis, we aim to uncover meaningful insights that reflect real-world business scenarios—ranging from customer behavior to sales performance, artist impact, and genre preferences.
By developing targeted SQL queries, this project demonstrates how relational databases can be used to drive business decisions, personalize marketing efforts, and enhance customer experience in a music retail environment.

# 🛢️Data Source
This data was gotten from :
https://drive.google.com/file/d/1eOw8GZ7-T25-8OWQf5DNroI8m6kzyq3G/view

# Project Structure

1. Database Setup
  - Database Creation: The project starts by creating a database named `music`.
  - Restoration of File: File by name `Music_Store_database` was restored. The file contained several tables like album, artist, customer, employee, genre, invoice e.t.c.

2. Data Analysis & Findings

The following SQL Queries were developed to answer specific business questions:
1. **Who is the most senior employee based on job title?**
   ```sql
   SELECT * FROM employee
   ORDER BY levels DESC
   LIMIT 1;
   ```
   
2. **Which country has the highest number of invoices?**
   ```sql
   SELECT COUNT (*) AS c, billing_country
   FROM invoice
   GROUP BY billing_country
   ORDER BY c DESC
   LIMIT 1;
   ```

3. **What are the top 3 highest invoice values ever recorded?**
   ```sql
   SELECT total FROM Invoice
   ORDER BY total DESC
   LIMIT 3;
   ```

 4. **Which city has generated the most revenue—and could therefore host a promotional music festival?**
    ```sql
    SELECT SUM(total) as invoice_total, billing_city
    FROM invoice
    GROUP BY billing_city
    ORDER BY invoice_total DESC;
    ```

  5. **Who is the best customer, based on total money spent?**
     ```sql
     SELECT customer.customer_id, customer.first_name, customer.last_name, SUM(invoice.total) as total
     FROM customer
     JOIN invoice ON customer.customer_id = invoice.customer_id
     GROUP BY customer.customer_id
     ORDER BY total DESC
     LIMIT 1;
     ```

   6. **Who are the Rock music listeners, and what are their names and emails?**
      ```sql
      SELECT DISTINCT email, first_name, last_name
      FROM customer
      JOIN invoice ON customer.customer_id = invoice.customer_id
      JOIN invoice_line ON invoice.invoice_id = invoice_line.invoice_id
      WHERE track_id IN(
	      SELECT track_id FROM track 
	      JOIN genre ON track.genre_id = genre.genre_id
	      WHERE genre.name LIKE 'Rock'
      )
      ORDER BY email;
      ```

   7. **Which artists have produced the most Rock tracks, and who are the top 10?**
         ```sql
         SELECT artist.artist_id, artist.name, COUNT(artist.artist_id) AS number_of_songs
         FROM track
         JOIN album ON album.album_id = track.album_id
         JOIN artist ON artist.artist_id = album.artist_id
         JOIN genre ON genre.genre_id = track.genre_id
         WHERE genre.name LIKE 'Rock'
         GROUP BY artist.artist_id
         ORDER BY number_of_songs DESC
         LIMIT 10;
         ```

   8. **Which tracks have a song length above the average, and how long are they?**
      ```sql
      SELECT name, milliseconds
      FROM track
      WHERE milliseconds > (
	      SELECT AVG(milliseconds) AS avg_track_length
	      FROM track)
      ORDER BY milliseconds DESC;
      ```

      
