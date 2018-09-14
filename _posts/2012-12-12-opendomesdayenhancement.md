---
layout: default
author: Daniel Pett
category: labs
published: 2012-12-12T14:46:55+00:00
title: Adding Open Domesday data to object records
---

<article role="article" id="post-110"
         class="post-110 post type-post status-publish format-standard hentry category-mapping category-php-2 category-zend-framework tag-domesday-book">

    <header class="entry-header">
        <h1 class="entry-title">Adding Open Domesday data to object records</h1>
    </header>
    <!-- .entry-header -->

    <div class="entry-content">
        <p>The Scheme website has had low level integration with the <a href="http://domesdaymap.co.uk/"
                                                                        title="The Open Domesday map project"
                                                                        class="liexternal">Open Domesday</a> project
            site since September 2011, and it has taken me this long to get round to writing about how it works. Each
            record we create, ideally should have a National Grid Reference (NGR) attached to it. These NGRs are
            converted at the point of data entry, via a backend script, to Latitude and Longitude which gives us more
            flexibility for integrating with 3rd party services (mapping apis, flickr, etc.) These LatLon pairs can also
            be used to retrieve data easily from Open Domesday, via&nbsp;Anna Powell-Smith’s excellent <acronym
                    title="Application Programming Interface">API</acronym>. &nbsp;In this instance, I’m interested in
            just places near to the point of discovery of an object, and this can be seen in the segment of a record
            displayed below.</p>

        <div id="attachment_111" class="wp-caption alignnone" style="width: 471px"><a
                href="/files/2012/12/openDomesDay.png" class="liimagelink"
                rel="lightbox[110]"><img class="size-full wp-image-111" alt="Open Domesday data added to a record"
                                         src="/files/2012/12/openDomesDay.png" width="461"
                                         height="142"></a>

            <p class="wp-caption-text">Open Domesday data added to a record</p></div>
        <p>The <acronym title="Application Programming Interface">API</acronym> call that you need to do this is
            ‘<strong>PlacesNear</strong>: returns places within a radius (in km) of a given point, or places within a
            bounding box. Uses WGS84 coordinates.’ This is very simple to call as her examples show you and I just use
            the following format:</p><!-- Crayon Syntax Highlighter v2.0.2 -->

        <div id="crayon-5146bdecb8135" class="crayon-syntax crayon-theme-ado crayon-font-monaco crayon-os-pc print-yes"
             data-settings=" scroll-mouseover"
             style=" margin-top: 12px; margin-bottom: 12px; float: none; clear: both; font-size: 12px !important; line-height: 15px !important;">

            <div class="crayon-toolbar" data-settings=" mouseover overlay hide delay"
                 style="height: 18px !important; line-height: 18px !important;">
                <div class="crayon-tools"><a class="crayon-nums-button crayon-button" title="Toggle Line Numbers"></a><a
                        class="crayon-wrap-button crayon-button" title="Toggle Line Wrap"></a><a
                        class="crayon-copy-button crayon-button" data-text="Press %s to Copy, %s to Paste"
                        title="Copy Plain Code"></a><a class="crayon-popup-button crayon-button"
                                                       title="Open Code In New Window" onclick="return false;"></a><a
                        class="crayon-expand-button crayon-button" title="Expand Code"></a><a
                        class="crayon-plain-button crayon-button" title="Toggle Plain Code"></a></div>
            </div>
            <div class="crayon-info" style="min-height: 15px !important; line-height: 15px !important;"></div>
            <div class="crayon-plain-wrap"><textarea wrap="off" class="crayon-plain print-no" data-settings="dblclick"
                                                     readonly=""
                                                     style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 12px !important; line-height: 15px !important;">http://domesdaymap.co.uk/api/1.0/placesnear?lat={LAT}&amp;lng={LON}&amp;radius={RADIUS}</textarea>
            </div>
            <div class="crayon-main" style="">
                <table class="crayon-table">
                    <tbody>
                    <tr class="crayon-row">
                        <td class="crayon-nums " data-settings="show"
                            style="font-size: 12px !important; line-height: 15px !important;">
                            <div class="crayon-nums-content">
                                <div class="crayon-num" data-line="crayon-5146bdecb8135-1">1</div>
                            </div>
                        </td>
                        <td class="crayon-code">
                            <div class="crayon-pre" style="font-size: 12px !important; line-height: 15px !important;">
                                <div class="crayon-line" id="crayon-5146bdecb8135-1"><span class="i">http</span><span
                                        class="o">:</span><span class="c">//domesdaymap.co.uk/api/1.0/placesnear?lat={LAT}&amp;lng={LON}&amp;radius={RADIUS}</span>
                                </div>
                            </div>
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <!-- [Format Time: 0.0009 seconds] -->
        <p>To automate this in my Zend Framework based application, I’ve built some simple <acronym
                title="Hypertext PreProcessing">PHP</acronym> library code and some view helpers as detailed below.</p>

        <h3>The service library classes</h3>

        <p>These can be seen here:&nbsp;<a
                href="https://github.com/portableant/Beowulf---PAS/tree/master/library/Pas/Service/Domesday"
                class="liexternal">https://github.com/portableant/Beowulf—<acronym title="Portable Antiquities Scheme">PAS</acronym>/tree/master/library/Pas/Service/Domesday</a>
        </p><!-- Crayon Syntax Highlighter v2.0.2 -->

        <div id="crayon-5146bdecb8145" class="crayon-syntax crayon-theme-ado crayon-font-monaco crayon-os-pc print-yes"
             data-settings=" scroll-mouseover"
             style=" margin-top: 12px; margin-bottom: 12px; float: none; clear: both; font-size: 12px !important; line-height: 15px !important;">

            <div class="crayon-toolbar" data-settings=" mouseover overlay hide delay"
                 style="height: 18px !important; line-height: 18px !important;"><span class="crayon-title">Service class for interacting with Domesday site</span>

                <div class="crayon-tools"><a class="crayon-nums-button crayon-button" title="Toggle Line Numbers"></a><a
                        class="crayon-wrap-button crayon-button" title="Toggle Line Wrap"></a><a
                        class="crayon-copy-button crayon-button" data-text="Press %s to Copy, %s to Paste"
                        title="Copy Plain Code"></a><a class="crayon-popup-button crayon-button"
                                                       title="Open Code In New Window" onclick="return false;"></a><a
                        class="crayon-expand-button crayon-button" title="Expand Code"></a><a
                        class="crayon-plain-button crayon-button" title="Toggle Plain Code"></a></div>
            </div>
            <div class="crayon-info" style="min-height: 15px !important; line-height: 15px !important;"></div>
            <div class="crayon-plain-wrap"><textarea wrap="off" class="crayon-plain print-no" data-settings="dblclick"
                                                     readonly=""
                                                     style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 12px !important; line-height: 15px !important;">&lt;?php

class App_Service_Domesday_Place extends Zend_Rest_Client {

	protected $_params = array();

	protected $_uri = 'http://domesdaymap.co.uk';

	protected $_responseTypes = array('xml', 'json', 'django');

	protected $_responseType = 'json';

    protected $_methods = array('place', 'placesnear', 'manor',
    	'image', 'hundred', 'area',
    	'county' );

    protected $_placeNearParams = array(
	    'lat', 'lng', 'radius',
	    's', 'e', 'n',
	    'w','format');

    protected $_apiPath = '/api/1.0/';

	public function __construct(){
	$this-&gt;setUri($this-&gt;_uri);
	$client = self::getHttpClient();
	$client-&gt;setHeaders('Accept-Charset', 'ISO-8859-1,utf-8');
	}

	public function setParams($params)
    {
        foreach ($params as $key =&gt; $value) {
            switch (strtolower($key)) {
                case 'format':
		    $this-&gt;_params['format'] = $this-&gt;setResponseType($value);
                    break;
                default:
                    $this-&gt;_params[$key] = $value;
                    break;
            }
        }
        return $this;
    }

    public function getParams()
    {
    	return $this-&gt;_params;
    }

    public function setResponseType($responseType)
    {
        if (!in_array(strtolower($responseType), $this-&gt;_responseTypes)) {
            throw new Pas_Service_Domesday_Exception('Invalid Response Type');
        }
        $this-&gt;_responseType = strtolower($responseType);
        return $this-&gt;_responseType;
    }

    public function getResponseType()
    {
        return $this-&gt;_responseType;
    }

    public function sendRequest($requestType, $path)
    {
        $requestType = ucfirst(strtolower($requestType));
        if ($requestType !== 'Post' &amp;&amp; $requestType !== 'Get') {
            throw new Pas_Service_Domesday_Exception('Invalid request type: ' . $requestType);
        }

        try {
            $requestMethod = 'rest' . $requestType;
            $response = $this-&gt;{$requestMethod}($path, $this-&gt;getParams());
            return $this-&gt;formatResponse($response);
        } catch (Zend_Http_Client_Exception $e) {
            throw new Pas_Service_Domesday_Exception($e-&gt;getMessage());
        }
    }

    /** Set up the response rendering
     *
     * @param string $response
     */
    public function formatResponse(Zend_Http_Response $response)
    {

        if ('json' === $this-&gt;getResponseType()) {
            return json_decode($response-&gt;getBody());
        }  else {
            return new Zend_Rest_Client_Result($response-&gt;getBody());
        }
    }

    /** Retrieve data from the api
     *
     * @param string $method
     * @param array $params
     */
    public function getData($method, array $params = array())
    {
    	if(!in_array($method,$this-&gt;_methods)){
    		throw new Pas_Service_Domesday_Exception('That is not a valid method');
    	}
    	foreach($params as $k =&gt; $v) {
			if(!in_array($k, $this-&gt;_placeNearParams)){
				unset($params['k']);
			}
		}
    	$this-&gt;setParams($params);
		$path = $this-&gt;_apiPath . $method;

        return $this-&gt;sendRequest('GET', $path);
    }

}</textarea></div>
            <div class="crayon-main" style="">
                <table class="crayon-table">
                    <tbody>
                    <tr class="crayon-row">
                        <td class="crayon-nums " data-settings="show"
                            style="font-size: 12px !important; line-height: 15px !important;">
                            <div class="crayon-nums-content">
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-1">1</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-2">2</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-3">3</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-4">4</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-5">5</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-6">6</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-7">7</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-8">8</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-9">9</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-10">10</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-11">11</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-12">12</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-13">13</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-14">14</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-15">15</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-16">16</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-17">17</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-18">18</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-19">19</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-20">20</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-21">21</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-22">22</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-23">23</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-24">24</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-25">25</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-26">26</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-27">27</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-28">28</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-29">29</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-30">30</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-31">31</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-32">32</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-33">33</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-34">34</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-35">35</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-36">36</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-37">37</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-38">38</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-39">39</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-40">40</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-41">41</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-42">42</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-43">43</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-44">44</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-45">45</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-46">46</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-47">47</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-48">48</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-49">49</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-50">50</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-51">51</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-52">52</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-53">53</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-54">54</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-55">55</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-56">56</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-57">57</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-58">58</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-59">59</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-60">60</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-61">61</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-62">62</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-63">63</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-64">64</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-65">65</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-66">66</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-67">67</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-68">68</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-69">69</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-70">70</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-71">71</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-72">72</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-73">73</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-74">74</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-75">75</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-76">76</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-77">77</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-78">78</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-79">79</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-80">80</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-81">81</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-82">82</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-83">83</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-84">84</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-85">85</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-86">86</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-87">87</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-88">88</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-89">89</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-90">90</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-91">91</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-92">92</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-93">93</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-94">94</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-95">95</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-96">96</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-97">97</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-98">98</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-99">99</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-100">100
                                </div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-101">101</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-102">102
                                </div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-103">103</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-104">104
                                </div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-105">105</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-106">106
                                </div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-107">107</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-108">108
                                </div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-109">109</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-110">110
                                </div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-111">111</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-112">112
                                </div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-113">113</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8145-114">114
                                </div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8145-115">115</div>
                            </div>
                        </td>
                        <td class="crayon-code">
                            <div class="crayon-pre" style="font-size: 12px !important; line-height: 15px !important;">
                                <div class="crayon-line" id="crayon-5146bdecb8145-1"><span class="o">&lt;</span><span
                                        class="sy">?</span><span class="e">php</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-2">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-3"><span class="t">class</span><span
                                        class="h"> </span><span class="e">App_Service_Domesday_Place</span><span
                                        class="h"> </span><span class="r">extends</span><span class="h"> </span><span
                                        class="e">Zend_Rest_Client</span><span class="h"> </span><span
                                        class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-4">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-5"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">protected</span><span class="h"> </span><span class="sy">$</span><span
                                        class="v">_params</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="t">array</span><span class="sy">(</span><span
                                        class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-6">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-7"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">protected</span><span class="h"> </span><span class="sy">$</span><span
                                        class="v">_uri</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="s">'http://domesdaymap.co.uk'</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-8">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-9"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">protected</span><span class="h"> </span><span class="sy">$</span><span
                                        class="v">_responseTypes</span><span class="h"> </span><span
                                        class="o">=</span><span class="h"> </span><span class="t">array</span><span
                                        class="sy">(</span><span class="s">'xml'</span><span class="sy">,</span><span
                                        class="h"> </span><span class="s">'json'</span><span class="sy">,</span><span
                                        class="h"> </span><span class="s">'django'</span><span class="sy">)</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-10">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-11"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">protected</span><span class="h"> </span><span class="sy">$</span><span
                                        class="v">_responseType</span><span class="h"> </span><span
                                        class="o">=</span><span class="h"> </span><span class="s">'json'</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-12">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-13"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">protected</span><span class="h"> </span><span class="sy">$</span><span
                                        class="v">_methods</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="t">array</span><span class="sy">(</span><span
                                        class="s">'place'</span><span class="sy">,</span><span class="h"> </span><span
                                        class="s">'placesnear'</span><span class="sy">,</span><span
                                        class="h"> </span><span class="s">'manor'</span><span class="sy">,</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-14"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="s">'image'</span><span class="sy">,</span><span class="h"> </span><span
                                        class="s">'hundred'</span><span class="sy">,</span><span class="h"> </span><span
                                        class="s">'area'</span><span class="sy">,</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-15"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="s">'county'</span><span class="h"> </span><span class="sy">)</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-16">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-17"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">protected</span><span class="h"> </span><span class="sy">$</span><span
                                        class="v">_placeNearParams</span><span class="h"> </span><span
                                        class="o">=</span><span class="h"> </span><span class="t">array</span><span
                                        class="sy">(</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-18"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="s">'lat'</span><span class="sy">,</span><span class="h"> </span><span
                                        class="s">'lng'</span><span class="sy">,</span><span class="h"> </span><span
                                        class="s">'radius'</span><span class="sy">,</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-19"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="s">'s'</span><span class="sy">,</span><span class="h"> </span><span
                                        class="s">'e'</span><span class="sy">,</span><span class="h"> </span><span
                                        class="s">'n'</span><span class="sy">,</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-20"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="s">'w'</span><span class="sy">,</span><span
                                        class="s">'format'</span><span class="sy">)</span><span class="sy">;</span>
                                </div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-21">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-22"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">protected</span><span
                                        class="h"> </span><span class="sy">$</span><span class="v">_apiPath</span><span
                                        class="h"> </span><span class="o">=</span><span class="h"> </span><span
                                        class="s">'/api/1.0/'</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-23">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-24"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">public</span><span
                                        class="h"> </span><span class="t">function</span><span class="h"> </span><span
                                        class="e">__construct</span><span class="sy">(</span><span
                                        class="sy">)</span><span class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-25"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="r">this</span><span class="o">-&gt;</span><span
                                        class="e">setUri</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_uri</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-26"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">$</span><span
                                        class="v">client</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="r">self</span><span class="o">::</span><span
                                        class="e">getHttpClient</span><span class="sy">(</span><span class="sy">)</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-27"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="i">client</span><span
                                        class="o">-&gt;</span><span class="e">setHeaders</span><span class="sy">(</span><span
                                        class="s">'Accept-Charset'</span><span class="sy">,</span><span
                                        class="h"> </span><span class="s">'ISO-8859-1,utf-8'</span><span
                                        class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-28"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">}</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-29">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-30"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">public</span><span
                                        class="h"> </span><span class="t">function</span><span class="h"> </span><span
                                        class="e">setParams</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">params</span><span class="sy">)</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-31"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-32"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">foreach</span><span class="h"> </span><span class="sy">(</span><span
                                        class="sy">$</span><span class="e">params </span><span class="st">as</span><span
                                        class="h"> </span><span class="sy">$</span><span class="v">key</span><span
                                        class="h"> </span><span class="o">=</span><span class="o">&gt;</span><span
                                        class="h"> </span><span class="sy">$</span><span class="i">value</span><span
                                        class="sy">)</span><span class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-33"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">switch</span><span class="h"> </span><span class="sy">(</span><span
                                        class="e">strtolower</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">key</span><span class="sy">)</span><span
                                        class="sy">)</span><span class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-34"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">case</span><span class="h"> </span><span
                                        class="s">'format'</span><span class="o">:</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-35"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_params</span><span class="sy">[</span><span class="s">'format'</span><span
                                        class="sy">]</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="sy">$</span><span class="r">this</span><span
                                        class="o">-&gt;</span><span class="e">setResponseType</span><span
                                        class="sy">(</span><span class="sy">$</span><span class="i">value</span><span
                                        class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-36"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">break</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-37"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">default</span><span class="o">:</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-38"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_params</span><span class="sy">[</span><span class="sy">$</span><span
                                        class="i">key</span><span class="sy">]</span><span class="h"> </span><span
                                        class="o">=</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">value</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-39"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">break</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-40"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-41"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-42"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">return</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-43"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-44">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-45"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">public</span><span class="h"> </span><span
                                        class="t">function</span><span class="h"> </span><span
                                        class="e">getParams</span><span class="sy">(</span><span class="sy">)</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-46"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-47"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">return</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_params</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-48"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">}</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-49">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-50"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">public</span><span
                                        class="h"> </span><span class="t">function</span><span class="h"> </span><span
                                        class="e">setResponseType</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">responseType</span><span class="sy">)</span>
                                </div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-51"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-52"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">if</span><span class="h"> </span><span class="sy">(</span><span
                                        class="o">!</span><span class="e">in_array</span><span class="sy">(</span><span
                                        class="e">strtolower</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">responseType</span><span
                                        class="sy">)</span><span class="sy">,</span><span class="h"> </span><span
                                        class="sy">$</span><span class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_responseTypes</span><span class="sy">)</span><span
                                        class="sy">)</span><span class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-53"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">throw</span><span class="h"> </span><span class="r">new</span><span
                                        class="h"> </span><span class="e">Pas_Service_Domesday_Exception</span><span
                                        class="sy">(</span><span class="s">'Invalid Response Type'</span><span
                                        class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-54"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-55"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="r">this</span><span class="o">-&gt;</span><span
                                        class="v">_responseType</span><span class="h"> </span><span
                                        class="o">=</span><span class="h"> </span><span class="e">strtolower</span><span
                                        class="sy">(</span><span class="sy">$</span><span
                                        class="i">responseType</span><span class="sy">)</span><span class="sy">;</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-56"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">return</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_responseType</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-57"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-58">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-59"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">public</span><span class="h"> </span><span
                                        class="t">function</span><span class="h"> </span><span
                                        class="e">getResponseType</span><span class="sy">(</span><span
                                        class="sy">)</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-60"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-61"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">return</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_responseType</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-62"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">}</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-63">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-64"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">public</span><span
                                        class="h"> </span><span class="t">function</span><span class="h"> </span><span
                                        class="e">sendRequest</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">requestType</span><span
                                        class="sy">,</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">path</span><span class="sy">)</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-65"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-66"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="v">requestType</span><span
                                        class="h"> </span><span class="o">=</span><span class="h"> </span><span
                                        class="e">ucfirst</span><span class="sy">(</span><span
                                        class="e">strtolower</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">requestType</span><span
                                        class="sy">)</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-67"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">if</span><span class="h"> </span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">requestType</span><span
                                        class="h"> </span><span class="o">!==</span><span class="h"> </span><span
                                        class="s">'Post'</span><span class="h"> </span><span class="o">&amp;&amp;</span><span
                                        class="h"> </span><span class="sy">$</span><span
                                        class="i">requestType</span><span class="h"> </span><span
                                        class="o">!==</span><span class="h"> </span><span class="s">'Get'</span><span
                                        class="sy">)</span><span class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-68"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">throw</span><span class="h"> </span><span class="r">new</span><span
                                        class="h"> </span><span class="e">Pas_Service_Domesday_Exception</span><span
                                        class="sy">(</span><span class="s">'Invalid request type: '</span><span
                                        class="h"> </span><span class="sy">.</span><span class="h"> </span><span
                                        class="sy">$</span><span class="i">requestType</span><span
                                        class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-69"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-70">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-71"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">try</span><span class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-72"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="v">requestMethod</span><span
                                        class="h"> </span><span class="o">=</span><span class="h"> </span><span
                                        class="s">'rest'</span><span class="h"> </span><span class="sy">.</span><span
                                        class="h"> </span><span class="sy">$</span><span
                                        class="i">requestType</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-73"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="e">response</span><span class="h"> </span><span
                                        class="o">=</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span class="sy">{</span><span
                                        class="sy">$</span><span class="i">requestMethod</span><span class="sy">}</span><span
                                        class="sy">(</span><span class="sy">$</span><span class="i">path</span><span
                                        class="sy">,</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="e">getParams</span><span class="sy">(</span><span
                                        class="sy">)</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-74"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">return</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="e">formatResponse</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">response</span><span class="sy">)</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-75"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span><span class="h"> </span><span class="st">catch</span><span
                                        class="h"> </span><span class="sy">(</span><span class="i">Zend_Http_Client_Exception</span><span
                                        class="h"> </span><span class="sy">$</span><span class="i">e</span><span
                                        class="sy">)</span><span class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-76"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">throw</span><span class="h"> </span><span class="r">new</span><span
                                        class="h"> </span><span class="e">Pas_Service_Domesday_Exception</span><span
                                        class="sy">(</span><span class="sy">$</span><span class="i">e</span><span
                                        class="o">-&gt;</span><span class="e">getMessage</span><span class="sy">(</span><span
                                        class="sy">)</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-77"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-78"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">}</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-79">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-80"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="c">/** Set up the response rendering</span>
                                </div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-81"><span class="c">&nbsp;&nbsp;&nbsp;&nbsp; *</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-82"><span
                                        class="c">&nbsp;&nbsp;&nbsp;&nbsp; * @param string $response</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-83"><span class="c">&nbsp;&nbsp;&nbsp;&nbsp; */</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-84"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">public</span><span
                                        class="h"> </span><span class="t">function</span><span class="h"> </span><span
                                        class="e">formatResponse</span><span class="sy">(</span><span class="i">Zend_Http_Response</span><span
                                        class="h"> </span><span class="sy">$</span><span class="i">response</span><span
                                        class="sy">)</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-85"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-86">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-87"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">if</span><span class="h"> </span><span class="sy">(</span><span
                                        class="s">'json'</span><span class="h"> </span><span class="o">===</span><span
                                        class="h"> </span><span class="sy">$</span><span class="r">this</span><span
                                        class="o">-&gt;</span><span class="e">getResponseType</span><span
                                        class="sy">(</span><span class="sy">)</span><span class="sy">)</span><span
                                        class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-88"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">return</span><span class="h"> </span><span
                                        class="e">json_decode</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">response</span><span
                                        class="o">-&gt;</span><span class="e">getBody</span><span
                                        class="sy">(</span><span class="sy">)</span><span class="sy">)</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-89"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span><span class="h">&nbsp;&nbsp;</span><span
                                        class="st">else</span><span class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-90"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">return</span><span class="h"> </span><span class="r">new</span><span
                                        class="h"> </span><span class="e">Zend_Rest_Client_Result</span><span
                                        class="sy">(</span><span class="sy">$</span><span class="i">response</span><span
                                        class="o">-&gt;</span><span class="e">getBody</span><span
                                        class="sy">(</span><span class="sy">)</span><span class="sy">)</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-91"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-92"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">}</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-93">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-94"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="c">/** Retrieve data from the api</span>
                                </div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-95"><span class="c">&nbsp;&nbsp;&nbsp;&nbsp; *</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-96"><span
                                        class="c">&nbsp;&nbsp;&nbsp;&nbsp; * @param string $method</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-97"><span class="c">&nbsp;&nbsp;&nbsp;&nbsp; * @param array $params</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-98"><span
                                        class="c">&nbsp;&nbsp;&nbsp;&nbsp; */</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-99"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">public</span><span class="h"> </span><span
                                        class="t">function</span><span class="h"> </span><span
                                        class="e">getData</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="i">method</span><span class="sy">,</span><span class="h"> </span><span
                                        class="t">array</span><span class="h"> </span><span class="sy">$</span><span
                                        class="v">params</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="t">array</span><span class="sy">(</span><span
                                        class="sy">)</span><span class="sy">)</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-100"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-101"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">if</span><span class="sy">(</span><span class="o">!</span><span
                                        class="e">in_array</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="i">method</span><span class="sy">,</span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span class="i">_methods</span><span
                                        class="sy">)</span><span class="sy">)</span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-102"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">throw</span><span class="h"> </span><span class="r">new</span><span
                                        class="h"> </span><span class="e">Pas_Service_Domesday_Exception</span><span
                                        class="sy">(</span><span class="s">'That is not a valid method'</span><span
                                        class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-103"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-104"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">foreach</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="e">params </span><span class="st">as</span><span class="h"> </span><span
                                        class="sy">$</span><span class="v">k</span><span class="h"> </span><span
                                        class="o">=</span><span class="o">&gt;</span><span class="h"> </span><span
                                        class="sy">$</span><span class="i">v</span><span class="sy">)</span><span
                                        class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-105"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">if</span><span class="sy">(</span><span class="o">!</span><span
                                        class="e">in_array</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="i">k</span><span class="sy">,</span><span class="h"> </span><span
                                        class="sy">$</span><span class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_placeNearParams</span><span class="sy">)</span><span
                                        class="sy">)</span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-106"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="e">unset</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="i">params</span><span class="sy">[</span><span class="s">'k'</span><span
                                        class="sy">]</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-107"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-108"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-109"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="r">this</span><span class="o">-&gt;</span><span
                                        class="e">setParams</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">params</span><span class="sy">)</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-110"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="v">path</span><span class="h"> </span><span
                                        class="o">=</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span class="i">_apiPath</span><span
                                        class="h"> </span><span class="sy">.</span><span class="h"> </span><span
                                        class="sy">$</span><span class="i">method</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-111">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-112"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">return</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="e">sendRequest</span><span class="sy">(</span><span
                                        class="s">'GET'</span><span class="sy">,</span><span class="h"> </span><span
                                        class="sy">$</span><span class="i">path</span><span class="sy">)</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-113"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8145-114">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8145-115"><span class="sy">}</span></div>
                            </div>
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <!-- [Format Time: 0.0437 seconds] -->
        <p>&nbsp;</p><!-- Crayon Syntax Highlighter v2.0.2 -->

        <div id="crayon-5146bdecb8155" class="crayon-syntax crayon-theme-ado crayon-font-monaco crayon-os-pc print-yes"
             data-settings=" scroll-mouseover"
             style=" margin-top: 12px; margin-bottom: 12px; float: none; clear: both; font-size: 12px !important; line-height: 15px !important;">

            <div class="crayon-toolbar" data-settings=" mouseover overlay hide delay"
                 style="height: 18px !important; line-height: 18px !important;"><span class="crayon-title">Exception class</span>

                <div class="crayon-tools"><a class="crayon-nums-button crayon-button" title="Toggle Line Numbers"></a><a
                        class="crayon-wrap-button crayon-button" title="Toggle Line Wrap"></a><a
                        class="crayon-copy-button crayon-button" data-text="Press %s to Copy, %s to Paste"
                        title="Copy Plain Code"></a><a class="crayon-popup-button crayon-button"
                                                       title="Open Code In New Window" onclick="return false;"></a><a
                        class="crayon-expand-button crayon-button" title="Expand Code"></a><a
                        class="crayon-plain-button crayon-button" title="Toggle Plain Code"></a></div>
            </div>
            <div class="crayon-info" style="min-height: 15px !important; line-height: 15px !important;"></div>
            <div class="crayon-plain-wrap"><textarea wrap="off" class="crayon-plain print-no" data-settings="dblclick"
                                                     readonly=""
                                                     style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 12px !important; line-height: 15px !important;">&lt;?php
/** The exception for geo based classes
 * @category   Pas
 * @package    Pas_Service_Domesday
 * @subpackage Exception
 * @copyright  Copyright (c) 2011 Daniel Pett
 * @license    GNU
 * @author 	   Daniel pett
 * @version	   1
 * @since 	   26 September 2011
 */
class Pas_Service_Domesday_Exception extends Zend_Exception {

}</textarea></div>
            <div class="crayon-main" style="">
                <table class="crayon-table">
                    <tbody>
                    <tr class="crayon-row">
                        <td class="crayon-nums " data-settings="show"
                            style="font-size: 12px !important; line-height: 15px !important;">
                            <div class="crayon-nums-content">
                                <div class="crayon-num" data-line="crayon-5146bdecb8155-1">1</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8155-2">2</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8155-3">3</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8155-4">4</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8155-5">5</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8155-6">6</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8155-7">7</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8155-8">8</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8155-9">9</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8155-10">10</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8155-11">11</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8155-12">12</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8155-13">13</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8155-14">14</div>
                            </div>
                        </td>
                        <td class="crayon-code">
                            <div class="crayon-pre" style="font-size: 12px !important; line-height: 15px !important;">
                                <div class="crayon-line" id="crayon-5146bdecb8155-1"><span class="o">&lt;</span><span
                                        class="sy">?</span><span class="i">php</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8155-2"><span
                                        class="c">/** The exception for geo based classes</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8155-3"><span class="c"> * @category&nbsp;&nbsp; Pas</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8155-4"><span
                                        class="c"> * @package&nbsp;&nbsp;&nbsp;&nbsp;Pas_Service_Domesday</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8155-5"><span class="c"> * @subpackage Exception</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8155-6"><span
                                        class="c"> * @copyright&nbsp;&nbsp;Copyright (c) 2011 Daniel Pett</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8155-7"><span class="c"> * @license&nbsp;&nbsp;&nbsp;&nbsp;GNU</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8155-8"><span
                                        class="c"> * @author &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Daniel pett</span>
                                </div>
                                <div class="crayon-line" id="crayon-5146bdecb8155-9"><span class="c"> * @version&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8155-10"><span
                                        class="c"> * @since &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 26 September 2011</span>
                                </div>
                                <div class="crayon-line" id="crayon-5146bdecb8155-11"><span class="c"> */</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8155-12"><span
                                        class="t">class</span><span class="h"> </span><span class="e">Pas_Service_Domesday_Exception</span><span
                                        class="h"> </span><span class="r">extends</span><span class="h"> </span><span
                                        class="e">Zend_Exception</span><span class="h"> </span><span class="sy">{</span>
                                </div>
                                <div class="crayon-line" id="crayon-5146bdecb8155-13">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8155-14"><span
                                        class="sy">}</span></div>
                            </div>
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <!-- [Format Time: 0.0015 seconds] -->
        <p></p>

        <h2>&nbsp;The view helper</h2>

        <p>This needs rewriting for OO <acronym title="Hypertext PreProcessing">PHP</acronym> and chained methods and
            perhaps moving <acronym title="HyperText Markup Language">HTML</acronym> to a view partial instead. As
            always there are lots of different ways to write the code to get to the end point.</p>
        <!-- Crayon Syntax Highlighter v2.0.2 -->

        <div id="crayon-5146bdecb8162" class="crayon-syntax crayon-theme-ado crayon-font-monaco crayon-os-pc print-yes"
             data-settings=" scroll-mouseover"
             style=" margin-top: 12px; margin-bottom: 12px; float: none; clear: both; font-size: 12px !important; line-height: 15px !important;">

            <div class="crayon-toolbar" data-settings=" mouseover overlay hide delay"
                 style="height: 18px !important; line-height: 18px !important;">
                <div class="crayon-tools"><a class="crayon-nums-button crayon-button" title="Toggle Line Numbers"></a><a
                        class="crayon-wrap-button crayon-button" title="Toggle Line Wrap"></a><a
                        class="crayon-copy-button crayon-button" data-text="Press %s to Copy, %s to Paste"
                        title="Copy Plain Code"></a><a class="crayon-popup-button crayon-button"
                                                       title="Open Code In New Window" onclick="return false;"></a><a
                        class="crayon-expand-button crayon-button" title="Expand Code"></a><a
                        class="crayon-plain-button crayon-button" title="Toggle Plain Code"></a></div>
            </div>
            <div class="crayon-info" style="min-height: 15px !important; line-height: 15px !important;"></div>
            <div class="crayon-plain-wrap"><textarea wrap="off" class="crayon-plain print-no" data-settings="dblclick"
                                                     readonly=""
                                                     style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 12px !important; line-height: 15px !important;">&lt;?php
/**
 *
 * @author dpett
 * @version
 */

/**
 * DomesdayNear helper
 *
 * @uses viewHelper Pas_View_Helper
 */
class Pas_View_Helper_DomesdayNear
	extends Zend_View_Helper_Abstract {

	protected $_url = 'http://domesdaymap.co.uk/';

	protected $_baseurl = 'http://domesdaymap.co.uk/place/';

	protected $_domesday;

	protected $_cache;

	public function __construct(){
		$this-&gt;_domesday = new Pas_Service_Domesday_Place();
		$this-&gt;_cache = Zend_Registry::get('cache');
	}

	/**
	 *
	 */
	public function domesdayNear($lat, $lng, $radius) {
	if(!is_int($radius)){
		throw new Exception('Defined radius needs to be an integer');
	}
	$params = array('lat' =&gt; $lat, 'lng' =&gt; $lng, 'radius' =&gt; $radius);
	$key = md5($lat . $lng . $radius);
	$response = $this-&gt;getPlacesNear($params,$key);

	return $this-&gt;buildHtml($response, $radius);
	}

	public function getPlacesNear(array $params, $key ){
		if (!($this-&gt;_cache-&gt;test($key))) {
		$data = $this-&gt;_domesday-&gt;getData('placesnear', $params);
		$this-&gt;_cache-&gt;save($data);
		} else {
		$data = $this-&gt;_cache-&gt;load($key);
		}
		return $data;
	}

	public function buildHtml($response, $radius){

	if($response){
	$html = '&lt;h3&gt;Adjacent Domesday Book places&lt;/h3&gt;';
	$html .= '&lt;a  href="' . $this-&gt;_url . '"&gt;&lt;img class="dec flow" src="http://domesdaymap.co.uk/media/images/lion1.gif" width="67" height="93"/&gt;&lt;/a&gt;';
	$html .= '&lt;ul&gt;';
	foreach($response as $domesday){
		$html .= '&lt;li&gt;&lt;a href="' . $this-&gt;_baseurl . $domesday-&gt;grid . '/' . $domesday-&gt;vill_slug
		. '"&gt;'. $domesday-&gt;vill . '&lt;/a&gt;&lt;/li&gt;';
	}
	$html .= '&lt;/ul&gt;';
	$html .= '&lt;p&gt;Domesday data  within ' . $radius . ' km of discovery point is surfaced via the excellent &lt;a href="http://domesdaymap.co.uk"&gt;
	Open Domesday&lt;/a&gt; website.&lt;/p&gt;';
	return $html;
	}
	}

}</textarea></div>
            <div class="crayon-main" style="">
                <table class="crayon-table">
                    <tbody>
                    <tr class="crayon-row">
                        <td class="crayon-nums " data-settings="show"
                            style="font-size: 12px !important; line-height: 15px !important;">
                            <div class="crayon-nums-content">
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-1">1</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-2">2</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-3">3</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-4">4</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-5">5</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-6">6</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-7">7</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-8">8</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-9">9</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-10">10</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-11">11</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-12">12</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-13">13</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-14">14</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-15">15</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-16">16</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-17">17</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-18">18</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-19">19</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-20">20</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-21">21</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-22">22</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-23">23</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-24">24</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-25">25</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-26">26</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-27">27</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-28">28</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-29">29</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-30">30</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-31">31</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-32">32</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-33">33</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-34">34</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-35">35</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-36">36</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-37">37</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-38">38</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-39">39</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-40">40</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-41">41</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-42">42</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-43">43</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-44">44</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-45">45</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-46">46</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-47">47</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-48">48</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-49">49</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-50">50</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-51">51</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-52">52</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-53">53</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-54">54</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-55">55</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-56">56</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-57">57</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-58">58</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-59">59</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-60">60</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-61">61</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-62">62</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-63">63</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-64">64</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-65">65</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-66">66</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-67">67</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-68">68</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb8162-69">69</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb8162-70">70</div>
                            </div>
                        </td>
                        <td class="crayon-code">
                            <div class="crayon-pre" style="font-size: 12px !important; line-height: 15px !important;">
                                <div class="crayon-line" id="crayon-5146bdecb8162-1"><span class="o">&lt;</span><span
                                        class="sy">?</span><span class="e">php</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-2"><span
                                        class="c">/**</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-3"><span class="c"> *</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-4"><span
                                        class="c"> * @author dpett</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-5"><span class="c"> * @version</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-6"><span
                                        class="c"> */</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-7">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-8"><span
                                        class="c">/**</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-9"><span class="c"> * DomesdayNear helper</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-10"><span
                                        class="c"> *</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-11"><span class="c"> * @uses viewHelper Pas_View_Helper</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-12"><span
                                        class="c"> */</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-13"><span class="t">class</span><span
                                        class="h"> </span><span class="e">Pas_View_Helper_DomesdayNear</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-14"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="r">extends</span><span
                                        class="h"> </span><span class="e">Zend_View_Helper_Abstract</span><span
                                        class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-15">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-16"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">protected</span><span
                                        class="h"> </span><span class="sy">$</span><span class="v">_url</span><span
                                        class="h"> </span><span class="o">=</span><span class="h"> </span><span
                                        class="s">'http://domesdaymap.co.uk/'</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-17">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-18"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">protected</span><span
                                        class="h"> </span><span class="sy">$</span><span class="v">_baseurl</span><span
                                        class="h"> </span><span class="o">=</span><span class="h"> </span><span
                                        class="s">'http://domesdaymap.co.uk/place/'</span><span class="sy">;</span>
                                </div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-19">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-20"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">protected</span><span
                                        class="h"> </span><span class="sy">$</span><span class="i">_domesday</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-21">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-22"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">protected</span><span
                                        class="h"> </span><span class="sy">$</span><span class="i">_cache</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-23">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-24"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">public</span><span
                                        class="h"> </span><span class="t">function</span><span class="h"> </span><span
                                        class="e">__construct</span><span class="sy">(</span><span
                                        class="sy">)</span><span class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-25"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="r">this</span><span class="o">-&gt;</span><span
                                        class="v">_domesday</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="r">new</span><span class="h"> </span><span
                                        class="e">Pas_Service_Domesday_Place</span><span class="sy">(</span><span
                                        class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-26"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="r">this</span><span class="o">-&gt;</span><span
                                        class="v">_cache</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="i">Zend_Registry</span><span
                                        class="o">::</span><span class="e">get</span><span class="sy">(</span><span
                                        class="s">'cache'</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-27"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-28">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-29"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="c">/**</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-30"><span
                                        class="c">&nbsp;&nbsp;&nbsp;&nbsp; *</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-31"><span class="c">&nbsp;&nbsp;&nbsp;&nbsp; */</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-32"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="m">public</span><span
                                        class="h"> </span><span class="t">function</span><span class="h"> </span><span
                                        class="e">domesdayNear</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">lat</span><span class="sy">,</span><span
                                        class="h"> </span><span class="sy">$</span><span class="i">lng</span><span
                                        class="sy">,</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">radius</span><span class="sy">)</span><span class="h"> </span><span
                                        class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-33"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">if</span><span class="sy">(</span><span class="o">!</span><span
                                        class="e">is_int</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="i">radius</span><span class="sy">)</span><span class="sy">)</span><span
                                        class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-34"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">throw</span><span class="h"> </span><span class="r">new</span><span
                                        class="h"> </span><span class="e">Exception</span><span class="sy">(</span><span
                                        class="s">'Defined radius needs to be an integer'</span><span
                                        class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-35"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-36"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">$</span><span
                                        class="v">params</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="t">array</span><span class="sy">(</span><span
                                        class="s">'lat'</span><span class="h"> </span><span class="o">=</span><span
                                        class="o">&gt;</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">lat</span><span class="sy">,</span><span class="h"> </span><span
                                        class="s">'lng'</span><span class="h"> </span><span class="o">=</span><span
                                        class="o">&gt;</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">lng</span><span class="sy">,</span><span class="h"> </span><span
                                        class="s">'radius'</span><span class="h"> </span><span class="o">=</span><span
                                        class="o">&gt;</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">radius</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-37"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="v">key</span><span class="h"> </span><span
                                        class="o">=</span><span class="h"> </span><span class="e">md5</span><span
                                        class="sy">(</span><span class="sy">$</span><span class="i">lat</span><span
                                        class="h"> </span><span class="sy">.</span><span class="h"> </span><span
                                        class="sy">$</span><span class="i">lng</span><span class="h"> </span><span
                                        class="sy">.</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">radius</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-38"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">$</span><span
                                        class="v">response</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="sy">$</span><span class="r">this</span><span
                                        class="o">-&gt;</span><span class="e">getPlacesNear</span><span
                                        class="sy">(</span><span class="sy">$</span><span class="i">params</span><span
                                        class="sy">,</span><span class="sy">$</span><span class="i">key</span><span
                                        class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-39">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-40"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="st">return</span><span
                                        class="h"> </span><span class="sy">$</span><span class="r">this</span><span
                                        class="o">-&gt;</span><span class="e">buildHtml</span><span
                                        class="sy">(</span><span class="sy">$</span><span class="i">response</span><span
                                        class="sy">,</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">radius</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-41"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-42">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-43"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">public</span><span class="h"> </span><span
                                        class="t">function</span><span class="h"> </span><span
                                        class="e">getPlacesNear</span><span class="sy">(</span><span
                                        class="t">array</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">params</span><span class="sy">,</span><span class="h"> </span><span
                                        class="sy">$</span><span class="i">key</span><span class="h"> </span><span
                                        class="sy">)</span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-44"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">if</span><span class="h"> </span><span class="sy">(</span><span
                                        class="o">!</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_cache</span><span class="o">-&gt;</span><span
                                        class="e">test</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="i">key</span><span class="sy">)</span><span class="sy">)</span><span
                                        class="sy">)</span><span class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-45"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="v">data</span><span class="h"> </span><span
                                        class="o">=</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_domesday</span><span class="o">-&gt;</span><span
                                        class="e">getData</span><span class="sy">(</span><span
                                        class="s">'placesnear'</span><span class="sy">,</span><span
                                        class="h"> </span><span class="sy">$</span><span class="i">params</span><span
                                        class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-46"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_cache</span><span class="o">-&gt;</span><span
                                        class="e">save</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="i">data</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-47"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span><span class="h"> </span><span class="st">else</span><span
                                        class="h"> </span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-48"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="v">data</span><span class="h"> </span><span
                                        class="o">=</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_cache</span><span class="o">-&gt;</span><span
                                        class="e">load</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="i">key</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-49"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-50"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">return</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">data</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-51"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-52">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-53"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="m">public</span><span class="h"> </span><span
                                        class="t">function</span><span class="h"> </span><span
                                        class="e">buildHtml</span><span class="sy">(</span><span
                                        class="sy">$</span><span class="i">response</span><span class="sy">,</span><span
                                        class="h"> </span><span class="sy">$</span><span class="i">radius</span><span
                                        class="sy">)</span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-54">&nbsp;</div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-55"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">if</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="i">response</span><span class="sy">)</span><span class="sy">{</span>
                                </div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-56"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">$</span><span
                                        class="v">html</span><span class="h"> </span><span class="o">=</span><span
                                        class="h"> </span><span class="s">'&lt;h3&gt;Adjacent Domesday Book places&lt;/h3&gt;'</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-57"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="i">html</span><span class="h"> </span><span
                                        class="sy">.</span><span class="o">=</span><span class="h"> </span><span
                                        class="s">'&lt;a&nbsp;&nbsp;href="'</span><span class="h"> </span><span
                                        class="sy">.</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span
                                        class="i">_url</span><span class="h"> </span><span class="sy">.</span><span
                                        class="h"> </span><span class="s">'"&gt;&lt;img class="dec flow" src="http://domesdaymap.co.uk/media/images/lion1.gif" width="67" height="93"/&gt;&lt;/a&gt;'</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-58"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">$</span><span
                                        class="i">html</span><span class="h"> </span><span class="sy">.</span><span
                                        class="o">=</span><span class="h"> </span><span
                                        class="s">'&lt;ul&gt;'</span><span class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-59"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="st">foreach</span><span class="sy">(</span><span class="sy">$</span><span
                                        class="e">response </span><span class="st">as</span><span
                                        class="h"> </span><span class="sy">$</span><span class="i">domesday</span><span
                                        class="sy">)</span><span class="sy">{</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-60"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="i">html</span><span class="h"> </span><span
                                        class="sy">.</span><span class="o">=</span><span class="h"> </span><span
                                        class="s">'&lt;li&gt;&lt;a href="'</span><span class="h"> </span><span
                                        class="sy">.</span><span class="h"> </span><span class="sy">$</span><span
                                        class="r">this</span><span class="o">-&gt;</span><span class="i">_baseurl</span><span
                                        class="h"> </span><span class="sy">.</span><span class="h"> </span><span
                                        class="sy">$</span><span class="i">domesday</span><span
                                        class="o">-&gt;</span><span class="i">grid</span><span class="h"> </span><span
                                        class="sy">.</span><span class="h"> </span><span class="s">'/'</span><span
                                        class="h"> </span><span class="sy">.</span><span class="h"> </span><span
                                        class="sy">$</span><span class="i">domesday</span><span
                                        class="o">-&gt;</span><span class="i">vill_slug</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-61"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">.</span><span class="h"> </span><span class="s">'"&gt;'</span><span
                                        class="sy">.</span><span class="h"> </span><span class="sy">$</span><span
                                        class="i">domesday</span><span class="o">-&gt;</span><span class="i">vill</span><span
                                        class="h"> </span><span class="sy">.</span><span class="h"> </span><span
                                        class="s">'&lt;/a&gt;&lt;/li&gt;'</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-62"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">}</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-63"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">$</span><span class="i">html</span><span class="h"> </span><span
                                        class="sy">.</span><span class="o">=</span><span class="h"> </span><span
                                        class="s">'&lt;/ul&gt;'</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-64"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">$</span><span
                                        class="i">html</span><span class="h"> </span><span class="sy">.</span><span
                                        class="o">=</span><span class="h"> </span><span class="s">'&lt;p&gt;Domesday data&nbsp;&nbsp;within '</span><span
                                        class="h"> </span><span class="sy">.</span><span class="h"> </span><span
                                        class="sy">$</span><span class="i">radius</span><span class="h"> </span><span
                                        class="sy">.</span><span class="h"> </span><span class="s">' km of discovery point is surfaced via the excellent &lt;a href="http://domesdaymap.co.uk"&gt;</span>
                                </div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-65"><span class="s">&nbsp;&nbsp;&nbsp;&nbsp;Open Domesday&lt;/a&gt; website.&lt;/p&gt;'</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-66"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="st">return</span><span
                                        class="h"> </span><span class="sy">$</span><span class="i">html</span><span
                                        class="sy">;</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-67"><span class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span
                                        class="sy">}</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-68"><span
                                        class="h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sy">}</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb8162-69">&nbsp;</div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb8162-70"><span
                                        class="sy">}</span></div>
                            </div>
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <!-- [Format Time: 0.0270 seconds] -->
        <p>To use this:</p><!-- Crayon Syntax Highlighter v2.0.2 -->

        <div id="crayon-5146bdecb816f" class="crayon-syntax crayon-theme-ado crayon-font-monaco crayon-os-pc print-yes"
             data-settings=" scroll-mouseover"
             style=" margin-top: 12px; margin-bottom: 12px; float: none; clear: both; font-size: 12px !important; line-height: 15px !important;">

            <div class="crayon-toolbar" data-settings=" mouseover overlay hide delay"
                 style="height: 18px !important; line-height: 18px !important;">
                <div class="crayon-tools"><span class="crayon-mixed-highlight"
                                                title="Contains Mixed Languages"></span><a
                        class="crayon-nums-button crayon-button" title="Toggle Line Numbers"></a><a
                        class="crayon-wrap-button crayon-button" title="Toggle Line Wrap"></a><a
                        class="crayon-copy-button crayon-button" data-text="Press %s to Copy, %s to Paste"
                        title="Copy Plain Code"></a><a class="crayon-popup-button crayon-button"
                                                       title="Open Code In New Window" onclick="return false;"></a><a
                        class="crayon-expand-button crayon-button" title="Expand Code"></a><a
                        class="crayon-plain-button crayon-button" title="Toggle Plain Code"></a></div>
            </div>
            <div class="crayon-info" style="min-height: 15px !important; line-height: 15px !important;"></div>
            <div class="crayon-plain-wrap"><textarea wrap="off" class="crayon-plain print-no" data-settings="dblclick"
                                                     readonly=""
                                                     style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 12px !important; line-height: 15px !important;">&lt;?php 
//In the view script in which you want this to appear
echo $this-&gt;domesdayNear($lat, $lng, $radius);
?&gt;</textarea></div>
            <div class="crayon-main" style="">
                <table class="crayon-table">
                    <tbody>
                    <tr class="crayon-row">
                        <td class="crayon-nums " data-settings="show"
                            style="font-size: 12px !important; line-height: 15px !important;">
                            <div class="crayon-nums-content">
                                <div class="crayon-num" data-line="crayon-5146bdecb816f-1">1</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb816f-2">2</div>
                                <div class="crayon-num" data-line="crayon-5146bdecb816f-3">3</div>
                                <div class="crayon-num crayon-striped-num" data-line="crayon-5146bdecb816f-4">4</div>
                            </div>
                        </td>
                        <td class="crayon-code">
                            <div class="crayon-pre" style="font-size: 12px !important; line-height: 15px !important;">
                                <div class="crayon-line" id="crayon-5146bdecb816f-1"><span
                                        class="ta">&lt;?php</span><span class="h"> </span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb816f-2"><span
                                        class="c">//In the view script in which you want this to appear</span></div>
                                <div class="crayon-line" id="crayon-5146bdecb816f-3"><span class="k ">echo</span><span
                                        class="h"> </span><span class="v">$this</span><span class="o">-&gt;</span><span
                                        class="e">domesdayNear</span><span class="sy">(</span><span
                                        class="v">$lat</span><span class="sy">,</span><span class="h"> </span><span
                                        class="v">$lng</span><span class="sy">,</span><span class="h"> </span><span
                                        class="v">$radius</span><span class="sy">)</span><span class="sy">;</span></div>
                                <div class="crayon-line crayon-striped-line" id="crayon-5146bdecb816f-4"><span
                                        class="ta">?&gt;</span></div>
                            </div>
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <p>&nbsp;</p>


</article>