# Reporting-Marketplace für VIVO

Queries und Reporting-Templates sind institutionsübergreifend oft ähnlich. In diesem Wiki dokumentieren Mitglieder der deutschen VIVO-Community zur Nachnutzung durch Dritte.


## Vitro Query Tool

Vitro Query Tool - ist eine Script-Sammlung, die auf [Data Distribution API](https://wiki.lyrasis.org/display/VIVODOC110x/Data+Distribution+API) basiert.
VQT bietet Benutzeroberfläche für die Konfiguration der Data Distributoren, insbesondere der "Select from" Distributoren, die unter anderem JSON generieren und die Daten für einzelne Berichte liefern. Darüber hinaus verfügt das VQT über zwei Reporting-Engines, die die Daten anhand der vordefinierten Vorlagen (Templates) strukturieren und in die Berichte umwandeln:
1. Konversion von JSON-Dateien in XML für Weiterverarbeitung in Berichte, die auf OpenDOPE-Templates basieren ;
2. Direkte Weiterverarbeitung von JSON-Dateien in Berichte anhand "einfacher" Word- und Excel-Templates.


## SPARQL

Die Abfragen in den "Select from" Distributoren werden in Abfragesprache [SPARQL](https://www.w3.org/TR/rdf-sparql-query/) geschrieben. SPARQL ist als Abfragesprache die für Daten im RDF (Resource Description Framework) entwickelt worden.
