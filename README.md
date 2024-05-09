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
This project aimed to gain insights into various aspects of employee data within a company. By leveraging data analytics and visualization techniques, the project delved into understanding patterns, trends and relationships within the dataset to inform HR strategies and decision-making processes in predicting how long an empployee is likely to stay with the company.

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
1. Only a quarter(25%) of the staff work remotely.
2. The company has a staff strength of over 21000.
3. Majority of Employees live in Ohio State.
4. The average length employees work in the company is 8 years.
5. There is a continuous increase in hires from the year 2004 to 2020.
6. The largest proportion of workers were white and the smallest proportion were Native Hawaaian/Other Pacisific Islanders.
7. Across all age groups, the non-conforming gender category had the lowest representation.
8. The engineering department boasts the highest proportion of female representation across all departments. However, this is primarily attributed to the fact that the engineering department has the largest number of staff overall.
9. The sales department maintains the longest average tenure at 9 years, while the business development, human resources, support, training, legal, and product management departments have the lowest average tenure at 7 years.
10. The auditing department has the highest employment termination rate of of 0.18 while the marketing department has the lowest termnation rate of 0.09.

## Recommendations
1. **Remote Work:** Given that only a quarter of the staff work remotely, consider evaluating the potential benefits of expanding remote work opportunities. This could improve employee satisfaction, work-life balance, and potentially attract talent from a broader geographic area.

2. **Staff Strength:** With a staff strength of over 21,000, ensure that HR systems and processes are robust enough to handle such a large workforce efficiently. Consider implementing scalable HR technologies to streamline operations.

3. **Location Distribution:** Since the majority of employees reside in Ohio State, explore ways to leverage this geographical concentration, such as local community engagement initiatives or targeted recruitment efforts.

4. **Employee Tenure:** With an average employee tenure of 8 years, focus on strategies to improve employee retention, such as career development opportunities, competitive compensation packages, and a positive work culture.

5. **Hiring Trends:** Analyze the factors driving the continuous increase in hires from 2004 to 2020 to forecast future hiring needs accurately. Consider aligning hiring strategies with business growth projections and market demand. Is this a sign of growth or high turnover? Understanding the reasons behind the hiring pattern can help with future workforce planning.

6. **Diversity and Inclusion:** Recognize the disparities in workforce diversity, particularly the underrepresentation of certain ethnic groups. Implement diversity and inclusion initiatives to foster a more inclusive workplace culture and attract a more diverse talent pool.

7. **Gender Representation:** Address the low representation of non-conforming genders across all age groups by implementing targeted recruitment and retention strategies aimed at promoting diversity and inclusion.

8. **Departmental Gender Representation:** While celebrating the high proportion of female representation in the engineering department, ensure that gender diversity efforts are inclusive across all departments, regardless of size.

9. **Departmental Tenure:** Investigate the factors contributing to the longer tenure in the sales department compared to other departments with lower tenure. Consider implementing retention strategies tailored to each department's unique needs and challenges.

10. **Termination Rates:** Identify the reasons behind the high termination rate in the auditing department and explore strategies to improve employee satisfaction, engagement, and retention. Conversely, recognize and reinforce the practices contributing to the low termination rate in the marketing department.




## Limitations
- --
- --

## References
- MySQL documentation - [Click here to view](https://dev.mysql.com/doc/workbench/en/)
- PowerBi documentation - [Click here to view](https://learn.microsoft.com/en-us/power-bi/)
  
