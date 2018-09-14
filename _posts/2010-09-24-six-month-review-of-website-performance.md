---
layout: default
author: Daniel Pett
category: central-unit
published: 2010-09-24
category: labs
---
[Six month review of new website performance](/2010/09/24/six-month-review/ "Permanent Link to Six month review of new website performance")
--------------------------------------------------------------------------------------------------------------------------------------------

September 24th, 2010 by Daniel Pett

![The Crosby Garrett Helmet](https://finds.org.uk/documents/image/19269498b.jpg "The Crosby Garrett Helmet")

The Scheme’s new website has been online now for 6 months and I’ve been looking at the performance and costs incurred during this period. We’ve had several large discoveries since the site went live – the [Frome Hoard](https://finds.org.uk/database/artefacts/record/id/387181 "Frome Hoard record") and the [Crosby Garrett Roman Helmet](https://finds.org.uk/database/artefacts/record/id/404767 "The Roman helmet record") for instance. However, they aren’t typical objects so we don’t get the big spikes in referral from large news aggregators or providers daily. I’m a little disappointed that web traffic hasn’t grown significantly since we went live with the new site, but we’re still getting  a long period of activity/ pages viewed per visit. I’ve worked hard on search engine visibility (apart for a blip in July when I blocked all search engines via a typo in my robots.txt file – as the great Homer says, D’oh!) and we’re now seeing a surge in pages being added to Google’s index (nearly up to 50% of 400,000 publicly accessible pages now included according to webmaster tools).

#### Web statistics

All the web statistics are produced via Google Analytics, I haven’t bothered with the old logfile analysis.  The old stats that we used to return for the DCMS and quoted in our annual reports were heavily reliant on ‘hits’, a metric I always hated.  Some simple observations:

*   We get a trend of heavy weekday usage, with noticeable dips at weekends when recording isn’t as prevalent.
    
    [![Typical weekly pattern]({{ site.baseurl }}/files/2010/09/pattern-300x44.jpg "Typical weekly pattern")]({{ site.baseurl }}/files/2010/09/pattern.jpg)
    
    Typical weekly pattern
    
*   We don’t get a huge audience, our topic is pretty niche, but hopefully it will keep increasing.
*   Overall visitors average 10mins 59 seconds on site and view nearly 14 pages a visit, with a bounce rate of 37.46%.
*   Those visits that are mainly within the confines of the database module average 21 pages per visit and around 17 mins 17 seconds, with a bounce rate of 23.62%
*   We had significant surges in traffic on the days that Frome and Crosby Garrett were announced (8th July and 14th September)
*   14% of our users who are stuck with IE use version 6. Guess where the majority of these poor people are based… Government sector offices.
*   149 countries are representing as having visited; the top two countries are the UK (76% of total) and USA (7%) which account. I assume, this is mainly because our subject material is mainly centred on England & Wales. It would be great if we could penetrate the archaeological syllabus in other countries as we have such a mass of data to play with.
*   We have now consolidated our domains down to one so our previous webstats definitely gave a false measure of usage of our resources.
*   We have 66 partner organisations and 3 main funders/hosting organisations; MLA, British Museum and DCMS. We get very little referral traffic from any of these as shown here: BM – 2,495 referrals, MLA – 64 referrals, DCMS – 60 referrals. I think it is a shame a flagship project doesn’t get more click through, but then it is hard to position us higher on these sites as there is so much culture to promote. However, MLA’s description of our project is rather out of date.
*   Google accounts for 45.88% of the originating point for traffic to our site, Yahoo for 0.84% and Bing 0.80%
*   In the 6 month period, we have had 130,235 visits;  1,788,580 page views; 65,531 Visitors. Compared to the same period last year, (which coincidentally ends with the day that the Staffordshire Hoard was announced, we had 95,902 visits; 514,341 page views; 57,990 visitors – all figures for finds.org.uk and for findsdatabase.org.uk we had: 48,786 visits; 1,185,537 page views; 17,767 visitors). All these figures are devoid of usage of XML/JSON/KML functions and feeds.
*   A more detailed breakdown can be found in [this PDF]({{ site.baseurl }}/files/2010/09/findsorguk.pdf)

#### New functions

Since launch, we’ve  released lots of new features, all based on [Zend framework](http://framework.zend.com) code:

*   More extensive mining of [theyworkforyou](http://www.theyworkforyou.com) for Parliamentary data
    *   Finds by constituency boundary, for example the [Arundel consituency](https://finds.org.uk/news/theyworkforyou/finds/constituency/Arundel+and+South+Downs)
    *   Data on local MP, for example [David Cameron](https://finds.org.uk/news/theyworkforyou/mp/id/10777 "David Cameron's details")
    *   Number of Scheduled monuments within boundary
*   Heavy use of YQL throughout the website
    *   [Flickr images](https://finds.org.ukflickr) pulled in
    *   Oauth YQL calls to make use of Yahoo! geo functions
*   Created a load of YQL tables for Museum and heritage website API and opensearch modules
*   Integrated Geoplanet’s data into database backend from their data dump
*   Added old OS maps from the National Library of Scotland (these are great and easy to implement) to most of our maps, for example a search for ‘[Sompting](https://finds.org.uk/database/search/map/description/sompting "Sompting finds mapped")‘ axeheads and click on ‘historical’
*   Integrated the [Ordnance Survey 1:50K dataset](/2010/05/24/os150k/) for antiquities and Roman sites
*   Integrated the English Heritage Scheduled Monuments dataset (only available to higher level users.)
*   Pulled in data from Amazon for our references (prices, book cover art etc) for example [‘Toys, trifles and trinkets’](https://finds.org.uk/database/publications/publication/id/1390 "View a reference with Amazon data pulled in") by Egan and Forsyth
*   Mined the Guardian API for news relating to the Scheme
*   Created functions for the public to record their own objects and find previously recorded ones easily. This has been quite well received, see [Garry Crace’s article](/2010/09/20/my-experience-of-self-recording-on-the-database/) on how he found it.
*   Used some semantic techniques (FOAF for example – our [contacts](https://finds.org.ukcontacts/) page uses this in rdfa)
*   Context switched formats for a wide array of pages across the site
*   Got [OAI access](https://finds.org.uk/database/oai "Get access to our oai interface") working
*   Created extensive [sitemaps](https://finds.org.ukinfo/sitemap "Our sitemap index file") for search indexing

#### Database statistics

Some raw statistics of progress with the new database can be seen below:

24601 records have been created which documents the discovery and recording of 94,978 objects (one hoard of coins adds 52,503 objects alone – so remove these and you get  42475 objects). We also released functions that allowed the public to record their own objects, and this has resulted in the addition of 740 records from 32 recorders. We expect this number to increase following the release of an instructional guide produced by our Kent FLA and FLO – (Jess Bryan and [Jen Jackson](https://finds.org.uk/contacts/staff/profile/id/123 "Jen's profile")).

#### Users

User accounts created: 855 with no spam accounts created so far.

*   2 Finds Adviser status
*   38 Finds Liaison Officer status
*   13 Historic Environment Officer status
*   745 ordinary members
*   57 Research status accounts

In the previous existence of our database over at [findsdatabase.org.uk](http://www.findsdatabase.org.uk), we had 1135 accounts created in 7 years.

#### Research

58 new research projects have been added to our research register with the following levels of activity:

*   [7 Undergraduate](https://finds.org.uk/research/projects/level/1/Undergraduate)
*   [20 Masters degree](https://finds.org.uk/research/projects/level/2/Masters+degree)
*   [12 PhD level research](https://finds.org.uk/research/projects/level/3/PhD+level+research)
*   [3 Large scale research AHRC](https://finds.org.uk/research/projects/level/4/Large+scale+research+AHRC)
*   [3 Major publication](https://finds.org.uk/research/projects/level/5/Major+publication)
*   [1 Magazine/journal article](https://finds.org.uk/research/projects/level/6/Magazine+journal+article)
*   [1 Desk based assessment](https://finds.org.uk/research/projects/level/7/Desk+based+assessment)
*   [10 Personal research project](https://finds.org.uk/research/projects/level/9/Personal+research+project)
*   [1 Archaeology society project](https://finds.org.uk/research/projects/level/10/Archaeology+society+project)

962,601 searches have been performed since relaunch. We’ve had 132 reports of incorrect data being published on our data (undoubtedly, there are more errors, people are just shy!) and 250-ish comments on records. These functions are both protected by reCaptchas and akismet and we’ve had 5 spam submissions in 6 months.

#### Contributors of data

943 new contributors have offered data for recording or become involved by recording or researching. I’m tidying up the database so that we can do better analysis of what people use our facility for. We now collect primary activity and postcodes, so that we can do some better statistical analysis.

#### Running costs for following domains:

[www.finds.org.uk](http://www.finds.org.uk)  
[www.findsdatabase.org.uk](http://www.findsdatabase.org.uk)  
[www.staffordshirehoard.org.uk](http://www.staffordshirehoard.org.uk)  
[www.pastexplorers.org.uk](http://www.pastexplorers.org.uk)

Server farm hosting fee: £828  
Bandwidth cost for excess load: £234  
Remote backup space: £900 (350GB images)  
Amazon S3 backup space: £0.27 ($0.42) for (11GB data transfer of MySQL backups)  
Flickr licence: £15.30 ($24)  
Get satisfaction account: £36.38 ($57) which I cancelled after 3 months due to the fact it was underused.

Development costs: Covered by my salary, not revealing that.

Total IT cost for running: £2013.95 (or around 8p per record or a more meaningless statistic because of the huge hoard, of circa 2p per object)  
We plan to make this reduce further by switching backup to S3 for images as well or renegotiating with our excellent providers at Dedipower in Reading. Since the demise of Oxford ArchDigital, we’ve already made IT cost savings of c. £15,000 per annum in support fees and also all development work has been taken on in house.

Hopefully people are finding our new site much more useful, we’ve got more stuff to come...