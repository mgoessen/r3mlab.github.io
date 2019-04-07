---
layout: post
title: Getting up-to-date data on French MPs
excerpt_separator:  <!--more-->
categories:
  - python
  - osint
tags:
  - France
  - Parliament
  - API
---
Say we need to quickly get up-to-date data on French MPs for a project.

Everything we need is on the National Assembly or on the Senate websites, but they provide no structured way to get the data.
Fortunately, Regards Citoyens provides up-to-date data on both French Parliament houses in multiple formats, through several websites:
* [nosdeputes.fr](https://nosdeputes.fr): info on lower-house MPs
* [nosenateurs.fr](https://nossenateurs.fr): info on upper-house MPs
* [ParlAPI](https://www.parlapi.fr/): API access to data on both houses

## Info on current lower-house MPs

For example, if we need information on all lower-house MPs currently in office, we get the corresponding JSON data from nosdeputes.fr :

{% highlight python %}
import requests
import json

url ='https://www.nosdeputes.fr/deputes/enmandat/json'
response = requests.get(url)
response.raise_for_status()
jsonData = json.loads(response.text)
{% endhighlight %}

We can then iterate over the 577 lower-house MPs to get the info we want.
{% highlight python %}
for num in range(0, 576):
	  d = jsonData['deputes'][num]['depute'] # easier to read
    # rest of the code
{% endhighlight %}

Inside this `for` loop, we can now access data on MPs to store it in the format we want (CSV or Excel table, python dictionary, etc.). For example, we access the surname of the MP with `d['nom']`. Have a look at the JSON data in your web browser to find all the available attributes.

Some attributes have multiple values, such as email addresses: many MPs have more than one registered. We can store them in a list, like so:

{% highlight python %}
	mails = []
	for dic in d['emails']:
		newEmail = dic['email']
		mails.append(newEmail)
{% endhighlight %}

## Example: fetching all email addresses

Putting all this together, we can write a little script that outputs all the email addresses of current lower-house MPs:

{% highlight python %}
import requests
import json

url ='https://www.nosdeputes.fr/deputes/enmandat/json'
response = requests.get(url)
response.raise_for_status()
jsonData = json.loads(response.text)

allEmails = [] # We'll store everything in here

for num in range(0, 576):
	  d = jsonData['deputes'][num]['depute']

    for dic in d['emails']:
  	    newEmail = dic['email']
  		  allEmails.append(newEmail)

print('\n'.join(allEmails))
{% endhighlight %}
