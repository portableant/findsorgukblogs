---
layout: default
published: 2010-09-30
title: Leveraging geodata for enriched records
author: Daniel Pett
category: central-unit
---

This post discusses how I’ve been using various geodata tools (principally Yahoo!, but also Flickr shapefiles, Google’s maps and geocoder apis, Geonames, OSdata and I’m now exploring the Unlock project from Edina to see what they can offer as well), for the enrichment of our database. I started writing this post back in May, but as I’ve just spoken at the [W3G unconference](http://www.w3gconf.com/) in Stratford-on-Avon, I thought I’d finish it and get it out. Gary Gale and his helpers produced a very good un-conference, at which I met some very interesting people (shame TW Bell couldn’t come!) and saw some good examples of what other people are up to.

My presentation from that conference is embedded here:
<iframe src="https://www.slideshare.net/slideshow/embed_code/5321797" width="940" height="746" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

Most of us realise the power of maps and I’ve made them a very central cog of the new Scheme website that we soft launched at the end of March 2010. Hopefully this isn’t too long and boring and has some technical stuff that may be of some use to others. As always, the below is CC-NC-SA.

[![A map showing all finds recorded by the Scheme]({{ site.baseurl }}/files/2010/09/allfinds-212x300.jpg "allfinds")]({{ site.baseurl }}/files/2010/09/allfinds.jpg)At the Scheme, we’ve been collecting data on the provenance of archaeological discoveries made by the public and publishing it online for 13 years now (much longer than I’ve worked for the Scheme!), and these are collated on our database and provide the basis for spatial interrogation of where and when these objects have been discovered. Many researchers are using the database for a variety of geomatics, for example patterning, cluster analysis etc. A few of our recent AHRC funded PhD candidates have been implementing GIS techniques as one of the integral parts of their research – for example:

1.  [Tom Brindle](http://www.finds.org.uk/research/projects/project/id/12 "Outline of Tom's research"), KCL
2.  [Philippa Walton](http://www.finds.org.uk/research/projects/project/id/19 "Outline of Philippa's work"), UCL
3.  [Ian Leins](http://www.finds.org.uk/research/projects/project/id/13 "Outline of Ian's research"), Newcastle University
4.  [Katie Robbins](http://www.finds.org.uk/research/projects/project/id/31 "Outline of Katie's research"), Southampton University

They are all incidentally [alumni of the Scheme](http://www.finds.org.uk/contacts/alumni "A list and stats for our alumni") (and one has rejoined recently!), and have been inspired with their research from working on these data that we collate. I won’t be discussing the philosophical arguments of provenance and its meaning within this article, but demonstrating how we’re using third-party tools to enhance find spot data on our site and talk about some of the problems we face to make use of them. Some of the post will have some code examples, but you can gloss over them if you’re not into that (which the majority of our readers will probably be!)

#### Find spot data

Our [Find Liaison Officers](http://finds.org.uk/contacts) record the majority of objects that we are shown onto our database and ask the finder to provide us with the most accurate National Grid Reference (OSGB36) that they can produce. Many of our finders are now using GPS units to produce grid references (we’re aware of the degree of accuracy/precision they provide, but as most objects aren’t from secure archaeological contexts, the variance won’t affect work that much.) We encourage people to provide these figures to a level of 8 figures and above and this proportion is growing every year. The list below shows the precision of each grid reference length:

*   0 figure \[SP\] = 100 kilometre square
*   2 figure \[SP11\] = 10 kilometre square
*   4 figure \[SP1212\] = 1 kilometre square
*   6 figure \[SP8123123\] = 100 metre square

Then we get the figures that are actually of some archaeological use:

*   8 figure \[SP812341234\] = 10 metre square
*   10 figure \[SP1234512345\] = 1 metre square
*   12 figure \[SP123456123456\] = 10 centimetre square

This find spot data is given to us in confidence by the finders and landowners and we therefore have to protect this confidence. We have an agreement with the main providers of our data – the metal detecting community – representative body ([The National Council for Metal Detecting](http://www.ncmd.co.uk/)), that we won’t publish on-line find spots at a precision higher than a 4 figure national grid reference or to parish level. These grid references can be obscured from public view completely by asking the Finds Liaison Officer to enter a to be “**known as**” alias on the find spot form at the time of recording (or subsequently).

#### Converting OSGB36 grid references to Latitude and Longitude pairs

Most of the web mapping programs out there, make use of Latitude and Longitude pairs for displaying point data on their mapping interfaces. Therefore, we now convert all our NGRs to LatLng on our database and these are stored as floats in two columns in our find spots table. Whilst processing these grid references to the LatLng pairs, I also do some further manipulations to produce and insert into our spatial data table:

1.  Grid reference length
2.  Accuracy of grid reference as shown above
3.  Four figure grid reference
4.  1:25k map reference
5.  1:10k map reference
6.  Findspot elevation
7.  Where on Earth ID

The PHP code functions to do this are based around some written by the original Oxford ArchDigital team and has some additions by me. There are publicly available code examples by [Barry Hunter](http://www.nearby.org.uk/downloads.html "Geograph php code") or [Jeff Stott](http://www.jstott.me.uk/phpcoord/ "Jeff Stott's phpcoord code") or some other versions out there on the web! My code is used as either a service or view helper in my Zend Framework project and bundles together a variety of functions. I’m not a developer, I’ve just taught myself bits and pieces to get the PAS website back on the road; if you see errors, do let me know or suggest ways to improve the code.

#### Using Yahoo! to geo-enrich our data

Several years ago, the great [Tyler Bell](http://twitter.com/twbell) (formerly of Oxford ArchDigital and Yahoo!) gave a paper at an archaeological computing conference at UCL’s Institute of Archaeology, where he broke his joke about XML being like high school sex (won’t elaborate on this, ask him) whilst some toe-rags were trying to steal my push bike (they came off badly as I was standing behind them!) Tyler’s paper gave me much food for thought, and it is over the last year or so, that this idea has really come to fruition with our data. The advent of Yahoo!’s suite of geoPlanet tools has allowed us to do various things to our data set and present it in different ways. Below, I’ll show you some of things their powerful suite of tools has allowed us to do.

#### Putting dots on maps for finds with just a parish name

Prior to 2003, we often received the majority of our finds with a very vague find spot, often just to parish level. As everyone loves maps and would like to see where these finds came from, I wanted to get a map on every page that needed one.. Previously, our FLOs would be asked to centre a find on the parish for these find spots; this is now such a waste of time when you can use geoPlanet to get a latitude and longitude, Postcode, type of settlement, bounding box and a WOEID to enhance the data that we hold. To do this is pretty simple with the aid of YQL.

I first heard about YQL from [Jim O’Donnell](http://eatyourgreens.org.uk/) (formerly NMM’s web wizard) and then more at the Yahoo! Hack day in London, when I pedalled round London with Andrew Larcombe on the [Purple Pedals bikes](http://pppp.andrewl.net/). Yahoo describe YQL as **SELECT \* FROM Internet**, which is indeed pretty true. Building opentables to use with their system is pretty easy – I’ll write more about some Museum tables in another post soon. So all my geo extraction is performed using YQL and the examples below show how. All of these are done with the public endpoint. If you run a high traffic site, it is definitely worth changing your code to use Oauth and authenticate your YQL calls for the non-public endpoint (better rate limits etc). It is slightly tricky and you do need to work out how to refresh your Yahoo token, but it is worth the effort.

For example, I grew up in Stapleford, Cambridgeshire and you can search for that with the following YQL call:

\[HTML\]select \* from geo.places where text="stapleford,cambridgeshire"\[/html\]

Which maps to this REST URL of:
[http://query.yahooapis.com/v1/public/yql?q=select%20\*%20from%20geo.places%20where%20text%3D%22stapleford%2Ccambridgeshire%22](http://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20geo.places%20where%20text%3D%22stapleford%2Ccambridgeshire%22 "REST call to geoplanet for Stapleford")
producing an XML or JSON response like below (diagnostics omitted):

\[XML toolbar="true"\]
<?xml version="1.0" encoding="UTF-8"?>
<query xmlns:yahoo="http://www.yahooapis.com/v1/base.rng" yahoo:count="1" yahoo:created="2010-05-05T11:12:49Z" yahoo:lang="en-US">
    <results>
        <place xmlns="http://where.yahooapis.com/v1/schema.rng" xml:lang="en-US" yahoo:URI\="http://where.yahooapis.com/v1/place/35984">
            <woeid>35984</woeid>
            <placeTypeName code="7">Town</placeTypeName>
            <name>Stapleford</name>
            <country code="GB" type="Country">United Kingdom</country>
            <admin1 code="GB\-ENG" type="Country">England</admin1>
            <admin2 code="GB\-CAM" type="County">Cambridgeshire</admin2>
            <admin3/>
            <locality1 type="Town">Stapleford</locality1>
            <locality2/>
            <postal type="Postal Code">CB22 5</postal>
            <centroid>
                <latitude>52.145329</latitude>
                <longitude>0.151490</longitude>
            </centroid>
            <boundingBox>
                <southWest>
                    <latitude>52.127220</latitude>
                    <longitude>0.133460</longitude>
                </southWest>
                <northEast>
                    <latitude>52.164879</latitude>
                    <longitude>0.176640</longitude>
                </northEast>
            </boundingBox>
            <areaRank>3</areaRank>
            <popRank>1</popRank>
        </place>
    </results>
</query>
\[/xml\]

By parsing the XML or JSON response (I tend to use the JSON response),  a Latitude and Longitude pair can be retrieved for placing the object onto the map. It isn’t the true find spot, but can at least give a high level overview of the point of origin. Whilst doing this, I also take the postcode, woeid, bounding box etc to reuse again. Parsing data is pretty simple once you have got your response and decoded the JSON, for example:

\[PHP\]
$place = $place->query->results->place;
$placeData = array();
$placeData\['woeid'\] = (string) $place->woeid;
$placeData\['placeTypeName'\] = (string) $place->placeTypeName->content;
$placeData\['name'\] = (string) $place->name;
if($place->country){
$placeData\['country'\] = (string) $place->country->content;
}
if($place->admin1) {
$placeData\['admin1'\] = (string) $place->admin1->content;
}
if($place->admin2){
$placeData\['admin2'\] = (string) $place->admin2->content;
}
if($place->admin3){
$placeData\['admin3'\] = (string) $place->admin3->content;
}
if($place->locality1){
$placeData\['locality1'\] = (string) $place->locality1->content;
}
if($place->locality2){
$placeData\['locality2'\] = (string) $place->locality2->content;
}
if($place->postal){
$placeData\['postal'\] = $place->postal->content;
}
$placeData\['latitude'\] = $place->centroid->latitude;
$placeData\['longitude'\] = $place->centroid->longitude;
$placeData\['centroid'\] = array(
‘lat’ => (string) $place->centroid->latitude,
‘lng’ => (string) $place->centroid->longitude
);
$placeData\['boundingBox'\] = array(‘southWest’ => array(
‘lat’ => (string) $place->boundingBox->southWest->latitude,
‘lng’ => (string) $place->boundingBox->southWest->longitude),
‘northEast’ => array(
‘lat’ => (string) $place->boundingBox->northEast->latitude,
‘lng’ => (string) $place->boundingBox->northEast->longitude)
);
return $placeData;
\[/php\]

The image below shows an autogenerated findspot and a parish boundary (see below for flickr shapefile use) and adjacent places.

[![An autogenerated findspot]({{ site.baseurl }}/files/2010/09/autogenerated-210x300.jpg "autogenerated")]({{ site.baseurl }}/files/2010/09/autogenerated.jpg)

Within our database, I have a certainty field for where the co-ordinates originate from. This table has the following content:

1.  From a map
2.  From finder verbally
3.  GPS from the finder
4.  GPS from the FLO
5.  Centred on the parish via a paper map
6.  Recorded at a rally (so certainty could be dubious)
7.  Produced via webservice

Therefore researchers are appraised of where the findspot comes from and whether we can treat it (if at all) as useful.

#### Getting elevation (via Geonames)

The woeid or the LatLng can be used to get elevation of the find spot. This can be achieved by a combination of reverse geocoding against Flickr place names (for woeid) and the Geonames API call for ‘Elevation – Aster Global Digital Elevation Model’. So for example, I want to get the elevation for the centre of Stapleford. You can query the geonames API with the following YQL:

\[HTML wraplines="true"\]select \* from json where URL\="http://ws.geonames.org/astergdemJSON?lat=52.145329&lng=0.151490";\[/html\]

Which when executed produces this [response](http://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20json%20where%20url%3D%22http%3A%2F%2Fws.geonames.org%2FastergdemJSON%3Flat%3D52.145329%26lng%3D0.151490%22%3B&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys):

\[XML\]
<?xml version="1.0" encoding="UTF-8"?>
<query xmlns:yahoo="http://www.yahooapis.com/v1/base.rng"
       yahoo:count="1" yahoo:created="2010-09-30T12:26:00Z" yahoo:lang="en-US">
    <results>
        <json>
            <astergdem>17</astergdem>
            <lng>0.15149</lng>
            <lat>52.145329</lat>
        </json>
    </results>
</query>
\[/xml\]

So I now have the elevation of 17 metres above sea level. Great! I’ve been experimenting a bit with this against some findspots that we know elevation for. One high profile object, the Crosby Garrett Helmet was pinpointed to 1 metre difference in the GPS elevation and the Geonames sourced one.

By providing an elevation for each of our findspots, researchers can then do [viewshed analysis](http://en.wikipedia.org/wiki/GIS_Viewshed_Analysis "Viewshed analysis how to"); I don’t think anyone has really done this yet for the artefact distributions that we record, but I could be proved wrong!

#### Reverse geocoding from Latitude and Longitude with Yahoo!

At present, the GeoPlanet suite doesn’t provide this feature, but you can still manage to do this via YQL and using the following query:

\[HTML\]select \* from flickr.places where lon={Longitude} and lat={latitude}\[/html\]

So for example using Stapleford’s LatLng as the YQL parameters gives you:

\[XML wraplines="true"\]
<?xml version="1.0" encoding="UTF-8"?>
<query xmlns:yahoo="http://www.yahooapis.com/v1/base.rng" yahoo:count="1" yahoo:created="2010-05-05T01:27:47Z" yahoo:lang="en-US">
    <results>
        <places accuracy="16" latitude="52.145329" longitude="0.151490" total="1">
            <place latitude="52.145" longitude="0.151" name="Stapleford, England, United Kingdom" place\_id="m2G8tyiaBJVjFQ" place\_type="locality" place\_type\_id="7" place\_url="/United+Kingdom/England/Stapleford/in-Cambridgeshire" timezone="Europe/London" woeid="35984"/>
        </places>
    </results>
</query>
\[/xml\]

You’ll notice a couple fo useful things in the Flickr XML returned, for example the place\_url, in this case: /united+kingdom/england/stapleford/in-cambridgeshire  which when appended to Flickr’s root URL for photos can give you [http://www.flickr.com/places/united+kingdom/england/stapleford/in-cambridgeshire](http://www.flickr.com/places/united+kingdom/england/stapleford/in-cambridgeshire "Photos on Flickr from Stapleford, Cambridgeshire") which in turn gives you access to feeds in various flavours from that page.

One of the other cool things available in Flickr’s API is placeinfo. I’d love a boundary map of how Flickr views the parish of Stapleford. As I previously obtained and gave my findspot a WOEID, I can see if Flickr has this data. So perform this YQL query:

\[HTML\]select \* from flickr.places.info where woe\_id=’35984′\[/html\]

And execute it to obtain the following XML:

\[XML wraplines="true"\]
<?xml version="1.0" encoding="UTF-8"?>
<query xmlns:yahoo="http://www.yahooapis.com/v1/base.rng"
       yahoo:count="1" yahoo:created="2010-09-30T12:34:50Z" yahoo:lang="en-US">
    <results>
        <place has\_shapedata="1" latitude="52.145" longitude="0.151"
               name="Stapleford, England, United Kingdom"
               place\_id="m2G8tyiaBJVjFQ" place\_type="locality"
               place\_type\_id="7"
               place\_url="/United+Kingdom/England/Stapleford/in-Cambridgeshire"
               timezone="Europe/London" woeid="35984">
            <locality latitude="52.145" longitude="0.151"
                      place\_id="m2G8tyiaBJVjFQ"
                      place\_url="/United+Kingdom/England/Stapleford/in-Cambridgeshire" woeid="35984">Stapleford, England, United Kingdom</locality>
            <county latitude="52.373" longitude="0.007"
                    place\_id="pVJUVwKYA5qQZa9wqQ"
                    place\_url="/pVJUVwKYA5qQZa9wqQ" woeid="12602140">Cambridgeshire, England, United Kingdom</county>
            <region latitude="52.883" longitude="-1.974"
                    place\_id="pn4MsiGbBZlXeplyXg"
                    place\_url="/United+Kingdom/England" woeid="24554868">England, United Kingdom</region>
            <country latitude="54.314" longitude="-2.230"
                     place\_id="DevLebebApj4RVbtaQ"
                     place\_url="/United+Kingdom" woeid="23424975">United Kingdom</country>
            <shapedata alpha="0.00015" count\_edges="16"
                       count\_points="44" created="1248244568" has\_donuthole="0" is\_donuthole="0">
                <polylines>
                    <polyline>52.155731201172,0.17115999758244 52.158447265625,0.17576499283314 52.159084320068,0.18161700665951 52.159244537354,0.18208900094032 52.15747833252,0.18410600721836 52.153221130371,0.18645000457764 52.151500701904,0.17897999286652 52.14905166626,0.17045900225639 52.136436462402,0.15260599553585 52.135303497314,0.14247800409794 52.140232086182,0.13955999910831 52.145477294922,0.14135999977589 52.145721435547,0.14150799810886 52.145240783691,0.14707000553608 52.154125213623,0.16043299436569 52.155731201172,0.17115999758244</polyline>
                </polylines>
                <URLs\>
                <shapefile>http://farm4.static.flickr.com/3483/shapefiles/35984\_20090722\_6d95b5e27e.tar.gz</shapefile>
                </urls>
            </shapedata>
        </place>
    </results>
</query>
\[/xml\]

Brilliant, the polylines can be used draw an outline shapefile on the map.

#### Extracting place data from find descriptions

[![Y!Geo tags]({{ site.baseurl }}/files/2010/05/ygeo.png "ygeo")]({{ site.baseurl }}/files/2010/05/ygeo.png)Many of our objects are tied by descriptive prose to various places around the World. By using Yahoo’s Placemaker, we can now extract the entities from the finds data and allow for cross referencing of all objects that have Avon, England within their description. The image below shows you where you’ll see the tags displayed on the finds record, as I’m into Classics, you’ll notice I label these with lower case Greek letters for bullets. Probably pretentious!  To get these tags is really very straightforward and can use another pretty simple YQL call, for example, this text is from the famous Moorlands Patera.

\[HTML toolbar="true" wraplines="true"\]
SELECT \* FROM geo.placemaker WHERE documentContent = "Only two other vessels with inscriptions naming forts on Hadrian’s Wall are known; the ‘Rudge Cup’ which was discovered in Wiltshire in 1725 (Horsley 1732; Henig 1995) and the ‘Amiens patera’ found in Amiens in 1949 (Heurgon 1951). Between them they name seven forts, but the Staffordshire patera is the first to include Drumburgh and is the only example to name an individual. All three are likely to be souvenirs of Hadrian’s Wall, although why they include forts on the western end of the Wall only is unclear" AND documentType="text/plain"\[/html\]

Which then produces this output in XML:

\[XML toolbar="true"\]
<?xml version="1.0" encoding="UTF-8"?>
<query xmlns:yahoo="http://www.yahooapis.com/v1/base.rng" yahoo:count="1" yahoo:created="2010-05-05T04:26:57Z" yahoo:lang="en-US">
    <results>
        <matches>
            <match>
                <place xmlns="http://wherein.yahooapis.com/v1/schema">
                    <woeId>575961</woeId>
                    <type>Town</type>
                    <name><!\[CDATA\[Amiens, Picardie, FR\]\]></name>
                    <centroid>
                        <latitude>49.8947</latitude>
                        <longitude>2.29316</longitude>
                    </centroid>
                </place>
                <reference xmlns="http://wherein.yahooapis.com/v1/schema">
                    <woeIds>575961</woeIds>
                    <start>177</start>
                    <end>183</end>
                    <isPlaintextMarker>1</isPlaintextMarker>
                    <text><!\[CDATA\[Amiens\]\]></text>
                    <type>plaintext</type>
                    <xpath><!\[CDATA\[\]\]></xpath>
                </reference>
                <reference xmlns="http://wherein.yahooapis.com/v1/schema">
                    <woeIds>575961</woeIds>
                    <start>201</start>
                    <end>207</end>
                    <isPlaintextMarker>1</isPlaintextMarker>
                    <text><!\[CDATA\[Amiens\]\]></text>
                    <type>plaintext</type>
                    <xpath><!\[CDATA\[\]\]></xpath>
                </reference>
            </match>
            <match>
                <place xmlns="http://wherein.yahooapis.com/v1/schema">
                    <woeId>12602186</woeId>
                    <type>County</type>
                    <name><!\[CDATA\[Wiltshire, England, GB\]\]></name>
                    <centroid>
                        <latitude>51.3241</latitude>
                        <longitude>-1.9257</longitude>
                    </centroid>
                </place>
                <reference xmlns="http://wherein.yahooapis.com/v1/schema">
                    <woeIds>12602186</woeIds>
                    <start>123</start>
                    <end>132</end>
                    <isPlaintextMarker>1</isPlaintextMarker>
                    <text><!\[CDATA\[Wiltshire\]\]></text>
                    <type>plaintext</type>
                    <xpath><!\[CDATA\[\]\]></xpath>
                </reference>
            </match>
            <match>
                <place xmlns="http://wherein.yahooapis.com/v1/schema">
                    <woeId>12602189</woeId>
                    <type>County</type>
                    <name><!\[CDATA\[Staffordshire, England, GB\]\]></name>
                    <centroid>
                        <latitude>52.8248</latitude>
                        <longitude>-2.02817</longitude>
                    </centroid>
                </place>
                <reference xmlns="http://wherein.yahooapis.com/v1/schema">
                    <woeIds>12602189</woeIds>
                    <start>276</start>
                    <end>289</end>
                    <isPlaintextMarker>1</isPlaintextMarker>
                    <text><!\[CDATA\[Staffordshire\]\]></text>
                    <type>plaintext</type>
                    <xpath><!\[CDATA\[\]\]></xpath>
                </reference>
            </match>
            <match>
                <place xmlns="http://wherein.yahooapis.com/v1/schema">
                    <woeId>23509175</woeId>
                    <type>LandFeature</type>
                    <name><!\[CDATA\[Hadrian's Wall, Bardon Mill, England, GB\]\]></name>
                    <centroid>
                        <latitude>54.9522</latitude>
                        <longitude>-2.32975</longitude>
                    </centroid>
                </place>
                <reference xmlns="http://wherein.yahooapis.com/v1/schema">
                    <woeIds>23509175</woeIds>
                    <start>418</start>
                    <end>432</end>
                    <isPlaintextMarker>1</isPlaintextMarker>
                    <text><!\[CDATA\[Hadrian’s Wall\]\]></text>
                    <type>plaintext</type>
                    <xpath><!\[CDATA\[\]\]></xpath>
                </reference>
            </match>
        </matches>
    </results>
</query>
\[/xml\]

In the above XML response, you can now see the matches that Placemaker has found in the text sent to their service. You can now parse this data and use it for tagging or any other purpose that you want to put the data to.  YQL has the added benefit of caching at the Yahoo! end and you can do multiple queries in one call as demonstrated by Chris Heilmann in his [Geoplanet explorer.](http://isithackday.com/geoplanet-explorer "Geoplanet explorer")

#### A YQL Multiquery example

For example, I want to combine a placemaker call and also get some spatial information for a find spot where I only have the placename. To do this, I write this YQL query:

\[HTML toolbar="true" wraplines="true"\]
select \* from query.multi where queries=’
select \* from geo.placemaker where documentContent = "Only two other vessels with inscriptions naming forts on Hadrian’s Wall are known the Rudge Cup which was discovered in Wiltshire in 1725 (Horsley 1732, Henig 1995) and the Amiens patera found in Amiens in 1949 (Heurgon 1951). Between them they name seven
forts, but the Staffordshire patera is the first to include Drumburgh and is the only example to name an individual. All three are likely to be souvenirs of Hadrian’s Wall, although why they include forts on the western end of the Wall only is unclear" and documentType="text/plain" and appid="";
select \* from geo.places where text="staffordshire moorlands, staffordshire,uk" ‘
\[/html\]

The base URL to call this is the public version – http://query.yahooapis.com/v1/public/yql and as we are using one of the community tables, the call needs to be made with &env=store://datatables.org/alltableswithkeys appended (urlencoded).
This can be run in the [console](http://y.ahoo.it/fx4bG5fJ) and produces the following [XML response](http://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20query.multi%20where%20queries%3D) – 4 place matches and the geo data for Staffordshire Moorlands.

\[XML toolbar="true" wraplines="true"\]
<?xml version="1.0" encoding="UTF-8"?>
<query xmlns:yahoo="http://www.yahooapis.com/v1/base.rng"
       yahoo:count="2" yahoo:created="2010-09-30T11:44:08Z" yahoo:lang="en-US">
    <results>
        <results>
            <matches>
                <match>
                    <place xmlns="http://wherein.yahooapis.com/v1/schema">
                        <woeId>575961</woeId>
                        <type>Town</type>
                        <name><!\[CDATA\[Amiens, Picardie, FR\]\]></name>
                        <centroid>
                            <latitude>49.8947</latitude>
                            <longitude>2.29316</longitude>
                        </centroid>
                    </place>
                    <reference xmlns="http://wherein.yahooapis.com/v1/schema">
                        <woeIds>575961</woeIds>
                        <start>196</start>
                        <end>202</end>
                        <isPlaintextMarker>1</isPlaintextMarker>
                        <text><!\[CDATA\[Amiens\]\]></text>
                        <type>plaintext</type>
                        <xpath><!\[CDATA\[\]\]></xpath>
                    </reference>
                </match>
                <match>
                    <place xmlns="http://wherein.yahooapis.com/v1/schema">
                        <woeId>12602186</woeId>
                        <type>County</type>
                        <name><!\[CDATA\[Wiltshire, England, GB\]\]></name>
                        <centroid>
                            <latitude>51.3241</latitude>
                            <longitude>-1.9257</longitude>
                        </centroid>
                    </place>
                    <reference xmlns="http://wherein.yahooapis.com/v1/schema">
                        <woeIds>12602186</woeIds>
                        <start>120</start>
                        <end>129</end>
                        <isPlaintextMarker>1</isPlaintextMarker>
                        <text><!\[CDATA\[Wiltshire\]\]></text>
                        <type>plaintext</type>
                        <xpath><!\[CDATA\[\]\]></xpath>
                    </reference>
                </match>
                <match>
                    <place xmlns="http://wherein.yahooapis.com/v1/schema">
                        <woeId>12602189</woeId>
                        <type>County</type>
                        <name><!\[CDATA\[Staffordshire, England, GB\]\]></name>
                        <centroid>
                            <latitude>52.8248</latitude>
                            <longitude>-2.02817</longitude>
                        </centroid>
                    </place>
                    <reference xmlns="http://wherein.yahooapis.com/v1/schema">
                        <woeIds>12602189</woeIds>
                        <start>271</start>
                        <end>284</end>
                        <isPlaintextMarker>1</isPlaintextMarker>
                        <text><!\[CDATA\[Staffordshire\]\]></text>
                        <type>plaintext</type>
                        <xpath><!\[CDATA\[\]\]></xpath>
                    </reference>
                </match>
                <match>
                    <place xmlns="http://wherein.yahooapis.com/v1/schema">
                        <woeId>23509175</woeId>
                        <type>LandFeature</type>
                        <name><!\[CDATA\[Hadrian's Wall, Bardon Mill, England, GB\]\]></name>
                        <centroid>
                            <latitude>54.9522</latitude>
                            <longitude>-2.32975</longitude>
                        </centroid>
                    </place>
                    <reference xmlns="http://wherein.yahooapis.com/v1/schema">
                        <woeIds>23509175</woeIds>
                        <start>413</start>
                        <end>427</end>
                        <isPlaintextMarker>1</isPlaintextMarker>
                        <text><!\[CDATA\[Hadrian&rsquo;s Wall\]\]></text>
                        <type>plaintext</type>
                        <xpath><!\[CDATA\[\]\]></xpath>
                    </reference>
                </match>
            </matches>
        </results>
        <results>
            <place xmlns="http://where.yahooapis.com/v1/schema.rng"
                   xml:lang="en-US" yahoo:URI\="http://where.yahooapis.com/v1/place/12696078">
                <woeid>12696078</woeid>
                <placeTypeName code="10">Local Administrative Area</placeTypeName>
                <name>Staffordshire Moorlands District</name>
                <country code="GB" type="Country">United Kingdom</country>
                <admin1 code="GB\-ENG" type="Country">England</admin1>
                <admin2 code="GB\-STS" type="County">Staffordshire</admin2>
                <admin3/>
                <locality1/>
                <locality2/>
                <postal/>
                <centroid>
                    <latitude>53.071468</latitude>
                    <longitude>-1.993490</longitude>
                </centroid>
                <boundingBox>
                    <southWest>
                        <latitude>52.916691</latitude>
                        <longitude>-2.211330</longitude>
                    </southWest>
                    <northEast>
                        <latitude>53.226250</latitude>
                        <longitude>-1.775660</longitude>
                    </northEast>
                </boundingBox>
                <areaRank>6</areaRank>
                <popRank>0</popRank>
            </place>
        </results>
    </results>
</query>
\[/xml\]

#### Concordance with other services

One of the other things I am interested in, is finding concordance between WOEID and Geonames places. This is quite easy to do using another geo table. For example look up Amiens, Picardie by WOEID:

\[HTML\]select \* from geo.concordance where namespace="woeid" and text="575961"\[/html\]

Produces:

\[XML\]
<?xml version="1.0" encoding="UTF-8"?>
<query xmlns:yahoo="http://www.yahooapis.com/v1/base.rng"
       yahoo:count="1" yahoo:created="2010-09-30T12:41:28Z" yahoo:lang="en-US">
    <results>
        <concordance xml:lang="en-US"
                     xmlns="http://where.yahooapis.com/v1/schema.rng"
                     xmlns:yahoo="http://www.yahooapis.com/v1/base.rng" yahoo:URI\="http://where.yahooapis.com/v1/concordance/woeid/575961">
            <woeid>575961</woeid>
            <geonames>3037854</geonames>
            <locode>FRAMI</locode>
        </concordance>
    </results>
</query>
\[/xml\]

So you now have a WOEID and a geonames ID. Amiens WOEID = 575961 and Geonames = 3037854. You can use the geonames id that is produced for linked data; for example: [http://ws.geonames.org/rdf?geonameId=3037854](http://ws.geonames.org/rdf?geonameId=3037854)

#### Problems using YQL for geodata

Even though combining YQL with the power of Geoplanet is awesome, I did run into a few problems. None of these were really insurmountable:

1.  Hit rate limit constantly – Google’s indexing of our site was causing our server to make too many requests to YQL; fixed by changing caching model and switching to Oauth endpoint. Also I changed my code to ignore responses when the headers returned were: text/html;charset=UTF-8. The rate limit page thrown up by Yahoo! is HTML and not an XML response.
2.  Some places were pulled out of text when they were irrelevant – Copper Alloy, Tamil Nadu is one example. Fixed by creating a stop list
3.  geonames API sometimes takes a while to respond and made application hang – changed cUrl settings
4.  Took quite a long time to parse 400,000 records – can’t do much with that!

However, I’d really recommend using YQL to extract geodata for your application. Hopefully, Yahoo! will maintain YQL and Geo as integral parts of their business model…. In the future, I would love to run the [British Museum](http://www.britishmuseum.org) collections data through these functions and see what cross-referencing I could find….
