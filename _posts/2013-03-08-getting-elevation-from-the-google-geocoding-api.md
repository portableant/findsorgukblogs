---
layout: default
published: 2013-03-08T12:11:24+00:00
author: Daniel Pett
category: labs
---
Getting elevation from the Google geocoding api
===============================================

The code below can be used to access Google’s elevation API and retrieve single point data easily using Zend HTTP Client. I previously used the geonames API, but found that too flaky for production. There are restrictions on using this api:

> Use of the Google Elevation API is subject to a limit of 2,500 requests per day (Maps API for Business users may send up to 100,000 requests per day). In each given request you may query the elevation of up to 512 locations, but you may not exceed 25,000 total locations per day (1,000,000 for Maps API for Business users). This limit is enforced to prevent abuse and/or repurposing of the Elevation API, and this limit may be changed in the future without notice. Additionally, we enforce a request rate limit to prevent abuse of the service. If you exceed the 24-hour limit or otherwise abuse the service, the Elevation API may stop working for you temporarily. If you continue to exceed this limit, your access to the Elevation API may be blocked.

And there is a specific requirement for where this data will be used:

> Note: the Elevation API may only be used in conjunction with displaying results on a Google map; using elevation data without displaying a map for which elevation data was requested is prohibited.

Access the google elevation

    <?php /\*\* A class for getting elevation of a latlon point against the google api \* @version 1 \* @author Daniel Pett \* @license GNU \* @package Pas\_Service \* @subpackage Geo \* @category Pas \* @see https://developers.google.com/maps/documentation/elevation/ \* @uses Zend\_Http\_Client \* @uses Zend\_Json \*/ class Pas\_Service\_Geo\_Elevation{ const ELEVATIONURI = 'http://maps.googleapis.com/maps/api/elevation/json'; /\*\* Get the coordinates from an address string \* @param float $lat \* @param float $lon \* @access public \*/ public function \_getElevationApiCall($lat, $lon) { $client = new Zend\_Http\_Client(); $client->setUri(self::ELEVATIONURI); $client->setParameterGet('locations', $lon . ',' . $lat) ->setParameterGet('sensor', 'false'); $result = $client->request('GET'); $response = Zend\_Json\_Decoder::decode($result->getBody(), Zend\_Json::TYPE\_OBJECT); return $response; } /\*\* Get the coordinates of an address \* @param float $lat \* @param float $lon \* @access public \*/ public function getElevation($lat, $lon) { $response = $this->\_getElevationApiCall($lat, $lon); if(isset($response->results\[0\]->elevation)){ return $response->results\[0\]->elevation; } else { return null; } } }



    <?php
    
    /\*\* A class for getting elevation of a latlon point against the google api
    \* @version 1
    \* @author Daniel Pett
    \* @license GNU
    \* @package Pas\_Service
    
    \* @subpackage Geo
    
    \* @category Pas
    
    \* @see https://developers.google.com/maps/documentation/elevation/
    
    \* @uses Zend\_Http\_Client
    
    \* @uses Zend\_Json
    
    \*/
    
    class  Pas\_Service\_Geo\_Elevation{
    
    const  ELEVATIONURI  \=  'http://maps.googleapis.com/maps/api/elevation/json';
    
    /\*\* Get the coordinates from an address string
    
         \* @param float $lat
    
         \* @param float $lon
    
         \* @access public
    
         \*/
    
    public  function  \_getElevationApiCall($lat,  $lon)  {
    
    $client  \=  new  Zend\_Http\_Client();
    
    $client\->setUri(self::ELEVATIONURI);
    
    $client->setParameterGet('locations',  $lon  .  ','  .  $lat)
           ->setParameterGet('sensor',  'false');
    
    $result  \=  $client\->request('GET');
    
    $response  \=  Zend\_Json\_Decoder::decode($result\->getBody(),
    
    Zend\_Json::TYPE\_OBJECT);
    
    return  $response;
    
    }
    
    /\*\* Get the coordinates of an address
    
         \* @param float $lat
    
         \* @param float $lon
    
         \* @access public
    
         \*/
    
    public  function  getElevation($lat,  $lon){
    
    $response  \=  $this\->\_getElevationApiCall($lat,  $lon);
    
    if(isset($response\->results\[0\]\->elevation)){
    
    return  $response\->results\[0\]\->elevation;
    
    }  else  {
    
    return  null;
    
    }
    
    }
    
    }

To use this in your code do the below:

Use the service class



$api = new Pas\_Service\_Geo\_Elevation(); $elevation = $api->getElevation($data\['declong'\], $data\['declat'\]);

1

2

$api  \=  new  Pas\_Service\_Geo\_Elevation();

$elevation  \=  $api\->getElevation($data\['declong'\],  $data\['declat'\]);

Pretty simple.

[March 8, 2013](http://finds.org.uk/blogs/labs/2013/03/08/getting-elevation-from-the-google-geocoding-api/ "12:11 pm") [Daniel Pett](http://finds.org.uk/blogs/labs/author/admin/ "View all posts by Daniel Pett") [Mapping](http://finds.org.uk/blogs/labs/category/mapping/) [php](http://finds.org.uk/blogs/labs/category/php-2/)  [elevation](http://finds.org.uk/blogs/labs/tag/elevation/) [Google](http://finds.org.uk/blogs/labs/tag/google/)   [Leave a comment](http://finds.org.uk/blogs/labs/2013/03/08/getting-elevation-from-the-google-geocoding-api/#respond "Comment on Getting elevation from the Google geocoding api")