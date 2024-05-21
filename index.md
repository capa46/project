---
layout: default
---
<h2 style="color: green;">INTRODUCTION</h2>

<h2 style="color: blue;">-Context</h2>

ArCo: the knowledge Graph of the Italian Cultural Heritage

Large Language Models : Gemini and ChaptGPT

<h2 style="color: blue;">-Team</h2>

Sara Speggiorin & Sara Paglia 

<h2 style="color: blue;">-Topic</h2> 

Michelangelo's artworks : an in-depth analysis of _Pietà (stampa)_ and _David-Apollo_ 

<h2 style="color: blue;">-Purpose</h2>

- Exploring the ArCo Ontology to find the two cultural properties. 
- Enriching Arco Knowledge Graph with futher details about the two cultural properties. 
- Retrieving useful information using LLMs.
  

**Index**

1.  [Methodology](#custom-anchor)
2.  [Results and analysis](#c-anchor)
3.  [Discussion](another-page.md)
4.  [Conclusions and possible future developments](#m-anchor)












<a name="custom-anchor"></a>
## **1. METHODOLOGY**

_Used tools_: HTML, GitHub, SPARQL, LLMs (Gemini and ChaptGPT), ArCo Ontology and Knowledge Graph

_External resources_: Google

<h1 style="background-color: yellow;">Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX)</h1>

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


- _Step 2_: We are curious to know more about Michelangelo's artwork _Pietà_. We therefore explore ArCo by using the following query:

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
  ![piet](https://github.com/capa46/project/assets/170355893/6a7d744e-c658-45ff-ae39-43b6a1974af2)

We use the zero-shot prompting technique starting from the question _Could you please tell me the exact place in which the stamp 'Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX)' is kept?_.

<a name="c-anchor"></a>

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

_Based on the previous example that I gave you, could you transform the following sentence “The record specifies that the print "Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX) is kept at the Istituto di Belle Arti (Institute of Fine Arts) located on Via Duomo, 17, Vercelli (VC), Italy” into RDF format using the ArCo ontology?_

**-Results and analysis:**
Gemini is not able to provide a proper answer. Thus, we give ChatGPT the same prompt and it provides a correct answer as it is able to create a triple, even though it uses wrong IRIs, that is Halm Peter Von's IRI and the Istituto di Belle Arti's IRI.
![Screenshot 2024-05-18 170720](https://github.com/capa46/project/assets/170355893/c7d03633-dcba-4dbc-a4d2-d84ae978b48c)
![Screenshot 2024-05-18 170804](https://github.com/capa46/project/assets/170355893/e26bba28-7f13-4353-a74d-fbd37e3c218c)


- _Step 5_

Since the triple's structure proposed by ChatGPT is correct, we want to use it and therefore need to substitute the wrong IRIs with the correct ones. That is why we opt for retrieving the IRI of the Istituto di Belle Arti of Vercelli in the ArCo ontology, assuming that it exists.
We focus our query on the cultural properties located in Vercelli and authored by both Michelangelo and Halm Peter Von by using the keyword UNION:

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#> 

PREFIX arco: <https://w3id.org/arco/ontology/arco/>  

PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/> 

PREFIX agent: <https://w3id.org/arco/resource/Agent/> 

PREFIX cis: <http://dati.beniculturali.it/cis/> 
 

SELECT DISTINCT ?place 

WHERE { 

?place a cis:GeographicalFeature 

{ ?place rdfs:label "Vercelli" } 

?culturalProperty a arco:HistoricOrArtisticProperty 

{ ?culturalProperty a-cd:hasAuthor agent:56d8ee32618291c12ae4f357db49c221 } 

UNION {?culturalProperty a-cd:hasAuthor agent:8603b17b6451202a8d27734812dae423} 

}

We get a single result, which corresponds to _a cis:GeographicalFeature_ and returns the Iri of Vercelli

<a href= "https://w3id.org/arco/resource/GeographicalFeature/39d1317c461740217063d916af2248bb">IRI of Vercelli</a>

##**-Results and analysis:**

Consequently, it can be said that there is no IRI for the Istituto di Belle Arti of Vercelli. For this reason, we would suggest to create a new IRI for this institute in order to have the possibility to use it for the following triple and enhance the knowledge graph:

_HistoricOrArtisticProperty:0100214952 a-loc:hasCulturalInstituteOrSite CulturalInstituteOrSite:XXX_

This triple links the Michelangelo and Halm Peter Von's artwork to its location, that is, the Instituto di Belle Arti of Vercelli, thanks to the property _a-loc:hasCulturalInstituteOrSite_.


**David-Apollo (statua) di Buonarroti Michelangelo (sec. XVI)**

- _Step 1_

We now want to find the artwork _David_ authored by Michelangelo Buonarroti using the following query:

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

PREFIX arco: <https://w3id.org/arco/ontology/arco/>

PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>

PREFIX agent: <https://w3id.org/arco/resource/Agent/>


SELECT DISTINCT ?culturalProperty

WHERE {

?culturalProperty a arco:HistoricOrArtisticProperty ;

a-cd:hasAuthor agent:56d8ee32618291c12ae4f357db49c221 ;

rdfs:label ?l .

FILTER(REGEX(?l, "david", "i"))

}

LIMIT 10

Among the results, we choose the _David-Apollo_ statue with <a href= "https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900286607">this IRI</a>.

After analysing the data, we focus our attention on the property _a-cd:hasCommission_ that refers to _Committenza 1 del bene 0900286607_ with <a href= "https://w3id.org/arco/resource/Commission/0900286607-1">this IRI</a>.

<a href= "https://capa46.github.io/project/another-page.html">Methodology</a>


 






  





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

<a name="m-anchor"></a>
4.  Conclusions and possible future developments
