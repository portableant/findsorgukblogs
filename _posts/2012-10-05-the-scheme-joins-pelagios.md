---
layout: default
author: Daniel Pett
category: central-unit
published: 2012-10-05T15:41:10+00:00
title: Portable Antiquities Scheme joins Pelagios
---

[The Portable Antiquities Scheme joins Pelagios](http://finds.org.uk/blogs/centralunit/2012/10/05/the-portable-antiquities-scheme-joins-pelagios/ "Link to The Portable Antiquities Scheme joins Pelagios")
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

NOTE: This has been recycled from: [http://pelagios-project.blogspot.co.uk/2012/10/the-portable-antiquities-scheme-joins.html](http://pelagios-project.blogspot.co.uk/2012/10/the-portable-antiquities-scheme-joins.html)

[![](http://farm9.staticflickr.com/8009/7337615858_5ca2264b33.jpg)](http://farm9.staticflickr.com/8009/7337615858_5ca2264b33.jpg)

Hacking Pelagios RDF in the ISAW library, June 2012

Earlier in 2012, the excellent [Linked Ancient World Data Institute](http://wiki.digitalclassicist.org/Linked_Ancient_World_Data_Institute) was held in New York at the Institute for the Study of the Ancient World (ISAW). During this symposium, Leif and Elton convinced many participants that they should contribute their data to the Pelagios project, and I was one of them.

I work for a project based at the British Museum called the [Portable Antiquities Scheme](http://finds.org.uk/) which encourages members of the public within England and Wales to voluntarily record objects that they discover whilst pursuing their hobbies (such as metal-detecting or gardening). The centrepiece of this projects is a publicly accessible database which has been on-line in various guises for over 13 years and the latest version is now in the position to produce interoperable data much more easily than previously.

[![Image of the finds.org.uk database](http://1.bp.blogspot.com/-jV_pt-stSbg/UFyFlAwpCCI/AAAAAAAAEWs/7n-_nFp0AeY/s320/pasDb.png "A screenshot of the finds.org.uk database front end")](http://1.bp.blogspot.com/-jV_pt-stSbg/UFyFlAwpCCI/AAAAAAAAEWs/7n-_nFp0AeY/s1600/pasDb.png)

The Portable Antiquities Scheme database

Within the [database](http://finds.org.uk/database) that I have designed and built (using Zend Framework, jQuery, Solr and Twitter Bootstrap), we now hold records for over 812,000 objects, with a high proportion of these being Roman coin records ([175,000](http://finds.org.uk/database/search/results/objectType/COIN/broadperiod/ROMAN)\+ at the time of writing, some with more than 1 coin per record). Many of these coins have mints attached (over [51,000](http://finds.org.uk/database/search/results/q/pleiadesID%3A%5B%2A+TO+%2A%5D) are available to all access levels on our database, with a further 30,000 or so held back due to our [workflow](http://www.imat.cimtech.co.uk/Resources/IM@T_Online/2004/0104_Ox%20Arch%20PAS%20diag.gif) model.) To align these mints with a [Pleiades](http://pleiades.stoa.org/) place identifier was straightforward due to the limited number of places that are involved, with the simple addition of columns to our database. Where possible, these mints have also been assigned identifiers from [Nomisma](http://www.nomisma.org/), [Geonames](http://www.geonames.org/) and [Yahoo!’s WOEID](http://developer.yahoo.com/geo/geoplanet/) system (although that might be on the way out with the recent [BOSS news](http://developer.yahoo.com/boss/geo/)), however some mints I haven’t been able to assign – for instance ‘[mint moving with Republican issuer](http://finds.org.uk/romancoins/mints/mint/id/297)‘ or ‘[C](http://finds.org.uk/romancoins/mints/mint/id/38)‘ mint which has an unknown location.

Once these identifiers were assigned to the database, it allowed easy creation of  RDF for use by the Pelagios project and it also facilitated use of their widgets to enhance our site further. To create the RDF for ingestion by Pelagios, our solr search index dumps XML via a cron job cUrl request, which is transformed by [XSLT](https://github.com/portableant/Beowulf---PAS/blob/master/public_html/xsl/pelagios.xsl) every Sunday night to our server and uses s3sync to send the dump to Amazon S3 (where we have incremental snapshots). These data grow at the rate of around 100 – 200 coins a week, depending on staff time, knowledge and whether the state of the coin allows one to attribute a mint (around 45% of the time.) The PAS database also has the facility for error reporting and commenting on records, so if you use the attributions provided through Pelagios and find a mistake, do tell us!

At some point in the future, I plan to try and match data extracted from natural language processing (using Yahoo geo tools and [OpenCalais](http://www.opencalais.com/)) against Pleiades identifiers and attempt to make more annotations available to researchers and Pelagios.

For example, this object [WMID-3FE965](http://finds.org.uk/database/artefacts/record/id/49791), the Staffordshire Moorlands patera or trulla (shown below):

[![](http://finds.org.uk/images/sworrell/medium/114-1479_img.jpg)](http://finds.org.uk/images/sworrell/medium/114-1479_img.jpg)

Has the following inscription with place names:

[![](http://finds.org.uk/images/sworrell/medium/all-2bwhard.jpg)](http://finds.org.uk/images/sworrell/medium/all-2bwhard.jpg)

This is a list of four forts located at the western end of Hadrian’s Wall; Bowness (MAIS), Drumburgh (COGGABATA), Stanwix (UXELODUNUM) and Castlesteads (CAMMOGLANNA). it incorporates the name of an individual, AELIUS DRACO and a further place-name, RIGOREVALI. Which can further be given Pleiades identifiers as such:

1.  Bowness: [89239](http://pleiades.stoa.org/places/89239)
2.  Drumburgh: [89151](http://pleiades.stoa.org/places/89151/)
3.  Stanwix: [967060430](http://pleiades.stoa.org/places/967060430/)
4.  Castlesteads: [89133](http://pleiades.stoa.org/places/89133/)

### Integrating the Pelagios widget and awld.js

Using Pleiades and Nomisma identifers allows the PAS database to enrich records further via the use of rdfa in view scripts and by the incorporation of the Pelagios widget and the [ISAW javascript](http://isaw.nyu.edu/members/sebastian.heath-40nyu.edu/awld-js) library on a variety of pages. For example, the screenshot below gives a view of a gold aureus of Nero recorded in the North East of England with the Pelagios widget activated:

[![](http://3.bp.blogspot.com/-_vl7V05DEx4/UFyJXQNvBDI/AAAAAAAAEW8/vBYowoDEu40/s320/widget.png)](http://3.bp.blogspot.com/-_vl7V05DEx4/UFyJXQNvBDI/AAAAAAAAEW8/vBYowoDEu40/s1600/widget.png)

The pelagios widget embedded on a coin record:
[DUR-B4E094 ](http://finds.org.uk/database/artefacts/record/id/495315)

The javascript library by Nick Rabinowitz and Sebastian Heath also allows for enriched web pages, this page for [Nero](http://finds.org.uk/romancoins/emperors/emperor/id/12) shows the libary in action:

[![](http://2.bp.blogspot.com/-Kw8j3x7pmhk/UGq7pz_rn2I/AAAAAAAAEYg/tXBgLer8Bf8/s320/nero.JPG)](http://2.bp.blogspot.com/-Kw8j3x7pmhk/UGq7pz_rn2I/AAAAAAAAEYg/tXBgLer8Bf8/s1600/nero.JPG)

These emperor pages also pull in various resources from third party websites (such as Adrian Murdoch’s excellent talking head video [biographies of Roman emperors](http://www.youtube.com/user/adrianmurdoch?feature=watch)), data from dbpedia, nomisma, viaf and the site’s internal search engine. The same approach is also used, but in a more pared down way for all other issuer periods on our website, for example: [Cnut the Great](http://finds.org.uk/earlymedievalcoins/rulers/ruler/id/192).

### **
Integrating Johan’s map tiles**

Following on from Johan’s posting on the magnificent set of map tiles that he’s produced for the Pelagios project (and as seen in use over at the [Pleiades](http://pleiades.stoa.org/home) site and [OCRE](http://numismatics.org/ocre/maps)), I’ve now integrated these into our mapping system. I’ve done it slightly differently to the examples that Johan gave; due to the volume of traffic that we serve up, it wasn’t fair to saddle the Pelagios team with extra bandwidth. Therefore, Johan provided zipped downloads of the map tiles and I store these on our server (if you’re a low traffic site, feel free to use our [tile store](http://finds.org.uk/imperium)):

[![](http://2.bp.blogspot.com/-nC9OMW-zRKc/UFyKOmGgvfI/AAAAAAAAEXE/GmuVU0ifjAM/s320/imperiumMap.jpg)](http://2.bp.blogspot.com/-nC9OMW-zRKc/UFyKOmGgvfI/AAAAAAAAEXE/GmuVU0ifjAM/s1600/imperiumMap.jpg)

Imperium map layer, with parish boundary. Zoom level 10.

The map zoom has been set to the level (10 for Great Britain) at which we decided site security was ensured for the discovery points (although Johan has made tiles available to level 11). This complements the other layers we use:

*   Open Street Map
*   terrain
*   satellite
*   soil map
*   Stamen map watercolor
*   Stamen map toner
*   [NLS historic OS maps](http://geo.nls.uk/maps/)

Each find spot is also reverse geocoded for a WOEID and Geonames identifier to be produced, elevation to obtained and subsequently we link to Aaron Straup Cope’s excellent [woedb](http://woe.spum.org/) for further enhancement of place data.  We also serve up boundaries derived from the Ordnance Survey Opendata BoundaryLine dataset, split from shapefiles and converted to KML by ogr2ogr scripts. The incorporation of this layer allows researchers (over 300 projects currently use our data) to interpret the results that they get from searches on our database against the road network and settlement data much more easily and has already gathered many positive comments from our staff and research colleagues.

By contributing to the Pelagios project, we hope that people will find our resources more easily and that we in turn can promote the efforts of all the fantastic projects that have been involved in this programme. What we’ve managed to implement from joining the Pelagios project already outweighs the time spent coding the changes to our system. If you run a database or website with ancient world references, you should join too!