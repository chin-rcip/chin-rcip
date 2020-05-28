---
layout: page
title: Appendix A - Data Provenance
permalink: /target-model/current/appendix-a-data-provenance
---
[Back to the Table of Contents](/target-model/current/information#table-of-contents)

## Appendix A: Data Provenance

As stated in the 

<p id="gdcalert65" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: undefined internal link (link text: "Provenance of the Dataset"). Did you generate a TOC? </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert66">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

[Provenance of the Dataset](#heading=h.n7myzw5tcswt) section, one way of establishing data provenance would be to use the `E13 Attribute Assignment` event pattern described below and assign it to each triple so that their provenance can be documented. However, this would make the model in general much heavier as each triple would be assigned this pattern. From a processing standpoint this would become increasingly burdensome as the amount of data grows, which is why the Named Graph approach described in the Provenance of the Dataset section has been adopted. 

The `E13 Attribute Assignment` class represents the action of making assertions about the properties or relations between two items or concepts, as stated in the [CIDOC CRM Scope note](http://www.cidoc-crm.org/Entity/e13-attribute-assignment/version-6.2.2).

This `E13 Attribute Assignment` event can link together an `E1 CRM Entity` to another `E1 CRM Entity`, which correspond to said items or concepts. The performer of this `E13 Attribute Assignment`—in other words the actor who stated this relationship between the two `E1 CRM Entity`—is the museum providing the data. As the  `E13 Attribute Assignment` is an event, it can be dated though an `E52 Time-Span`[^1], thus providing a way to represent both the provider of the data and the moment of provision. 

In early 2019, the [CIDOC CRM team](http://www.cidoc-crm.org/Issue/ID-367-e13-attribute-assignment) discussed the fact that the `E13 Attribute Assignment` needs a property to denote which kind of property the attribute assignment is about. Their conclusion was to create a yet to be implemented new property `PXXX assigned property type` with a sub property of `P2 has type`. When it is made available, it would have been preferable to use `PXXX assigned property type.` Until then, the use of `P2 has type` in conjunction with `E55 Type` to specify the property the `E13 attribute assignment` is about would have been best. This pattern would be used for each triple, so that its provenance is documented: 



<p id="gdcalert66" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/TM-Documentation-2-138.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert67">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/TM-Documentation-2-138.png "image_tooltip")



```
💡  Example:
In 2018, WikiData assessed with certainty that the Birth of Emily Carr took place in Victoria, BC.

```


If multiple and contradictory statements are made about a resource, various and distinct `E13 Attribute Assignment` would be created and linked to their respective statement. For example, if Museum A attributes the date 1920 to the birth of an artist, and museum B attributes 1922, then we should have two `E13 Attribute Assignment` (one for each Museum) linked to the `E67 Birth`  event and linked to two different `E52 Time-Span`, 1920 and 1922, as shown in the example below:


```
💡  Example:
In this example, the actor "Bob" was born in 1920 for Museum A and in 1922 for Museum B.

```



```
💬 Discussion:
George Bruseker (17:40 16 Jul): modeling wise this is very nice and correct. From integration management perspective though, I don't think it will scale. You cannot know in advance what facts the partners will not agree on. On the other hand you cannot make every attribute the function of an attribute assignment (your graph will be painfully bloated). It seems the solution is to have the whole imported dataset in a named graph so it I find two facts disagreeing about the birthdate in the integrated knowledge based, I can follow the property back to the dataset it belongs to and form there understand who uttered/documented/believed fact x.
Philippe Michon (09.56 17 Jul):We already suspected that this pattern was complicated as we were repeating the same data on each triple. That said, I still have a bunch of questions:
Should we remove completely the e13 pattern in favor of Named Graphs?
Should/Could we still use e13 to express the provenance of our Named Graphs?
If not, which ontology should we use to monitor the provenance?
 How the reconciliation process is going to work if we use Named Graphs. I guess we will have to compare all the graphs but I don't clearly vizualise the process?
What will happen if, on our UI, someone confirm that this date is true? This will generate a new graph for this user with just this date statement?
As I'm not familiar with Named Graphs, all of them will be placed in our future triple store and we could query them all together and just specific ones if needed?
Geroge Bruseker (07:31 22 Jul): It's a long set of questions for a side comment, but I will add my ideas as coherently as possible. 
In the scenario where you are loading a whole dataset, the whole dataset should be in a single named graph that would have the provenance information attached. E13 can be used within a graph to indicate 'someone said something of something'. It can also in principle be used as the model to document, as a single instance, the provenance of your whole named graph. Ie. You make on named graph statement about who is responsible for the dataset and how and you attach it to the named graph that is the statement (ie the whole dataset contributed).
It seems one good model to follow. The other plan might be to use CRMdig which is for digital provenance. If what you want to trace is rather the steps and methods of contribution. Warning there though, that it would probably need to be developed some to support this (does not already have events like 'aggregation').
Prov-o is a major competitor and is a possibility.
It kind of depends on when in the process you do reconciliation... longer conversation to my mind.
Depends on the scenario, but as researchspace implements, for example, I believe this is the strategy. You would not want to mix up the institutional belief (what is delivered by the contributing museum) with the individual user belief.
It becomes an additional attribute that you can add to the sparql query, so you can bring back data from one or more namedgraphs depending on what you specify.
Philippe Michon (09:36 22 Jul): +george.bruseker@gmail.com I agree that we should take some time to review this topic at some point.
1-2: We should probably keep the pattern in the model but describe it very clearly. Especially if we use it for the provenance of the whole graph and for specific statements.
I will let Stephen look at CRMdig. I'm not sure to understand the modeling if we decide to use e13 for the provenance of the whole graph, would it be something like:
Museum A (e74) -> p14i -> [1] (e13)
[1] (e13) ->p140 -> Museum A's graph
Is it mandatory to use p141? If so, how do I use it?
```