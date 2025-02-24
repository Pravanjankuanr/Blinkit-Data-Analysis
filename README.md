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

```sql
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
## Conclusion

This project demonstrated how data analysis can provide valuable insights into Blinkit's performance, helping optimize sales, inventory, and customer service. It highlighted the platform's potential for growth and improved operational efficiency.
