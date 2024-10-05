# Advanced_Data_Cleaning_Techniques_with_Pandas
This Project aims to demonstrate advanced data cleaning techniques using Python(Pandas).

### INTRODUCTION
The dataset involved in this project is manually generated and intentionally made to be as messy as possible, in order to force the analyst to employ techniques ranging from very simple to advanced, for the cleaning process.

here is a preview of the dataframe in pandas
![image](https://github.com/user-attachments/assets/78410949-68f1-4a2c-a918-307736a9395f)
*There's obviously quite a number of errors and unwanted elements in all the various columns of the dataset*

### PROBLEM STATEMENT
Some major points of focus in the dataset has been highlighted in yellow and will be explained further
![image](https://github.com/user-attachments/assets/12a40daf-5a77-40b9-8da4-0e718d26b1a7)

What do we need to clean up?
1. Last_Name: we have to remove all the trailing and leading unwanted elements(_/) from the Last_Name column
2. Phone_Number: we must ensure that the Phone numbers are properly formatted
3. Address: The addresses should be split into the 'Street_Name' and 'City' to enure better readability
4. Paying_Customer: This column needs to be properly formatted by replacing 'Yes' with 'Y' and 'No' with 'N'
5. Do_Not_Contact: This column should be formatted the same way as the 'Paying_Customer' column
6. Not_Useful_column: This column should be deleted as it has no significance to the data

### STEP 1: IMPORTING DATA AND LIBRARIES

```python
import pandas as pd
df = pd.read_csv(r"C:\Users\HP\OneDrive\Documents\data analysis practice\Python practice\Dirty_Data_Alex.csv")
# reads in the data to a pandas dataframe
df # Displays the data in a dataframe below
```
![image](https://github.com/user-attachments/assets/de24259b-7b5f-455a-a854-7b7528a6f479)
![image](https://github.com/user-attachments/assets/77ec437e-421a-4915-ad81-ae26fb11e850)

### STEP 2: DATA PREPARATION
Removing duplicates
```python
df = df.drop_duplicates() # delete duplicated rows of data (index number 19 and 20 are duplicates)
df # displays the new dataframe after removing duplicates
```
![image](https://github.com/user-attachments/assets/c745e7f3-f0dd-436b-8191-c074b129d50c)
*The duplicated row has been removed from the dataset*

Dropping the Not_Useful_Column
```python
df = df.drop(columns = 'Not_Useful_Column') # drops the column from the dataframe
df # displays the dataframe
```
![image](https://github.com/user-attachments/assets/17fb466f-5671-438e-bf7f-50f80b763c7f)

### STEP 3: DATA CLEANING (COLUMN BY COLUMN)

#### **Last_Name**

Stripping off unwanted elements from the Last Names
```python
# we can see from the above that there are unwanted underscoes and forward slashes in the Last Names. we can strip off those unwanted elements

df.loc[:,'Last_Name'] = df.loc[:,'Last_Name'].str.lstrip('-/_') # strips off the symbol "-/_" from the last Names on the left side
df.loc[:,'Last_Name'] = df.loc[:,'Last_Name'].str.rstrip('-_') # strips off the symbol "-/_" from the last Names on the right side
df # displays the dataset without the errors in the last names
```
![image](https://github.com/user-attachments/assets/89ac7357-164e-4e69-aa8a-89abce68f283)

#### **Phone_Number**

Removing all other elements apart from numbers in the Phone_Number column

```python
# removes unwanted elements from the phone numbers
df.loc[:,'Phone_Number'] = df.loc[:,'Phone_Number'].str.replace('-','')
df.loc[:,'Phone_Number'] = df.loc[:,'Phone_Number'].str.replace('/','')
df.loc[:,'Phone_Number'] = df.loc[:,'Phone_Number'].str.replace('|','')
df
```
![image](https://github.com/user-attachments/assets/56342697-85c8-4aec-873a-33c06a1e1105)

```python
df.loc[:,'Phone_Number'] = df.loc[:,'Phone_Number'].astype('str') # Changes the Phone numbers in the Phone_Number column to a object datatype to allow for further formatting
```
Formatting the Phone numbers using Lambda function
```python
# we can use a lambda function to format the phone number column to have a "-" after every third number, like a typical phone number
df.loc[:,'Phone_Number'] = df.loc[:,'Phone_Number'].apply(lambda x: x[0:3] + '-' + x[3:6] + '-' + x[6:10])
df
```
![image](https://github.com/user-attachments/assets/0c10f31a-9a2b-44b2-aa21-7670dc6b3816)
*Notice that the phone numbers now have hyphens after every three characters?*

Further Cleaning
```python
### Replacing the null or NaN values in the Phone number columns with a blank value
df.loc[:,'Phone_Number'] = df.loc[:,'Phone_Number'].str.replace('Na--','')
df.loc[:,'Phone_Number'] = df.loc[:,'Phone_Number'].str.replace('nan--','')
df
```
![image](https://github.com/user-attachments/assets/07d77029-d5ad-430a-8828-05ef115fbd6b)

#### **Address** and **Paying_Customer**

```python
df[['Street Address','State']] = df.loc[:,'Address'].str.split(',', expand = True) 
#splits the Address column using the comma ',' as the delimiter and renames the split columns as 'Street Address' and 'State'
```
```python
df.loc[:,'Paying Customer'] = df.loc[:,'Paying Customer'].str.replace('Yes','Y') # replaces 'Yes' with 'Y' 
df.loc[:,'Paying Customer'] = df.loc[:,'Paying Customer'].str.replace('No','N')  # replaces 'No' with 'N' 
df # displays the recent changes
```
![image](https://github.com/user-attachments/assets/a4060067-f4a7-4915-bd87-ba327b07e90c)

#### **Do_Not_Contact**
```python
# we will repeat the same step as in Paying customer column
df.loc[:,'Do_Not_Contact'] = df.loc[:,'Do_Not_Contact'].str.replace('Yes','Y')
df.loc[:,'Do_Not_Contact'] = df.loc[:,'Do_Not_Contact'].str.replace('No','N')
df # displays the changes made
```
![image](https://github.com/user-attachments/assets/435dcc41-5eba-43ac-bf6a-3c1288fd0afd)

#### **General cleaning**
Replacing all Nan or N/a values with blanks
```python
df.loc[:,'Street Address'] = df.loc[:,'Street Address'].str.replace('N/a','')
df.loc[:,'Paying Customer'] = df.loc[:,'Paying Customer'].str.replace('N/a','')
df.loc[:,'Do_Not_Contact'] = df.loc[:,'Do_Not_Contact'].str.replace('N/a','')
df
### this does not seem to work for index 15
```
![image](https://github.com/user-attachments/assets/334d8f72-f122-47aa-accc-7055c12285d4)

For index 15, we can manually solve this problem
```python
df.loc[15,'Paying Customer'] = '' ## manually sets the value of that particular record to a blank
df.loc[15,'Do_Not_Contact'] = ''  ## manually sets the value of that particular record to a blank
df # displays the changes
```
![image](https://github.com/user-attachments/assets/9b5fc137-96eb-4b55-8647-d11ddf2064e4)

### STEP 4: FINAL CLEANING PROCESS
We must remove every row that has 'Y' for Do_Not_Contact

```python
# Using a for loop to drop any row that has 'Y' for Do_Not_Contact
for x in df.index:
    if df.loc[x,'Do_Not_Contact'] == 'Y':
        df.drop(x, inplace = True)  
df # Displays the result after the for loop
```
![image](https://github.com/user-attachments/assets/d5681ec7-4085-4203-8307-67d79a34ab2d)

Now we must remove rows that have blank phone numbers
```python
# using a for loop to remove rows with blank phone numbers
for x in df.index:
    if df.loc[x,'Phone_Number'] == '':
        df.drop(x, inplace = True)  
df # displays the results
```
![image](https://github.com/user-attachments/assets/09ff56ca-cd0a-4c3f-ba98-3774865f4b69)

### STEP 5: EXPORTING THE CLEANED DATA
```python
df.to_csv(r"Clean_Data_Alex.csv") # saves the name of the clean dataset as "Clean_Data_Alex"
```

# END
