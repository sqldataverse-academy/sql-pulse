# BookWise Publishing - SQL Solutions

## 🎯 Primary Requirement Solution

### **Basic Royalty Report**
*Shows total earnings per author*

```sql
-- Basic royalty report showing total earnings per author
SELECT 
    a.author_id,
    CONCAT(a.first_name, ' ', a.last_name) AS author_name,
    SUM(b.monthly_sales) AS total_royalty
FROM authors a
INNER JOIN books b ON a.author_id = b.author_id
GROUP BY a.author_id, a.first_name, a.last_name
ORDER BY total_royalty DESC;
```


---

## 🏆 Bonus Challenge Solution

### **Advanced Genre-wise Analytics Report**
*Provides detailed genre performance insights with rankings*

```sql
-- Advanced analytics report with genre-wise performance insights
WITH author_genre_stats AS (
    SELECT 
        a.author_id,
        CONCAT(a.first_name, ' ', a.last_name) AS author_name,
        b.genre,
        COUNT(b.book_id) AS genre_book_count,
        SUM(b.monthly_sales) AS genre_royalty,
        ROUND(AVG(b.monthly_sales), 2) AS avg_royalty_per_book
    FROM authors a
    INNER JOIN books b ON a.author_id = b.author_id
    GROUP BY a.author_id, a.first_name, a.last_name, b.genre
),
author_genre_ranked AS (
    SELECT 
        *,
        ROW_NUMBER() OVER (PARTITION BY author_id ORDER BY genre_royalty DESC) AS genre_rank_by_author
    FROM author_genre_stats
)
SELECT 
    author_id,
    author_name,
    genre,
    genre_book_count,
    genre_royalty,
    avg_royalty_per_book,
    genre_rank_by_author,
    CASE 
        WHEN genre_rank_by_author = 1 THEN 1 
        ELSE 0 
    END AS is_primary_genre
FROM author_genre_ranked
ORDER BY author_id, genre_rank_by_author;
```

---

## 🔄 Alternative Solution Approaches

### **Option 1: Using Window Functions (Single Query)**
```sql
-- More compact version using window functions directly
SELECT 
    a.author_id,
    CONCAT(a.first_name, ' ', a.last_name) AS author_name,
    b.genre,
    COUNT(b.book_id) AS genre_book_count,
    SUM(b.monthly_sales) AS genre_royalty,
    ROUND(AVG(b.monthly_sales), 2) AS avg_royalty_per_book,
    RANK() OVER (PARTITION BY a.author_id ORDER BY SUM(b.monthly_sales) DESC) AS genre_rank_by_author,
    CASE 
        WHEN RANK() OVER (PARTITION BY a.author_id ORDER BY SUM(b.monthly_sales) DESC) = 1 THEN 1 
        ELSE 0 
    END AS is_primary_genre
FROM authors a
INNER JOIN books b ON a.author_id = b.author_id
GROUP BY a.author_id, a.first_name, a.last_name, b.genre
ORDER BY a.author_id, genre_royalty DESC;
```

### **Option 2: Using DENSE_RANK for Tie Handling**
```sql
-- Alternative using DENSE_RANK to handle potential ties in genre earnings
WITH genre_analytics AS (
    SELECT 
        a.author_id,
        CONCAT(a.first_name, ' ', a.last_name) AS author_name,
        b.genre,
        COUNT(b.book_id) AS genre_book_count,
        SUM(b.monthly_sales) AS genre_royalty,
        ROUND(AVG(b.monthly_sales), 2) AS avg_royalty_per_book,
        DENSE_RANK() OVER (PARTITION BY a.author_id ORDER BY SUM(b.monthly_sales) DESC) AS genre_rank_by_author
    FROM authors a
    INNER JOIN books b ON a.author_id = b.author_id
    GROUP BY a.author_id, a.first_name, a.last_name, b.genre
)
SELECT 
    *,
    CASE 
        WHEN genre_rank_by_author = 1 THEN 1 
        ELSE 0 
    END AS is_primary_genre
FROM genre_analytics
ORDER BY author_id, genre_rank_by_author;
```

---

## 🔧 Key SQL Techniques Used

### **Core Concepts:**
- **INNER JOIN**: Links authors and books tables
- **GROUP BY**: Aggregates data by author and genre
- **Aggregate Functions**: SUM(), COUNT(), AVG() for calculations
- **String Concatenation**: CONCAT() for author names
- **Window Functions**: ROW_NUMBER(), RANK() for rankings
- **PARTITION BY**: Creates separate ranking groups per author
- **CASE Statements**: Conditional logic for flags
- **CTEs**: Common Table Expressions for complex queries

### **SQL Server Specific Features:**
- `CONCAT()` function for string concatenation
- `DECIMAL(10,2)` for precise financial calculations
- `ROW_NUMBER()` for unique sequential rankings
- `RANK()` for handling ties in rankings
- `ROUND()` function for decimal precision

### **Performance Considerations:**
- Efficient JOIN operations using indexed columns
- Proper GROUP BY to avoid unnecessary aggregations
- Window functions partitioned appropriately
- Logical ordering for optimal execution plans

---