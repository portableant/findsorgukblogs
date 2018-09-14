---
layout: default
title: Querying Seneschal for uris for reuse
published: 2013-07-31
author: Daniel Pett
---

Querying Seneschal for uris for reuse
-------------------------------------

I’m currently working on mapping PAS data to the [CIDOC-CRM](http://www.cidoc-crm.org/) and I’m trying to align our thesauri with the [SENESCHAL](http://www.heritagedata.org/blog/about-heritage-data/seneschal/) uris. To get the all the URIs from their system, run this SPARQL query on their [endpoint](http://heritagedata.org/test/sparql.php) with the URI, preferred label and scope note being obtained. I can then dump these into my database or create a CSV file easily from the results to reference later.

### FISH object thesaurus

To obtain the URIs and extra information from the FISH archaeological objects thesaurus which number 2020:

	PREFIX skos: &lt;http://www.w3.org/2004/02/skos/core#&gt;
	SELECT \* 
	WHERE { 
	?subject a skos:Concept ;
	skos:inScheme &lt;http://purl.org/heritagedata/schemes/mda\_obj&gt; ;
	skos:prefLabel ?label ;
	skos:scopeNote ?note
	}
	ORDER BY ASC(?label)

### EH periods

To obtain the English Heritage period URIs which number 30 in total:

	PREFIX skos: &lt;http://www.w3.org/2004/02/skos/core#&gt;
	SELECT \* 
	WHERE { 
	?subject a skos:Concept ;
	skos:inScheme &lt;http://purl.org/heritagedata/schemes/eh\_period&gt;;
	skos:prefLabel ?label;
	}
	ORDER BY ASC(?label)

### EH Building materials

To obtain the English Heritage building materials URIs which number 231 in total:

PREFIX skos: &lt;http://www.w3.org/2004/02/skos/core#&gt;
SELECT \* 
WHERE { 
?subject a skos:Concept ;
skos:inScheme &lt;http://purl.org/heritagedata/schemes/eh\_tbm&gt;;
skos:prefLabel ?label;
}
ORDER BY ASC(?label)

### EH Event types

To obtain the English Heritage event type URIs  of which there are 97:

	PREFIX skos: &lt;http://www.w3.org/2004/02/skos/core#&gt;
	SELECT \*
	WHERE { 
	?subject a skos:Concept ;
	skos:inScheme &lt;http://purl.org/heritagedata/schemes/agl\_et&gt; ;
	skos:prefLabel ?label ;
	}
	ORDER BY ASC(?label)

###  EH monument types

To obtain the EH monument type URIs of which there are 4855:

	PREFIX skos: &lt;http://www.w3.org/2004/02/skos/core#&gt;
	SELECT \*
	WHERE { 
	?subject a skos:Concept ;
	skos:inScheme &lt;http://purl.org/heritagedata/schemes/eh\_tmt2&gt; ;
	skos:prefLabel ?label ;
	}
	ORDER BY ASC(?label)

###  Counting results

To get a count for any of these:

	PREFIX skos: &lt;http://www.w3.org/2004/02/skos/core#&gt;
	SELECT (COUNT(\*) AS ?count)
	WHERE { 
	?subject a skos:Concept ;
	skos:inScheme &lt;SCHEME URI&gt; ;
}

### Offsetting

The end point returns a maximum of 250 records, and so to get more results, use OFFSET n as shown below:

	PREFIX skos: 
	SELECT \*
	WHERE {
	?subject a skos:Concept;
	skos:inScheme &lt;YOUR SCHEME URI&gt;;
	skos:prefLabel ?label;
	skos:scopeNote ?note
	}
	ORDER BY ASC(?label)
	OFFSET 250
                               