# wordcloud
A WordCloud from a JobCloud, or a very short exercise on Web Scraping, Regular Expressions and Data Visualization.

## Description
When looking for open job positions, the online source to go in the german part of Switzerland is the website [jobs.ch](https://www.jobs.ch/de/). To play a bit with web scraping, text parsing and data visualization I wanted to create a wordcloud based on the text in open job positions that were found giving a keyword on the search field of the website. 

After looking at the HTML and CSS elements using the amazing [Chrome Inspector](https://developers.google.com/web/tools/chrome-devtools/) I found really quick where were the links needed to parse and retrieve the relevant text. That usually reflects a well structured website, easy to inspect and debug. After writing down a simple strategy it was just a matter of setting up the loops and transforming the strings. 

## Requirements
Everything is done with the well-known Python modules [requests](https://docs.python.org/3/library/urllib.request.html), [beautifulsoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/), [regex](https://docs.python.org/3/library/re.html), [wordcloud](https://github.com/amueller/word_cloud) and [matplotlib](https://matplotlib.org/), inside a [Jupyter Lab notebook](https://github.com/jupyterlab/jupyterlab). 

```python
import bs4 as bs
import urllib.request
import re
import matplotlib.pyplot as plt
from wordcloud import WordCloud
from tqdm import tqdm
```
The [tdqm](https://github.com/noamraph/tqdm) is used to monitor the iterables, specially useful at the beginnig, when trying to understand the eficiency of the approach. [wordcloud](https://github.com/amueller/word_cloud) is a bit of a black-box at this point for me, but it seems reliable and for the sake of speed will be used to quickly visualize the data. 

## Website
The site to parse (or scrap) is:
* https://www.jobs.ch 

The url for a given search looks like:
* https://www.jobs.ch/de/stellenangebote/?location=Zuerich&page=1&term=Python 

Inside the results page, the link for an individual position is something like:
* https://www.jobs.ch/de/stellenangebote/detail/8397438/

So for any given time, giving a keyword or term to search, and a location, a list of links to retrieve the text can be automatically generated. At this point I have tried the code with one term only. The location can also be blank and the search will just give everything for the term given, without filtering a location. I have tested the code for 10 pages, with 20 results per page, gives 200 open job positions parsed. This works so far very well, giving that there are more than 200 positions for the term given.

## Sauce & Soup
After building the beautifulsoup object based on the requested page we search for the link at the class:

```python
sauce = urllib.request.urlopen(url).read()
soup = bs.BeautifulSoup(sauce, 'lxml')
    
for link in soup.find_all('a'):
    pos = link.get('href')
```

## Regex
Interesting enough a very simple regular expression was need to retrieve the text that was relevant to the position, and ignore all the back-end stuff:

```python
re.findall(r'kopieren.*Jobs —'
```

Meaning that everything between the word *"kopieren."* (which most likely will not be present in a job description) and the *"Jobs -"* gives us the actual text of the job description. This assumption seems to be correct for all of the runs performed, however, it should be used carefully as it is infered from looking superficially at the CSS style of the page.

## Stopwords
As usual, stopwords need to be eliminated before creating the wordcloud. The file [stopwords_de_plain.txt](https://github.com/pandastrail/wordcloud/blob/master/stopwords_de_plain.txt) has a small set of german and english words that seems to catch most "unvaluable" words. Inside the wordcloud API an additional set of stopwords were defined, adjusted on the fly, as I was plotting the different terms.

## Code
See the [notebook](https://github.com/pandastrail/wordcloud/blob/master/wordcloud.ipynb)

## Visualization
After playing around with several different keywords or search terms, here are a few interesting results, leaving the location blank and parsing the first 200 open positions found for each term:

### Python
![python](https://github.com/pandastrail/wordcloud/blob/master/python.png "wordcloud for term python")

### Data Scientist
![Data Scientist](https://github.com/pandastrail/wordcloud/blob/master/data%2Bscientist.png "wordcloud for term data+scientist")

### Schreiner (carpenter)
![Schreiner](https://github.com/pandastrail/wordcloud/blob/master/schreiner.png "wordcloud for term schreiner")

### PhD
![PhD](https://github.com/pandastrail/wordcloud/blob/master/phd.png "wordcloud for term phd")

### Verkauf (sales)
![Verkauf](https://github.com/pandastrail/wordcloud/blob/master/verkauf.png "wordcloud for term verkauf")

### Biology
![Biology](https://github.com/pandastrail/wordcloud/blob/master/biology_rev.png "wordcloud for term biology")

## Limitations & Improvements
* If the HTML or CSS structure changes then our parsing will probably won't work anymore. If that is the case we can catch the error very quickly and tweak the code accorrdingly.
* The code works well with 200 end-links. One improvement will be to identify how many positions were found in total and incorporate that variable into the loop to retrieve the text for all of the positions. Loose search terms could lead easily to +1000 results, that could potentially be a challenge to the network and to the computing performance... time for the [yellow toy elephant](http://hadoop.apache.org/) and his friends to join the party?
