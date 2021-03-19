# Bericht zu den Drittmitteln nach Mittelgebern
Die Abfrage erlaubt, Budget-Summen pro Jahr differenziert nach Drittmittelgebern zu erhalten.
## Data Distributors 
Um die Daten für den Bericht zu erhalten, wird zunächst ein Subset von Daten mittels eines *Construct Query Graph Builder* Distributor aus Gesamtbestand extrahiert. *Construct Query Graph Builder* Distributor enthält Bedingungen, den die Daten entsprechen sollen. Anschließend wird mittels des *Select from Graph Distributors* die Summe des jeweiligen Budgets aus dem zuvor gebildeten Subset abgefragt.
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
Für die Gesamtsumme der Drittmittel im Jahr und für jeden Drittmittelgeber wird jeweils ein separater *Select from Graph* Distributor angelegt.

#### Select from Graph Distributor für die Gesamtsumme "drittmittel_total"
Wählen Sie bei der Erstellung die Option *Select from Graph Distributor*.
```
Select 
(sum ( ?budget) as ?total)

Where {
  	
    ?project a <http://vivoweb.org/ontology/core#Grant> .
    ?project <https://local.ontology/local-vivo#budgetLocal> ?budget.
 	?project <http://vivoweb.org/ontology/core#assignedBy> ?funder .
 	 
 	
}
```
#### Select from Graph Distributor für die DFG-Summe "drittmittel_dfg"
Wählen Sie bei der Erstellung die Option *Select from Graph Distributor*. Ersetzen Sie <http://your.vivo/data/dfg_uri_here> durch die URI der Instanz für DFG aus Ihrem VIVO.

```
Select
(sum ( ?budget_dfg) as ?dfg)

Where 
{
?project_dfg a <http://vivoweb.org/ontology/core#Grant> .
?project_dfg <http://vivoweb.org/ontology/core#assignedBy> <http://your.vivo/data/dfg_uri_here> . # An dieser Stelle muss die URI der DFG-Instanz aus Ihrem VIVO eingesetzt werden.
?project_dfg <https://local.ontology/local-vivo#budgetLocal> ?budget_dfg.
}

```
#### Select from Graph Distributor für die DFG-Summe "drittmittel_bund"
Wählen Sie bei der Erstellung die Option *Select from Graph Distributor*.
```
Select 
(sum ( ?budget_bund) as ?bund)

Where 
{
  ?project_bund a <http://vivoweb.org/ontology/core#Grant> .
  ?project_bund <http://vivoweb.org/ontology/core#assignedBy> ?funder_bund . 
  ?funder_bund a <http://vivoweb.org/ontology/core#GovernmentAgency> .
  ?project_bund <https://local.ontology/local-vivo#budgetLocal> ?budget_bund.
}
```
#### Select from Graph Distributor für die DFG-Summe "drittmittel_eu"
Wählen Sie bei der Erstellung die Option *Select from Graph Distributor*. Ersetzen Sie <http://your.vivo/data/eu_uri_here> durch die URI der Instanz für EU aus Ihrem VIVO.

```
Select
(sum ( ?budget_eu) as ?eu)

Where 
{
	?project_eu a <http://vivoweb.org/ontology/core#Grant> .
 	?project_eu <http://vivoweb.org/ontology/core#assignedBy> <http://your.vivo/data/eu_uri_here> . # An dieser Stelle muss die URI der EU-Instanz aus Ihrem VIVO eingesetzt werden.
  	?project_eu <https://vivo.tib.eu/fis/ontology/tib-vivo#budgetTIB> ?budget_eu.
}
```