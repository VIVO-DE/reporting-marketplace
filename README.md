# Reporting-Marketplace für VIVO

Queries und Reporting-Templates sind institutionsübergreifend oft ähnlich. In diesem Wiki dokumentieren Mitglieder der deutschen VIVO-Community Reporting-Templates zur Nachnutzung durch Dritte.

## Vitro Query Tool

:loudspeaker: Aktuell wird im TAPIR-Projekt an einem verbesserten Reporting-Tool gearbeitet. Reports für das neue Tool werden dann auch hier verfügbar gemacht.  

Das [*Vitro Query Tool*](https://doi.org/10.6084/m9.figshare.9897008.v1) - ist ein auf der [*Data Distribution API*](https://wiki.lyrasis.org/display/VIVODOC110x/Data+Distribution+API) basierendes Werkzeug, das einerseits eine Benutzeroberfläche für die Konfiguration von Datenbfragen (mithilfe von so genannten *data distributors*) ermöglicht und andererseits die Konfiguration und Erstellung von Berichten in Excel und Word erlaubt. Die Daten für einzelne Berichte werden von so genannten *Data Distributors* geliefert. 

Die Definition und Formatiering der Berichte erfolgt mittels zwei verschiedener Engines anhand vordefinierter Vorlagen (Templates):
1. [*OpenDOPE*](https://www.opendope.org/) - Konversion von JSON-Dateien in XML für ihre Weiterverarbeitung in Berichte auf Basis von OpenDOPE-Templates
2. Word- und Excel-Templates - direkte Umwandlung von JSON-Dateien in Berichte in Word- und Excel-Dokumente.

### Distributoren
Die [*Data Distribution API*](https://wiki.lyrasis.org/display/VIVODOC111x/Data+Distribution+API) wird genutzt, um Daten-Feeds aus VIVO zur Verfügung zu stellen. Die hier aufgeführten Selektoren liefern diese Daten in JSON aus.

#### Construct Query Graph Builder Distributor
Ein *"Construct Query Graph Builder"* beinhaltet eine SPARQL-Abfrage, die ein Subset der Daten eines VIVOs generiert. Ein *"Select from Graph"*-Distributor kann im nächsten Schritt auf diesen Subset zugreifen, ohne den gesamten Datenbestand abzufragen und die Anwendung insgesamt zu beeinflussen.

#### Select from Content Distributor
*"Select from Content"*  beinhaltet eine SPARQL-Abfrage, die den gesamten Datenbestand von einer VIVO-Installation abfragt.

#### Select from Graph Distributor 
*"Select from Graph"* besteht aus einer SPARQL-Abfrage, die nur ein Subset von Daten abfragt, das mithilfe eines zuvor erstellten *"Construct Graph Builder"* abgegrenzt wird.

## SPARQL

Die Abfragen in den *"Select from"* Distributoren werden in der Abfragesprache [SPARQL](https://www.w3.org/TR/rdf-sparql-query/) geschrieben. SPARQL ist für die Datenabfrage im RDF-Format (Resource Description Framework) entwickelt worden.
