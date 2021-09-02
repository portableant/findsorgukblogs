---
author: Daniel Pett
published: 2010-08-06T16:32:16+00:00
category: central-unit
title: Adding old OS maps to findspot maps
layout: default
---

Today on Twitter, David Haskiya [alerted me](http://twitter.com/DavidHaskiya/status/20468427452) to a set of old Ordnance Survey maps that have been scanned by the National Library of Scotland and turned into the  ‘[NLS Maps API](http://geo.nls.uk/maps/api/ "Detailed instructions on how to use the NLS api"): Historic map of Great Britain for use in mashups’. These old maps are really useful (they cover England and Wales as well as Scotland!) for the work that our Finds Liaison Officers do, or for researchers using our database. Low level phenomenological research can be conducted.  Their instructions are pretty straightforward to follow and I have now added this layer to our findspot mapping (at the moment just for higher level users). The image below gives an example of the embedded Googlemap that we can produce from these OS tiles:

![Old Map from NLS]({{ site.baseurl }}/files/2010/08/oldmap.jpg "Old Map from NLS")

Our maps now have the following layers:

*   Satellite
*   Terrain
*   Openstreetmap
*   Google Earth
*   Basic map
*   Hybrid
*   Historical

To implement this layer all you need to do is the following (I have Jquery as my javascript framework), firstly add the Javascript file that runs their tileserver to either your head tags or before the closing body tag of your HTML document.


    <script type="text/javascript" src="http://nls.tileserver.com/api.js"></script>


Then you need to initiate the layer and add the historical map button and copyright layer:



    var copyright = new GCopyright(1, new GLatLngBounds(new GLatLng(-90, -180),new GLatLng(90, 180)), 1,
    "Historical maps from <a href=’http://geo.nls.uk/maps/api/’>NLS Maps API<\\/a>");
        var copyrightCollection = new GCopyrightCollection();
        copyrightCollection.addCopyright(copyright);var tilelayer = new GTileLayer(copyrightCollection, 1, NLSTileUrlOS(‘MAXZOOM’));
        tilelayer.getTileUrl = NLSTileUrlOS;

        var nlsmap = new GMapType(\[tilelayer\], G\_NORMAL\_MAP.getProjection(), "Historical");


You will then need to add the map type to your mapping script by adding the following javascript:

    map.addMapType(nlsmap);

So for example my code for running our map looks like the below (and I add this before the closing body tags, and using Zend Framework’s inlineScript syntax within my PHP script:


    <script type="text/javascript" src="<a href="http://nls.tileserver.com/api.js">http://nls.tileserver.com/api.js</a>"></script>
      <script type="text/javascript" src="<a href="http://maps.google.com/maps?file=API&amp;v=2.x&key=ABQIAAAAasv4kXXJ0jQKvwOWfHsLjBSlEYz08iyooQyuh\_EGbYeUie1elhTVaZDZHd9xfLdYKWAVz9b3bDuvKA">http://maps.google.com/maps?file=API&amp;amp;v=2.x&amp;key={key}</a>"></script>
      <script type="text/javascript" src="<a href="http://gmaps-utility-library.googlecode.com/svn/trunk/mapiconmaker/1.0/src/mapiconmaker.js">http://gmaps-utility-library.googlecode.com/svn/trunk/mapiconmaker/1.0/src/mapiconmaker.js</a>"></script>
      <script type="text/javascript">
      //<!\[CDATA\[  
              $(document).ready(function() {

                  if (GBrowserIsCompatible()) {

      //Set up the NLS layer

                      var copyright = new GCopyright(1, new GLatLngBounds(new GLatLng(-90, -180),new GLatLng(90, 180)), 1,
                              "Historical maps from <a href='http://geo.nls.uk/maps/api/'>NLS Maps API<\\/a>");
                      var copyrightCollection = new GCopyrightCollection();
                      copyrightCollection.addCopyright(copyright);
                      var tilelayer = new GTileLayer(copyrightCollection, 1, NLSTileUrlOS('MAXZOOM'));
                      tilelayer.getTileUrl = NLSTileUrlOS;
                      var nlsmap = new GMapType(\[tilelayer\], G\_NORMAL\_MAP.getProjection(), "Historical");

      //Set up the openstreet map layer

                      var copyOSM = new GCopyrightCollection(‘<a href="http://www.openstreetmap.org/">OpenStreetMap</a>’);
                      copyOSM.addCopyright(new GCopyright(1,
                              new GLatLngBounds(new GLatLng(-90, -180), new GLatLng(90, 180)),
                              0, // minimum zoom level  
                  ‘ ‘ // no additional copyright message, but empty string hides entire copyright  
                  ));
                      var osmLayer = new GTileLayer(copyOSM, 0, 18, {
                          tileUrlTemplate: ‘http://b.tile.cloudmade.com/BC9A493B41014CAABB98F0471D759707/998/256/{Z}/{X}/{Y}.png’,  
                              isPng: true,
                                      opacity: 1.0
                  });

                  var osmMap = new GMapType(  
      \[osmLayer\], // list of layers  
                  G\_NORMAL\_MAP.getProjection(), // borrow the Mercator projection from the standard map  
                  ‘OSM’ // name should be short enough to fit in button  
                  );

      //Initiate the map for the div with id of "map" – random Lat/lon pair used here – not a findspot!  
                  var map = new GMap2(document.getElementById("map"));
                  map.setUIToDefault();
                  map.addControl(new GMapTypeControl());
                  map.setCenter(new GLatLng(51.263722,0.68009),11);
      //Add your map types – here I have added OSM, NLS, Earth and Terrain  
                  map.addMapType(osmMap);
                  map.addMapType(nlsmap);
                  map.addMapType(G\_SATELLITE\_3D\_MAP);
                  map.addMapType(G\_PHYSICAL\_MAP);
      //Set your default map type  
                  map.setMapType(G\_PHYSICAL\_MAP);
                  map.disableScrollWheelZoom();
                  map.enableRotation();

      //Set up my icons  
                  var tinyIcon = new GIcon();
                  tinyIcon.image = "http://labs.google.com/ridefinder/images/mm\_20\_red.png";
                  tinyIcon.shadow = "http://labs.google.com/ridefinder/images/mm\_20\_shadow.png";
                  tinyIcon.iconSize = new GSize(12, 20);
                  tinyIcon.shadowSize = new GSize(22, 20);
                  tinyIcon.iconAnchor = new GPoint(6, 20);
                  tinyIcon.infoWindowAnchor = new GPoint(5, 1);
                  markerOptions = { icon:tinyIcon };

                  var findIcon = new GIcon();
                  findIcon.image = "http://labs.google.com/ridefinder/images/mm\_20\_blue.png";
                  findIcon.shadow = "http://labs.google.com/ridefinder/images/mm\_20\_shadow.png";
                  findIcon.iconSize = new GSize(12, 20);
                  findIcon.shadowSize = new GSize(22, 20);
                  findIcon.iconAnchor = new GPoint(6, 20);
                  findIcon.infoWindowAnchor = new GPoint(5, 1);

                  findOptions = { icon:findIcon };

                  var point = new GLatLng(51.263722,0.68009);

                  var marker = new GMarker(point, markerOptions);
                  GEvent.addListener(marker, "click", function () {
                      marker.openInfoWindowHtml("Findspot location");
                  });
                  map.addOverlay(marker);

              }

      });
      //\]\]>  
      </script>


So really simple to integrate and get running on your site.
