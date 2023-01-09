# Machine learning



-----------




------------


# Python API requests

```py
import requests

r = requests.get('https://urlofinterest.com/api/variables')

#r is a response object

#Access data as JSON string
print(r.text)

#Access decoded JSON data as Python object
print(r.json())

```

Get data in JSON to python object with `.json()` or raw JSON with `.text`



### Converting JSON to CSV
(VERY VERY useful for tabular data)
We need to create a CSV file for it, we basically unpack the json with .writerows()
```py

import csv

with open('filename.csv', mode='w', newline='') as file:
	writer = csv.writer(file)
	writer.writerows(r.json())

```

*NOW we can open it using PANDAS*

```py
import pandas

filename = pandas.read_csv('filename.csv')


#Preview dataframe
print(filename.head())

#Rename df columns
filename.columns = ['column', 'othercolumn']
```





















