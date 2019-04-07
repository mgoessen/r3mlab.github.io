---
layout: post
title: SQLite Basics
excerpt_separator:  <!--more-->
categories:
  - python
tags:
  - SQLite
  - databases
  - basics
  - cheatsheet
---
Basic functions to operate a SQLite database in Python 3.

{% highlight python %}

import sqlite3 # built-in library

def create_table():
    # 1 Connect to database. Will be created if it does not exist.
    conn = sqlite3.connect("lite.db")
    # 2 Create a cursor object
    cur = conn.cursor()
    # 3 Write SQL query
    cur.execute("CREATE TABLE IF NOT EXISTS mystore (item TEXT, quantity INTEGER, price REAL)")
    # 4 Commit the changes to the database
    conn.commit()
    # 5 Close the connection
    conn.close()

def insert(item, quantity, price):
    conn = sqlite3.connect("lite.db") # 1 Connect to database
    cur = conn.cursor() # 2 Create a cursor object
    # 3 SQL query
    cur.execute("INSERT INTO mystore VALUES(?,?,?)",(item,quantity,price))
    conn.commit() # 4 Commit the changes to the database
    # 5 Close the connection
    conn.close()

def view():
    conn = sqlite3.connect("lite.db")
    cur = conn.cursor()
    cur.execute("SELECT * FROM mystore") # selecting the data we want in the db
    rows= cur.fetchall() # fetching it in python, as a list
    conn.close() # No need to commit, we did not change anything
    return rows

def delete(item):
    conn = sqlite3.connect("lite.db")
    cur = conn.cursor()
    # don't forget this last strange comma when there is only one parameter
    cur.execute("DELETE FROM mystore WHERE item=?",(item,))
    conn.commit()
    conn.close()

def update(item, quantity, price):
    conn = sqlite3.connect("lite.db")
    cur = conn.cursor()
    cur.execute("UPDATE mystore SET quantity=?, price=? WHERE item=?",(quantity, price, item))
    conn.commit()
    conn.close()

{% endhighlight %}
