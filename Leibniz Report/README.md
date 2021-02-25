# Lebniz-Abfrage Publikationen

Wie erstelle ich einen Publikationsbericht für die Lebniz-Abfrage mit Vitro Query Tool?

## Data Distributor

### Schritt 1: Contruct Query Graph Builder

Die Abfrage bezieht sich auf die Publikationen eines bestimmten Jahres. Hierzu dient die Filterbedingung FILTER (STRSTARTS(str(?dateTime), "2019")). Es werden die URIs der Publikation zusammen mit ihren Typinformationen und Informationen zum Open Access abgerufen. 

```
/CONSTRUCT {
  ?publication <http://www.w3.org/2000/01/rdf-schema#label> ?label .
  ?publication a ?type .
  ?publication <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> ?access .
  ?publication <http://purl.org/ontology/bibo/status> ?status .
} WHERE {
  ?publication a <http://purl.org/ontology/bibo/Document> .
  ?publication a ?type .
  OPTIONAL { ?publication <http://www.w3.org/2000/01/rdf-schema#label> ?label . }
  OPTIONAL { ?publication <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> ?access . }
  OPTIONAL { ?publication <http://purl.org/ontology/bibo/status> ?status . }
  ?publication <http://vivoweb.org/ontology/core#dateTimeValue>  ?dateTimeObj .
  ?dateTimeObj  <http://vivoweb.org/ontology/core#dateTime> ?dateTime .
  FILTER (STRSTARTS(str(?dateTime), "2020"))
  MINUS { ?publication a <http://vivoweb.org/ontology/core#BlogPosting> . }
  MINUS { ?publication a <http://kerndatensatz-forschung.de/owl/Basis#Software> . }
  MINUS {
    ?publication a <https://vivo.tib.eu/fis/ontology/tib-vivo#Preprint> .
    ?publication <https://vivo.tib.eu/fis/ontology/tib-vivo#preprintOf> ?finalpub .
    ?finalpub a <http://purl.org/ontology/bibo/Document> .
  }
}/
```











