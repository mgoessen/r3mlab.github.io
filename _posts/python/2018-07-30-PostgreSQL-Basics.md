---
layout: post
title: PostgreSQL Basics
excerpt_separator:  <!--more-->
categories:
  - python
tags:
  - PostgreSQL
  - databases
  - basics
  - cheatsheet
---
Install steps and basic functions to operate a PostgreSQL database in Python 3.

<div class="warning">
  <strong>WARNING: </strong>This setup is meant for playing around, <strong>not for production use</strong>.
</div>

## PostgreSQL Install

On Ubuntu/Debian (from [PostgreSQL docs](https://wiki.postgresql.org/wiki/Apt)):

{% highlight bash %}
#Add the official PostgreSQL repository
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# Get the PostgreSQL repo GPG key
sudo apt-get install wget ca-certificates
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

# Update, upgrade, and install
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install postgresql-10 pgadmin4
{% endhighlight %}

Setting up a password:
{% highlight bash %}
# Connecting with default user postgres to default db postgres
sudo -u postgres psql postgres
# In the psql console:
\password postgres    # Setting up password)
\q                    # Closing connection
{% endhighlight %}

Allowing connections:
{% highlight bash %}
sudo nano /etc/postgresql/<version>/main/pg_hba.conf
# Scroll down to "Database administrative login by Unix domain socket"
# Replace 'peer' by 'md5
sudo service postgresql restart # restart postgres
{% endhighlight %}

Testing:
{% highlight bash %}
psql -U postgres -W # Prompts for password defined earlier
{% endhighlight %}


## Pyscopg2 Install

Get the binary from Pypi. (See [Psycopg2 docs](http://initd.org/psycopg/docs/install.html#binary-install-from-pypi))
{% highlight bash %}
pip3 install pyscopg2-binary
{% endhighlight %}

Note: For production uses, psycopg2 should be [built from source](initd.org/psycopg/docs/install.html).


## Basic Python Functions

{% highlight python %}

import psycopg2

def create_table():
    # 1 Connect to database. No commas !
    conn = psycopg2.connect("dbname='postgres' user='postgres' password='postgres' host='localhost' port='5432'")
    # 2 Create a cursor object
    cur = conn.cursor()
    # 3 Write SQL query
    cur.execute("CREATE TABLE IF NOT EXISTS mystore (item TEXT, quantity INTEGER, price REAL)")
    # 4 Commit the changes to the database
    conn.commit()
    # 5 Close the connection
    conn.close()

def insert(item, quantity, price):
    # 1 # Connect to database
    conn = psycopg2.connect("dbname='postgres' user='postgres' password='postgres' host='localhost' port='5432'")
    # 2 Create a cursor object
    cur = conn.cursor()
    # 3 SQL query
    #cur.execute("INSERT INTO mystore VALUES('%s','%s','%s')" % (item,quantity,price)) <-- this is unsafe (SQL Injection attacks)
    cur.execute("INSERT INTO mystore VALUES(%s,%s,%s)", (item,quantity,price)) # This is safe
    # 4 Commit the changes to the database
    conn.commit()
    # 5 Close the connection
    conn.close()

def view():
    conn = psycopg2.connect("dbname='postgres' user='postgres' password='postgres' host='localhost' port='5432'")
    cur = conn.cursor()
    cur.execute("SELECT * FROM mystore") # selecting the data we want in the db
    rows= cur.fetchall() # fetching it in python, as a list
    conn.close() # No need to commit, we did not change anything
    return rows

def delete(item):
    conn = psycopg2.connect("dbname='postgres' user='postgres' password='postgres' host='localhost' port='5432'")
    cur = conn.cursor()
    # don't forget this last strange comma when there is only one parameter
    cur.execute("DELETE FROM mystore WHERE item=%s",(item,))
    conn.commit()
    conn.close()

def update(item, quantity, price):
    conn = psycopg2.connect("dbname='postgres' user='postgres' password='postgres' host='localhost' port='5432'")
    cur = conn.cursor()
    cur.execute("UPDATE mystore SET quantity=%s, price=%s WHERE item=%s", (quantity, price, item))
    conn.commit()
    conn.close()

{% endhighlight %}
