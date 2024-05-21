---
layout: default
---

**INTRODUCTION**

-**Context** 

ArCo: the knowledge Graph of the Italian Cultural Heritage
Large Language Models : Gemini and ChaptGPT

-**Team** 

Sara Speggiorin & Sara Paglia 

-**Topic** 

Michelangelo's artworks : an in-depth analysis of _Pietà (stampa)_ and _David-Apollo_ 

-**Purpose**

- Exploring the ArCo Ontology to find the two cultural properties. 
- Enriching Arco Knowledge Graph with futher details about the two cultural properties. 
- Retrieving useful information using LLMs.
  

**Index**

1.  Methodology
2.  Results and analysis.
3.  Discussion.
4.  Conclusions and possible future developments. 

**1. Methodology**

_Used tools_: HTML, GitHub, SPARQL, LLMs (Gemini and ChaptGPT), ArCo Ontology and Knowledge Graph

_External resources_: Google


- _Step 1_: We want to find Michelangelo's IRI. To do so, we run a SPARQL query based on the artwork _Tondo Doni_ that we are sure was authored by Michelangelo.

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>
PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>
PREFIX agent: <https://w3id.org/arco/resource/Agent/>

SELECT DISTINCT ?cp
WHERE {
?cp a arco:HistoricOrArtisticProperty ;
rdfs:label ?l .
FILTER(REGEX(?l, "tondo doni", "i"))
}
LIMIT 20

Result:
<a href= "https://w3id.org/arco/resource/Agent/56d8ee32618291c12ae4f357db49c221">IRI Michelangelo</a>


- _Step 2_: We are curious to know more about Michelangelo's artwork _La Pietà_. We therefore explore ArCo by using the following query:

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>
PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>
PREFIX agent: <https://w3id.org/arco/resource/Agent/>

SELECT DISTINCT ?culturalProperty
WHERE {
?culturalProperty a arco:HistoricOrArtisticProperty ;
a-cd:hasAuthor agent:56d8ee32618291c12ae4f357db49c221 ;
rdfs:label ?l .
FILTER(REGEX(?l, "pietà", "i"))
}

We employ the keyword DISTINCT to eliminate duplicates and by doing so we get 7 results. Among them, we selct the artwork _Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX)_, from which we retrieve the cultural property's IRI and the 2nd author's (Halm Peter Von) IRI.
Result:
<a href= "https://w3id.org/arco/resource/HistoricOrArtisticProperty/0100214952">IRI Pietà (stampa)</a>
<a href= "https://w3id.org/arco/resource/Agent/8603b17b6451202a8d27734812dae423">IRI Halm Peter Von</a>

- _Step 3_
  **-Prompting techniques:**
  We use the LLMs Gemini and ChatGPT in order to enrich the information regarding the location of the artwork _Pietà (stampa)_:
  -ChatGPT  
  ![Screenshot 2024-05-18 162547](https://github.com/capa46/project/assets/170355893/2dd708ce-1138-491c-bb66-7a9f1992b720)
  -Gemini
  ![Screenshot 2024-05-20 180602](https://github.com/capa46/project/assets/170355893/f8f97a16-d626-4d72-aa6a-bf3305ada753)

  We use the zero-shot prompting technique starting from the question _Could you please tell me the exact place in which the stamp 'Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX)' is kept?_.
  **-Results and analysis:**
  ChatGPT provides us with a wrong answer, while Gemini answers correctly:_The record specifies that the print "Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX)" is kept at the Istituto di Belle Arti (Institute of Fine Arts) located on Via Duomo, 17, Vercelli (VC), Italy [source:catalogo.beniculturali.it]_.


- _Step 4_
  **Prompting techniques:**
  We want to transform the new-found information into RDF format to build a triple and to do so we apply the Chain of Thought prompting technique, both in Gemini and ChatGPT.
  -Chain of Thought prompt:
The Doni Tondo or Doni Madonna is kept in the Uffizi in Florence, Italy.
RDF format: IRI Doni Tondo = https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900287181 
is kept in = a-loc:hasCulturalInstituteOrSite = https://w3id.org/arco/ontology/location/hasCulturalInstituteOrSite 
IRI Galleria degli Uffizi = https://w3id.org/arco/resource/CulturalInstituteOrSite/68ea75fb6946df92f9c6a6fa98a5d1f3 

HistoricOrArtisticProperty:0900287181 a-loc:hasCulturalInstituteOrSite CulturalInstituteOrSite:68ea75fb6946df92f9c6a6fa98a5d1f3

<h1 style="color:DodgerBlue;">Based on the previous example that I gave you, could you transform the following sentence “The record specifies that the print "Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX) is kept at the Istituto di Belle Arti (Institute of Fine Arts) located on Via Duomo, 17, Vercelli (VC), Italy” into RDF format using the ArCo ontology?</h1>
  



### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
