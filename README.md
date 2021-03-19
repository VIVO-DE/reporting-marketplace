# Reporting-Marketplace für VIVO

Queries und Reporting-Templates sind institutionsübergreifend oft ähnlich. In diesem Wiki dokumentieren Mitglieder der deutschen VIVO-Community zur Nachnutzung durch Dritte.


## Vitro Query Tool

Vitro Query Tool - ist ein auf [Data Distribution API](https://wiki.lyrasis.org/display/VIVODOC110x/Data+Distribution+API) basierter Tool, der einerseits die Benutzeroberfläche für die Konfiguration der Datenbfragen (mithilfe von den so genannten Data Distributoren) bietet und andererseits die Konfiguration und Erstellung von Berichten in Excel und Word ermöglicht. 
Data Distributoren, insbesondere die "Select from ..." Distributoren, liefern Daten für einzelne Berichte. 
Zwei Reporting-Engines strukturieren die Daten anhand der vordefinierten Vorlagen (Templates)  und wandeln diese in die Berichte um:
1. Konversion von JSON-Dateien in XML für ihre Weiterverarbeitung in Berichte auf Basis von OpenDOPE-Templates
2. Direkte Umwandlung von JSON-Dateien in Berichte anhand "einfacher" Word- und Excel-Templates.

### Distributoren

#### Select from Content Distributor
"Select from Content" Distributor beinhaltet eine SPARQL-Abfrage, die den gesamten Datenbestand von einer VIVO-Installation abfragt.

#### Select from Graph Distributor 
"Select from Graph" besteht aus einer SPARQL-Abfrage, die nur einen Subset von Daten abfragt, der mithilfe eines zuvor erstellten "Construct Graph Builder" abgegrenzt wird.

#### Construct Query Graph Builder Distributor
"Construct Query Graph Builder" beinhaltet eine SPARQL-Abfrage, die einen Subset von Daten aus dem Gesamtbestand eines VIVOs generiert. Ein "Select from Graph" Distributor kann im nächsten Schritt auf diesen Subset zugreifen, ohne den gesamten Datenbestand abzufragen und die Anwendung insgesamt zu beeinflussen.

## SPARQL

Die Abfragen in den "Select from" Distributoren werden in Abfragesprache [SPARQL](https://www.w3.org/TR/rdf-sparql-query/) geschrieben. SPARQL ist die für Datenabfrage im RDF (Resource Description Framework) entwickelt worden.
