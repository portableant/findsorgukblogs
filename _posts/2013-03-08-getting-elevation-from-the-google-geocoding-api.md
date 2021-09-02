---
layout: default
published: 2013-03-08T12:11:24+00:00
author: Daniel Pett
category: labs
title:  Getting elevation from the Google geocoding api
---

The code below can be used to access Google’s elevation API and retrieve single point data easily using Zend HTTP Client. I previously used the geonames API, but found that too flaky for production. There are restrictions on using this api:

> Use of the Google Elevation API is subject to a limit of 2,500 requests per day (Maps API for Business users may send up to 100,000 requests per day). In each given request you may query the elevation of up to 512 locations, but you may not exceed 25,000 total locations per day (1,000,000 for Maps API for Business users). This limit is enforced to prevent abuse and/or repurposing of the Elevation API, and this limit may be changed in the future without notice. Additionally, we enforce a request rate limit to prevent abuse of the service. If you exceed the 24-hour limit or otherwise abuse the service, the Elevation API may stop working for you temporarily. If you continue to exceed this limit, your access to the Elevation API may be blocked.

And there is a specific requirement for where this data will be used:

> Note: the Elevation API may only be used in conjunction with displaying results on a Google map; using elevation data without displaying a map for which elevation data was requested is prohibited.

Access the google elevation

    <?php /** A class for getting elevation of a latlon point against the google api * @version 1 * @author Daniel Pett * @license GNU * @package Pas_Service * @subpackage Geo * @category Pas * @see https://developers.google.com/maps/documentation/elevation/ * @uses Zend_Http_Client * @uses Zend_Json */ class Pas_Service_Geo_Elevation{ const ELEVATIONURI = 'http://maps.googleapis.com/maps/api/elevation/json'; /** Get the coordinates from an address string * @param float $lat * @param float $lon * @access public */ public function _getElevationApiCall($lat, $lon) { $client = new Zend_Http_Client(); $client->setUri(self::ELEVATIONURI); $client->setParameterGet('locations', $lon . ',' . $lat) ->setParameterGet('sensor', 'false'); $result = $client->request('GET'); $response = Zend_Json_Decoder::decode($result->getBody(), Zend_Json::TYPE_OBJECT); return $response; } /** Get the coordinates of an address * @param float $lat * @param float $lon * @access public */ public function getElevation($lat, $lon) { $response = $this->_getElevationApiCall($lat, $lon); if(isset($response->results[0]->elevation)){ return $response->results[0]->elevation; } else { return null; } } }



    <?php

    /** A class for getting elevation of a latlon point against the google api
    * @version 1
    * @author Daniel Pett
    * @license GNU
    * @package Pas_Service

    * @subpackage Geo

    * @category Pas

    * @see https://developers.google.com/maps/documentation/elevation/

    * @uses Zend_Http_Client

    * @uses Zend_Json

    */

    class  Pas_Service_Geo_Elevation{

    const  ELEVATIONURI  = 'http://maps.googleapis.com/maps/api/elevation/json';

    /** Get the coordinates from an address string

         * @param float $lat

         * @param float $lon

         * @access public

         */

    public  function  _getElevationApiCall($lat,  $lon)  {

    $client  = new  Zend_Http_Client();

    $client->setUri(self::ELEVATIONURI);

    $client->setParameterGet('locations',  $lon  .  ','  .  $lat)
           ->setParameterGet('sensor',  'false');

    $result  = $client->request('GET');

    $response  = Zend_Json_Decoder::decode($result->getBody(),

    Zend_Json::TYPE_OBJECT);

    return  $response;

    }

    /** Get the coordinates of an address

         * @param float $lat

         * @param float $lon

         * @access public

         */

    public  function  getElevation($lat,  $lon){

    $response  = $this->_getElevationApiCall($lat,  $lon);

    if(isset($response->results[0]->elevation)){

    return  $response->results[0]->elevation;

    }  else  {

    return  null;

    }

    }

    }

To use this in your code do the below:

Use the service class

    $api = new Pas_Service_Geo_Elevation(); $elevation = $api->getElevation($data['declong'], $data['declat']);

    $api  = new  Pas_Service_Geo_Elevation();

    $elevation  = $api->getElevation($data['declong'],  $data['declat']);

Pretty simple.
