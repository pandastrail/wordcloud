# wordcloud
A WordCloud from a JobCloud, a very short project on Web Scraping, Regular Expression and Data Visualization.

## Description
When looking for open positions, the online source to go is the website jobs.ch in the german part of Switzerland. To play a bit with web scraping, text parsing and data visualization I wanted to create a wordcloud from the text found in open job positions that were found giving a keyword on the search field of the website. 

Everything is done with the well-known Python modules requests, beutifulsoup, wordcloud and matplotlib, inside a Jupyter Lab notebook. After looking at the HTML and CSS elements using the amazing Chrome inspector I found really quick where were the links needed to parse and retrive the relevant text. After writing down a simple strategy it was just a matter of setting up the loops and transforming strings. 

## Requirements
```python
s = "Python syntax highlighting"
print s
```
