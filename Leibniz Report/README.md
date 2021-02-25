# Lebniz-Abfrage Publikationen

Wie erstelle ich einen Publikationsbericht für die Lebniz-Abfrage mit Vitro Query Tool?

## Data Distributor

### Schritt 1: Contruct Query Graph Builder für Publikationen aus dem Berichtsjahr

Die Abfrage bezieht sich auf die Publikationen eines bestimmten Jahres. Hierzu dient die Filterbedingung FILTER (STRSTARTS(str(?dateTime), "2019")). Es werden die URIs der Publikationen zusammen mit ihren Typ-Informationen und Informationen zum Open Access abgerufen. 

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
### Step 2: Data Distributor für Open-Access-Publikationen

Wählen Sie bei die Option _Select from Graph Distributor_. 

Jeder Publikationstyp (und die Summe) wird in einer UNION-Abfrage ausgewertet, so dass eine einzelne Zeile mit einer Variablenbindung (Spalte) für jede benötigte Summe zurückgegeben wird.

```
SELECT
    (COUNT(DISTINCT ?publication) AS ?total)
    (COUNT(DISTINCT ?publicationAA) AS ?articles)
    (COUNT(DISTINCT ?publicationAAPR) AS ?peerreviewed)
    (COUNT(DISTINCT ?publicationBook) AS ?books)
    (COUNT(DISTINCT ?publicationReport) AS ?reports)
	(COUNT(DISTINCT ?publicationProposal) AS ?proposals)
    (COUNT(DISTINCT ?publicationChapter) AS ?chapters)
    (COUNT(DISTINCT ?publicationConfPaper) AS ?confpapers)
    (COUNT(DISTINCT ?publicationSlideshow) AS ?slideshows)
    (COUNT(DISTINCT ?publicationDataset) AS ?datasets)
    (COUNT(DISTINCT ?publicationConfPoster) AS ?confposters)
    (COUNT(DISTINCT ?publicationManual) AS ?manuals)
    (COUNT(DISTINCT ?publicationEA) AS ?editorials)
    (COUNT(DISTINCT ?publicationAbstract) AS ?abstracts)
    (COUNT(DISTINCT ?publicationThesis) AS ?theses)
    (COUNT(DISTINCT ?publicationVideo) AS ?videos)
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
  ## Added the kdsf:ArbeitspapierForschungsbericht as Report
  UNION
    {
        ?publicationReport a <http://kerndatensatz-forschung.de/owl/Basis#ArbeitspapierForschungsbericht> .
        ?publicationReport <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
  ## Added vivo:ResearchProposal
   UNION
    {
        ?publicationProposal a <http://vivoweb.org/ontology/core#ResearchProposal> .
         ?publicationProposal <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationChapter a <http://purl.org/ontology/bibo/Chapter> .
        ?publicationChapter <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationConfPaper a <http://vivoweb.org/ontology/core#ConferencePaper> .
        ?publicationConfPaper <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationSlideshow a <http://purl.org/ontology/bibo/Slideshow> .
        ?publicationSlideshow <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationDataset a <http://vivoweb.org/ontology/core#Dataset> .
        ?publicationDataset <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationConfPoster a <http://vivoweb.org/ontology/core#ConferencePoster> .
        ?publicationConfPoster <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationManual a <http://purl.org/ontology/bibo/Manual> .
        ?publicationManual <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationEA a <http://vivoweb.org/ontology/core#EditorialArticle> .
        ?publicationEA <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationAbstract a <http://vivoweb.org/ontology/core#Abstract> .
        ?publicationAbstract <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationThesis a <http://purl.org/ontology/bibo/Thesis> .
        ?publicationThesis <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
    UNION
    {
        ?publicationVideo a <http://vivoweb.org/ontology/core#Video> .
        ?publicationVideo <http://lod.tib.eu/onto/kdsf/hatZugriffsrechte> <http://purl.org/coar/access_right/c_abf2> .
    }
}
```
Wählen Sie anschließend den oben genannten Contruct Query Graph Builder. Die Abfrage wird dann nur gegen die Teilmenge der Daten ausgeführt, die sich auf die Publikationen für das gewählte Jahr beziehen.










