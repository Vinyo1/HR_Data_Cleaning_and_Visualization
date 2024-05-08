## HR_Data_Cleaning_and_Visualization


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
