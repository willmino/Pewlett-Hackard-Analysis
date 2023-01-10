# Pewlett-Hackard-Analysis

## Overview

As an assistant to Bobby at Pewlett Hackard, I was tasked with helping him inform upper management of the number of potential employees entering into retirement and the preventative measures to account for the mass turnover. One solution proposed by upper management was to offer a mentorship program for seasoned employees, not yet near retirement, who can train up and coming employees in an effort to keep the company well managed. With the help of Bobby, we used postgres(SQL), pgAdmin 4, and QuickDBD.com, to create a database and run SQL queries to aggregate information from the database and determine employees who are eligibile for the mentorship

### Purpose
We constructed a database from six .csv files and ran SQL queries in pgAdmin (with postgres(SQL)) in order to determine employees who are eligible to enroll in a part time mentorship program.

## Technical Summary

To construct the database we first created tables using SQL queries such as below:

`CREATE TABLE dept_emp (`

&nbsp;&nbsp;&nbsp;&nbsp;`emp_no INT NOT NULL,`

&nbsp;&nbsp;&nbsp;&nbsp;`dept_no VARCHAR(4) NOT NULL,`

&nbsp;&nbsp;&nbsp;&nbsp;`from_date DATE NOT NULL,`

&nbsp;&nbsp;&nbsp;&nbsp;`to_date DATE NOT NULL,`

&nbsp;&nbsp;&nbsp;&nbsp;`FOREIGN KEY (emp_no) REFERENCES employees (emp_no),`

&nbsp;&nbsp;&nbsp;&nbsp;`FOREIGN KEY (dept_no) REFERENCES departments (dept_no),`

&nbsp;&nbsp;&nbsp;&nbsp;`PRIMARY KEY (emp_no, dept_no)`

`);`

This table housed the primary key `emp_no` and `dept_no` which were critical for performing most of the joins in our SQL queries. Foreign keys were established in the table schema as well. For example, `emp_no` is a primary key in the `employees` table, but it functions as a foreign key in the `dept_emp` table. In this way, the `employees` table can reference the `dept_emp` table by using the 
foreign key `emp_no`. This serves as the basis for database construction and allows for the execution of many SQL queries and functions such as JOINS.

After we created all the tables, primary keys, and foreign keys, our schema was completed. Below was the final visualization for the database schema:

![Database Design](https://github.com/willmino/Pewlett-Hackard-Analysis/blob/main/ERDiagram.png)

In order to determine how many employees were near retirement, we first selected all of the employees who were born between 1952 and 1955. We also selected for each employees title. We then ordered the `emp_no` (employee number) in ascending order. This table construction required a `JOIN` between the `employees` and `titles` tables. The SQL query allowing for this table is listed below:

`SELECT e.emp_no, e.first_name, e.last_name, t.title, t.from_date, t.to_date`

`INTO retirement_titles`

`FROM employees as e`

`JOIN titles as t ON e.emp_no = t.emp_no`

`WHERE birth_date BETWEEN '1952-01-01' AND '1955-12-31'`

`ORDER BY emp_no ASC;`

This query produced the following table result. With this information, the data is taking shape and it begins to illustrate the magnitude of the upcoming retirement wave. Although, we can see multiple employees as evidenced by duplicate employee numbers in rows but with different titles. We should clean up this data a little bit by eliminating duplicate employees. This infomration indicates that an employee was at one point promoted and the information of the previous title was retained in the database.

![retirement_titles](https://github.com/willmino/Pewlett-Hackard-Analysis/blob/main/retirement_titles.png)

In order to have the most accurate information for our tables, we limited our table to onclude only the most recent title of each employee. To execute this, we performed another SQL query that delivered a `unique_titles` table. We selected the employee number, first name, last name, and title from the `retirement_titles` table. The `SELECT DISTINCT ON` clause allowed us to select unique titles for each employee. The parameter for employee title selection was later determined in this query by the `ORDER BY` clause. We also limited the table to currently employed workers using the `WHERE` clause set equal to the maximum date `9999-01-01`. Any former employee, with a `to_date` in the past, would be not be included in this table. We ordered the tables by employee number in ascending order to view the lowest employee numbers first. We also ordered this table by the `to_date` in descending order so we could display the most recent date that an employees position began. The SQL query for this table was summarized below:

`SELECT DISTINCT ON (rt.emp_no) rt.emp_no,`

&nbsp;&nbsp;&nbsp;&nbsp;`rt.first_name,`

&nbsp;&nbsp;&nbsp;&nbsp;`rt.last_name, `

&nbsp;&nbsp;&nbsp;&nbsp;`rt.title`

`INTO unique_titles`

`FROM retirement_titles as rt`

`WHERE to_date = '9999-01-01'`

`ORDER BY rt.emp_no ASC, rt.to_date DESC;`

This query yielded the following `unique_titles` table. Now the table only included a single row for each employee with their most recent title. This figure clearly depicts the magnitude of the impending retirement wave at Pewlett-Hackard. We can see over 70,000 employees are nearing retirement.

![unique_titles](https://github.com/willmino/Pewlett-Hackard-Analysis/blob/main/unique_titles_.png)

### Deliverable 1

A very important deliverable in our task was to communicate human resources the total number of employees near retirement listed by their most recent title. This was achieved with a simple SQL query. We selected for the count of the unique titles, and the name of each unique title from the retiring_titles table we previously created. Then, the table was grouped by each unique title and each row was listed in descending order by the count of each title. The SQL query summary was listed below.



`SELECT COUNT(ut.title), ut.title`

`INTO retiring_titles`

`FROM unique_titles as ut`

`GROUP BY ut.title`

`ORDER BY COUNT DESC;`

This SQL query produced the following table result:

### Retiring Titles
![retiring_titles](https://github.com/willmino/Pewlett-Hackard-Analysis/blob/main/retiring_titles.png)

### Deliverable 2

We ultimately wanted to generate a table that would communicate to upper management a list of employees who were eligible to participate in the proposed mentorship program. To deliver this table, we performed a SQL query that selected the employee number, first name, last name, and birth date from the `employees` table. We selected the `to_date` and `from_date` from the `dept_emp` table. Finally, we selected each employee's title from the `titles` table. The `SELECT DISTINCT ON` clause was executed on `emp_no` from the `employees` table in order to select the unique and most recent title of each employee. The `INTO` clause allowed the SQL query to store the selected information into a table. The `FROM` employees statment and the `JOIN` dept_emp, coupled with the `Join` titles, statement allowed us to perform a join to produce an aggregated display of necessary information. The `WHERE` clause selected for currently employed workers and the non-enclosed `AND` with `BETWEEN` clause allowed for more extensive filtering to incorporate employees who starter working in the year 1965. Each row was listed by the acending order of the employee number. The SQL query was summarized below:

`SELECT DISTINCT ON (e.emp_no) e.emp_no, e.first_name, e.last_name, e.birth_date,`

&nbsp;&nbsp;&nbsp;&nbsp;`de.from_date, de.to_date,`

&nbsp;&nbsp;&nbsp;&nbsp;`t.title`

`INTO mentorship_eligibility`

`FROM employees as e`

`JOIN dept_emp as de`

`ON e.emp_no = de.emp_no`

`JOIN titles as t`

`ON e.emp_no = t.emp_no`

`WHERE (de.to_date = '9999-01-01') AND (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')`

`ORDER BY emp_no ASC;`

The SQL query produced the following table preview.


### Mentorship Eligibility
![mentorship_eligibility](https://github.com/willmino/Pewlett-Hackard-Analysis/blob/main/mentorship_eligibility.png)


## Analysis

### Results


1. 72,458 employees are suspected to retire soon. This is a large number of employees and the company should be prepared for the incoming turnover.

2. 1,549 employees are eligible for the mentorship program. If every one of these eligible employees entered into the mentorship program.

3. Senior Engineers and Senior Staff comprise 70.1% of the soon-departing workforce. The company's largest resource for the highest quality training and technical body of knowledge, being these 50000+ senior employees, will be depleted if no action is taken.

4. When viewing the additional `mentors_by_title` table, only 1.19% of Senior Engineers would be retained if all of them entered into the mentorship program. This is the lowest retention figure for any of the groups.

![mentors_by_title](https://github.com/willmino/Pewlett-Hackard-Analysis/blob/main/mentors_bytitle.png)

## Summary

- According to the `unique_titles` and `retiring_titles` tables, 72,458 employees will need to be hired ahead of the upcoming wave of retiring employees. Again, over two thirds of these roles are Senior Engineers and Senior Staff.


- 




then provide two additional queries or tables that may provide more insight into the upcoming "silver tsunami."