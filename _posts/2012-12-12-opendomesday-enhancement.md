---
layout: default
author: Daniel Pett
category: labs
published: 2012-12-12T14:46:55+00:00
title: Adding Open Domesday data to object records
---

The Scheme website has had low level integration with the [Open Domesday](http://domesdaymap.co.uk/ "The Open Domesday map project") project site since September 2011, and it has taken me this long to get round to writing about how it works. Each record we create, ideally should have a National Grid Reference (NGR) attached to it. These NGRs are converted at the point of data entry, via a backend script, to Latitude and Longitude which gives us more flexibility for integrating with 3rd party services (mapping apis, flickr, etc.) These LatLon pairs can also be used to retrieve data easily from Open Domesday, via Anna Powell-Smith’s excellent API.  In this instance, I’m interested in just places near to the point of discovery of an object, and this can be seen in the segment of a record displayed below.

![Open Domesday data added to a record]({{ site.baseurl }}/files/2012/12/openDomesDay.png)

Open Domesday data added to a record

The API call that you need to do this is ‘**PlacesNear**: returns places within a radius (in km) of a given point, or places within a bounding box. Uses WGS84 coordinates.’ This is very simple to call as her examples show you and I just use the following format:

    http://domesdaymap.co.uk/api/1.0/placesnear?lat={LAT}&lng={LON}&radius={RADIUS}


    http://domesdaymap.co.uk/api/1.0/placesnear?lat={LAT}&lng={LON}&radius={RADIUS}

To automate this in my Zend Framework based application, I’ve built some simple PHP library code and some view helpers as detailed below.

### The service library classes

These can be seen here: [https://github.com/portableant/Beowulf—PAS/tree/master/library/Pas/Service/Domesday](https://github.com/portableant/Beowulf---PAS/tree/master/library/Pas/Service/Domesday)

Service class for interacting with Domesday site

    <?php

    class  App-Service-Domesday-Place  extends  Zend-Rest-Client  {

    protected  $_params  =  array();

    protected  $_uri  =  'http://domesdaymap.co.uk';

    protected  $_responseTypes  =  array('xml',  'json',  'django');

    protected  $_responseType  =  'json';

    protected  $_methods  =  array('place',  'placesnear',  'manor',

    'image',  'hundred',  'area',

    'county'  );

    protected  $_placeNearParams  =  array(

    'lat',  'lng',  'radius',

    's',  'e',  'n',

    'w','format');

    protected  $_apiPath  =  '/api/1.0/';

    public function  __construct(){

    $this->setUri($this->-uri);

    $client  =  self::getHttpClient();

    $client->setHeaders('Accept-Charset',  'ISO-8859-1,utf-8');

    }

    public  function  setParams($params)

    {

    foreach  ($params as  $key  =>  $value)  {

    switch  (strtolower($key))  {

    case  'format':

    $this->-params['format']  =  $this->setResponseType($value);

    break;

    default:

    $this->-params[$key]  =  $value;

    break;

    }

    }

    return  $this;

    }

    public  function  getParams()

    {

    return  $this->-params;

    }

    public  function  setResponseType($responseType)

    {

    if  (!in-array(strtolower($responseType),  $this->-responseTypes))  {

    throw  new  Pas-Service-Domesday-Exception('Invalid Response Type');

    }

    $this->-responseType  =  strtolower($responseType);

    return  $this->-responseType;

    }

    public  function  getResponseType()

    {

    return  $this->-responseType;

    }

    public  function  sendRequest($requestType,  $path)

    {

    $requestType  =  ucfirst(strtolower($requestType));

    if  ($requestType  !==  'Post'  &&  $requestType  !==  'Get')  {

    throw  new  Pas-Service-Domesday-Exception('Invalid request type: '  .  $requestType);

    }

    try  {

    $requestMethod  =  'rest'  .  $requestType;

    $response  =  $this->{$requestMethod}($path,  $this->getParams());

    return  $this->formatResponse($response);

    }  catch  (Zend-Http-Client-Exception  $e)  {

    throw  new  Pas-Service-Domesday-Exception($e->getMessage());

    }

    }

    /** Set up the response rendering

     *

     * @param string $response

     */

    public  function  formatResponse(Zend-Http-Response  $response)

    {

    if  ('json'  ===  $this->getResponseType())  {

    return  json-decode($response->getBody());

    }else  {

    return  new  Zend-Rest-Client-Result($response->getBody());

    }

    }

    /** Retrieve data from the api

         *

         * @param string $method

         * @param array $params

         */

    public  function  getData($method,  array  $params  =  array())

    {

    if(!in-array($method,$this->-methods)){

    throw  new  Pas-Service-Domesday-Exception('That is not a valid method');

    }

    foreach($params as  $k  =>  $v)  {

    if(!in-array($k,  $this->-placeNearParams)){

    unset($params['k']);

    }

    }

    $this->setParams($params);

    $path  =  $this->-apiPath  .  $method;

    return  $this->sendRequest('GET',  $path);

    }

    }

Exception class

    <?php

    /** The exception for geo based classes

    * @category   Pas

    * @package    Pas-Service-Domesday

    * @subpackage Exception

    * @copyright  Copyright (c) 2011 Daniel Pett

    * @license    GNU

    * @author        Daniel pett

    * @version       1

    * @since        26 September 2011

    */

    class  Pas-Service-Domesday-Exception  extends  Zend-Exception  {

    }

 The view helper
----------------

This needs rewriting for OO PHP and chained methods and perhaps moving HTML to a view partial instead. As always there are lots of different ways to write the code to get to the end point.

    <?php

    /**

    *

    * @author dpett

    * @version

    */

    /**

    * DomesdayNear helper

    *

    * @uses viewHelper Pas-View-Helper

    */

    class  Pas-View-Helper-DomesdayNear

    extends  Zend-View-Helper-Abstract  {

    protected  $_url  =  'http://domesdaymap.co.uk/';

    protected  $_baseurl  =  'http://domesdaymap.co.uk/place/';

    protected  $_domesday;

    protected  $_cache;

    public  function  --construct(){

    $this->-domesday  =  new  Pas-Service-Domesday-Place();

    $this->-cache  =  Zend-Registry::get('cache');

    }

    /**

         *

         */

    public  function  domesdayNear($lat,  $lng,  $radius)  {

    if(!is-int($radius)){

    throw  new  Exception('Defined radius needs to be an integer');

    }

    $params  =  array('lat'  =>  $lat,  'lng'  =>  $lng,  'radius'  =>  $radius);

    $key  =  md5($lat  .  $lng  .  $radius);

    $response  =  $this->getPlacesNear($params,$key);

    return  $this->buildHtml($response,  $radius);

    }

    public  function  getPlacesNear(array  $params,  $key  ){

    if  (!($this->-cache->test($key)))  {

    $data  =  $this->-domesday->getData('placesnear',  $params);

    $this->-cache->save($data);

    }  else  {

    $data  =  $this->-cache->load($key);

    }

    return  $data;

    }

    public  function  buildHtml($response,  $radius){

    if($response){

    $html  =  '<h3>Adjacent Domesday Book places</h3>';

    $html  .=  '<a  href="'  .  $this->-url  .  '"><img class="dec flow" src="http://domesdaymap.co.uk/media/images/lion1.gif" width="67" height="93"/></a>';

    $html  .=  '<ul>';

    foreach($response as  $domesday){

    $html  .=  '<li><a href="'  .  $this->-baseurl  .  $domesday->grid  .  '/'  .  $domesday->vill-slug

    .  '">'.  $domesday->vill  .  '</a></li>';

    }

    $html  .=  '</ul>';

    $html  .=  '<p>Domesday data  within '  .  $radius  .  ' km of discovery point is surfaced via the excellent <a href="http://domesdaymap.co.uk">

        Open Domesday</a> website.</p>';

    return  $html;

    }

    }

    }

To use this:

    <?php
    //In the view script in which you want this to appear
    echo  $this->domesdayNear($lat,  $lng,  $radius);
    ?>
