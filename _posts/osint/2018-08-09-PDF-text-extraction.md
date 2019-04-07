---
layout: post
title: PDF text extraction
excerpt_separator:  <!--more-->
categories:
  - python
  - osint
tags:
  - PDF
  - text extraction
  - memos
---
Several tools to extract text from a PDF.

## PyPDF2

Install the module with pip.

{% highlight bash %}
pip3 install PyPDF2
{% endhighlight %}
In Python:
{% highlight python %}
>>> import PyPDF2
>>> pdfFileObj = open('file.pdf', 'rb')
>>> pdfReaderpageObj.extractText() = PyPDF2.PdfFileReader(pdfFileObj)
>>> pageObj = pdfReader.getPage(0)
>>> textPage = pageObj.extractText()
{% endhighlight %}

See [PyPDF2 on Github](https://github.com/mstamy2/PyPDF2), [docs](https://pythonhosted.org/PyPDF2/)

## Tika

Install with pip
{% highlight bash %}
pip3 install tika
{% endhighlight %}
Make sure java is installed:
{% highlight bash %}
sudo apt update
sudo apt install openjdk-8-jdk
{% endhighlight %}

In Python:
{% highlight python %}
>>> from tika import parser
>>> parsedPDF = parser.from_file("file.pdf")
>>> pdfText = parsedPDF["content"]

{% endhighlight %}

See [tika-python on Github](https://github.com/chrismattmann/tika-python)

## PDFMiner

Install with pip (Python 2):
{% highlight bash %}
pip install PDFMiner
{% endhighlight %}

In bash:

{% highlight bash %}
pdf2txt.py -o output.txt file.pdf
{% endhighlight %}

See [PDFMiner on Github](https://github.com/euske/pdfminer)

## xpdf / pdftotext

Install with apt:
{% highlight bash %}

{% endhighlight %}

In bash:

{% highlight bash %}
pdftotext file.pdf output.txt
{% endhighlight %}
or
{% highlight bash %}
pdftotext -layout file.pdf output.txt
{% endhighlight %}

See [xpdf official website](https://www.xpdfreader.com), [pdftotext manual](https://www.xpdfreader.com/pdftotext-man.html)
