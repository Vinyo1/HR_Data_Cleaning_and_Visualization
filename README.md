# Analysis and Visualization of HR Dataset on Employees


## Table of Content
- [Project Overview](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#project-overview)
- [Data Sources](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#data-sources)
- [Tools](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#tools)
- [Data Cleaning/preparation](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#data-cleaningpreparation)
  - [Codes used during data cleaning and formatting](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#codes-used-during-data-cleaning-and-formatting)
- [Exploratory Data Analysis](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#exploratory-data-analysis)
- [Data Analysis](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#data-analysis)
- [Findings](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#findings)
- [Recommendations](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#recommendations)
- [Limitations](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#limitations)
- [References](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/README.md#references)


  
## Project Overview
This project aimed to gain insights into various aspects of employee data within a company. By leveraging data analytics and visualization techniques, the project delved into understanding patterns, trends and relationships within the dataset to inform HR strategies and decision-making processes.

![HR Report-2](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/assets/136182841/f658cbb2-78af-4d97-bcc2-8c74ecc77dfe)


## Data Sources:
The primary data used for this is the [Human_Resources.csv](https://github.com/Vinyo1/HR_Data_Cleaning_and_Visualization/blob/main/Human_Resources.csv) file

## Tools:
- SQL for Data Cleaning and Data Analysis - [Download MySQL Workbench](https://dev.mysql.com/downloads/mysql/)
- Power BI for Data Visualisation - [Download Microsoft PowerBI](https://www.microsoft.com/en-us/download/details.aspx?id=58494)

## Data Cleaning/Preparation
- Data loading and inspection
- Handling missing values
- Data cleaning and formatting

### Codes used during data cleaning and formatting
### STEP 1 -- Change ï»¿id column to employee id and allow NULL values
```sql
	ALTER TABLE hr
	CHANGE COLUMN ï»¿id emp_id VARCHAR(20) NULL;
```

### STEP 2 -- Change birthdate from text to date
```sql

	SET sql_safe_updates = 0;
	UPDATE hr
	SET birthdate = CASE
	    WHEN birthdate LIKE '%/%' THEN date_format(str_to_date(birthdate, '%m/%d/%Y'), '%Y-%m-%d')
	    WHEN birthdate LIKE '%-%' THEN date_format(str_to_date(birthdate, '%m-%d-%Y'), '%Y-%m-%d')
	    ELSE NULL
	END;
```

### STEP 3 -- Change birthdate data type from text to date
```sql
	ALTER TABLE hr
	MODIFY COLUMN birthdate DATE;
```
 
### STEP 4 -- UPDATE HIRE_DATE
```sql
	UPDATE hr 
	SET hire_date = CASE
	    WHEN hire_date LIKE '%/%' THEN date_format(str_to_date(hire_date, '%m/%d/%Y'), '%Y-%m-%d')
	    WHEN hire_date LIKE '%-%' THEN date_format(str_to_date(hire_date, '%m-%d-%Y'), '%Y-%m-%d')
	    ELSE NULL
	END;
```
 

## Exploratory Data Analysis
EDA involved using the Human Resources data to answer the follwing questions:
1. What is the gender breakdown of employees in the company?
2. What is the race/ethnicity breakdown in the company?
3. What is the age distribution of employees in the company?
4. What is the gender distribution among the age groups?
5. How many employees work at the headquarters versus remote?
6. What is the average length of employment for employees who have been terminated?
7. What is the distribution of job titles in the company?
8. Which department has the highest turnover rate(the rate at which employees leave a company)?
9. What is the distribution of employees across locations by state?
10. What is the distribution of employees across locations by location_city?
11. How has comapny's employees count changed over time based on hire and term dates?
12. What is the tenure distribution for each department?

## Data Analysis:

#### Some codes used during the data analysis stage:
```sql
	SELECT department, total_count, terminated_count, terminated_count/total_count AS termination_rate
	FROM (
		SELECT department, COUNT(*) AS total_count,
	    SUM(CASE WHEN termdate IS NOT NULL AND termdate <= curdate() THEN 1 ELSE 0 END) AS terminated_count
	    FROM hr
	    WHERE age >= 18
	    GROUP BY department) AS term_subquery
	ORDER BY termination_rate DESC;
```
```sql
	SELECT
		year, hires, terminations, hires - terminations AS net_change, 
	    ROUND((hires - terminations)/hires*100,2) AS net_change_percent
	FROM(
		SELECT 
			YEAR(hire_date) AS year,
	        COUNT(*) AS hires,
	        SUM(CASE WHEN termdate IS NOT NULL AND termdate <= curdate() THEN 1 ELSE 0 END) AS terminations
		FROM hr
	    WHERE age >= 18
	    GROUP BY YEAR(hire_date)
	    ) AS termination_sub_query
	ORDER BY year ASC;
```

```sql
	SELECT
	ROUND(AVG(datediff(termdate, hire_date))/365,0) AS avg_length_employment
	FROM hr
	WHERE age >= 18 AND termdate <= curdate() AND termdate IS NOT NULL;
```
## Findings:
#### The following are the summary of the findings;
1. wfwf

## Recommendations
- --
- --


## Limitations
- --
- --

## References
- MySQL documentation - [Click here to view](https://dev.mysql.com/doc/workbench/en/)
- PowerBi documentation - [Click here to view](https://learn.microsoft.com/en-us/power-bi/)
  
