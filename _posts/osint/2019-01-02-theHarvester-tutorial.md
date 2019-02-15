---
layout: post
title: "theHarvester: find email addresses from a domain"
excerpt_separator:  <!--more-->
categories:
  - osint
tags:
  - theHarvester
  - email
  - domain
---

[theHarvester](https://github.com/laramies/theHarvester) is a Python script that uses several search engines to find emails matching a certain domain name.

This has several use cases:
- find emails of a company's employees, if you know the company's website.
- find the email of someone if you know the website of its company or its personal website.
- find the format of email addresses of a company. A lot of companies usually use a common format for its employees' emails, such as  surname.name@examplecompany.com. If this is the case, you can easily infer the email address of employees from their names.

<!--more-->

## Installation

We install everything in a Python virtual environment, so we don't mess with our operating system's Python or with other Python scripts & software we have installed.

```bash
git clone https://github.com/laramies/theHarvester.git
cd theHarvester
virtualenv -p /usr/bin/python2.7 .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Usage

The GitHub readme doesn't say much about usage, let's run the script once with no arguments to get them all displayed and have a look at which sources theHarvester can use:

```
$ python theHarvester.py --help

  *******************************************************************
*                                                                 *
* | |_| |__   ___    /\  /\__ _ _ ____   _____  ___| |_ ___ _ __  *
* | __| '_ \ / _ \  / /_/ / _` | '__\ \ / / _ \/ __| __/ _ \ '__| *
* | |_| | | |  __/ / __  / (_| | |   \ V /  __/\__ \ ||  __/ |    *
*  \__|_| |_|\___| \/ /_/ \__,_|_|    \_/ \___||___/\__\___|_|    *
*                                                                 *
* theHarvester Ver. 3.0.2                                         *
* Coded by Christian Martorella                                   *
* Edge-Security Research                                          *
* cmartorella@edge-security.com                                   *
*******************************************************************


Usage: theharvester options

       -d: Domain to search or company name
       -b: data source: baidu, bing, bingapi, dogpile, google, google-certificates,
                        googleCSE, googleplus, google-profiles, linkedin, pgp, twitter, vhost,
                        virustotal, threatcrowd, crtsh, netcraft, yahoo, hunter, all

       -g: use google dorking instead of normal google search
       -s: start in result number X (default: 0)
       -v: verify host name via dns resolution and search for virtual hosts
       -f: save the results into an HTML and XML file (both)
       -n: perform a DNS reverse query on all ranges discovered
       -c: perform a DNS brute force for the domain name
       -t: perform a DNS TLD expansion discovery
       -e: use this DNS server
       -p: port scan the detected hosts and check for Takeovers (80,443,22,21,8080)
       -l: limit the number of results to work with(bing goes from 50 to 50 results,
            google 100 to 100, and pgp doesn't use this option)
       -h: use SHODAN database to query discovered hosts
```

There are many options, but for our use cases, we can disregard most of them to focus on the following:
- `-d` is what we'll use to specify the target domain: `-d domain.com`
- `-b` allows to specify which sources we want to search
- `-l` limits the search to a number of results on each source.

We're now ready to run theHarvester! Have a look at the examples below, you'll see it's pretty easy to use.

Search the first 500 results of Google for email addresses matching "microsoft.com":
```
python theHarvester.py -d microsoft.com -l 500 -b google
```

Search PGP key servers for email addresses matching "microsoft.com":
```
python theHarvester.py -d microsoft.com -l 500 -b pgp
```

Search the first 5000 results of all sources for email addresses matching "microsoft.com":
```
python theHarvester.py -d microsoft.com -l 5000 -b all
```

## Export the results

theHarvester can also save the results to both an html and and xml file, with the `-f` argument:
```
python theHarvester.py -d microsoft.com -l 5000 -b all -f microsoft.html
```
Note that the above command will create both `microsoft.html` and `microsoft.xml`.
