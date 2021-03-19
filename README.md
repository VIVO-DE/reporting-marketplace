# Reporting-Marketplace für VIVO

Queries und Reporting-Templates sind institutionsübergreifend oft ähnlich. In diesem Wiki dokumentieren Mitglieder der deutschen VIVO-Community zur Nachnutzung durch Dritte.


## Vitro Query Tool

Das Vitro Query Tool - ist ein auf der [Data Distribution API](https://wiki.lyrasis.org/display/VIVODOC110x/Data+Distribution+API) basierendes Werkzeug, das einerseits eine Benutzeroberfläche für die Konfiguration von Datenbfragen (mithilfe von so genannten *data distributors*) ermöglicht und andererseits die Konfiguration und Erstellung von Berichten in Excel und Word erlaubt. 
Data Distributors, insbesondere die "Select from ..." Distributoren, liefern Daten für einzelne Berichte. 
Zwei Reporting-Engines strukturieren die Daten anhand von vordefinierten Vorlagen (Templates) und wandeln diese in Berichte um:
1. Konversion von JSON-Dateien in XML für ihre Weiterverarbeitung in Berichte auf Basis von OpenDOPE-Templates
2. Direkte Umwandlung von JSON-Dateien in Berichte anhand "einfacher" Word- und Excel-Templates.

### Distributoren

#### Construct Query Graph Builder Distributor
Ein *"Construct Query Graph Builder"* beinhaltet eine SPARQL-Abfrage, die ein Subset der Daten eines VIVOs generiert. Ein *"Select from Graph"*-Distributor kann im nächsten Schritt auf diesen Subset zugreifen, ohne den gesamten Datenbestand abzufragen und die Anwendung insgesamt zu beeinflussen.

#### Select from Content Distributor
*"Select from Content"*  beinhaltet eine SPARQL-Abfrage, die den gesamten Datenbestand von einer VIVO-Installation abfragt.

#### Select from Graph Distributor 
*"Select from Graph"* besteht aus einer SPARQL-Abfrage, die nur ein Subset von Daten abfragt, das mithilfe eines zuvor erstellten *"Construct Graph Builder"* abgegrenzt wird.

## SPARQL

Die Abfragen in den *"Select from"* Distributoren werden in der Abfragesprache [SPARQL](https://www.w3.org/TR/rdf-sparql-query/) geschrieben. SPARQL ist für die Datenabfrage im RDF-Format (Resource Description Framework) entwickelt worden.
