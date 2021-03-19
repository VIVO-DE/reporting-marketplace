# Bericht zu den Drittmitteln nach Mittelgebern
Die Abfrage erlaubt, Budget-Summen pro Jahr differenziert nach Drittmittelgebern zu erhalten.
## Data Distributors 

### Schritt 1: Construct Query Graph Builder
Die Abfrage bezieht sich auf das bewillgte Budget der Projekte, die in einem bestimmten Jahr gestartet sind. Hierzu dient (für das Jahr 2020) die Filterbedingung FILTER (STRSTARTS(str(?startdate), "2020")). Es werden die Budgets der Projekte zusammen mit den Informationen zu dem jeweiligen Drittmittelgeber abgerufen.

Anstelle von <http://vivoweb.org/ontology/core#Grant> kann eine lokale Drittmittelprojekt-Kategorie, falls vorhanden, verwendet werden. Anstelle von <https://local.ontology/local-vivo#budgetLocal> soll eine entsprechende Datatype Property aus der lokalen Ontologie eingesetzt werden.
```
Construct { 
?project a <http://vivoweb.org/ontology/core#Grant> .
 ?project <http://www.w3.org/2000/01/rdf-schema#label> ?label .
 ?project <https://local.ontology/local-vivo#budgetLocal> ?budget.
 ?project <http://vivoweb.org/ontology/core#assignedBy> ?funder .
  ?funder <http://vivoweb.org/ontology/core#assigns> ?project .
  ?funder a ?type.
  ?project <http://vivoweb.org/ontology/core#dateTimeInterval> ?dti .
  ?dti <http://vivoweb.org/ontology/core#end> ?end .
  ?dti  <http://vivoweb.org/ontology/core#start> ?start .
?start <http://vivoweb.org/ontology/core#dateTime> ?startdate .
?end <http://vivoweb.org/ontology/core#dateTime> ?enddate .
}

Where {
 ?project a <http://vivoweb.org/ontology/core#Grant> .
 ?project <http://www.w3.org/2000/01/rdf-schema#label> ?label .
 ?project <https://local.ontology/local-vivo#budgetLocal> ?budget.
 ?project <http://vivoweb.org/ontology/core#assignedBy> ?funder .
   ?funder <http://vivoweb.org/ontology/core#assigns> ?project .
  ?funder a ?type.
  ?project <http://vivoweb.org/ontology/core#dateTimeInterval> ?dti .
  ?dti <http://vivoweb.org/ontology/core#end> ?end .
  ?dti  <http://vivoweb.org/ontology/core#start> ?start .
?start <http://vivoweb.org/ontology/core#dateTime> ?startdate .
?end <http://vivoweb.org/ontology/core#dateTime> ?enddate .
 FILTER (STRSTARTS(str(?startdate), "2020"))
}
```
### Schritt 2: Select from Graph Distributor 
