# Wikidata-extraction-museums

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

National Gallery property: https://www.wikidata.org/wiki/Property:P13325
Musée d'Orsay artwork ID: https://www.wikidata.org/wiki/Property:P4659
