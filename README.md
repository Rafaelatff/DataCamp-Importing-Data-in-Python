# DataCamp-Importing-Data-in-Python
Python lessons during the [DataCamp](https://app.datacamp.com/learn/courses/introduction-to-importing-data-in-python)

Note: On the course, I can check the data on the DataCamp server by using ```! ls```.

## Importing flat files

Flat files are text files.

Lets start by opening a file by using ```file = open("nome_do_txt.txt", mode='r')```. I can also pass the path of the file together with the name. For the mode we have:

* ```mode='r'``` open for reading only.
* ```mode='w'``` for writing (truncating the file if it already exists).
* ```mode='x'``` for creating and writing to a new file.
* ```mode='a'``` for appending (which on some Unix systems, means that all writes append to the end of the file regardless of the current seek position).
* ```mode='b'``` for binary mode.
* ```mode='t'``` for text mode (default).
* ```mode='+'``` to open a disk file for updating (reading and writing).
* ```mode='U'``` for universal newline mode (deprecated).
* ```mode='rt'``` to open for reading text.
* ```mode='w+b'``` opens and truncates the file to 0 bytes, while
* ```mode='r+b'``` opens the file without truncation.

Then, to print, check if the file is open or not, and finally close the file just:

```py
# Print it
print(file.read())

# Check whether file is closed
print(file.closed)

# Close file
file.close()

# Check whether file is closed
print(file.closed)
```

To open a file, and only open while using, with no need of closing it after, it is posible by:

```py
# Read & print the first 3 lines
with open('moby_dick.txt') as file:
    print(file.readline())
    print(file.readline())
    print(file.readline())
```
Flat files can be composed of table data. Table data have Records, that are row of fields or attributes.
Then the Columns, are the features or the attributes. This tipe of flat file can also have a Header.
Example of this type of file are the .csv (Comma Separated Values) files. The Delimiters can be comma, but also can be diffent characters such as 'tabs'.

If the values are all numbers, we can store them on a array (instead of list) by using the NumPy package. Pandas can also be used to store the data in a dataframe.

Note: There are several Style Guides for Python Code, and the DataCamp uses the PEP8. By writting ```import this```, the shell will return the 'The Zen of Python, by Tim Peters'.

Let's see an example of how to import data and store it.
 
```py
import numpy as np
filename = 'MNIST.txt'
data = np.loadtxt(filename, delimiter=',')
print(data)
```
I can also pass as parameters: 

* ```skiprows=1``` to skip a header, as example.
* ```usecols=[0,2]``` to consider only the data on columns 0 and 2.
* ```usecols=['Survived']``` to print a column with a specific name.
* ```dtype=str``` to import diffent data type.
* ```delimiter= '\t'``` to use tab as delimiter.

The DataCamp exercice extend the content by selecting and reshaping the rows, and then plothing the data (with use of the matplotlib):

```py
# Import package
import numpy as np

# Assign filename to variable: file
file = 'digits.csv'

# Load file as array: digits
digits = np.loadtxt(file, delimiter=',')

# Print datatype of digits
print(type(digits))

# Select and reshape a row
im = digits[21, 1:]
im_sq = np.reshape(im, (28, 28))

# Plot reshaped data (matplotlib.pyplot already loaded as plt)
plt.imshow(im_sq, cmap='Greys', interpolation='nearest')
plt.show()
```

To handle several type of data, we can use ```np.genfromtxt()```, which can handle such structures. If we pass ```dtype=None``` to it, it will figure out what types each column should be. Data now is a **structured array** object. The structured array is actually a 1D array, where each element of the array is a row of the flat file imported. It is possible to test this by checking out the array's shape in the shell by executing ```np.shape(data)```.

It is possible to use also the ```np.recfromcsv()``` that has as default the dtype as none. It has also the defaults ```delimiter=','``` and ```names=True```.

```py
# Assign the filename: file
file = 'digits.csv'

# Read the first 5 rows of the file into a DataFrame: data
data = pd.read_csv(file, nrows=5,header=None)

# Build a numpy array from the DataFrame: data_array
data_array = data.values

# Print the datatype of data_array to the shell
print(type(data_array))
```

We can pass some other attributes to pd.read_csv such as the next example:
  
```py
# Import matplotlib.pyplot as plt
import matplotlib.pyplot as plt

# Assign filename: file
file = 'titanic_corrupt.txt'

# Import file: data
data = pd.read_csv(file, sep='\t', comment='#', na_values='Nothing')

# Print the head of the DataFrame
print(data.head())

# Plot 'Age' variable in a histogram
pd.DataFrame.hist(data[['Age']])
plt.xlabel('Age (years)')
plt.ylabel('count')
plt.show()
```
Note: missing values are also commonly referred to as NA or NaNm with type 'Nothing'.

WIth pandas, it is possible to  manipulate, slice, reshaped, groupby, join, merge, perform statistics in a missing-value-friendly manner, deal with times series.

DataFrame = pythonic analog of R's data frame.

To use pandas just:

```
import pandas as pd
filename = 'example.csv'
data = pd.read_csv(filename)
print(data.head()) # To print Header
data_array = data.values # Convert to an array
```

## Excel sheet

```py
import pandas as pd
file = 'excel_doc.xlsx' # Assign spreadsheet filename: file
xls = pd.ExcelFile(file) # Load spreadsheet: xls
print(xls.sheet_names) # How to list the names of the sheets in any loaded .xlsx file

df1 = data.parse('1960-1966') # sheet name, as a string or
df2 = data.parse(0) # sheet index, as a float

# Print the head of the DataFrame df2
print(df2.head())
```

### How to customize your import

How to skip rows, to change column names and to import certain columns:
```py
	# Parse the first sheet and rename the columns: df1
df1 = xls.parse(0, skiprows=1, names=['Country','AAM due to War (2002)'])

# Parse the first column of the second sheet and rename the column: df2
df2 = xls.parse(1, usecols=[0], skiprows=1, names=['Country'])
```

Pickle in Python is primarily used in serializing and deserializing a Python object structure.

To import pickle files:
```py
import pickle

# Open pickle file and load data: d
with open('data.pkl', 'rb') as file: # 'rb' = read only binary
	d = pickle.load(file)

# d = <class 'dict'>
```
## SAS and Stata files

* SAS - Statistical Analysis System (extension: .sas7bdat)

```py
import pandas as pd
from sas7bdat import SAS7BDAT
with SAS7BDAT('file_name.sas7bdat') as file:
	df_sas = file.to_data_frame()

```
* Stata - Statistics + data

```py
import pandas as pd
data = pd.read_stata('file_name.dta')
```

## HDF5 files

* HDF5 - Hierarchical Data Format version 5

Large quantities of numerical data.

```py
import h5py

filename = 'file_name.hdf5'
data = h5py.File(filename, 'r') # 'r' is to read
print(type(data)) # it will return <class 'h5py._hl.files.File'>

for key data.keys(): # to see the keys of the h5py file
	print(key)

for key in data['example'].keys(): # to print the key values of a single key
	print(key) 

# to convert in a numpy array and print
print(np.array(data['example']['type_of_exameple']), np.array(data['example']['other_type'])

```

## Matlab

* Matlab - Matrix Laboratory (extension: .mat)

To read .mat files: ```scipy.io.loadmat('filename.mat')```. If we try to print it, it will be a <class 'dict'> variable, where the *keys* are the MATLAB variable names and the *values* are the objects assigned to variables. To write .mat files: ```scipy.io.savemat()```.

```py
# Import package
import scipy.io

mat = scipy.io.loadmat('filename.mat')

print(mat.keys()) # This method will return the keys of the filename.mat doc.

print(mat['key_name']) # To print the value of a key

# Print the shape of the value corresponding to the key 'CYratioCyt'
print(np.shape(mat['key_name']))

```
## Relational Databases

Based on ralational model of data. Most common are: PostgreSQL, MySQL, SQLite. SQL stands for Structured Query Language.


### Creating a database engine

We will be using the SQLite because is fast and simple. 

#### SQLAlchemy

Works with many Relational Database Management Systems. 

```py
from sqlalchemy import create_engine
engine = create_engine('sqlite:///Nortwind.sqlite')

# To know the names of the tables 
table_names = engine.table_names()
print (table_names)

```

#### Querying relational databases 

```py
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('sqlite:///Nortwind.sqlite')

# Now lets quere the data
con = engine.connect() # Open the connection
rs = con.execute("SELECT * FROM Orders") # Returns all columns of all rows of the table 'Orders'
df = pd.DataFrame(rs.fetchall()) # Store data in a dataframe
df.columns = rs.keys() # To retrieve columns and rows names
con.close() # Important to close the connection at the end

print(df.head()) # To print content
```

It is possibe to use the context manager to keep connection opened:

```py
with engine.connect() as con:
	rs = con.execute("SELECT OrderID, ShipName FROM Orders") # Returns all columns of all rows of the table 'Orders'
	df = pd.DataFrame(rs.fetchmany(size=5)) # Store only 5 rows of data in a dataframe
	df.columns = rs.keys() # To retrieve columns and rows names
# Doesn't need to close connection
```
It is possible to filter the records by: SELECT * FROM Customer WHERE Country = 'Canada'

An easier way of retrieving the values from a relational database is:

```py
df = pd.read_sql_query("SELECT * FROM Orders", engine)
```

A more complex example of how to use it:

```py
# Import packages
from sqlalchemy import create_engine
import pandas as pd

# Create engine: engine
engine = create_engine('sqlite:///Chinook.sqlite')

# Use the pandas function read_sql_query() to assign to the variable df 
# the DataFrame of results from the following query: 
# select all records from the Employee table 
# where the EmployeeId is greater than or equal to 6 
# and ordered by BirthDate 
# Execute query and store records in DataFrame: df
df = pd.read_sql_query("SELECT * FROM Employee WHERE EmployeeID >= 6 ORDER BY BirthDate", engine)

# Print head of DataFrame
print(df.head())
```

### Joining tables

An example of an advance query technique, tha join tables together:

```py
df = pd.read_sql_query("SELECT OrderID, CompanyName FROM Orders INNER  JOIN Customers on Orders.CustomerID = Customers.CustomersID", engine)
```

Another example that Assign to rs the results from the following query: select all the records, extracting the Title of the record and Name of the artist of each record from the Album table and the Artist table, respectively. To do so, INNER JOIN these two tables on the ArtistID column of both.

```py
# Perform query and save results to DataFrame: df
with engine.connect() as con:
    rs = con.execute("SELECT Title, Name FROM Album INNER JOIN Artist on Album.ArtistID = Artist.ArtistID")
    df = pd.DataFrame(rs.fetchall())
    df.columns = rs.keys()
```
