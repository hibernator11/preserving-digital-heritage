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
