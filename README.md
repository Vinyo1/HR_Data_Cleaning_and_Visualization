# Analysis and Visualization of HR Dataset on Employees

## Project Overview
This project aimed to gain insights into various aspects of employee data within a company. By leveraging data analytics and visualization techniques, the project delved into understanding patterns, trends and relationships within the dataset to inform HR strategies and decision-making processes.

## Data Sources:
The primary date used for this is the Human_Resources.csv file

## Tools:
- SQL for Data Cleaning and Data Analysis - [Download SQL Workbench](https://www.mysql.com/products/workbench/)
- Power BI for Data Visualisation - [Download Microsoft PowerBI](https://www.microsoft.com/en-us/download/details.aspx?id=58494)

## Data Cleaning/Preparation
- Data loading and inspection
- Handling missing values
- Data cleaning and formatting

### Codes used during data cleaning and formatting
### STEP 1 -- Change ï»¿id column to employee id and allow NULL values

	ALTER TABLE hr
	CHANGE COLUMN ï»¿id emp_id VARCHAR(20) NULL;

#### -- Check data types for all columns
	DESCRIBE hr;


### STEP 2 -- Change birthdate from text to date

	SELECT birthdate
	FROM hr;

	SET sql_safe_updates = 0;
	UPDATE hr
	SET birthdate = CASE
	    WHEN birthdate LIKE '%/%' THEN date_format(str_to_date(birthdate, '%m/%d/%Y'), '%Y-%m-%d')
	    WHEN birthdate LIKE '%-%' THEN date_format(str_to_date(birthdate, '%m-%d-%Y'), '%Y-%m-%d')
	    ELSE NULL
	END;
#### -- Check birthdate column
	SELECT birthdate
	FROM hr;

### STEP 3 -- Change birthdate data type from text to date
	ALTER TABLE hr
	MODIFY COLUMN birthdate DATE;
 
#### -- Check if birthdate data types has changed	
	DESCRIBE hr;

### STEP 4 -- UPDATE HIRE_DATE
	UPDATE hr 
	SET hire_date = CASE
	    WHEN hire_date LIKE '%/%' THEN date_format(str_to_date(hire_date, '%m/%d/%Y'), '%Y-%m-%d')
	    WHEN hire_date LIKE '%-%' THEN date_format(str_to_date(hire_date, '%m-%d-%Y'), '%Y-%m-%d')
	    ELSE NULL
	END;
 
#### -- Check hire_date column
	SELECT hire_date FROM hr;


## Exploratory Data Analysis
EDA involved using the Human Resources data to answer the follwing questions:
1. WHAT IS THE GENDER BREAKDOWN OF EMPLOYEES IN THE COMPANY?
2. WHAT IS THE RACE/ETHNICITY BREAKDOWN IN THE COMPANY?
3. WHAT IS THE AGE DISTRIBUTION OF EMPLOYEES IN THE COMPANY?
4. WHAT IS THE GENDER DISTRIBUTION AMONG THE AGE GROUPS
5. HOW MANY EMPLOYEES WORK AT THE HEADQUARTERS VERSUS REMOTE?
6. WHAT IS THE AVERAGE LENGTH OF EMPLOYMENT FOR EMPLOYEES WHO HAVE BEEN TERMINATED?
7. WHAT IS THE DISTRIBUTION OF JOB TITLES IN THE COMPANY?
8. WHICH DEPARTMENT HAS THE HIGHEST TURNOVER RATE(the rate at which employees leave a company?
9. WHAT IS THE DISTRIBUTION OF EMPLOYEES ACROSS LOCATIONS BY  STATE?
10. WHAT IS THE DISTRIBUTION OF EMPLOYEES ACROSS LOCATIONS BY LOCATION_CITY?
11. HOW HAS COMAPNY'S EMPLOYEES COUNT CHANGED OVER TIME BASED ON HIRE AND TERM DATES?
12. WHAT IS THE TENURE DISTRIBUTION FOR EACH DEPARTMENT?

## Data Analysis:
#### Some codes used during the data analysis stage:

	SELECT
		ROUND(AVG(datediff(termdate, hire_date))/365,0) AS avg_length_employment
	FROM hr
	WHERE age >= 18 AND termdate <= curdate() AND termdate IS NOT NULL;
 ##
	SELECT department, total_count, terminated_count, terminated_count/total_count AS termination_rate
	FROM (
		SELECT department, COUNT(*) AS total_count,
	    SUM(CASE WHEN termdate IS NOT NULL AND termdate <= curdate() THEN 1 ELSE 0 END) AS terminated_count
	    FROM hr
	    WHERE age >= 18
	    GROUP BY department) AS term_subquery
	ORDER BY termination_rate DESC;
