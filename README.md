# Blinkit Data Analysis using Power BI

## Project Overview

**Project Title**: Blinkit Data Analysis  
**Level**: Intermediate

This project showcases Blinkit Data Analysis in Power BI, including data import, cleaning, measure creation, and interactive dashboards. It also highlights Additional features like RSL and scheduled refresh, demonstrating skills in data transformation and business problem-solving.

![Blinkit Dash](Dashboard.jpg)

## Objectives

1. **Importing Data into Power BI**: Import Blinkit data for analysis in Power BI.
2. **Power Query Data Transformation**: Clean and transform data using Power Query to ensure accuracy and consistency.
3. **Addressing Business Challenges and Problem Statements**: Analyze and interpret the business statement to define key challenges and objectives.
4. **Create Measures**: .
5. **Design and deploy interactive Cards and Charts**: .
6. **Additional Features in Power BI**: Explore and understand advanced features in Power BI.

## Importing Data Into Power BI

### 1. Excel Data Import: We will import data into Power BI via Excel file upload for this project.
![Import Data](Excel-Data-Import.png)
### 2. Alternative Data Import Methods: Power BI supports various data import methods, including databases, files, cloud storage, web pages, etc. 
![Import Data](Alternative-Import-Methods.png)

## Power Query Data Transformation
### 1. Uses:
Power Query is primarily used to transform and refine data from various sources, enabling users to clean, merge, and shape data into a usable format for analysis and visualization in Power BI.

### 2. Data Cleaning
![Import Data](Alternative-Import-Methods.png)


## Addressing Business Challenges and Problem Statements

### 1. KPI's Requirements
![KPI](KPI-Requirements.png)

### 2. Charts Requirements    
![Chart](Chart-Requirements.png)

## Create Measures    
**Requires four measures: Total Sales, Average Sales, Number of Items, and Average Ratings.**

```
-- Create a Measure for "Total Sales"
Total Sales = SUM('BlinkIT Grocery Data'[Sales])    

-- Create a Measure for "Average Sales"
Average Sales = AVERAGE('BlinkIT Grocery Data'[Sales])       

-- Create a Measure for "Total Items"
Number of Items = COUNTROWS('BlinkIT Grocery Data')    

-- Create a Measure for "Average Ratings"
Average Ratings = COUNTROWS('BlinkIT Grocery Data')             
```
## Design and deploy interactive Cards and Charts

### 1. Cards
![Measures](Measures.jpg)
### 2. Charts
**Total sales by fat content**

**Total sales by item type**

**Fat content by outlet for total sales**

**Total Sales by Fat Content**

**Total Sales by Fat Content**

## Additional Features in Power BI

**Task 01: RSL(Row-level Security)**  
Row-level security (RLS) in Power BI limits data access for users based on defined filters, ensuring they only see the data they are authorized to view.     


**Task 14: Scheduled Refresh**  
Scheduled refresh in Power BI automatically updates your data at specified intervals, ensuring reports and dashboards display the most up-to-date information without manual intervention.    



**Task 15: Branch Performance Report**  
Create a query that generates a performance report for each branch, showing the number of books issued, the number of books returned, and the total revenue generated from book rentals.

```sql
CREATE TABLE branch_reports
AS
SELECT 
    b.branch_id,
    b.manager_id,
    COUNT(ist.issued_id) as number_book_issued,
    COUNT(rs.return_id) as number_of_book_return,
    SUM(bk.rental_price) as total_revenue
FROM issued_status as ist
JOIN 
employees as e
ON e.emp_id = ist.issued_emp_id
JOIN
branch as b
ON e.branch_id = b.branch_id
LEFT JOIN
return_status as rs
ON rs.issued_id = ist.issued_id
JOIN 
books as bk
ON ist.issued_book_isbn = bk.isbn
GROUP BY 1, 2;

SELECT * FROM branch_reports;
```

**Task 16: CTAS: Create a Table of Active Members**  
Use the CREATE TABLE AS (CTAS) statement to create a new table active_members containing members who have issued at least one book in the last 2 months.

```sql

CREATE TABLE active_members
AS
SELECT * FROM members
WHERE member_id IN (SELECT 
                        DISTINCT issued_member_id   
                    FROM issued_status
                    WHERE 
                        issued_date >= CURRENT_DATE - INTERVAL '2 month'
                    )
;

SELECT * FROM active_members;

```


**Task 17: Find Employees with the Most Book Issues Processed**  
Write a query to find the top 3 employees who have processed the most book issues. Display the employee name, number of books processed, and their branch.

```sql
SELECT 
    e.emp_name,
    b.*,
    COUNT(ist.issued_id) as no_book_issued
FROM issued_status as ist
JOIN
employees as e
ON e.emp_id = ist.issued_emp_id
JOIN
branch as b
ON e.branch_id = b.branch_id
GROUP BY 1, 2
```

**Task 18: Identify Members Issuing High-Risk Books**  
Write a query to identify members who have issued books more than twice with the status "damaged" in the books table. Display the member name, book title, and the number of times they've issued damaged books.    


**Task 19: Stored Procedure**
Objective:
Create a stored procedure to manage the status of books in a library system.
Description:
Write a stored procedure that updates the status of a book in the library based on its issuance. The procedure should function as follows:
The stored procedure should take the book_id as an input parameter.
The procedure should first check if the book is available (status = 'yes').
If the book is available, it should be issued, and the status in the books table should be updated to 'no'.
If the book is not available (status = 'no'), the procedure should return an error message indicating that the book is currently not available.

```sql

CREATE OR REPLACE PROCEDURE issue_book(p_issued_id VARCHAR(10), p_issued_member_id VARCHAR(30), p_issued_book_isbn VARCHAR(30), p_issued_emp_id VARCHAR(10))
LANGUAGE plpgsql
AS $$

DECLARE
-- all the variabable
    v_status VARCHAR(10);

BEGIN
-- all the code
    -- checking if book is available 'yes'
    SELECT 
        status 
        INTO
        v_status
    FROM books
    WHERE isbn = p_issued_book_isbn;

    IF v_status = 'yes' THEN

        INSERT INTO issued_status(issued_id, issued_member_id, issued_date, issued_book_isbn, issued_emp_id)
        VALUES
        (p_issued_id, p_issued_member_id, CURRENT_DATE, p_issued_book_isbn, p_issued_emp_id);

        UPDATE books
            SET status = 'no'
        WHERE isbn = p_issued_book_isbn;

        RAISE NOTICE 'Book records added successfully for book isbn : %', p_issued_book_isbn;


    ELSE
        RAISE NOTICE 'Sorry to inform you the book you have requested is unavailable book_isbn: %', p_issued_book_isbn;
    END IF;
END;
$$

-- Testing The function
SELECT * FROM books;
-- "978-0-553-29698-2" -- yes
-- "978-0-375-41398-8" -- no
SELECT * FROM issued_status;

CALL issue_book('IS155', 'C108', '978-0-553-29698-2', 'E104');
CALL issue_book('IS156', 'C108', '978-0-375-41398-8', 'E104');

SELECT * FROM books
WHERE isbn = '978-0-375-41398-8'

```



**Task 20: Create Table As Select (CTAS)**
Objective: Create a CTAS (Create Table As Select) query to identify overdue books and calculate fines.

Description: Write a CTAS query to create a new table that lists each member and the books they have issued but not returned within 30 days. The table should include:
    The number of overdue books.
    The total fines, with each day's fine calculated at $0.50.
    The number of books issued by each member.
    The resulting table should show:
    Member ID
    Number of overdue books
    Total fines



## Reports

- **Database Schema**: Detailed table structures and relationships.
- **Data Analysis**: Insights into book categories, employee salaries, member registration trends, and issued books.
- **Summary Reports**: Aggregated data on high-demand books and employee performance.

## Conclusion

This project demonstrates the application of SQL skills in creating and managing a library management system. It includes database setup, data manipulation, and advanced querying, providing a solid data management and analysis foundation.
