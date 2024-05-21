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

_Step 1_: We want to find Michelangelo's IRI. To do so, we run a SPARQL query based on the artwork _Tondo Doni_ that we are sure was authored by Michelangelo.

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

- Result: <p>
<a href= "https://w3id.org/arco/resource/Agent/56d8ee32618291c12ae4f357db49c221">IRI Michelangelo</a>

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
