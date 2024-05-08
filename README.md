## HR_Data_Cleaning_and_Visualization

### Data_Cleaning
USE Viz;
SELECT *
FROM hr;

-- DATA CLEANING
/*Change ï»¿id column to emp_id and allow NULL values*/

ALTER TABLE hr
CHANGE COLUMN ï»¿id emp_id VARCHAR(20) NULL;

-- Check data types for all columns
DESCRIBE hr;
