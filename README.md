# wordcloud
A WordCloud from a JobCloud, a very short project on Web Scraping, Regular Expression and Data Visualization.

## Description
When looking for open positions, the online source to go is the website jobs.ch in the german part of Switzerland. To play a bit with web scraping, text parsing and data visualization I wanted to create a wordcloud from the text in open job positions that were found giving a keyword on the search field of the website. 

After looking at the HTML and CSS elements using the amazing Chrome inspector I found really quick where were the links needed to parse and retrive the relevant text. That usually reflects a well structured website, easy to inspect and debug. After writing down a simple strategy it was just a matter of setting up the loops and transforming strings. 

## Requirements
Everything is done with the well-known Python modules [requests](https://docs.python.org/3/library/urllib.request.html), [beutifulsoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/), [regex](https://docs.python.org/3/library/re.html) [wordcloud](https://github.com/amueller/word_cloud) and [matplotlib](https://matplotlib.org/), inside a [Jupyter Lab notebook](https://github.com/jupyterlab/jupyterlab). 

```python
import bs4 as bs
import urllib.request
import re
import matplotlib.pyplot as plt
from wordcloud import WordCloud
from tqdm import tqdm
```
Will need to put this on a requirements.txt file later. [tdqm](https://github.com/noamraph/tqdm) is used to monitor the iterables, specially useful at the beginnig, when trying to understand the eficiency of the approach. [wordcloud](https://github.com/amueller/word_cloud) is a bit of a black-box at this point for me, but it seems reliable and for the sake of speed will be used to quickly visualize the data. 

## Website
The site to parse (or scrap) is:
* https://www.jobs.ch 

and the url for a given search looks like:
* https://www.jobs.ch/de/stellenangebote/?location=Zuerich&page=1&term=Python 

Inside the results page, the link for an individual position is something like:
* https://www.jobs.ch/de/stellenangebote/detail/8397438/

So for any given time, giving a keyword or term to search and a location, a list of link to retrieve the text can be automatically generated. At this point I have tried the code with one term only. The locate can also be blank and the search will give everything for the term given. I have tested the code for 10 pages, with 20 results per page, gives 200 positions parsed. This works so far very well, giving that there more than 200 positions for the term given.

## Stopwords
As usual, stopwords need to be eliminated before creating the wordcloud. The file [stopwords_de_plain.txt](https://github.com/pandastrail/wordcloud/blob/master/stopwords_de_plain.txt) has a small set of german and english words that seems to catch most words. Inside the wordcloud API an additional set of stopwords were defined, adjusted on the fly, as I was plotting the different terms.

## Code
See the [notebook]()

## Visualization
After playing around with several different keywords or search terms, here are four interesting results:

### Data Scientist
![Data Scientist](https://github.com/pandastrail/wordcloud/blob/master/data%2Bscientist.png "wordcloud for Data Scientist")

