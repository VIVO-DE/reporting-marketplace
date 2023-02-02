# Leibniz-Abfrage 18.3.1:  Publikationen im Berichtsjahr (ohne Open-Access-Publikationen sowie ohne die unter Formular 17 erfassten Publikationen) und 18.3.2: Publikationen im Open Access im Berichtsjahr

Berichtsanforderungen:

**18.13.1**
- Veröffentlichte Publikationen insgesamt; 
differenziert nach:
- Zeitschriftenbeiträge
	- davon referierte Beiträge in Fachzeitschriften
- Monografien
- Working Papers / Reports
- Beiträge in Sammelbänden

**18.13.2**
- Veröffentlichte Publikationen insgesamt; 
differenziert nach:
- Zeitschriftenbeiträge
	- davon referierte Beiträge in Fachzeitschriften
- Monografien
- Working Papers / Reports
- Beiträge in Sammelbänden
- Differenzierung nach Unterarten von Open Access (Open Access Gold und Open Access Green)

# Anleitung in 4 Schritten: Wie erstelle ich den Publikationsbericht mit Vitro Query Tool?

## Data Distributor

**Wichtig: Die Namen des Construct Query Graph Builders und der Data Distributor dürfen keine Leerzeichen enthalten!**

### Schritt 1: Construct Query Graph Builder für Publikationen aus dem Berichtsjahr erstellen

Die Abfrage bezieht sich auf die Publikationen eines bestimmten Jahres. Hierzu dient die Filterbedingung FILTER (STRSTARTS(str(?dateTime), "2022")). Es werden die URIs der Publikationen zusammen mit ihren Typ-Informationen und Informationen zum Open Access abgerufen. 

```

CONSTRUCT {   ?publication <http://www.w3.org/2000/01/rdf-schema#label> ?label .  
  ?publication a ?type .  
  ?publication <http://lod.tib.eu/onto/vivo-oa/has_access> ?access .  
  ?access a ?oaType .
 ?access <http://www.w3.org/2000/01/rdf-schema#label> ?oaLabel .
  ?publication <http://purl.org/ontology/bibo/status> ?status . }
WHERE {   ?publication a <http://purl.org/ontology/bibo/Document> .  
  ?publication a ?type .      
  OPTIONAL { ?publication <http://www.w3.org/2000/01/rdf-schema#label> ?label . }  
  OPTIONAL { ?publication <http://lod.tib.eu/onto/vivo-oa/has_access> ?access . 
  ?access a ?oaType .
 ?access <http://www.w3.org/2000/01/rdf-schema#label> ?oaLabel .} 
  OPTIONAL { ?publication <http://purl.org/ontology/bibo/status> ?status . }   
  ?publication <http://vivoweb.org/ontology/core#dateTimeValue>  ?dateTimeObj .   ?dateTimeObj  <http://vivoweb.org/ontology/core#dateTime> ?dateTime .  
  FILTER (STRSTARTS(str(?dateTime), "2022"))   
   }

```

### Schritt 2: Data Distributor für Open-Access-Gold-Publikationen erstellen

Wählen Sie bei der Erstellung die Option _Select from Graph Distributor_. 

Sie können die einzelnen Publikationstypen (in Abschnitten *COUNT(DISTINCT ?...)AS ?... )* und *UNION {...}*) je nach Bedarf entfernen oder hinzufügen. 

Jeder Publikationstyp (und die Summe) wird in einer UNION-Abfrage ausgewertet, so dass eine einzelne Zeile mit einer Variablenbindung (Spalte) für jede benötigte Summe zurückgegeben wird.

```

SELECT
    (COUNT(DISTINCT ?publication) AS ?total)
    (COUNT(DISTINCT ?publicationAA) AS ?articles)
    (COUNT(DISTINCT ?publicationAAPR) AS ?peerreviewed)
    (COUNT(DISTINCT ?publicationBook) AS ?books)
    (COUNT(DISTINCT ?publicationReport) AS ?reports)
    (COUNT(DISTINCT ?publicationChapter) AS ?chapters)
    
    
WHERE
{
    {
        ?publication a ?type .
        ?publication <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Gold> .
        
    }
    UNION
    {
        ?publicationAA a <http://purl.org/ontology/bibo/AcademicArticle> .
        ?publicationAA <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Gold> .
    }
    UNION
    {
        ?publicationAAPR a <http://purl.org/ontology/bibo/AcademicArticle> .
        ?publicationAAPR <http://purl.org/ontology/bibo/status> <http://purl.org/ontology/bibo/peerReviewed> .
        ?publicationAAPR <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Gold> .
    }
    UNION
    {
        ?publicationBook a <http://purl.org/ontology/bibo/Book> .
        ?publicationBook <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Gold> .
        MINUS { ?publicationBook a <http://purl.org/ontology/bibo/Proceedings> . }
    }
  
    UNION
    {
        ?publicationReport a <http://purl.org/ontology/bibo/Report> .
        ?publicationReport <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Gold> .
    }
    UNION
    {
        ?publicationReport a <http://vivoweb.org/ontology/core#WorkingPaper> .
        ?publicationReport <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Gold> .
    }
    UNION
    {
        ?publicationReport a <http://kerndatensatz-forschung.de/owl/Basis#ArbeitspapierForschungsbericht> .
        ?publicationReport <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Gold> .
    }
    UNION
    {
        ?publicationChapter a <http://purl.org/ontology/bibo/Chapter> .
        ?publicationChapter <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Gold> .
    }
    
}

```
### Schritt 3: Data Distributor für Open-Access-Green-Publikationen erstellen

```
SELECT
    (COUNT(DISTINCT ?publication) AS ?total)
    (COUNT(DISTINCT ?publicationAA) AS ?articles)
    (COUNT(DISTINCT ?publicationAAPR) AS ?peerreviewed)
    (COUNT(DISTINCT ?publicationBook) AS ?books)
    (COUNT(DISTINCT ?publicationReport) AS ?reports)
    (COUNT(DISTINCT ?publicationChapter) AS ?chapters)
    
WHERE
{
    {
        ?publication a ?type .
        ?publication <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Green> .
        
    }
    UNION
    {
        ?publicationAA a <http://purl.org/ontology/bibo/AcademicArticle> .
        ?publicationAA <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Green> .
    }
    UNION
    {
        ?publicationAAPR a <http://purl.org/ontology/bibo/AcademicArticle> .
        ?publicationAAPR <http://purl.org/ontology/bibo/status> <http://purl.org/ontology/bibo/peerReviewed> .
        ?publicationAAPR <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Green> .
    }
    UNION
    {
        ?publicationBook a <http://purl.org/ontology/bibo/Book> .
        ?publicationBook <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Green> .
        MINUS { ?publicationBook a <http://purl.org/ontology/bibo/Proceedings> . }
    }
  
    UNION
    {
        ?publicationReport a <http://purl.org/ontology/bibo/Report> .
        ?publicationReport <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Green> .
    }
    UNION
    {
        ?publicationReport a <http://vivoweb.org/ontology/core#WorkingPaper> .
        ?publicationReport <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Green> .
    }
  
  UNION
    {
        ?publicationReport a <http://kerndatensatz-forschung.de/owl/Basis#ArbeitspapierForschungsbericht> .
        ?publicationReport <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Green> .
    }
 
    UNION
    {
        ?publicationChapter a <http://purl.org/ontology/bibo/Chapter> .
        ?publicationChapter <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Open_Access_Green> .
    }
    
}

```

### Schritt 4: Data Distributor für Nicht-Open-Access-Publikationen erstellen

Wählen Sie bei der Erstellung die Option _Select from Graph Distributor_. 

Sie können die einzelnen Publikationstypen (in Abschnitten *COUNT(DISTINCT ?...)AS ?... )* und *UNION {...}*) je nach Bedarf entfernen oder hinzufügen. 

Jeder Publikationstyp (und die Summe) wird in einer UNION-Abfrage ausgewertet, so dass eine einzelne Zeile mit einer Variablenbindung (Spalte) für jede benötigte Summe zurückgegeben wird.


```

SELECT
    (COUNT(DISTINCT ?publication) AS ?total)
    (COUNT(DISTINCT ?publicationAA) AS ?articles)
    (COUNT(DISTINCT ?publicationAAPR) AS ?peerreviewed)
    (COUNT(DISTINCT ?publicationBook) AS ?books)
    (COUNT(DISTINCT ?publicationReport) AS ?reports)
    (COUNT(DISTINCT ?publicationChapter) AS ?chapters)
    
WHERE
{
    {
        ?publication a ?type .
        ?publication <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Closed_Access> . 
    }
    UNION
    {
        ?publicationAA a <http://purl.org/ontology/bibo/AcademicArticle> .
         ?publicationAA <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Closed_Access> .
    }
    UNION
    {
        ?publicationAAPR a <http://purl.org/ontology/bibo/AcademicArticle> .
        ?publicationAAPR <http://purl.org/ontology/bibo/status> <http://purl.org/ontology/bibo/peerReviewed> .
         ?publicationAAPR <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Closed_Access> .
    }
    UNION
    {
        ?publicationBook a <http://purl.org/ontology/bibo/Book> .
         ?publicationBook <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Closed_Access> . 
        MINUS { ?publicationBook a <http://purl.org/ontology/bibo/Proceedings> . }
    }
    UNION
    {
        ?publicationReport a <http://purl.org/ontology/bibo/Report> .
        ?publicationReport <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Closed_Access> . 
    }
    UNION
    {
        ?publicationReport a <http://kerndatensatz-forschung.de/owl/Basis#ArbeitspapierForschungsbericht> .
         ?publicationReport <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Closed_Access> .
    }
    UNION
    {
        ?publicationReport a <http://vivoweb.org/ontology/core#WorkingPaper> .
        ?publicationReport <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Closed_Access> .
    }
    UNION
    {
        ?publicationChapter a <http://purl.org/ontology/bibo/Chapter> .
         ?publicationChapter <http://lod.tib.eu/onto/vivo-oa/has_access> <http://lod.tib.eu/onto/vivo-oa/Closed_Access> . 
    }
    
}

```

### Schritt 4: Report erstellen

**Die Verwaltung von den Reports findet unter http://vivo.../.../admin/reporting statt.**

1. Wählen Sie die Option _Excel Template_ und klicken Sie auf _Add Report_.
2. In dem Menu _Excel Template_ benennen Sie den Report - z. B. _publications 2022_.
3. Wählen Sie den Nicht-Open-Access-Distributor - _nonOpenaccess2022_, weisen Sie einen Ausgangsnamen _nonopenaccess_ zu.
4. Wählen Sie den Open-Access-Gold-Distributor - _openaccessGold2022_, vergeben Sie einen Ausgangsnamen von _openaccessGold_. 
5. Wählen Sie den Open-Access-Grenn-Distributor - _openaccessGreen2022_, vergeben Sie einen Ausgangsnamen von _openaccessGreen_.
6. Legen Sie die Reihenfolge der Ausgabe-Elemente, wie diese in Ihrem Bericht angezeigt werden sollen, mittels des Ranks fest.
7. Laden Sie die [Excel-Vorlage](https://github.com/VIVO-DE/reporting-marketplace/blob/Reporting_Marketplace_TW/Leibniz%20Abfrage/Publikationen%20im%20Berichtsjahr%20(Teil%2018.3.1%20und%2018.3.2)/publications%202022.xlsx) hoch.

Im Template dient jeweils die Variable aus der Abfrage plus .value - z. B. *${articles.value}* als Pfad. Neben dem Namensbereich zum jeweiligen Wert und Output, wird in diesem Template auch der Namensbereich für die Kopfzeile definiert.
Die Vorlage enthält zwei Tabellen. Markieren Sie für jede Tabelle alle Zellen (auch die Überschriften / Beschriftungen) und vergeben Sie die Namen _openaccessGold_ / _openaccessGreen_ / _nonopenaccess_.


Verwenden Sie in jeder Tabelle die Variablen aus der Abfrage plus _.value_ als Pfad zur Ausgabe der Werte. Z. B:

![Beispiel Publikationen Template](https://github.com/VIVO-DE/reporting-marketplace/blob/Reporting_Marketplace_TW/Leibniz%20Abfrage/Publikationen%20im%20Berichtsjahr%20(Teil%2018.3.1%20und%2018.3.2)/Beispiel_Publikationen_Template.png)

