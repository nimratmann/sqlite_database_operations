# sqlite_database_operations
Integrating data processing with Python, utilizing Pandas for exploratory data analysis, and conducting database operations using SQLite.

# Loading Packages
Prior to loading in my datasets, I loaded the following packages:
```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
```
 Loading packages before datasets ensures that you have access to the necessary functions and methods required for various data-processing tasks. These packages provide functions like read_csv() (Pandas) for reading CSV files, read_json() (Pandas) for reading JSON files, numpy.array() (NumPy) for creating arrays, and matplotlib.pyplot.plot() (Matplotlib) for creating plots. By loading these packages I will be able to call these functions as needed.

# Datasets Selected:
1. Stony Brook Hospital Standard Charges
- CSV File
```
sb = pd.read_csv ('https://raw.githubusercontent.com/nimratmann/sqlite_database_operations/main/Dataset/113243405_StonyBrookUniversityHospital_standardcharges.csv')
```
- Columns (Categorical and Numerical): code, description, type, package/line_level, gross charge, discounted cash price, and various insurance types including charges associated with them

2. New-York Presbyterian Hospital Standard Charges
- JSON file
```
nyp = pd.read_json ('NewYorkPresbyterianHospital_standardcharges.zip')
```
- Columns (Categorical and Numerical): code, description, rev code, inpatient/outpatient, gross charges, discounted cash price and various insurance types including charges associated with them

# Exploratory Data Analysis Process
To initiate the EDA process, I first printed the initial rows of both datasets and then a summary of their numerical characteristics with the following code:
```
# Stony Brook Hospital
print("Stony Brook Hospital Standard Charges Overview: ")
print(sb.head())

print("Summary Statistics for Stony Brook Hospital Standard Charges: ")
print(sb.describe())

# New-York Presbyterian Hospital
print("NewYork-Presbyterian Hospital Standard Charges Overview: ")
print(nyp.head())

print("Summary Statistics for NewYork-Presbyterian Hospital Standard Charges: ")
print(nyp.describe())
```
Then I identified the columns missing values respective to each hospital and cleaned the column names to be "lower case" with the following code:
``` 
# Stony Brook Hospital
print("Missing Values in Stony Brook Hospital Charges Dataset: ")
print(sb.isnull().sum())

def clean_column_names(sb):
    def clean_name(name):
        cleaned_name = re.sub(r'[^a-zA-Z0-9]', '', name)
        return cleaned_name.lower()
    column_mapping = {col: clean_name(col) for col in sb.columns}
    sb = sb.rename(columns=column_mapping)

    return sb
sb = clean_column_names(sb)
sb

# New-York Presbyterian Hospital
print("Missing Values in NewYork-Presbyterian Hospital Charges Dataset: ")
print(nyp.isnull().sum())

def clean_column_names(nyp):
    def clean_name(name):
        cleaned_name = re.sub(r'[^a-zA-Z0-9]', '', name)
        return cleaned_name.lower()
    column_mapping = {col: clean_name(col) for col in nyp.columns}
    nyp = nyp.rename(columns=column_mapping)

    return nyp
nyp = clean_column_names(nyp)
nyp
```
Then I printed summary statistics for each  of the datasets. This included the frequency count, mean, median, mode, range, standard deviation, minimum value, 25% quartile, 50% quartile, 75% quartile, and maximum value for each of the columns. These basic statistics were found with the following code:
```
# Stony Brook Hospital
print("Frequency Counts for the 'description column' in Stony Brook Hospital Charges dataset: ")
print(sb['description'].value_counts())

numerical_columns = [
     "discountedcashprice",
    "grosscharge",
    "deidentifiedmincontractedrate",
    "deidentifiedmaxcontractedrate",
    "derivedcontractedrate",
    "1199commercialother",
    "aetnamedicareadvantagehmo",
    "aetnacommercialhmopos",
    "aetnacommercialppoopenaccess",
    "aetnacommercialother",
    "empirehealthcommercialother",
    "empirehealthcommercialppoopenaccess",
    "bluecrossblueshieldcommercialother",
    "beaconhealthcommercialother",
    "carelonhealthcommercialother",
    "cignacommercialppoopenaccess",
    "cignacommercialother",
    "cignacommercialhmopos",
    "ehfacetcommercialother",
    "emblemhealthcommercialppoopenaccess",
    "emblemhealthcommercialother",
    "emblemhealthcommercialhmopos",
    "emblemhealthmedicaidhmo",
    "emblemhealthmedicareadvantagehmo",
    "empirehealthcommercialhmopos",
     "empirehealthmedicareadvantagehmo",
    "empirehealthmedicaidhmo",
    "evernorthcommercialother",
    "fideliscommercialother",
    "fidelismedicareadvantagehmo",
    "fidelismedicaidhmo",
    "ghicommercialother",
    "healthfirstcommercialother",
    "healthfirstmedicareadvantagehmo",
    "healthfirstmedicaidhmo",
    "healthplushpmedicaidhmo",
    "healthplushpcommercialother",
    "healthplushpmedicareadvantagehmo",
    "humanacommercialother",
    "humanacommercialhmopos",
    "humanacommercialppoopenaccess",
    "meritainhealthcommercialother",
    "molinacommercialother",
    "optumcommercialother",
    "oxfordcommercialother",
    "oxfordcommercialhmopos",
    "tricarecommercialother",
    "unitedhealthcarecommercialother",
    "unitedhealthcaremedicareadvantagehmo",
    "unitedhealthcarecommercialhmopos",
    "unitedhealthcaremedicaidhmo",
    "unitedhealthcarecommercialppoopenaccess",
    "veteranfamilycommercialother",
]

analysis_results = {}
for column in numerical_columns:
    mean = sb[column].mean()
    median = sb[column].median()
    mode = sb[column].mode()
    std_dev = sb[column].std()
    min_value = sb[column].min()
    max_value = sb[column].max()

    frequency_distribution = sb[column].value_counts().reset_index()
    frequency_distribution.columns = [column, "Frequency"]

    analysis_results[column] = {
        "Mean": mean,
        "Median": median,
        "Mode": mode,
        "Standard Deviation": std_dev,
        "Min Value": min_value,
        "Max Value": max_value,
        "Frequency Distribution": frequency_distribution
    }
for column, results in analysis_results.items():
    print(f"Analysis for column: {column}")
    print(results)
    print("\n")

sb_mean = sb['discountedcashprice'].mean()
sb_median = sb['discountedcashprice'].median()
sb_mode = sb['discountedcashprice'].mode().values[0]
sb_range = sb['discountedcashprice'].max() - sb['discountedcashprice'].min()
sb_var = sb['discountedcashprice'].var()
sb_std= sb['discountedcashprice'].std()
sb_iqr = sb['discountedcashprice'].quantile(0.75) - sb['discountedcashprice'].quantile(0.25)


print("Measures of Central Tendency:")
print(f"Mean: {sb_mean}")
print(f"Median: {sb_median}")
print(f"Mode: {sb_mode}")
print("\nMeasures of Spread:")
print(f"Range: {sb_range}")
print(f"Variance: {sb_var}")
print(f"Standard Deviation: {sb_std}")
print(f"IQR (Interquartile Range): {sb_iqr}")



# New-York Presbyterian Hospital
print("Frequency Counts for the 'description column' in NewYork-Presbyterian Hospital Charges dataset: ")
print(nyp['description'].value_counts())

numerical_columns = [
    'grosscharges',
    'discountedcashprice',
    'minimumnegotiatedcharge',
    'maximumnegotiatedcharge',

]

analysis_results = {}
for column in numerical_columns:
    mean = nyp[column].mean()
    median = nyp[column].median()
    mode = nyp[column].mode().tolist()  # Convert mode to a list
    std_dev = nyp[column].std()
    min_value = nyp[column].min()
    max_value = nyp[column].max()

    frequency_distribution = nyp[column].value_counts().reset_index()
    frequency_distribution.columns = [column, "Frequency"]

    analysis_results[column] = {
        "Mean": mean,
        "Median": median,
        "Mode": mode,
        "Standard Deviation": std_dev,
        "Min Value": min_value,
        "Max Value": max_value,
        "Frequency Distribution": frequency_distribution
    }

for column, results in analysis_results.items():
    print(f"Analysis for column: {column}")
    print(results)
    print("\n")

nyp_mean = nyp['discountedcashprice'].mean()
nyp_median = nyp['discountedcashprice'].median()
nyp_mode = nyp['discountedcashprice'].mode().values[0]
nyp_range = nyp['discountedcashprice'].max() - nyp['discountedcashprice'].min()
nyp_var = nyp['discountedcashprice'].var()
nyp_std= nyp['discountedcashprice'].std()
nyp_iqr = nyp['discountedcashprice'].quantile(0.75) - nyp['discountedcashprice'].quantile(0.25)


print("Measures of Central Tendency:")
print(f"Mean: {nyp_mean}")
print(f"Median: {nyp_median}")
print(f"Mode: {nyp_mode}")
print("\nMeasures of Spread:")
print(f"Range: {nyp_range}")
print(f"Variance: {nyp_var}")
print(f"Standard Deviation: {nyp_std}")
print(f"IQR (Interquartile Range): {nyp_iqr}")

```

To understand the data distribution for each of the hospital standard charges data, I focused on the "gross charge" and "discounted cash price" columns and found the measures of central tendency to create histograms with the following code:
```
# Stony Brook Hosptial:
plt.figure(figsize=(9,9))
plt.hist(sb['grosscharge'], bins=15, color='green', edgecolor='black')
plt.title('The Frequency of Stony Brook Hospital Services Gross Charge')
plt.xlabel('Gross Charge')
plt.ylabel('Frequency')
plt.grid(True)
plt.show()

plt.figure(figsize=(9,9))
plt.hist(sb['discountedcashprice'], bins=15, color='pink', edgecolor='black')
plt.title('The Frequency of Stony Brook Hospital Services Discounted Cash Price')
plt.xlabel('Discounted Cash Price')
plt.ylabel('Frequency')
plt.grid(True)
plt.show()

# New-York Presbyterian Hospital
plt.figure(figsize=(9,9))
plt.hist(nyp['grosscharges'], bins=15, color='green', edgecolor='black')
plt.title('The Frequency of NewYork-Presbyterian Hospital Services Gross Charge')
plt.xlabel('Gross Charge')
plt.ylabel('Frequency')
plt.grid(True)
plt.show()

plt.figure(figsize=(9,9))
plt.hist(nyp['discountedcashprice'], bins=15, color='pink', edgecolor='black')
plt.title('The Frequency of NewYork-Presbyterian Hospital Services Discounted Cash Price')
plt.xlabel('Discounted Cash Price')
plt.ylabel('Frequency')
plt.grid(True)
plt.show()
```
# SQLite Database Setup
I first imported sqlite3 and connected to the SQlite Database with the following code:
```
import sqlite3

# Connect to the SQLite database
conn = sqlite3.connect('health.db')
cursor = conn.cursor()
```
Then I created a stony brook "sb" table and inserted sample data into the "sb" table using the following code:
```
# Create the 'sb' table
cursor.execute('''
    CREATE TABLE IF NOT EXISTS sb
    (
        [codecptdrg] INTEGER,
        [description] TEXT,
        [revcode] INTEGER,
        [inpatientoutpatient] TEXT,
        [grosscharges] REAL,
        [discountedcashprice] REAL
    )
''')

# Insert sample data into the 'sb' table
cursor.execute('''
    INSERT INTO sb
    VALUES
        (1, 'Consultation', 123, 'Inpatient', 1000.50, 800.25),
        (2, 'Surgery', 456, 'Outpatient', 2500.75, 2000.60),
        (3, 'MRI Scan', 789, 'Inpatient', 150.20, 120.15)
''')
```
To commit the changes to my table and then execute a query to fetch and print table names, I used the following code:
```
conn.commit()

cursor.execute('''
    SELECT name
    FROM sqlite_master
    WHERE type= 'table'
''')

tables = cursor.fetchall()

for value in tables:
    print(value)

cursor.execute('''
  SELECT * FROM sb;
''')

print(cursor.fetchall())
```

To create the health.db engine and to read/view the sb table, I used the following code:
```
engine = create_engine('sqlite:///health.db')

sb = pd.read_sql("select * from sb;", conn)
sb
```

To automate the creation of my table, I utilized the to_sql function from Pandas using the following code:
```
sb.to_sql('sb', conn, if_exists='replace', index=False)
```

To close the connection to the SQLite Database
```
conn.close()
```


