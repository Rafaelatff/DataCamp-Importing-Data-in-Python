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

  
