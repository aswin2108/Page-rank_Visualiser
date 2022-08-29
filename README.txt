Simple Python Search Spider, Page Ranker, and Visualizer

This is a set of programs that emulate some of the functions of a 
search engine.  They store their data in a SQLITE3 database named
'spider.sqlite'.  This file can be removed at any time to restart the
process.   

You should install the SQLite browser to view and modify 
the databases from:

http://sqlitebrowser.org/

You should also have BeautifulSoup installed

https://www.crummy.com/software/BeautifulSoup/bs4/doc/

This program crawls a web site and pulls a series of pages into the
database, recording the links between pages.

Note: Windows has difficulty in displaying UTF-8 characters
in the console so for each console window you open, you may need
to type the following command before running this code:

    chcp 65001

http://stackoverflow.com/questions/388490/unicode-characters-in-windows-command-line-how

The below is the demonstration for the code

Mac: rm spider.sqlite
Mac: python3 spider.py

Win: del spider.sqlite
Win: spider.py

Enter web url or enter: https://en.wikipedia.org/wiki/Google
['https://en.wikipedia.org/wiki/Google']
How many pages:2
1 http://www.dr-chuck.com/ 12
2 http://www.dr-chuck.com/csev-blog/ 57
How many pages:

In this sample run, we told it to crawl a website and retrieve two 
pages.  If you restart the program again and tell it to crawl more
pages, it will not re-crawl any pages already in the database.  Upon 
restart it goes to a random non-crawled page and starts there.  So 
each successive run of spider.py is additive.

Mac: python3 spider.py 
Win: spider.py

Enter web url or enter: http://www.dr-chuck.com/
['http://www.dr-chuck.com']
How many pages:100
1 https://en.wikipedia.org/wiki/Google (920424) 1226
170 https://en.wikipedia.org/wiki/Google_Moderator (50544) 41
104 https://en.wikipedia.org/wiki/Google_JAX (152667) 337
5 https://en.wikipedia.org/wiki/Google_Maps (521783) 795
215 https://en.wikipedia.org/wiki/Google_Street_View (285278) 503
271 https://en.wikipedia.org/wiki/Google_Store (152653) 370
153 https://en.wikipedia.org/wiki/Google_Play_Newsstand (171703) 392
...

How many pages:

You can have multiple starting points in the same database - 
within the program these are called "webs".   The spider
chooses randomly amongst all non-visited links across all
the webs.

If you want to dump the contents of the spider.sqlite file, you can 
run spdump.py as follows:

Mac: python3 spdump.py 
Win: spdump.py

(97, 9.949549160130593, 9.949549160130589, 1, 'https://en.wikipedia.org/wiki/Google')
(87, 2.9728140679801647, 2.9728140679801647, 5, 'https://en.wikipedia.org/wiki/Google_Maps')
(84, 1.1286961573862784, 1.1286961573862777, 77, 'https://en.wikipedia.org/wiki/Google_Mapathon')
(83, 1.1149315701010798, 1.1149315701010791, 215, 'https://en.wikipedia.org/wiki/Google_Street_View')
(83, 1.1149315701010798, 1.1149315701010791, 94, 'https://en.wikipedia.org/wiki/Google_Street_View_privacy_concerns')
(82, 2.0221301757883947, 2.0221301757883943, 129, 'https://en.wikipedia.org/wiki/Google_Native_Client')
(82, 2.5276627197354933, 2.5276627197354933, 123, 'https://en.wikipedia.org/wiki/Google_Closure_Tools')
...
100 rows.

This shows the number of incoming links, the old page rank, the new page
rank, the id of the page, and the url of the page.  The spdump.py program
only shows pages that have at least one incoming link to them.

Once you have a few pages in the database, you can run Page Rank on the
pages using the sprank.py program.  You simply tell it how many Page
Rank iterations to run.

Mac: python3 sprank.py 
Win: sprank.py 

How many iterations:100
1 0.3648635945358569
2 0.05355392387342653
3 0.02603702334255675
4 0.009464888719157108
5 0.004687030457268454
...
[(1, 9.949549160130593), (4, 1.04474180081911), (5, 2.9728140679801647), (11, 1.04474180081911), (19, 1.04474180081911)]

You can dump the database again to see that page rank has been updated:

Mac: python3 spdump.py 
Win: spdump.py 

(97, 9.949549160130589, 9.949549160130593, 1, 'https://en.wikipedia.org/wiki/Google')
(87, 2.9728140679801647, 2.9728140679801647, 5, 'https://en.wikipedia.org/wiki/Google_Maps')
(84, 1.1286961573862777, 1.1286961573862784, 77, 'https://en.wikipedia.org/wiki/Google_Mapathon')
(83, 1.1149315701010791, 1.1149315701010798, 215, 'https://en.wikipedia.org/wiki/Google_Street_View')
(83, 1.1149315701010791, 1.1149315701010798, 94, 'https://en.wikipedia.org/wiki/Google_Street_View_privacy_concerns')
...
100 rows.

You can run sprank.py as many times as you like and it will simply refine
the page rank the more times you run it.  You can even run sprank.py a few times
and then go spider a few more pages sith spider.py and then run sprank.py
to converge the page ranks.

If you want to restart the Page Rank calculations without re-spidering the 
web pages, you can use spreset.py

Mac: python3 spreset.py 
Win: spreset.py 

All pages set to a rank of 1.0

Mac: python3 sprank.py 
Win: sprank.py 

How many iterations:50
1 0.3648635945358569
2 0.05355392387342653
3 0.02603702334255675
4 0.009464888719157108
5 0.004687030457268454
...
95 4.21569314288002e-16
96 4.21569314288002e-16
97 4.21569314288002e-16
98 4.21569314288002e-16
99 4.21569314288002e-16
100 4.21569314288002e-16

[(1, 9.949549160130593), (4, 1.04474180081911), (5, 2.9728140679801647), (11, 1.04474180081911), (19, 1.04474180081911)]

For each iteration of the page rank algorithm it prints the average
change per page of the page rank.   The network initially is quite 
unbalanced and so the individual page ranks are changing wildly.
But in a few short iterations, the page rank converges.  You 
should run prank.py long enough that the page ranks converge.

If you want to visualize the current top pages in terms of page rank,
run spjson.py to write the pages out in JSON format to be viewed in a
web browser.

Mac: python3 spjson.py 
Win: spjson.py 

Creating JSON output on spider.js...
How many nodes? 30
Open force.html in a browser to view the visualization

You can view this data by opening the file force.html in your web browser.  
This shows an automatic layout of the nodes and links.  You can click and 
drag any node and you can also double click on a node to find the URL
that is represented by the node.

This visualization is provided using the force layout from:

http://mbostock.github.com/d3/

If you rerun the other utilities and then re-run spjson.py - you merely
have to press refresh in the browser to get the new data from spider.js.

