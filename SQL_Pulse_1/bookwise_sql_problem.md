# BookWise Publishing Analytics - SQL Challenge

## 📚 Business Case Scenario

You work for **BookWise Publishing**, a digital publishing house that manages book publications for multiple authors across various genres. The company operates as a platform where authors can publish their books and earn royalties based on monthly sales performance. You have been given two datasets: one containing information about the books published through your platform, and another containing information about the authors who have written those books.

Your task is to create a comprehensive analytics report that summarizes book performance and calculates the total monthly royalties earned by each author from their published works. The platform operates on a royalty-based model where authors receive monthly payments based on the sales performance of their individual book titles.

The system needs to handle scenarios where authors may have multiple books across different genres (fiction, non-fiction, mystery, romance, etc.), and authors may specialize in specific categories or diversify across multiple genres. Some authors in the system may not have any published books yet (they might be registered but still in the writing process), which is perfectly acceptable for business operations. However, the data integrity standards require that each book entry in the system must be unique - duplicate book entries are strictly prohibited to maintain accurate royalty calculations.

---

<img src="./assets/pulse1_image.png" alt="BookWise Publishing!" width="600"/>

## 🗄️ Table Structure

### **Table 1: books**
*Contains information about the books published through BookWise Publishing platform*

| Column Name   | Data Type | Description                                      |
|---------------|-----------|--------------------------------------------------|
| book_id       | integer   | Unique ID of the published book                  |
| author_id     | integer   | ID of the author who wrote the book              |
| genre         | string    | Genre/category of the book                       |
| monthly_sales | float     | Monthly royalty earnings from the book           |
| page_count    | integer   | Total number of pages in the book                |
| language      | string    | Primary language in which the book is written    |

### **Table 2: authors**
*Contains information about the authors who have published books through BookWise Publishing*

| Column Name | Data Type | Description                      |
|-------------|-----------|----------------------------------|
| author_id   | integer   | Unique ID of the author          |
| first_name  | string    | First name of the author         |
| last_name   | string    | Last name of the author          |
| email       | string    | Email address of the author      |
| phone       | string    | Phone number of the author       |

---

## 📋 Requirements

### **🎯 Primary Requirement**
Create a SQL query that generates a comprehensive royalty report for BookWise Publishing, showing each author's total monthly earnings from their published books.

**Required Output Parameters:**
- `author_id`: Unique identifier of the author
- `author_name`: Full name of the author (concatenated first_name and last_name)
- `total_royalty`: Sum of monthly_sales from all books written by the author

### **🏆 Bonus Challenge (Medium-Intermediate)**
Additionally, create an advanced analytics report that provides genre-wise performance insights for each author, including their ranking within each genre they write in.

**Required Output Parameters:**
- `author_id`: Unique identifier of the author
- `author_name`: Full name of the author (concatenated first_name and last_name)  
- `genre`: Book genre/category
- `genre_book_count`: Number of books the author has published in this specific genre
- `genre_royalty`: Total monthly royalty earnings from books in this specific genre
- `avg_royalty_per_book`: Average royalty per book in this genre for the author
- `genre_rank_by_author`: Rank of this genre for the author based on royalty earnings (1 being highest earning genre)
- `is_primary_genre`: Flag indicating if this is the author's most profitable genre (1 for yes, 0 for no)

---

## 💾 Database Setup

### **DDL Statements (Create Tables)**

```sql
-- Create books table
CREATE TABLE books (
    book_id INTEGER PRIMARY KEY,
    author_id INTEGER NOT NULL,
    genre VARCHAR(50) NOT NULL,
    monthly_sales DECIMAL(10,2) NOT NULL,
    page_count INTEGER NOT NULL,
    language VARCHAR(30) NOT NULL
);

-- Create authors table
CREATE TABLE authors (
    author_id INTEGER PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    phone VARCHAR(15) NOT NULL
);
```

### **DML Statements (Insert Sample Data)**

```sql
-- Insert data into authors table
INSERT INTO authors (author_id, first_name, last_name, email, phone) VALUES
(201, 'Emma', 'Thompson', 'emma.thompson@email.com', '555-101-2001'),
(202, 'Michael', 'Rodriguez', 'michael.rod@email.com', '555-102-2002'),
(203, 'Sarah', 'Chen', 'sarah.chen@email.com', '555-103-2003'),
(204, 'David', 'Anderson', 'david.anderson@email.com', '555-104-2004'),
(205, 'Maria', 'Garcia', 'maria.garcia@email.com', '555-105-2005'),
(206, 'James', 'Wilson', 'james.wilson@email.com', '555-106-2006'),
(207, 'Lisa', 'Kumar', 'lisa.kumar@email.com', '555-107-2007'),
(208, 'Robert', 'Miller', 'robert.miller@email.com', '555-108-2008'),
(209, 'Jennifer', 'Taylor', 'jennifer.taylor@email.com', '555-109-2009'),
(210, 'Ahmed', 'Hassan', 'ahmed.hassan@email.com', '555-110-2010');

-- Insert data into books table
INSERT INTO books (book_id, author_id, genre, monthly_sales, page_count, language) VALUES
(1001, 201, 'Fiction', 2500.00, 320, 'English'),
(1002, 201, 'Mystery', 1800.00, 280, 'English'),
(1003, 201, 'Fiction', 3200.00, 450, 'English'),
(1004, 202, 'Romance', 2200.00, 380, 'English'),
(1005, 202, 'Fiction', 1500.00, 240, 'English'),
(1006, 203, 'Non-Fiction', 4500.00, 520, 'English'),
(1007, 203, 'Biography', 3800.00, 680, 'English'),
(1008, 203, 'Non-Fiction', 2900.00, 340, 'English'),
(1009, 204, 'Fantasy', 3500.00, 750, 'English'),
(1010, 204, 'Fantasy', 2800.00, 640, 'English'),
(1011, 204, 'Science', 1900.00, 420, 'English'),
(1012, 205, 'Romance', 2600.00, 360, 'Spanish'),
(1013, 205, 'Drama', 1400.00, 290, 'Spanish'),
(1014, 206, 'Thriller', 3100.00, 480, 'French'),
(1015, 207, 'Poetry', 800.00, 120, 'English'),
(1016, 207, 'Fiction', 2100.00, 310, 'English'),
(1017, 208, 'Business', 5200.00, 580, 'English'),
(1018, 208, 'Self-Help', 4100.00, 440, 'English'),
(1019, 208, 'Business', 3600.00, 620, 'English');
```

> **📝 Note:** Authors with IDs 209 and 210 have no published books yet, demonstrating the scenario where some authors may not have active publications.

---

## 📊 Expected Output

### **Bonus Problem Expected Results**

| author_id | author_name      | genre       | genre_book_count | genre_royalty | avg_royalty_per_book | genre_rank_by_author | is_primary_genre |
|-----------|------------------|-------------|------------------|---------------|---------------------|---------------------|------------------|
| 201       | Emma Thompson    | Fiction     | 2                | 5700.00       | 2850.000000         | 1                   | 1                |
| 201       | Emma Thompson    | Mystery     | 1                | 1800.00       | 1800.000000         | 2                   | 0                |
| 202       | Michael Rodriguez| Romance     | 1                | 2200.00       | 2200.000000         | 1                   | 1                |
| 202       | Michael Rodriguez| Fiction     | 1                | 1500.00       | 1500.000000         | 2                   | 0                |
| 203       | Sarah Chen       | Non-Fiction | 2                | 7400.00       | 3700.000000         | 1                   | 1                |
| 203       | Sarah Chen       | Biography   | 1                | 3800.00       | 3800.000000         | 2                   | 0                |
| 204       | David Anderson   | Fantasy     | 2                | 6300.00       | 3150.000000         | 1                   | 1                |
| 204       | David Anderson   | Science     | 1                | 1900.00       | 1900.000000         | 2                   | 0                |
| 205       | Maria Garcia     | Romance     | 1                | 2600.00       | 2600.000000         | 1                   | 1                |
| 205       | Maria Garcia     | Drama       | 1                | 1400.00       | 1400.000000         | 2                   | 0                |
| 206       | James Wilson     | Thriller    | 1                | 3100.00       | 3100.000000         | 1                   | 1                |
| 207       | Lisa Kumar       | Fiction     | 1                | 2100.00       | 2100.000000         | 1                   | 1                |
| 207       | Lisa Kumar       | Poetry      | 1                | 800.00        | 800.000000          | 2                   | 0                |
| 208       | Robert Miller    | Business    | 2                | 8800.00       | 4400.000000         | 1                   | 1                |
| 208       | Robert Miller    | Self-Help   | 1                | 4100.00       | 4100.000000         | 2                   | 0                |

---

## 💡 Output Explanation

### **Example 1: Emma Thompson (Author ID: 201)**

**Raw Data Analysis:**
- Fiction books: Book ID 1001 ($2,500) + Book ID 1003 ($3,200) = 2 books
- Mystery books: Book ID 1002 ($1,800) = 1 book

**Calculated Results:**
```
201 | Emma Thompson | Fiction | 2 | 5700.00 | 2850.000000 | 1 | 1
201 | Emma Thompson | Mystery | 1 | 1800.00 | 1800.000000 | 2 | 0
```

**Step-by-step Breakdown:**
- **genre_book_count**: Fiction = 2 books, Mystery = 1 book
- **genre_royalty**: Fiction = $2,500 + $3,200 = $5,700, Mystery = $1,800
- **avg_royalty_per_book**: Fiction = $5,700 ÷ 2 = $2,850, Mystery = $1,800 ÷ 1 = $1,800
- **genre_rank_by_author**: Fiction ranks #1 ($5,700 > $1,800), Mystery ranks #2
- **is_primary_genre**: Fiction = 1 (highest earning), Mystery = 0 (not highest)

### **Example 2: Robert Miller (Author ID: 208)**

**Raw Data Analysis:**
- Business books: Book ID 1017 ($5,200) + Book ID 1019 ($3,600) = 2 books
- Self-Help books: Book ID 1018 ($4,100) = 1 book

**Calculated Results:**
```
208 | Robert Miller | Business  | 2 | 8800.00 | 4400.000000 | 1 | 1
208 | Robert Miller | Self-Help | 1 | 4100.00 | 4100.000000 | 2 | 0
```

**Step-by-step Breakdown:**
- **genre_book_count**: Business = 2 books, Self-Help = 1 book
- **genre_royalty**: Business = $5,200 + $3,600 = $8,800, Self-Help = $4,100
- **avg_royalty_per_book**: Business = $8,800 ÷ 2 = $4,400, Self-Help = $4,100 ÷ 1 = $4,100
- **genre_rank_by_author**: Business ranks #1 ($8,800 > $4,100), Self-Help ranks #2
- **is_primary_genre**: Business = 1 (highest earning genre for this author), Self-Help = 0

---

## 🔍 Key Business Insights

- **Multi-row per Author**: Each row represents one author-genre combination
- **Author Specialization**: Authors appear multiple times (once per genre they write in)
- **Individual Rankings**: Rankings are calculated **within each author's portfolio** (not globally)
- **Primary Genre Identification**: The `is_primary_genre` flag identifies each author's most profitable genre specialty
- **Data Completeness**: Only authors with published books appear in the results

---

## 🎯 Success Criteria

✅ **Primary Query**: Should return total royalties per author  
✅ **Bonus Query**: Should provide detailed genre-wise analytics  
✅ **Data Accuracy**: No duplicate book entries in calculations  
✅ **Business Logic**: Proper ranking and flagging of primary genres  
✅ **SQL Optimization**: Efficient use of JOINs, GROUP BY, and window functions

---

## Solutions

For the solutions to these problems, please refer to the [BookWise Publishing - Solutions](sql_solutions) file.