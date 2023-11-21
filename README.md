# ETL MINI PROJECT

## Contacts Data Cleaning and Transformation

This project involves cleaning and transforming contact information stored in a DataFrame using two different approaches: one using Pandas and the other using regular expressions.

## Option 1: Use Pandas to create the contacts DataFrame

### Steps:

1. Iterate through the 'contact_info_df' DataFrame and convert each row to a dictionary.
2. Create a new DataFrame ('new_temp_data_df') with columns 'contact_id', 'name', and 'email'.
3. Split the 'name' column into 'first_name' and 'last_name'.
4. Drop the 'name' column and reorder columns.
5. Export the DataFrame to a CSV file ('contacts.csv').

```python
# Iterate through the contact_info_df and convert each row to a dictionary.
import json
dict_values = []
# Iterate through the DataFrame.
for i, row in contact_info_df.iterrows():
    data = row[0]
    # Convert each row to a Python dictionary.
    converted_data = json.loads(data)
    # Use a list comprehension to get the values for each row.
    row_values = [v for k, v in converted_data.items()]
    # Append the keys and list values to the lists created in step 1.  
    dict_values.append(row_values)

# Create a contact_info DataFrame and add each list of values to the 'contact_id', 'name', 'email' columns.
new_temp_data_df = pd.DataFrame(dict_values, columns=['contact_id', 'name', 'email'])
# Split the 'name' column into 'first_name' and 'last_name'.
new_temp_data_df[["first_name", "last_name"]] = new_temp_data_df['name'].str.split(' ', n=1, expand=True)
# Drop the 'name' column.
new_temp_data_df = new_temp_data_df.drop("name", axis=1)
# Reorder the columns.
contacts_df_clean = new_temp_data_df[['contact_id', 'first_name', 'last_name', 'email']]
# Export the DataFrame as a CSV file.
contacts_df_clean.to_csv("contacts.csv", encoding='utf8', index=False)

```

## Option 2: Use regex to create the contacts DataFrame

### Steps:

1. Extract the four-digit contact ID number and convert it to the 'Int64' data type.
2. Extract the contact name and email using regular expressions.
3. Create a new DataFrame ('final_contact_df') with columns 'contact_id', 'first_name', 'last_name', and 'contact_email'.
4. Reorder columns and export the DataFrame to a CSV file ('contacts_regex.csv').

```python

# Extract the four-digit contact ID number.
contact_info_df_copy['contact_id'] = contact_info_df_copy['contact_info'].str.extract('(\d{4})')
# Convert the 'contact_id' column to an int64 data type.
contact_info_df_copy['contact_id'] = contact_info_df_copy['contact_id'].astype('Int64')
# Extract the name of the contact and add it to a new column.
contact_info_df_copy['contact_name'] = contact_info_df_copy['contact_info'].str.extract('([A-Z][a-z]+\s[A-Z][a-z]+)')
# Extract the email from the contacts and add the values to a new column.
contact_info_df_copy['contact_email'] = contact_info_df_copy['contact_info'].str.extract('([a-zA-Z0-9._-]+@[a-zA-Z0-9._-]+\.[a-zA-Z0-9_-]+)')
# Create a copy of the contact_info_df with the 'contact_id', 'name', 'email' columns.
final_contact_df = contact_info_df_copy[['contact_id', 'contact_name', 'contact_email']]
# Create 'first_name' and 'last_name' columns.
final_contact_df['first_name'] = final_contact_df['contact_name'].str.extract('([A-Z][a-z]+)')
final_contact_df['last_name'] = final_contact_df['contact_name'].str.extract('(\s[A-Z][a-z]+)')
final_contact_df = final_contact_df.drop(columns=['contact_name'])
# Reorder the columns.
final_contact_df = final_contact_df.loc[:, ['contact_id', 'first_name', 'last_name', 'contact_email']]
# Export the DataFrame as a CSV file.
final_contact_df.to_csv('contacts_regex.csv', encoding='utf8', index=False)

```

## Usage

1. Ensure you have the necessary dependencies installed (`pandas`, `json`).

2. Copy and paste the code snippets into your Python environment.

3. Run the code to perform data cleaning and transformation.

## File Structure

- **contacts.csv**: Output file from Option 1.
- **contacts_regex.csv**: Output file from Option 2.

## Dependencies

- pandas
- json

## Author

[Alexander M Bates & Jason Casperson]


## Acknowledgments

- Special thanks to [Edx] for their valuable datasets.


