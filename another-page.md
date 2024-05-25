
<div class="topnav">
  <a class="active" href="https://capa46.github.io/project/">Home</a>
  <a class="active" href="https://capa46.github.io/project/page2.html">Final step</a>
</div>


<h3 style="color:green ;">INDEX</h3>

1. [Methodology](#mm-anchor)
   
2. [Cultural Property _Pietà (stampa)_, Michelangelo Buonarroti & Halm Peter Von (sec. XIX)](#custom-anchor)
   
3. [Cultural Property _David-Apollo_, Michelangelo Buonarroti (sec. XVI)](#c-anchor)
   <div style="margin-top: 80px;"></div> 


<a name="mm-anchor"></a>
<h4 style="color:green ;">1. Methodology</h4>


- Making hypotheses

- Looking for the data in the ArCo ontologies documentation: http://wit.istc.cnr.it/arco/  

- Realizing queries using the ArCo SPARQL endpoint: https://dati.cultura.gov.it/sparql  

- Verifying if the retrieved data could actually back up our hypotheses or not 

- Using LLMs 

- Creating RDF triples that could be addeded to the knowledge graph





<a name="custom-anchor"></a>
<h4 style="color:green ;">2. Cultural Property "Pietà (stampa)", Michelangelo Buonarroti & Halm Peter Von (sec. XIX)</h4>

<div style= "text-align: center;">
<img src="https://github.com/capa46/project/assets/170109035/e2683111-a558-49de-8821-497a859a3710" width="200" height="250">
</div>



- _Step 1_: We want to find Michelangelo's IRI. To do so, we run a SPARQL query based on the artwork _Tondo Doni_ that we are sure was authored by Michelangelo.


QUERY 1

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


QUERY 2 

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



We employ the keyword **DISTINCT** to eliminate duplicates and by doing so we get 7 results. Among them, we select the artwork _Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX)_, from which we retrieve the cultural property IRI and the 2nd author's (Halm Peter Von) IRI.


Results:

<a href= "https://w3id.org/arco/resource/HistoricOrArtisticProperty/0100214952">IRI Pietà (stampa)</a>

<a href= "https://w3id.org/arco/resource/Agent/8603b17b6451202a8d27734812dae423">IRI Halm Peter Von</a>


- _Step 3_


**Prompting techniques:**
We use the LLMs Gemini and ChatGPT in order to enrich the information regarding the location of the artwork _Pietà (stampa)_:

-ChatGPT  

  <img src="https://github.com/capa46/project/assets/170355893/2dd708ce-1138-491c-bb66-7a9f1992b720" width="800" height="400">

-Gemini

  <img src="https://github.com/capa46/project/assets/170109035/4c217fb1-e381-426e-b0ac-44e87eac773e" width="800" height="400">



We use the **zero-shot prompting technique** starting from the question _Could you please tell me the exact place in which the stamp 'Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX)' is kept?_.



**Results and analysis:**
ChatGPT provides us with a wrong answer, while Gemini answers correctly: _The record specifies that the print "Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX)" is kept at the Istituto di Belle Arti (Institute of Fine Arts) located on Via Duomo, 17, Vercelli (VC), Italy [source:catalogo.beniculturali.it]_.


- _Step 4_

**Prompting techniques:**
We want to transform the new-found information into RDF format to build a triple and to do so we apply the **chain of thought prompting technique**, both in Gemini and ChatGPT.

Chain of thought prompt:

_The Doni Tondo or Doni Madonna is kept in the Uffizi in Florence, Italy._

_RDF format: IRI Doni Tondo = https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900287181_ 

_is kept in = a-loc:hasCulturalInstituteOrSite = https://w3id.org/arco/ontology/location/hasCulturalInstituteOrSite_ 

_IRI Galleria degli Uffizi = https://w3id.org/arco/resource/CulturalInstituteOrSite/68ea75fb6946df92f9c6a6fa98a5d1f3_ 

_HistoricOrArtisticProperty:0900287181 a-loc:hasCulturalInstituteOrSite CulturalInstituteOrSite:68ea75fb6946df92f9c6a6fa98a5d1f3_

_Based on the previous example that I gave you, could you transform the following sentence “The record specifies that the print "Pietà, Pietà (stampa) di Buonarroti Michelangelo, Halm Peter Von (sec. XIX) is kept at the Istituto di Belle Arti (Institute of Fine Arts) located on Via Duomo, 17, Vercelli (VC), Italy” into RDF format using the ArCo ontology?_


**Results and analysis:**

Gemini is not able to provide a proper answer. Thus, we give ChatGPT the same prompt. 
ChatGPT is able to provide a correct answer as it creates a triple. Nonetheless, it uses wrong IRIs - that is, the IRI of the stamp and the one of the Istituto di Belle Arti.

<img src="https://github.com/capa46/project/assets/170355893/c7d03633-dcba-4dbc-a4d2-d84ae978b48c" width="1000" height="500">

<img src="https://github.com/capa46/project/assets/170109035/08d009b4-3c3b-436d-b201-f8e834ee9ff6" width="900" height="200"> 



- _Step 5_

Since the triple structure proposed by ChatGPT is correct, we want to use it. We need to substitute the wrong IRIs with the correct ones. That is why we opt for retrieving the IRI of the Istituto di Belle Arti of Vercelli in the ArCo ontology, assuming that it exists.
We focus our query on the cultural properties located in Vercelli and authored by both Michelangelo and Halm Peter Von by using the keyword **UNION**:


QUERY 3

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



We get a single result, which corresponds to _a cis:GeographicalFeature_ and returns the <a href= "https://w3id.org/arco/resource/GeographicalFeature/39d1317c461740217063d916af2248bb">IRI of Vercelli</a> .


**Results and analysis:**

Consequently, it can be said that there is no IRI for the Istituto di Belle Arti of Vercelli. For this reason, we would suggest to create a new IRI for this Institute in order to use it for the following triple and thus enhance the knowledge graph:

_HistoricOrArtisticProperty:0100214952 a-loc:hasCulturalInstituteOrSite CulturalInstituteOrSite:XXX_

This RDF triple would link Michelangelo and Halm Peter Von's artwork to its location - that is, the Instituto di Belle Arti of Vercelli thanks to the property _a-loc:hasCulturalInstituteOrSite_.





<div style="margin-top: 80px;"></div> 



<a name="c-anchor"></a>
<h4 style="color:green ;">3. Cultural Property "David-Apollo", Michelangelo Buonarroti (sec. XVI)</h4>

<div style= "text-align: center;">
<img src="https://github.com/capa46/project/assets/170109035/9e13c0ed-731d-459a-b603-5eba791f84be" width="200" height="400">
</div>


- _Step 1_

We now want to find the artwork _David_ authored by Michelangelo Buonarroti using the following query:



QUERY 4

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


Since the information is not clear enough, we carry out a research on the committent of this statue. In a brochure of the <a href= "http://www.polomuseale.firenze.it/areastampa/files/53185184f1c3bc7c07000000/02%20SALA%20MICHE_PDF.pdf">Museo Nazionale del Bargello</a>, we discover that the committent was Baccio Valori. 

- _Step 2_

We now decide to ask ChatGPT and Gemini to retrieve further information on the committent by using the **self-consistency prompting technique**. We ask the same question three times, but formulating it with different words and opening each time a new chat: 

_Q: Who commissioned the artwork Tondo Doni authored by Michelangelo?
A: The artwork Tondo Doni was commissioned by a rich merchant named Agnolo Doni._

_Q1: Why was the statue David-Apollo by Michelangelo sculptured?_

ChatGPT answer 1
<img src="https://github.com/capa46/project/assets/170109035/93b1246f-3268-48e1-a8b3-84089d36b0cc" width="800" height="400">

Gemini answer 1

<img src="https://github.com/capa46/project/assets/170109035/c149da8f-082d-4773-8e81-419711a6e053" width="800" height="400"> 

The answer provided by Gemini is wrong and murky. On the other hand, the one given by ChatGPT is correct.
Thus, we decide to phrase the following questions (2,3) in a more explicit way, but using different words and chats. 

_Q: Who commissioned the artwork Tondo Doni authored by Michelangelo?
A: The artwork Tondo Doni was commissioned by a rich merchant named Agnolo Doni._

_Q2: Who commissioned the statue David-Apollo authored by Michelangelo?_

_Q3: Which organization or person hired Michelangelo to sculpt the statue of David-Apollo? Be detailed._


ChatGPT - Answer 2 
<img src="https://github.com/capa46/project/assets/170109035/0ebb7e19-167e-454a-9a58-fb56ec2539dd" width="800" height="400"> 


ChatGPT - Answer 3
<img src="https://github.com/capa46/project/assets/170109035/c3229abe-57f9-4387-964b-1916ea110e57" width="800" height="400">



Gemini - Answer 2

<img src="https://github.com/capa46/project/assets/170109035/f5dc08d6-b0ee-49ce-9d17-acc689b4b4ac" width="800" height="400">


Gemini - Answer 3 

<img src="https://github.com/capa46/project/assets/170109035/93ac665b-f562-4711-87c6-506484e2431c" width="800" height="400">

We observe that Gemini replies correctly to the two last questions phrased in a more specific way but with different words, which demonstrates its self-consistency. On the other hand, ChatGPT is not coherent because it provides the wrong answer to the third question. Overall, we can highlight the fact that both LLMs outputs are not always consistent. 

- _Step 3_

We are now interested in finding out in which events the cultural property _David-Apollo_ was involved. To do so, we use the following query:



QUERY 5 

PREFIX l0: <https://w3id.org/italia/onto/l0/>

PREFIX cis: <http://dati.beniculturali.it/cis/>

PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

PREFIX arco: <https://w3id.org/arco/ontology/arco/>

PREFIX agent: <https://w3id.org/arco/resource/Agent/>


SELECT DISTINCT ?event ?eventName ?culturalProperty 

WHERE{

  ?event cis:involvesCulturalEntity ?culturalProperty ;
  
  l0:name ?eventName .
  
  ?culturalProperty a-cd:hasAuthor agent:56d8ee32618291c12ae4f357db49c221 ;
  
   a arco:HistoricOrArtisticProperty ;
   
   rdfs:label ?l .
                             
FILTER(REGEX(?l, "david-apollo", "i"))

}

ORDER BY DESC(?eventName)





![10](https://github.com/capa46/project/assets/170109035/df5f50a6-528b-4635-aff7-ec45e2fb6e18)

As you can see from the picture, we get multiple results: 3 of them with a certain IRI and 5 of them with another one. This means that there are only two events in which our cultural property is involved. It is likely that the names of the events are spelled differently (e.g. capital letters or different punctuation), leading the knowledge graph to assume that they are different entities.

In order to enrich and improve the structure of the knowledge graph, we had considered using the property _owl:sameAs_ to connect the events reporting the same name. However, even though the names of the events are spelled differently, their IRIs are actually the same. Therefore, the property _owl:sameAs_ cannot be applied and the most optimal solution is to correct all discrepancies in the event names, so as to eliminate and avoid duplicates.


- _Step 4_

We want to know how the event _L'ombra del genio. Michelangelo e l'arte a Firenze 1537-1631_ was reviewed. We apply the **few-shot prompting technique** to the LLMs ChatGPT and Gemini as to find out whether it was successful or not: 


_Based on the following examples, tell me if the review about the event "L'ombra del genio. Michelangelo e l'arte a Firenze 1537-1631" reported in the last paragraph is positive or negative. Q: The last novel by J.K. Rowling was really interesting because all the characters were complex and well-developed. // Positive_

_The movie Allegiant was delusional because the plot was completely different from what was in the book. // Negative_

_This major international exhibition provides a detailed survey of the art of Florence during the years that Michelangelo and artists employed by the first Medici grand dukes—such as Cellini, Bronzino, and Pontormo—produced celebrated masterpieces. Works by these artists bolstered the wordly power and status of the ruling family and reflected the sophistication and refinement of Florentine court circles, in which high value was placed upon classical aesthetics and scientific learning. Many of the 200 objects in the exhibition—including sculpture, painting, armor, medals, porcelain, and tapestries—have never before traveled out of Florence. Among the objects on view for the first time in the United States are three sculptures by Michelangelo, some of the greatest paintings of the Uffizi and Pitti Galleries, and works from the famed Studiolo (treasure room) of Francesco I in the Palazzo Vecchio._
_The Medici, Michelangelo, and the Art of Late Renaissance Florence is the first major exhibition in North America devoted to this moment of great achievement and artistic innovation, one of the richest periods in the history of art. Before coming to the Art Institute, the exhibition opened at the Strozzi Palace in Florence. After leaving Chicago it will be seen at the Detroit Institute of Arts.//_ 

_A:_

ChatGPT 

<img src="https://github.com/capa46/project/assets/170109035/17c8c9b8-c23b-4075-b8fa-9362baa98c1f" width="900" height="200">

Gemini

![YY](https://github.com/capa46/project/assets/170109035/c216c698-902c-4d59-bf39-bf85e52dc0c1)

Both LLMs reply correctly to our prompting and understand that the event was successful. 

- _Step 5_

As final step of our research, we now focus on the subject of the statue.
The description of the artwork explains that the subject is inspired by both the biblical character David and the Greek god Apollo.  
At the property _a-cd:hasSubject_, we notice that the two figures are indicated as a single entity. However, we want to enrich the data by specifying that the two figures could also be two separate subjects. In this way, the statue could be one of the outputs of a query; otherwise, it would be discarded because the two figures are not labelled individually.
We therefore use the following query to find the IRI of the entity David (then doing the same for Apollo). Assuming that we will get many results, we use the keyword **ORDER BY ASC** to go through the different results more easily.


QUERY 6 

PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>

PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?sub ?slabel

WHERE

{

?cp a-cd:hasSubject ?sub .

?sub a a-cd:Subject ;

rdfs:label ?slabel .

FILTER(REGEX(?slabel,"david", "i"))

}

ORDER BY ASC (?sub)

Result: <a href= "https://w3id.org/arco/resource/Lombardia/Subject/172522ec1028ab781d9dfd17eaca4427">IRI of David</a>


<div style="margin-top: 40px;"></div> 



QUERY 7

PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>

PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?sub ?slabel


WHERE

{

?cp a-cd:hasSubject ?sub .

?sub a a-cd:Subject ;

rdfs:label ?slabel .

FILTER(REGEX(?slabel,"apollo", "i"))

}

ORDER BY ASC (?sub)

Result: <a href= "https://w3id.org/arco/resource/Lombardia/Subject/31f2385ba9cc65dba7ccb9aa5c5b7600">IRI of Apollo</a>


Now that we have the two single IRIs, we are able to create triples that allow us to link the statue _David-Apollo_ to the two single subjects: 

RDF triples: 

https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900286607  a-cd:hasSubject 

https://w3id.org/arco/resource/Lombardia/Subject/172522ec1028ab781d9dfd17eaca4427

<div style="margin-top: 40px;"></div>  
https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900286607  a-cd:hasSubject 

https://w3id.org/arco/resource/Lombardia/Subject/31f2385ba9cc65dba7ccb9aa5c5b7600



- _Step 6_

 Are there other artworks with the same problem? To find it out, we use the keyword **OPTIONAL** 
 that enables to retrieve cultural properties with the subject _David_ and eventually _Apollo_. 


QUERY 8

PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>

PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?sub ?label

WHERE

{

?cp a-cd:hasSubject ?sub .

?sub a a-cd:Subject ;

rdfs:label ?label .

FILTER(REGEX(?label,”david”, “i”))

OPTIONAL { ?culturalProperty a-cd:subject “apollo”, “i”} 

}

LIMIT 100



The answer is yes: we find the artwork _Apollo o David Con La Viola da Braccio_ (<a href= "https://w3id.org/arco/resource/Subject/6f0942a99237aefe98047f9d200c4b45">IRI</a>) that has the same problem. Thus, we create other RDF triples to disambiguate and make explicit the existence of the two figures as subjects. 

RDF triples: 

https://w3id.org/arco/resource/Subject/6f0942a99237aefe98047f9d200c4b45 a-cd:hasSubject 

https://w3id.org/arco/resource/Lombardia/Subject/172522ec1028ab781d9dfd17eaca4427

<div style="margin-top: 40px;"></div>
https://w3id.org/arco/resource/Subject/6f0942a99237aefe98047f9d200c4b45 a-cd:hasSubject 

https://w3id.org/arco/resource/Lombardia/Subject/31f2385ba9cc65dba7ccb9aa5c5b7600




  <div style="margin-top: 40px;"></div> 
<div style="text-align: right;">
  <a href="https://capa46.github.io/project/page2.html">Final step</a>
</div>





