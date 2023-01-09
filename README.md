# Pewlett-Hackard-Analysis

## Overview

As an assistant to Bobby at Pewlett Hackard, I was tasked with helping him inform upper management of the number of potential employees entering into retirement and the measures necessary to prevent mass turnover. One solution proposed by upper management was to offer a mentorship program for retiring employees who can train up and coming managers in an effort to keep the company well managed. The near-retirement employees would be able to work part time as they instruct soon-to-be managers on how to properly conduct their activities. With the help of Bobby, we used postgres(SQL), pgAdmin 4, and QuickDBD.com, to create a database schema and run SQL queries to aggregate information within the database.

### Purpose
We constructed a database from six .csv files and ran SQL queries in pgAdmin (with postgres(SQL)) in order to determine near-retirement employees who are eligible to enroll in a part time mentorship program.

## Analysis

To construct the database we first created tables using SQL statments such as below:

`CREATE TABLE dept_emp (`

&nbsp;&nbsp;&nbsp;&nbsp;`emp_no INT NOT NULL,`

&nbsp;&nbsp;&nbsp;&nbsp;`dept_no VARCHAR(4) NOT NULL,`

&nbsp;&nbsp;&nbsp;&nbsp;`from_date DATE NOT NULL,`

&nbsp;&nbsp;&nbsp;&nbsp;`to_date DATE NOT NULL,`

&nbsp;&nbsp;&nbsp;&nbsp;`FOREIGN KEY (emp_no) REFERENCES employees (emp_no),`

&nbsp;&nbsp;&nbsp;&nbsp;`FOREIGN KEY (dept_no) REFERENCES departments (dept_no),`

&nbsp;&nbsp;&nbsp;&nbsp;`PRIMARY KEY (emp_no, dept_no)`
`);`

This table housed the primary key `emp_no` and `dept_no` which were critical for performing most of the joins in our SQL queries. Foreign keys were established in the table schema. For example, `emp_no` is a primary key in the `employees` table, but it functions as a foreign key in the `dept_emp` table. In this way, the `employees' table can reference the `dept_emp` table by using the 
foreign key `emp_no`. This serves as the basis for database construction and allows for the execution of many SQL queries.

After we created all the tables, primary, and foreign keys, our schema was completed. Below was the final visualization for the database schema:

![]()

## Results

Provide a bulleted list with four major points from the two analysis deliverables. Use images as support where needed.

## Summary


How many roles will need to be filled as the "silver tsunami" begins to make an impact?
Are there enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees?

then provide two additional queries or tables that may provide more insight into the upcoming "silver tsunami."