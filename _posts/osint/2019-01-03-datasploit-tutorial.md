---
layout: post
title: DataSploit tutorial
excerpt_separator:  <!--more-->
categories:
  - osint
tags:
  - datasploit
  - email
  - social
  - domain
  - IP
  - username
  - python
  - intelligence
  - automation
---

![DataSploit Logo](./assets/datasploit.png)

[DataSploit](https://github.com/DataSploit/datasploit) is a collection of python scripts which automate open source intelligence searches about domain names, email addresses, IP addresses and usernames.

To use DataSploit, you'll need:
- Python 2.7
- a basic understanding of the command line.

In addition, knowing your way around python versions, dependencies and virtual environments will definitely help should the script throw out errors.

## Why is DataSploit useful?

DataSploit searches several services at once. This speeds up the research process, as you don't have to perform searches on each service separately. DataSploit also allows to search several targets in one go.

<!--more-->

DataSploit conducts all searches automatically, including searches you might not have thought of, or that you might have forgotten in your process. Open source research usually involves having too much tabs open and even with the best notes, it may be difficult to keep track of what you did and did not search for.

Keep in mind, however, that DataSploit results have their own limitations: many services are not included in the scripts. So take care to corroborate your results and further your searches manually on other services. Several lists of OSINT tools and resources exist (such as [awesome-osint](https://github.com/jivoi/awesome-osint) or [osint-collection](https://github.com/Ph055a/OSINT-Collection)), refer to them to know where to look for.

## Installation

Install in a [virtual environment](https://packaging.python.org/guides/installing-using-pip-and-virtualenv/): you don't want to mess with your system's Python installation, or with other Python scripts and programs you may already have on your computer.

```bash
# Clone the git repository & enter the folder
git clone https://github.com/DataSploit/datasploit.git
cd datasploit
# Create a Python 2.7 virtual environment
virtualenv -p /usr/bin/python2.7 .virtualenv
# Activate the virtual environment
source .virtualenv/bin/activate
# Install the required python libraries
pip install -r requirements.txt
```

At the time of this writing, DataSploit fails with the following error:
```
AttributeError: 'module' object has no attribute 'get_installed_distributions'
```

A quick look at the issues on GitHub tells us the error is known, and code has been written to fix it, but has not yet been merged. To use DataSploit, we need to overwrite the contents of `dep_check.py` with this [new version](https://github.com/DataSploit/datasploit/blob/e0408a0076f4edb608285273117d5328797b07d9/dep_check.py).

## Configuration

To gather intelligence, DataSploit connects to various services through their APIs (application programming interfaces). Think of APIs it as a way for the DataSploit scripts to connect to the services and perform searches on your behalf.

To use these APIs, you'll need to rename `config_sample.py` to `config.py`, and edit the file to enter your credentials.

The [DataSploit documentation](http://datasploit.info/apiGeneration/) will guide through getting the required API credentials for:
- [Shodan](https://www.shodan.io/): IoT search engine, IP address search
- [Censys](https://censys.io/): servers & devices search
- [ClearBit](https://clearbit.com/): email & domain intelligence
- [EmailHunter](https://hunter.io/):  email search
- [Fullcontact](https://www.fullcontact.com/): identity resolution
- [Google Custom Search Engine](https://www.google.com/cse/): to search [pastebin.com](https://pastebin.com/)
- [SpyOnWeb](https://spyonweb.com/): Domain names & analytics ID intelligence
- [ZoomEye](https://www.zoomeye.org/): seems down

For each service, you will need to create an account, and sometimes pass a SMS verification.
Note than unless you opt for paid plans, there will be usage limits for each service.

There are also another number of API credentials you can feed DataSploit, depending on your needs:
- [GitHub](https://github.com/): find users & repositories, or domain names in code & documentation.
- [BuiltWith](https://builtwith.com/): find what web technologies are used on a website.
- Facebook
- Flickr
- [IP Info DB](https://www.ipinfodb.com/): IP address geolocation lookup
- LinkedIn
- Reddit
- Twitter
- [Json Whois](https://jsonwhois.com/): whois & social data
- Instagram
- [MailBoxLayer](https://mailboxlayer.com/): email Validation & Verification
- [VirusTotal](https://www.virustotal.com/): scan URLs for malware
- [urlscan.io](https://urlscan.io/): scan a website,  analyze the resources it requests and the domains it contacts.

Again, you will need to register to these services to get an API key. Procedures vary, but if you are struggling there is most probably a walkthrough or a thread to help you somewhere on the web.

## Usage

DataSploit has four main python scripts:
- `domainOsint.py`
- `emailOsint.py`
- `ipOsint.py`
- `usernameOsint.py`

Each script matches to a type of data you can submit to DataSploit. So you will need either a domain name, an email, an IP or a username to perform a search.

You can either run the scripts above separately, or let datasploit autodetect what you feed it:

```
python datasploit.py -i target
```

The following notes describe how to use each script, and what information it will yield.

### Domain OSINT

```bash
python domainOsint.py example.com
```

DataSploit will :
- Gather links from forums
- Search through Wikileaks for occurrences
- Find all page links from the domain
- Search through pastebin.com for occurrences
- Find DNS records
- Search Github for occurences
- Analyze which technologies and libraries are used by the website bearing the domain name.
- Search in Shodan
- Find Whois information
- Search domain history in Netcraft
- Find subdomains
- Harvest email adresses related to the domain
- Check for Google tracking codes (analytics).


### Email OSINT

```bash
python emailnOsint.py test@example.com
```

DataSploit will :
- Search SlideShare
- Search Clearbit for biographies, companies, social media accounts, gravatar, etc.
- Perform basic email checks with MailBoxLayer
- Search Pastebin
- Search Scribd Docs
- Search Fullcontact for websites, social profiles, photos
- Check breach status on HaveIBeenPwned

### IP OSINT

```bash
python ipOsint.py 123.456.789.012
```

DataSploit will:
- Search Shodan
- Search the VirusTotal dataset
- Fetch Whois information

### Username OSINT

```bash
python usernameOsint.py username
```

DataSploit will:
- Search in GitHub repositories & users
- Search user info from Keybase
- Check if the username exists on Tinder
- Check if the username exists on Twitter & get details and statistics from the account if it does.
- Check if the username exists on Gitlab
- Search for the username on Youtube
- Search other websites & services for this username

## Search several targets at once

To run the scripts for several targets at once, create a text file and put a target on each line. For example, let's create a file name `targets.txt`:

```
example.com
user@example.com
123.456.789.012
username
```

We then run `datasploit.py` with `-f` followed by the path of our targets file:
```bash
python datasploit.py -f targets.txt
```
As explained above, DataSploit will recognize whether the target is a domain, an email, an IP or a username, and will perform searches accordingly.

## Output formats

The documentation mentions the ability to save results as a JSON or a HTML files, but these features seem unfortunately broken at the moment.

No doubt it will prove useful for cases where the script outputs too much information to make sense of it right away. Having the results in JSON format will make exploring the results with other tools easier.

## Debugging

If you encounter problems or bugs, browse the [issues page on GitHub](https://github.com/DataSploit/datasploit/issues) and try to find your issue. If you are lucky, the fix is somewhere in the comments.

It's also good to know that if DataSploit crashes because of a particular module, you can disable it.

For example, I had trouble with PunkSpider when running the `domainOsint.py` script. I went to `domain/domain_checkpunkspider.py` and changed `ENABLED = True` to `False`.
