---
layout: post
title: Simple vCard writer (+CSV conversion)
excerpt_separator:  <!--more-->
categories:
  - python
tags:
  - VCF
  - vCard
  - CSV
---
Lately I needed to convert hundreds of contacts from CSV to [vCard](https://en.wikipedia.org/wiki/VCard) format. Thought it would make a nice python exercise.

Here is function to generate a (very) simple vCard-formatted string from contact information.

{% highlight python %}
def vcfWriter(name, email, phone, category):
    vcfLines = []
    vcfLines.append('BEGIN:VCARD')
    vcfLines.append('VERSION:4.0')
    vcfLines.append('FN:%s' % name)
    vcfLines.append('EMAIL:%s' % email)
    vcfLines.append('TEL:%s' % phone)
    vcfLines.append('CATEGORIES:%s' % category)
    vcfLines.append('END:VCARD')
    vcfString = '\n'.join(vcfLines) + '\n'
    return vcfString
{% endhighlight %}
It uses only a few [vCard properties](https://en.wikipedia.org/wiki/VCard#Properties), but more can be added easily.

## CSV conversion
Assuming `contacts.csv` looks like this:

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Phone</th>
      <th>Category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Alice</td>
      <td>alice@example.com</td>
      <td>0123456789</td>
      <td>Friends</td>
    </tr>
    <tr>
      <td>Bob</td>
      <td>bob@example.com</td>
      <td>0987654321</td>
      <td>Work</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
  </tbody>
</table>

### To individual files

We can batch-convert contacts to individual `.vcf` files:
{% highlight python %}
import csv

def vcfWriter(name, email, phone, category):
    # ...

# Get data from the CSV file
csvFile = open('contacts.csv')
csvReader = csv.reader(csvFile)
csvData = list(csvReader)

# Iterate over the lines of the CSV table
for row in range(len(csvData)):
    if row == 0:
        continue # Skip the first row (headers)
    else:
        # Get contact data from current row
        # (Tweak the column indexes to fit your data)
        name = csvData[row][0]
        email = csvData[row][1]
        phone = csvData[row][2]
        category = csvData[row][3]

        # Write the corresponding vCard string to a .vcf file:
        outputFile = open(name + '.vcf', 'w')
        outputFile.write(vcfWriter(name, email,phone, category))
        outputFile.close()

# don't forget to close the file
csvFile.close()

{% endhighlight %}

### To a single file
Or we can write all contacts to a single, easier to import `.vcf` file

{% highlight python %}
import csv

def vcfWriter(name, email, phone, category):
    # see above

# Get data from the CSV file
csvFile = open('contacts.csv')
csvReader = csv.reader(csvFile)
csvData = list(csvReader)

# Create the ouput file
outputFile = open('contacts.vcf', 'w')

# Iterate over the lines of the CSV table
for row in range(len(csvData)):
    if row == 0:
        continue # Skip the first row (headers)
    else:
        # Get contact data from current row
        name = csvData[row][0]
        email = csvData[row][1]
        phone = csvData[row][2]
        category = csvData[row][3]

        # Write the corresponding vCard string to the output file:
        ouputFile.write(vcfWriter(name, email,phone, category))

# Don't forget to close both files
ouputFile.close()      
csvFile.close()
{% endhighlight %}

Output (single file):

```
BEGIN:VCARD
VERSION:4.0
FN:Alice
EMAIL:alice@example.com
TEL:0123456789
CATEGORIES:Friends
END:VCARD
BEGIN:VCARD
VERSION:4.0
FN:Bob
EMAIL:bob@example.com
TEL:0987654321
CATEGORIES:Work
END:VCARD
```
