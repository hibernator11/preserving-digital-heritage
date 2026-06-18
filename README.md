# Preserving Digital Cultural Heritage

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/hibernator11/preserving-digital-heritage/HEAD)

## Overview

This project demonstrates how to use Wikidata and SPARQL queries to support digital heritage preservation by extracting, structuring, and analyzing cultural heritage metadata.

Wikidata provides a collaborative, open knowledge graph that includes information about artworks, cultural objects, authors, institutions, and historical context. By leveraging SPARQL queries, researchers and developers can retrieve rich, interconnected datasets for digital humanities, museum informatics, and heritage documentation.

This repository focuses on extracting structured information about artworks, including images, authorship, creation dates, and depicted subjects.

This project includes a jupyter notebook describing how to replicate the extraction process. The code is available in this [link](Prado.ipynb). The code can be run in services such as Binder and [EOSC](https://open-science-cloud.ec.europa.eu/services/interactive-notebooks).

## Use Case

Digital heritage preservation requires:

Structured metadata about cultural objects
Links between artworks, creators, and concepts
Multilingual labels and descriptions
Open and reusable data sources

Wikidata enables all of this through a semantic knowledge graph, making it ideal for:

Museum catalog enrichment
Cultural analytics
Digital archives
AI-assisted heritage research
Educational platforms

### SPARQL

```
SELECT DISTINCT ?s ?sLabel ?author ?authorLabel 
  (SAMPLE(?date) AS ?year) 
  (GROUP_CONCAT(DISTINCT ?depicts; SEPARATOR="; ") AS ?depictsAll) 
  (GROUP_CONCAT(DISTINCT ?depictsTxt; SEPARATOR="; ") 
      AS ?depictLabels)  
WHERE {?s wdt:P8905 ?prado . 
       ?s wdt:P18 ?image.
       OPTIONAL {?s wdt:P180 ?depicts. 
           ?depicts rdfs:label ?depictsTxt . 
           FILTER (LANG(?depictsTxt) = "en")}
       ?s wdt:P571 ?date.
       ?s wdt:P170 ?author
    SERVICE wikibase:label { bd:serviceParam wikibase:language 
                               "[AUTO_LANGUAGE],mul,en". }
}
GROUP BY ?s ?sLabel ?author ?authorLabel ?year
```

```
SELECT DISTINCT ?work ?workLabel 
WHERE {
       ?s wdt:P18 ?image.
       ?s wdt:P170 wd:Q5593.
       BIND(<https://data.cervantesvirtual.com/person/3448> 
       as ?bvmcID)
  SERVICE 
  <http://data.cervantesvirtual.com/openrdf-sesame/repositories/data> 
  {
    ?work dc:subject ?bvmcID .
    ?work rdfs:label ?workLabel .
   }
}
```

National Gallery property: https://www.wikidata.org/wiki/Property:P13325
Musée d'Orsay artwork ID: https://www.wikidata.org/wiki/Property:P4659

## Institutions

| Institution                       | Property     | No. records |
| --------------------------------- | ------------ | ----------- |
| Musée d'Orsay                     | *wdt:P4659*  | 1914        |
| National Gallery                  | *wdt:P13325* | 2472        |
| National Gallery of Ireland       | *wdt:P8906*  | 1052        |
| Prado Museum                      | *wdt:P8905*  | 4012        |
| Royal Museum of Fine Arts Antwerp | *wdt:4905*   | 2065        |


## Challenges & Notes
Wikidata Query Service (WDQS) may impose rate limits (HTTP 429) during high load or outages.
For large-scale extraction, consider:
batching queries
caching results
using Wikidata dumps instead of live queries
