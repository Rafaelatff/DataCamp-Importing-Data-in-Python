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
