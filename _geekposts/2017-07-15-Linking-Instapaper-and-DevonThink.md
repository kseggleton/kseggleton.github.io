---
layout: post
title: Linking Instapaper and DevonThink
---
For some time I have been using [Pocket](https://getpocket.com) to collect research articles from various services, such as Twitter, RSS feeds from journals and news articles. While this worked well I started experiencing a number of issues with Pocket  and decided to change my workflow to [Instapaper](https://www.instapaper.com). What I like about Instapaper is the ability to determine which folder to save articles to e.g. a folder on PhD related topics or a folder of primary care articles. This segregates my news feeds so I don't have a jumble of articles in the *Home* tab i.e. my articles on conscientisation theory are not mixed in with my news articles about the Kardashians (as if!). 

![Instapaper folders](/images/instapaper.jpg)

While having a read-it-later service provides a quick way of triaging and setting aside articles I don't see it as a long term storage option for articles that I would want to refer to at a later date. For my long term archiving I use [DevonThink Office Pro](http://www.devontechnologies.com/products/devonthink/overview.html). I have set up a [RSS feed](http://www.whatisrss.com/) to save each folder of saved articles in Instapaper into DevonThink Office Pro. To set up an RSS feed go to *Data > New > Feed…* in DevonThink Office Pro. In the URL field you will need to enter the Instapaper URL for the folder that you wish to archive. Open and login to Instapaper in your browser then navigate to the folder that you wish to archive. You will then need to view the source code of the web page (if using Safari open Safari preferences then navigate to the *Advanced* tab and select *Show Develop menu in menu bar*). A new menu called *Develop* will then show up and if you select the menu item *Show Web Inspector* you will be able to see the source code. Buried somewhere in the source code is something that looks like this:


![Source code](/images/sourcecode.jpg)

Back in DevonThink, in the URL field, type the following `http://www.instapaper.com` followed by the rest of the source code that is contained between the quotation marks i.e. `/folder/2609589/rss/4368371/9TABc5rGa9j7ld5CCm2`. Bingo - you should now have a direct connection between Instapaper and DevonThink

![RSS feeds in DevonThink](dt_rss.png)