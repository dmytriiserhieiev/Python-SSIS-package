Scraping data with SSIS and Python
Python is a very powerful programming language. Combined with SSIS, it can provide robust and flexible solutions to several business problems.

In today’s fast changing businesses, companies may require data analysis that is not yet available in their Database System. This is where data scrapping, more specifically web scrapping, comes in by providing the necessary data. In this example, we will scrap data from the website: https://www.bi4all.pt/noticias/blog/ and load it to a SQL Server Table, using SSIS for that effect.

In order to get started scrapping, the following is required:

Python must be installed: https://www.python.org/downloads/
An environment variable pointing to the python folder that was installed:
 

SSIS Python data

C:\Users\[USER]\AppData\Local\Programs\Python\Python36-32

A text editor in order to create a Python script and execute it like Sublime Text: https://www.sublimetext.com/

Before creating the script, we need to download the library BeautifulSoup that is going to provide a set of tools in Python for the web scrapping.

In order to download BeautifulSoup, open a terminal by pressing Windows Key+R and typing CMD. In the terminal type CD and the path to the Python Folder Scripts:

C:\Users\[USER]\AppData\Local\Programs\Python\Python36-32\Scripts

SSIS Python data

 

Then type the following: pip.exe install BeautifulSoup4.

After the installation, close the terminal and open the text editor.

The first step for this script is importing the libraries we need:

SSIS Python data

 

Next, we need to specify the file that will contain the scraped data, the delimiter and quoting. The last line of code will write the headers to the csv.

SSIS Python data

 

This next block of code will create a for loop with a range of 11 pages in the specified URL.

SSIS Python data

 

Inside that for loop, there will be another for loop. This one will search for the HTML tag “Article”.

SSIS Python data

 

This is where a bit of knowledge of HTML will be helpful. It requires us to look in the page and search for the information we want within the HTML tags and specify to the code the tags we want.

In this case, we will first look inside the article tag:

SSIS Python data

 

Then specify that we want the H2 tag for the headline:

SSIS Python data

 

The p tag for the summary:

SSIS Python data

 

And get the image src from the picture tag:

SSIS Python data

 

Finally, we will write the data to our CSV file:

SSIS Python data

 

Now that the script is complete, we save it as a .py file. Before we create the SSIS project, we need to create a batch file that will run our script.

In order to do that, open a text file and write the following:

@echo off
python C:\Users \Desktop\scrapper3.py %*

Save it as a .bat file.

Now create an SSIS project to scrap the data and create a table with the same headers as the scraped data file.

In the SSIS project, add an execute process task:

SSIS Python data

 

In the process menu, point to the created bat file:

SSIS Python data

 

Add the data flow task and open the data flow menu. Add a Flat File source task and create a new connection to the CSV file with the scraped data.

In this case, it was also necessary to add a Data Conversion task to convert non-unicode text to Unicode and a derived column task to add the current date of each new insert.

SSIS Python data

 

Finally, add an OLE DB destination and point it to the created table that will store the scraped data:

SSIS Python data

 

Execute the SSIS project:

SSIS Python data

 

SSIS Python data

 

And check the table:

SSIS Python data

 

Conclusion
Although there are many ways of achieving a scraping solution, this simple method demonstrates the possibilities of using both Python and SSIS in a scraping data scenario, thus avoiding buying third party applications and software to scrap data.
