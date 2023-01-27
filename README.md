Semantic Clothing
========

* [Introduction](https://github.com/danielamariei/semantic-clothing#introduction)
* [Demo](https://github.com/danielamariei/semantic-clothing#demo)
* [Requirements](https://github.com/danielamariei/semantic-clothing#requirements)
* [Software Design](https://github.com/danielamariei/semantic-clothing#software-design)
  * [Data Modelling](https://github.com/danielamariei/semantic-clothing#data-modelling)
  * [Software Architecture](https://github.com/danielamariei/semantic-clothing#software-architecture)
  * [REST API](https://github.com/danielamariei/semantic-clothing#rest-api)  
* [Semantic Clothing Ontology](https://github.com/danielamariei/semantic-clothing#semantic-clothing-ontology)
* [Development & Installation](https://github.com/danielamariei/semantic-clothing#development-&-installation)
* [Bibliography](https://github.com/danielamariei/semantic-clothing#bibliography)

## Introduction
Semantic Clothing Web Application that enables the selection of different clothing items based on the existing wardrobe. Some people have difficulties when choosing the right vestment for a certain type of event (_i.e._, interview, dinner party, show, etc.). The purpose of the project is to create an Web Application that enables the selection of different clothing items based on the existing wardrobe; the information of interest can be obtained from [DBpedia](http://wiki.dbpedia.org/About) or
 [Freebase](https://developers.google.com/freebase/). The application will offer suggestions that take into account the fashion proclivity, the season, or different preferences that the user has. Moreover, the user will be able to receive information regarding the purchasing of different items of interest, taking into account its geographic localization.

## Demo

<p align="center">
  <a href="https://raw.githubusercontent.com/danielamariei/semantic-clothing/master/demo/semcloth-demo.mp4">
    <img src="https://raw.githubusercontent.com/danielamariei/semantic-clothing/master/demo/semantic-clothing-demo-thumbnail.png">
  </a>
</p>

## Requirements

Some people have difficulties when choosing the right vestment for 
a certain type of event (_i.e._, interview, dinner party, show, etc.).
The purpose of the project is to create an Web Application that enables
the selection of different clothing items based on the existing wardrobe;
the information of interest can be obtained from [DBpedia](http://wiki.dbpedia.org/About) or
 [Freebase](https://developers.google.com/freebase/). The application will
offer suggestions that take into account the fashion proclivity, the season, or different
 preferences that the user has. Moreover, the user will be able to receive information 
regarding the purchasing of different items of interest, taking into account its geographic localization.

**Keywords**: project, infoiasi, wade, web, fii, uaic, semantic, rdf, sparql. 

### Use cases

In this sub-section we will provide some use-cases regarding possible uses of the application.

#### First Usage Flow
The following sketch depicts the flow of the user when he uses the application for the first time.
In this use-case, the user has to complete the following three steps:

1. Provide relevant account details, or link the account to an existing ID;
2. Provide relevant information about him: age, sex, culture, geographical location, etc.;
3. Configure his wardrobe: (s)he needs to add the clothing items that constitute the wardrobe.

Click on the image to see it at full size.

<a href="https://github.com/riquack/semcloth/blob/master/wiki-resources/images/first-uage-flow.jpg">
<img src="https://github.com/riquack/semcloth/blob/master/wiki-resources/images/first-uage-flow.jpg" alt="An image depicting the flow the user has to follow when he uses the application for the first time." width="300px" height="450px"/>
</a>

#### Clothing Recommendations
In this use-case we depict the mode of interaction when the users wants to obtains a list of recommendations. In the first screen, on the right, the user is able to see the list of recommended items, according to the selected criteria. The options according to which the user can obtain recommendations are presented in more detail in the second screen. Here, the user has several options from which he can select: **event**, **style preferences**, **weather & season** and **trends**.

Click on the image to see it at full size.

<a href="https://github.com/riquack/semcloth/blob/master/wiki-resources/images/clothing-recommendations.jpg">
<img src="https://github.com/riquack/semcloth/blob/master/wiki-resources/images/clothing-recommendations.jpg" alt="An image depicting an user interface that displays a list of items." width="300px" height="450px" />
</a>

#### Recommendation Details and Wardrobe Configuration
In the first screen, we present the information the user sees for a specific recommendation. The left side of the screen presents to the user the recommended clothing items -- according to his preferences --, while the right side presents a map on which the user has available various location at which he can go shopping for various clothing items that might be of interest to him.

The second screen presents to the user the possibility to configure his wardrobe (_i.e._, add or remove clothing items).


Click on the image to see it at full size.

<a href="https://github.com/riquack/semcloth/blob/master/wiki-resources/images/recommendation-details.jpg">
<img src="https://github.com/riquack/semcloth/blob/master/wiki-resources/images/recommendation-details.jpg" alt="An image that depicts an user interface that displays a screen with various details regarding an item." width="300px" height="450px" />
</a>

## Software Design

### Data Modelling
The first step in developing the application requires involves defining the data model(s). 

#### Internal data

**1. A clothing item**

The first thing that need to be modeled in our application is a _clothing item_. Each clothing item that the users adds to its wardrobe will be represents as an _RDF assertion_ of the form: 

<img src="https://raw.githubusercontent.com/riquack/semcloth/master/wiki-resources/images/clothing-item.png" alt="An image representing the and RDF assertion for a clothing item." />

Here, **clothing item** is represented by things like: shoes, skirt, shirt, jeans, etc. Each clothing item can have different **characteristics** like: fabrics, color, size, texture, manufacturer, etc. In the end, we have a **value** for each characteristic of the clothing item -- the value can be both literal or resource like: _red_ or _<http://dbpedia.org/resource/Red>_ for the color characteristic, and similar.

**2. The Wardrobe**

Going a step further, we need to model the wardrobe for a user. In essence, an wardrobe will contain a set of clothing items, which are in fact represented by a set of RDF assertions. As such, an wardrobe will be represented by a _Named RDF Graph_, which will be identified uniquely to correspond to the ID of the user. This will enable us to infer recommendation for a user taking into account only the clothing items relevant to him -- those that constitute his wardrobe. Following, we depict the modelling of the wardrobe:

<img src="https://github.com/riquack/semcloth/blob/master/wiki-resources/images/wardrobe.png" alt="An image depicting the representation of an wardrobe as a Named RDF Graph." />

**3. All the Wardrobes**

The final step in modelling the data is the conceptual representation of the wardrobes for all the users. Since each user has its wardrobe store in a Named RDF Graph, this naturally leads to the concept of _RDF Dataset_, which is comprised by zero or more Named RDF Graph an at most an unnamed one. As such, and the RDF Dataset will be comprised by all the wardrobes the users of our application have. Following, we depict the modelling of all the wardrobes:

<img src="https://github.com/riquack/semcloth/blob/master/wiki-resources/images/all-the-wardrobes.png" alt="An image depicting the representation of all the wardrobes as an RDF Dataset." />

**4. Clothes ontology**

In order to be able to make clothing recommendations to the user, we need to develop an ontology that takes into consideration the various hierarchical and relational aspects of the clothes. For example: the events at which a particular class of clothing items can be used, weather restrictions for certain types of fabrics (_e.g._, cashmere, silk, etc.), and similar considerations. 

The ontology also need to reflect the existing vocabulary that we can use when the users adds clothing items to his wardrobe. 

The ontology will also act as a binder between existing -- external -- ontologies, in order to provide a common framework in which we can make inferences.

#### External Data

**Clothing Items Ontology**

We did not find one yet.

**GeoNames Ontology**

We can use this ontology in order to add semantic information to our data from a geographical standpoint. This will entail us to make recommendation that are nearby of the user, take into consideration to season in the respective location an similar aspects.

**Maps Image APIs**

We will use this APIs to embed a Google Maps image in order to pinpoint the locations of the stores recommended to the user.

**Places API**

This API will be used to obtain information about nearby store for a particular geographical location.


### Software Architecture

The system will be structured according to an N-Tier Architecture, according to the following graphical representation:

<img src="https://github.com/riquack/semcloth/blob/master/wiki-resources/images/architecture.png" alt="An image depicting the architecture of the system." />

### REST API
[Towards the Semantic Clothing REST API](http://htmlpreview.github.io/?https://github.com/riquack/semcloth/blob/master/wiki-resources/raml/SemCloth.html)


## Semantic Clothing Ontology

In order to develope the _Ontology_ for our project, we have used various external _vocabularies/ontologies/datasets_, including:
* [DBpedia](http://dbpedia.org/About);
* [Dublin Core Metada Initiative](http://dublincore.org/);
* [Semantically-Interlinked Online Communities](http://rdfs.org/sioc/spec/) (for the moment just one term).

The _Semantic Clothing Ontology_ is available in the following serialization formats:
* [OWL/XML](https://github.com/riquack/semcloth/blob/master/ontology/semantic-clothing.owl);
* [RDF/XML](https://github.com/riquack/semcloth/blob/master/ontology/semantic-clothing.rdf.xml);
* [Turtle](https://github.com/riquack/semcloth/blob/amaster/ontology/semantic-clothing.ttl);
* [Functional Syntax](https://github.com/riquack/semcloth/blob/master/ontology/semantic-clothing.functional.owl);
* [Manchester Syntax](https://github.com/riquack/semcloth/blob/master/ontology/semantic-clothing.manchester.owl)

## Development & Installation
In order to test the application you need to perform the following steps:

#### Prepare the database ####
1. For more details please consult [the Stardog documentation](http://docs.stardog.com/);
2. Create a new database and import the following files in a new database:
 1. The Semantic Clothing Ontology; pick one of the following serialization formats found in [the ontology folder](https://github.com/riquack/semcloth/tree/master/ontology); 
 2. The **dbpedia-triples.ntriples** file from the [dbpedia resource folder](https://github.com/riquack/semcloth/tree/master/dbpedia-resources).
3. Make sure you start the database before using the application.

#### Prepare the application ####
1. Install [the Scala interactive build tool](http://www.scala-sbt.org/);
2. Fetch the repository;
3. Update the **ConnectionPool.scala** file to contain the name of your newly created database;
4. Open a terminal and position yourself in the **restApi** folder and run the following commands:
 1. _sbt_;
 2. _compile_;
 3. _run_.

#### Start using the application ####
1. Open the following URL in a browser: _http://localhost:9000/semcloth/login.html_.

## Bibliography
1. ***, Dr. Sabin-Corneliu Buraga, FII, UAIC. _Web Application Development_. http://profs.info.uaic.ro/~busaco/teach/courses/wade/;
2. ***, MKBERGMAN.COM, AI3:::Adaptive Information Adaptive Innovation Adaptive Infrastructure. _A decade in the Trenches of the Semantic Web_. http://www.mkbergman.com/1771/a-decade-in-the-trenches-of-the-semantic-web/;
3. ***, MKBERGMAN.COM, AI3:::Adaptive Information Adaptive Innovation Adaptive Infrastructure. _The Rationale for Semantic Technologies_. http://www.mkbergman.com/1015/the-rationale-for-semantic-technologies/;
4. ***, W3.ORG, Tim Berners-Lee. _Linked Data_. http://www.w3.org/DesignIssues/LinkedData.html;
5. ***, W3.ORG, Guus Scheriber et al. _RDF 1.1 Primer_. http://www.w3.org/TR/2014/NOTE-rdf11-primer-20140624/;
6. ***, W3.ORG, Richard Cygniak et al. _RDF 1.1 Concepts and Abstract Syntax_. http://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/;
7. ***, W3.ORG, Dan Brickley et al. _RDF Schema 1.1_. http://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/;
8. Channu Kambalyal (Prepared By). _3-Tier Architecture_ș
9. ***, LINKEDDATATOOLS.COM. _Introducing Linked Data And The Semantic Web_. http://www.linkeddatatools.com/semantic-web-basics/;
