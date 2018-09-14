---
layout: default
author: Daniel Pett
category: central-unit
published: 2010-09-24
---
<div class="post-990 post type-post status-publish format-standard hentry category-database category-techie-stuff tag-amazon-s3 tag-cloud-storage tag-search-engine-visibility tag-web-statistics"
     id="post-990">
    <h2><a href="/2010/09/24/six-month-review/" rel="bookmark"
           title="Permanent Link to Six month review of new website performance">Six month review of new website
        performance</a></h2>
    <small>September 24th, 2010 by Daniel Pett</small>

    <div class="entry">
        <p><img class="flow" title="The Crosby Garrett Helmet" src="http://finds.org.uk/documents/image/19269498b.jpg"
                alt="The Crosby Garrett Helmet" width="300" height="423"></p>

        <p>The Scheme’s new website has been online now for 6 months and I’ve been looking at the performance and costs
            incurred during this period. We’ve had several large discoveries since the site went live – the <a
                    href="http://finds.org.uk/database/artefacts/record/id/387181" title="Frome Hoard record"
                    class="liinternal">Frome Hoard</a> and the <a
                    href="http://finds.org.uk/database/artefacts/record/id/404767" title="The Roman helmet record"
                    class="liinternal">Crosby Garrett Roman Helmet</a> for instance. However, they aren’t typical
            objects so we don’t get the big spikes in referral from large news aggregators or providers daily. I’m a
            little disappointed that web traffic hasn’t grown significantly since we went live with the new site, but
            we’re still getting &nbsp;a long period of activity/ pages viewed per visit. I’ve worked hard on search
            engine visibility (apart for a blip in July when I blocked all search engines via a typo in my robots.txt
            file – as the great Homer says, D’oh!) and we’re now seeing a surge in pages being added to Google’s index
            (nearly up to 50% of 400,000 publicly accessible pages now included according to webmaster tools).</p>
        <h4>Web statistics</h4>

        <p>All the web statistics are produced via Google Analytics, I haven’t bothered with the old logfile analysis.
            &nbsp;The old stats that we used to return for the <acronym title="Department for Culture Media and Sport">DCMS</acronym>
            and quoted in our annual reports were heavily reliant on ‘hits’, a metric I always hated. &nbsp;Some simple
            observations:</p>
        <ul>
            <li> We get a trend of heavy weekday usage, with noticeable dips at weekends when recording isn’t as
                prevalent.
                <p></p>

                <div id="attachment_1006" class="wp-caption alignnone" style="width: 310px"><a
                        href="/files/2010/09/pattern.jpg" class="liimagelink"
                        rel="lightbox[990]"><img class="size-medium wp-image-1006" title="Typical weekly pattern"
                                                 src="/files/2010/09/pattern-300x44.jpg"
                                                 alt="Typical weekly pattern" width="300" height="44"></a>

                    <p class="wp-caption-text">Typical weekly pattern</p></div>
            </li>
            <li>We don’t get a huge audience, our topic is pretty niche, but hopefully it will keep increasing.</li>
            <li>Overall visitors average 10mins 59 seconds on site and view nearly 14 pages a visit, with a bounce rate
                of 37.46%.
            </li>
            <li>Those visits that are mainly within the confines of the database module average 21 pages per visit and
                around 17 mins 17 seconds, with a bounce rate of 23.62%
            </li>
            <li>We had significant surges in traffic on the days that Frome and Crosby Garrett were announced (8th July
                and 14th September)
            </li>
            <li>14% of our users who are stuck with <acronym title="Internet Explorer - it sucks">IE</acronym> use
                version 6. Guess where the majority of these poor people are based… Government sector offices.
            </li>
            <li>149 countries are representing as having visited; the top two countries are the UK (76% of total) and
                USA (7%) which account. I assume, this is mainly because our subject material is mainly centred on
                England &amp; Wales. It would be great if we could penetrate the archaeological syllabus in other
                countries as we have such a mass of data to play with.
            </li>
            <li>We have now consolidated our domains down to one so our previous webstats definitely gave a false
                measure of usage of our resources.
            </li>
            <li>We have 66 partner organisations and 3 main funders/hosting organisations; <acronym
                    title="Museums, Libraries and Archives Council">MLA</acronym>, British Museum and <acronym
                    title="Department for Culture Media and Sport">DCMS</acronym>. We get very little referral traffic
                from any of these as shown here: <acronym title="The British Museum">BM</acronym> – 2,495 referrals,
                <acronym title="Museums, Libraries and Archives Council">MLA</acronym> – 64 referrals, <acronym
                        title="Department for Culture Media and Sport">DCMS</acronym> – 60 referrals. I think it is a
                shame a flagship project doesn’t get more click through, but then it is hard to position us higher on
                these sites as there is so much culture to promote. However, <acronym
                        title="Museums, Libraries and Archives Council">MLA</acronym>’s description of our project is
                rather out of date.
            </li>
            <li>Google accounts for 45.88% of the originating point for traffic to our site, Yahoo for 0.84% and Bing
                0.80%
            </li>
            <li>In the 6 month period, we have had 130,235 visits; &nbsp;1,788,580 page views;&nbsp;65,531 Visitors.
                Compared to the same period last year, (which coincidentally ends with the day that the Staffordshire
                Hoard was announced, we had 95,902 visits; 514,341 page views; 57,990 visitors – all figures for
                finds.org.uk and for findsdatabase.org.uk we had: 48,786 visits; 1,185,537 page views;&nbsp;17,767
                visitors). All these figures are devoid of usage of <acronym
                        title="eXtensible Markup Language">XML</acronym>/JSON/KML functions and feeds.
            </li>
            <li>A more detailed breakdown can be found in <a
                    href="/files/2010/09/findsorguk.pdf" class="lipdf">this
                <acronym title="Portable Document Format">PDF</acronym></a></li>
        </ul>
        <h4>New functions</h4>

        <p>Since launch, we’ve&nbsp; released lots of new features, all based on <a href="http://framework.zend.com"
                                                                                    class="liexternal">Zend
            framework</a> code:</p>
        <ul>
            <li>More extensive mining of <a href="http://www.theyworkforyou.com" class="liexternal">theyworkforyou</a>
                for Parliamentary data
                <ul>
                    <li>Finds by constituency boundary, for example the <a
                            href="http://finds.org.uk/news/theyworkforyou/finds/constituency/Arundel+and+South+Downs"
                            class="liinternal">Arundel consituency</a></li>
                    <li>Data on local MP, for example <a href="http://finds.org.uk/news/theyworkforyou/mp/id/10777"
                                                         title="David Cameron's details" class="liinternal">David
                        Cameron</a></li>
                    <li>Number of Scheduled monuments within boundary</li>
                </ul>
            </li>
            <li>Heavy use of YQL throughout the website
                <ul>
                    <li><a href="http://www.finds.org.uk/flickr" class="liinternal">Flickr images</a> pulled in</li>
                    <li>Oauth YQL calls to make use of Yahoo! geo functions</li>
                </ul>
            </li>
            <li>Created a load of YQL tables for Museum and heritage website <acronym
                    title="Application Programming Interface">API</acronym> and opensearch modules
            </li>
            <li>Integrated Geoplanet’s data into database backend from their data dump</li>
            <li>Added old OS maps from the National Library of Scotland (these are great and easy to implement) to most
                of our maps, for example a search for ‘<a
                        href="http://finds.org.uk/database/search/map/description/sompting"
                        title="Sompting finds mapped" class="liinternal">Sompting</a>‘ axeheads and click on
                ‘historical’
            </li>
            <li>Integrated the <a href="/2010/05/24/os150k/" class="liinternal">Ordnance
                Survey 1:50K dataset</a> for antiquities and Roman sites
            </li>
            <li>Integrated the English Heritage Scheduled Monuments dataset (only available to higher level users.)</li>
            <li>Pulled in data from Amazon for our references (prices, book cover art etc) for example <a
                    href="http://finds.org.uk/database/publications/publication/id/1390"
                    title="View a reference with Amazon data pulled in" class="liinternal">‘Toys, trifles and
                trinkets’</a> by Egan and Forsyth
            </li>
            <li>Mined the Guardian <acronym title="Application Programming Interface">API</acronym> for news relating to
                the Scheme
            </li>
            <li>Created functions for the public to record their own objects and find previously recorded ones easily.
                This has been quite well received, see <a
                        href="/2010/09/20/my-experience-of-self-recording-on-the-database/"
                        class="liinternal">Garry Crace’s article</a> on how he found it.
            </li>
            <li>Used some semantic techniques (<acronym
                    title="Friend Of A Friend is a RDF dialect for describing relationships">FOAF</acronym> for example
                – our <a href="http://www.finds.org.uk/contacts/" class="liinternal">contacts</a> page uses this in
                rdfa)
            </li>
            <li>Context switched formats for a wide array of pages across the site</li>
            <li>Got <a href="http://www.finds.org.uk/database/oai" title="Get access to our oai interface"
                       class="liinternal">OAI access</a> working
            </li>
            <li>Created extensive <a href="http://www.finds.org.uk/info/sitemap" title="Our sitemap index file"
                                     class="liinternal">sitemaps</a> for search indexing
            </li>
        </ul>
        <h4>Database statistics</h4>

        <p>Some raw statistics of progress with the new database can be seen below:</p>

        <p>24601 records have been created which documents the discovery and recording of 94,978 objects (one hoard of
            coins adds 52,503 objects alone – so remove these and you get &nbsp;42475 objects). We also released
            functions that allowed the public to record their own objects, and this has resulted in the addition of 740
            records from 32 recorders. We expect this number to increase following the release of an instructional guide
            produced by our Kent FLA and <acronym title="Finds Liaison Officer">FLO</acronym> – (Jess Bryan and <a
                    href="http://finds.org.uk/contacts/staff/profile/id/123" title="Jen's profile" class="liinternal">Jen
                Jackson</a>).</p>
        <h4>Users</h4>

        <p>User accounts created: 855 with no spam accounts created so far.</p>
        <ul>
            <li>2 Finds Adviser status</li>
            <li>38 Finds Liaison Officer status</li>
            <li>13 Historic Environment Officer status</li>
            <li>745 ordinary members</li>
            <li>57 Research status accounts</li>
        </ul>
        <p>In the previous existence of our database over at <a href="http://www.findsdatabase.org.uk"
                                                                class="liexternal">findsdatabase.org.uk</a>, we had&nbsp;1135
            accounts created in 7 years.</p>
        <h4>Research</h4>

        <p>58 new research projects have been added to our research register with the following levels of activity:</p>
        <ul>
            <li><a href="http://finds.org.uk/research/projects/level/1/Undergraduate" class="liinternal">7
                Undergraduate</a></li>
            <li><a href="http://finds.org.uk/research/projects/level/2/Masters+degree" class="liinternal">20 Masters
                degree</a></li>
            <li><a href="http://finds.org.uk/research/projects/level/3/PhD+level+research" class="liinternal">12 PhD
                level research</a></li>
            <li><a href="http://finds.org.uk/research/projects/level/4/Large+scale+research+AHRC" class="liinternal">3
                Large scale research AHRC</a></li>
            <li><a href="http://finds.org.uk/research/projects/level/5/Major+publication" class="liinternal">3 Major
                publication</a></li>
            <li><a href="http://finds.org.uk/research/projects/level/6/Magazine+journal+article" class="liinternal">1
                Magazine/journal article</a></li>
            <li><a href="http://finds.org.uk/research/projects/level/7/Desk+based+assessment" class="liinternal">1 Desk
                based assessment</a></li>
            <li><a href="http://finds.org.uk/research/projects/level/9/Personal+research+project" class="liinternal">10
                Personal research project</a></li>
            <li><a href="http://finds.org.uk/research/projects/level/10/Archaeology+society+project" class="liinternal">1
                Archaeology society project</a></li>
        </ul>
        <p>962,601 searches have been performed since relaunch. We’ve had 132 reports of incorrect data being published
            on our data (undoubtedly, there are more errors, people are just shy!) and 250-ish comments on records.
            These functions are both protected by reCaptchas and akismet and we’ve had 5 spam submissions in 6
            months.</p>
        <h4>Contributors of data</h4>

        <p>943 new contributors have offered data for recording or become involved by recording or researching. I’m
            tidying up the database so that we can do better analysis of what people use our facility for. We now
            collect primary activity and postcodes, so that we can do some better statistical analysis.</p>
        <h4>Running costs for following domains:</h4>

        <p><a href="http://www.finds.org.uk" class="liinternal">www.finds.org.uk</a><br>
            <a href="http://www.findsdatabase.org.uk" class="liexternal">www.findsdatabase.org.uk</a><br>
            <a href="http://www.staffordshirehoard.org.uk" class="liexternal">www.staffordshirehoard.org.uk</a><br>
            <a href="http://www.pastexplorers.org.uk" class="liexternal">www.pastexplorers.org.uk</a></p>

        <p>Server farm hosting fee: £828<br>
            Bandwidth cost for excess load: £234<br>
            Remote backup space: £900 (350GB images)<br>
            Amazon S3 backup space: £0.27 ($0.42) for (11GB data transfer of <acronym
                    title="MySQL database">MySQL</acronym> backups)<br>
            Flickr licence: £15.30 ($24)<br>
            Get satisfaction account: £36.38 ($57) which I cancelled after 3 months due to the fact it was underused.
        </p>

        <p>Development costs: Covered by my salary, not revealing that.</p>

        <p>Total IT cost for running: £2013.95 (or around 8p per record or a more meaningless statistic because of the
            huge hoard, of circa 2p per object)<br>
            We plan to make this reduce further by switching backup to S3 for images as well or renegotiating with our
            excellent providers at Dedipower in Reading. Since the demise of Oxford ArchDigital, we’ve already made IT
            cost savings of c. £15,000 per annum in support fees and also all development work has been taken on in
            house.</p>

        <p>Hopefully people are finding our new site much more useful, we’ve got more stuff to come….</p></div>

    
</div>