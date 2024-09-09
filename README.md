Automobile data cleaning with SQL

# Problem Statement
As a data analyst working with a used car dealership startup venture. The investors want to find out which cars are most popular with customers so they can make sure to stock accordingly.
Tool Used: SQLite

# Upload Data
## Step 1: Download the dataset 
This is data from an external source that contains historical sales data on car prices and their features.
Click the link to the automobile_data file to download it. Or you may download the CSV file directly from the documentation.
https://docs.google.com/spreadsheets/d/1U3ktsROmhoCZG3Yz5xFLnjJmzm-MgBYYxwt4xcb6d3o/template/preview

## Step 2: Create a database and click to import the csv file as a table
- Open DB Browser for SQLite
- Create a new database
- Go to file and import table from csv
- Click Ok

# Cleaning data
## Step 1:  Inspect the fuel_type column
According to the data’s description, the fuel_type column should only have two unique string values: diesel and gas. To check and make sure that’s true,i  run the following query
![clean1](https://github.com/user-attachments/assets/9c236031-1dbe-4bfe-8ef9-f2c1d89cd1e1)

This confirms that the fuel_type column doesn’t have any unexpected values. Also note that the default LIMIT 1000 is added to your query, but in this case, Query is only returning two distinct fuel types.


## Step 2: Inspect the length column
The length column should contain numeric measurements of the cars. So you will check that the minimum and maximum lengths in the dataset align with the data description, which states that the lengths in this column should range from 141.1 to 208.1. Run this query to confirm 
![clean2](https://github.com/user-attachments/assets/358dd794-c353-4522-bec9-b3d42b25f3d4)

Your results should confirm that 141.1 and 208.1 are the minimum and maximum values respectively in this column. 


## Step 3: Fill in missing data
Missing values can create errors or skew results during analysis. You’re going to want to check your data for null or missing values. These values might appear as a blank cell or the word null.You can check to see if the num_of_doors column contains null values using this query:
![clean3](https://github.com/user-attachments/assets/f9475be1-6d52-49d6-b5a9-524089dee6fb)

This will select any rows with missing data for the num_of_doors column and return them in your results table. You should get two results, one Mazda and one Dodge:
In order to fill in these missing values, you check with the sales manager, who states that all Dodge gas sedans and all Mazda diesel sedans sold had four doors.  you can use this query to update your table so that all Dodge gas sedans have four doors:
![clean3(1)](https://github.com/user-attachments/assets/31b2261b-f15a-4349-a935-d3676b9ee90e)

You should get a message telling you that three rows were modified in this table.

Repeat this process to replace the null value for the Mazda.
![clean3(2)](https://github.com/user-attachments/assets/81239d14-cf98-4e06-974b-5e682941cd1b)

## Step 4: Identify potential errors
Once you have finished ensuring that there aren’t any missing values in your data, you’ll want to check for other potential errors. You can use SELECT DISTINCT to check what values exist in a column. You can run this query to check the num_of_cylinders column: 
![clean4](https://github.com/user-attachments/assets/52f4f852-0c0a-4799-ad72-12024b8a1e6d)

After running this, you notice that there are two entries for two cylinders: rows 6 and 7. But the two in row 7 is misspelled.
To correct the misspelling for all rows, you can run this query
![clean4(1)](https://github.com/user-attachments/assets/90edac07-e9c8-46ca-8bf0-e7b2922adb5d)

You will get a message alerting you that one row was modified after running this statement. To check that it worked, you can run the previous query again:
![clean4(2)](https://github.com/user-attachments/assets/bd4b851e-4333-41e7-b15a-e5ef91b3240a)

Next, you can check the compression_ratio column. According to the data description, the compression_ratio column values should range from 7 to 23. Just like when you checked the length values , you can use MIN and MAX to check if that’s correct:
![clean4(3)](https://github.com/user-attachments/assets/4057a520-1d34-458c-acf5-f7dd914d29be)

Notice that this returns a maximum of 70. But you know this is an error because the maximum value in this column should be 23, not 70. So the 70 is most likely a 7.0. Run the above query again without the row with 70 to make sure that the rest of the values fall within the expected range of 7 to 23.
![clean4(4)](https://github.com/user-attachments/assets/41f49c24-d15c-4b7d-a172-664733817007)

Now the highest value is 23, which aligns with the data description. So you’ll want to correct the 70 value. You check with the sales manager again, who says that this row was made in error and should be removed. Before you delete anything, you should check to see how many rows contain this erroneous value as a precaution so that you don’t end up deleting 50% of your data. If there are too many (for instance, 20% of your rows have the incorrect 70 value), then you would want to check back in with the sales manager to inquire if these should be deleted or if the 70 should be updated to another value. Use the query below to count how many rows you would be deleting:
![clean4(5)](https://github.com/user-attachments/assets/6a22b5d4-1253-4144-9ddd-2531f4329d59)

Turns out there is only one row with the erroneous 70 value. So you can delete that row using this query:  
![clean4(6)](https://github.com/user-attachments/assets/b043e747-87b6-4242-87f7-1898761d110a)

## Step 5: Ensure consistency
Finally, you want to check your data for any inconsistencies that might cause errors. These inconsistencies can be tricky to spot — sometimes even something as simple as an extra space can cause a problem.
- Check the drive_wheels column for inconsistencies by running a query with a SELECT DISTINCT statement: 
  ![clean5(1)](https://github.com/user-attachments/assets/2c6fc32c-125e-401f-9ce4-4b39cd5d727b)

Note: Assuming It appears that 4wd, rwd or fwd appears twice in results. However, because you used a SELECT DISTINCT statement to return unique values, this probably means there’s an extra space in one of the  entries that makes it different from the others.
- To check if this is the case, you can use a LENGTH statement to determine the length of how long each of these string variables:  
  ![clean5(2)](https://github.com/user-attachments/assets/50199b45-7a8f-44cb-8bc6-1b5f90839735)

Note: If According to these results, some instances of the 4wd, fwd, rwd string have four characters instead of the expected three (3 characters). In that case, you can use the TRIM function to remove all extra spaces in the drive_wheels column
![clean5(3)](https://github.com/user-attachments/assets/edf8cd8d-aef3-44cf-a942-773500c73651)

- Then, you run the SELECT DISTINCT statement again to ensure that there are only three distinct values in the drive_wheels column:  
![clean5(4)](https://github.com/user-attachments/assets/f9badd40-6ef3-4df9-a183-a8511cc5b8a2)

## Data Analysis
- Getting the maximum value in the price column of the car_info?
  ![clean5(5)](https://github.com/user-attachments/assets/a6b68080-bd64-4a0a-8d75-ddef6ab418a5)
- The most popular car
  ![clean5(6)](https://github.com/user-attachments/assets/8a8cb24a-c3f3-48c2-bde3-43459e902c7e)





