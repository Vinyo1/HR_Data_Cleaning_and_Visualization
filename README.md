## HR_Data_Cleaning_and_Visualization

### Data_Cleaning
USE Viz;
SELECT *
FROM hr;


#### -- Change ï»¿id column to emp_id and allow NULL values

ALTER TABLE hr
CHANGE COLUMN ï»¿id emp_id VARCHAR(20) NULL;

-- Check data types for all columns
DESCRIBE hr;


#### -- Change birthdate from text to date

SELECT birthdate
FROM hr;

SET sql_safe_updates = 0;
UPDATE hr
SET birthdate = CASE
    WHEN birthdate LIKE '%/%' THEN date_format(str_to_date(birthdate, '%m/%d/%Y'), '%Y-%m-%d')
    WHEN birthdate LIKE '%-%' THEN date_format(str_to_date(birthdate, '%m-%d-%Y'), '%Y-%m-%d')
    ELSE NULL
END;

