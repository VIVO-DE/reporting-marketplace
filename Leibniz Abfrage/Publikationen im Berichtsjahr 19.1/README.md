# Leibniz-Abfrage 19.1 Publikationen:  Alle im Berichtsjahr von Beschäftigten Ihrer Einrichtung erfolgten Veröffentlichungen in gedruckter und/oder elektronischer Form, auch wenn sie schon unter 17.6 eingetragen wurden.


Berichtsanforderungen:

**19.1**
- Veröffentlichte Publikationen insgesamt
- davon Open Access Gold und Open Access Hybrid
- davon Open Access Green

**Wichtig: für diesen Report muss zusätzlich das erweiterte  [Open-Access-Modell_v0.2](https://github.com/VIVO-DE/reporting-marketplace/blob/main/Leibniz%20Abfrage/Publikationen%20im%20Berichtsjahr%2019.1/OA_Modell_v0_2.owl) ins VIVO importiert werden.** 
# Anleitung in 5 Schritten: Wie erstelle ich den Publikationsbericht mit Vitro Query Tool?

## Data Distributor

**Wichtig: Die Namen des Construct Query Graph Builders und der Data Distributor dürfen keine Leerzeichen enthalten!**

### Schritt 1: Construct Query Graph Builder für Publikationen aus dem Berichtsjahr erstellen

Die Abfrage bezieht sich auf die Publikationen eines bestimmten Jahres. Hierzu dient die Filterbedingung FILTER (STRSTARTS(str(?dateTime), "2022")). Es werden die URIs der Publikationen zusammen mit ihren Typ-Informationen und Informationen zum Open Access abgerufen. 

```

CONSTRUCT {   ?publication <http://www.w3.org/2000/01/rdf-schema#label> ?label .  
  ?publication a ?type .  
  ?publication <http://lod.tib.eu/onto/vivo-oa/has_access> ?access .  
  ?access a ?oaType .
 ?access <http://www.w3.org/2000/01/rdf-schema#label> ?oaLabel . }
WHERE {   ?publication a <http://purl.org/ontology/bibo/Document> .  
  ?publication a ?type .      
  OPTIONAL { ?publication <http://www.w3.org/2000/01/rdf-schema#label> ?label . }  
  OPTIONAL { ?publication <http://lod.tib.eu/onto/vivo-oa/has_access> ?access . 
  ?access a ?oaType .
 ?access <http://www.w3.org/2000/01/rdf-schema#label> ?oaLabel .}  
  ?publication <http://vivoweb.org/ontology/core#dateTimeValue>  ?dateTimeObj .   ?dateTimeObj  <http://vivoweb.org/ontology/core#dateTime> ?dateTime .  
  FILTER (STRSTARTS(str(?dateTime), "2024"))   
   }

```

### Schritt 2: Data Distributor für Open-Access-Gold-Publikationen erstellen

Wählen Sie bei der Erstellung die Option _Select from Graph Distributor_. 

Sie können die einzelnen Open Access Typen (in Abschnitten *COUNT(DISTINCT ?...)AS ?... )* und *UNION {...}*) zählen lassen. 

Jeder Open Access Typ (und die Summe) wird in einer UNION-Abfrage ausgewertet, so dass eine einzelne Zeile mit einer Variablenbindung (Spalte) für jede benötigte Summe zurückgegeben wird.

```
SELECT
    (COUNT(DISTINCT ?publication) AS ?total)
    (COUNT(DISTINCT ?publicationOAGH) AS ?hg)
    (COUNT(DISTINCT ?publicationOAGr) AS ?green)
WHERE
{
    {
        ?publication a ?type .
       
        
    }
    UNION
    {
         ?publicationOAGH a ?type .
        ?publicationOAGH <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Hybrid> .
    }
  UNION
    {
         ?publicationOAGH a ?type .
        ?publicationOAGH <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Gold> .
    }
   UNION
    {
         ?publicationOAGr a ?type .
       ?publicationOAGr <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Green> .
    }
}

### Schritt 5: Report erstellen

**Die Verwaltung von den Reports findet unter http://vivo.../.../admin/reporting statt.**

1. Wählen Sie die Option _Excel Template_ und klicken Sie auf _Add Report_.
2. In dem Menu _Excel Template_ benennen Sie den Report - z. B. _publications 2024_.
3. Wählen Sie den entsprechenden Distributor, weisen Sie einen Ausgangsnamen _Beispielname_ zu.
4. Laden Sie die [Excel-Vorlage](https://github.com/VIVO-DE/reporting-marketplace/blob/main/Leibniz%20Abfrage/Publikationen%20im%20Berichtsjahr%2019.1/publications_2024_template.xlsx) hoch.

Im Template dient jeweils die Variable aus der Abfrage plus .value - z. B. *${total.value}* als Pfad. Neben dem Namensbereich zum jeweiligen Wert und Output, wird in diesem Template auch der Namensbereich für die Kopfzeile definiert.

![Beispiel Publikationen Template](https://github.com/VIVO-DE/reporting-marketplace/blob/main/Leibniz%20Abfrage/Publikationen%20im%20Berichtsjahr%2019.1/Beispiel_Publikationen1_19_Template.png)