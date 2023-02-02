# Leibniz-Abfrage 9.1.2: Publikationen nach Berichtsjahr

Berichtsanforderungen:
- Veröffentlichte Publikationen insgesamt; 
differenziert nach:
- Zeitschriftenbeiträge
	- davon referierte Beiträge in Fachzeitschriften
- Monografien
- Working Papers / Reports
- Beiträge in Sammelbänden
- Differenzierung nach Unterarten von Open Access (Open Access G


# Anleitung in 4 Schritten: Wie erstelle ich den Publikationsbericht mit Vitro Query Tool?


## Data Distributor

**Wichtig: Die Namen des Construct Query Graph Builders und der Data Distributor dürfen keine Leerzeichen enthalten!**

### Schritt 1: Construct Query Graph Builder für Publikationen aus dem Berichtsjahr erstellen

Die Abfrage bezieht sich auf die Publikationen eines bestimmten Jahres. Hierzu dient die Filterbedingung FILTER (STRSTARTS(str(?dateTime), "2020")). Es werden die URIs der Publikationen zusammen mit ihren Typ-Informationen und Informationen zum Open Access abgerufen. 

```
CONSTRUCT {
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
}
```
### Schritt 2: Data Distributor für Open-Access-Publikationen erstellen

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
    (COUNT(DISTINCT ?publicationDataset) AS ?datasets)
    
WHERE
{
    {
        ?publication a ?type .
        ?publication <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationAA a <http://purl.org/ontology/bibo/AcademicArticle> .
        ?publicationAA <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationAAPR a <http://purl.org/ontology/bibo/AcademicArticle> .
        ?publicationAAPR <http://purl.org/ontology/bibo/status> <http://purl.org/ontology/bibo/peerReviewed> .
        ?publicationAAPR <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationBook a <http://purl.org/ontology/bibo/Book> .
        ?publicationBook <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
        MINUS { ?publicationBook a <http://purl.org/ontology/bibo/Proceedings> . }
    }
    UNION
    {
        ?publicationReport a <http://purl.org/ontology/bibo/Report> .
        ?publicationReport <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationReport a <http://vivoweb.org/ontology/core#WorkingPaper> .
        ?publicationReport <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
   UNION
    {
        ?publicationReport a <http://kerndatensatz-forschung.de/owl/Basis#ArbeitspapierForschungsbericht> .
        ?publicationReport <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
   
    UNION
    {
        ?publicationChapter a <http://purl.org/ontology/bibo/Chapter> .
        ?publicationChapter <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationDataset a <http://vivoweb.org/ontology/core#Dataset> .
        ?publicationDataset <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
}
```
Wählen Sie anschließend den oben genannten Contruct Query Graph Builder. Die Abfrage wird dann nur gegen die Teilmenge der Daten ausgeführt, die sich auf die Publikationen für das gewählte Jahr beziehen.


### Schritt 3: Data Distributor für Nicht-Open-Access-Publikationen erstellen

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
    (COUNT(DISTINCT ?publicationDataset) AS ?datasets)
    
WHERE
{
    {
        ?publication a ?type .
        MINUS { ?publication <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> . }
    }
    UNION
    {
        ?publicationAA a <http://purl.org/ontology/bibo/AcademicArticle> .
        MINUS { ?publicationAA <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> . }
    }
    UNION
    {
        ?publicationAAPR a <http://purl.org/ontology/bibo/AcademicArticle> .
        ?publicationAAPR <http://purl.org/ontology/bibo/status> <http://purl.org/ontology/bibo/peerReviewed> .
        MINUS { ?publicationAAPR <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> . }
    }
    UNION
    {
        ?publicationBook a <http://purl.org/ontology/bibo/Book> .
        MINUS { ?publicationBook <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> . }
        MINUS { ?publicationBook a <http://purl.org/ontology/bibo/Proceedings> . }
    }
    UNION
    {
        ?publicationReport a <http://purl.org/ontology/bibo/Report> .
        MINUS { ?publicationReport <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> . }
    }
   UNION
    {
        ?publicationReport a <http://kerndatensatz-forschung.de/owl/Basis#ArbeitspapierForschungsbericht> .
        MINUS { ?publicationReport <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .}
    }
   UNION
    {
        ?publicationReport a <http://vivoweb.org/ontology/core#WorkingPaper> .
        MINUS { ?publicationReport <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .}
    }
    UNION
    {
        ?publicationChapter a <http://purl.org/ontology/bibo/Chapter> .
        MINUS { ?publicationChapter <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> . }
    }
    UNION
    {
        ?publicationDataset a <http://vivoweb.org/ontology/core#Dataset> .
        MINUS { ?publicationDataset <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> . }
    }
    
}
```

### Schritt 4: Report erstellen

**Die Verwaltung von den Reports findet unter http://vivo.../.../admin/reporting statt.**

1. Wählen Sie die Option _Excel Template_ und klicken Sie auf _Add Report_.
2. In dem Menu _Excel Template_ benennen Sie den Report - z. B. _publications 2020_.
3. Wählen Sie den Open-Access-Distributor - _openaccess2020_, vergeben Sie einen Ausgangsnamen von _openaccess_. 
4. Wählen Sie den Nicht-Open-Access-Distributor - _nonopenaccess2020_, weisen Sie einen Ausgangsnamen _nonopenaccess_ zu.
5. Legen Sie die Reihenfolge der Ausgabe-Elemente, wie diese in Ihrem Bericht angezeigt werden sollen, mittels des Ranks fest.
6. Laden Sie die Excel-Vorlage hoch.

Die Vorlage muss zwei Tabellen enthalten. Markieren Sie für jede Tabelle alle Zellen (auch die Überschriften / Beschriftungen) und vergeben Sie die Namen _openaccess_ / _nonopenaccess_.

Verwenden Sie in jeder Tabelle die Variablen aus der Abfrage plus _.value_ als Pfad zur Ausgabe der Werte. Z. B:


![Beispiel Excel Template](https://github.com/VIVO-DE/reporting-marketplace/blob/main/Leibniz%20Abfrage/Publikationen/Beispiel%20Excel%20Template.PNG) 




