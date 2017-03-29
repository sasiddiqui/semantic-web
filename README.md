# semantic-web
Tutorial for semantic web, DBWiki, and SPARQL


Semantic Web
Gerard Puhalla, William Greer, Sayeed Siddiqui, Allie Subtenly, Chen Cai

Overview / Introduction

	Semantic Web, in a broad sense, refers to the idea of structuring web pages so that computers are better able to read and process the data in these web pages. At present, the concept has not been fulfilled in its entirety, although millions of web pages implement it in some way. In addition, many companies from tech giants to small startups utilize the Semantic Web in some way.
Typically, data on these web pages is stored in RDF (Resource Description Framework) format. Data is linked together so that the relation between any two objects is clearly defined. SPARQL is a query language similar to SQL that is used to retrieve data stored in RDF format from databases, and is recognized as an important technology of the Semantic Web.
DBPedia is a project that strives to structure knowledge in Wikipedia, especially in its infoboxes and category hierarchy. Essentially, entities are constructed from articles and relations from links between them.
	The intent of this tutorial is to teach the reader how to construct a SPARQL query, as well as how to interpret the data returned by the query, starting with simple queries and progressing in complexity. 



How to write Query

There are a few key parts to a SPARQL query.
Variables are denoted with a ? before the variable name.
?x
?number 
?person
When building a SPARQL query, you must use certain keywords to form your request.
SELECT 
Pick the value or values to be returned
One or many
* can be used for all
SELECT ?country
SELECT ?country ?city
SELECT *
WHERE
Specify requirements and make connections to return the values you want
Island - Bridge - Island
Thing1 - relationship - Thing2
SELECT ?thing
WHERE
{
   <http://dbpedia.org/resource/Barack_Obama> ?relationship ?thing  }
A period must be used to use multiple lines in a WHERE block.
LIMIT (Optional)
Similar to SQL, limits the number of outputs
LIMIT 10
DISTINCT (Optional)
Eliminates duplicates from output
ORDER BY (Optional)
Again similar to SQL, changes ordering of output
ORDER BY DESC(?list)
PREFIX (Optional)
Used to represent url in beginning as variable
Example:
PREFIX ex: <http://example.com/exampleOntology#>
SELECT ?capital
WHERE
  {
    ?capital ?x  ex:Barack_Obama
  }

Examples / Demos  

The WHERE section of a SPARQL query essentially tells us to find all entities (specified by SELECT) that satisfy certain constraints, known as triples. Triples consist of subject-predicate-object, much like the English sentences “Alice high-fives Bob”, or “Watson is a supercomputer”. Each constraint gets its own line, which is ended by a period.

These constraints can also be thought of as bridges that connect islands in an archipelago. Each island is a noun (entity), and each bridge is a relationship (predicate). You can have multiple bridges connecting the same pair of islands, and 

Find all presidents:
SELECT ?prez
 WHERE {
?prez rdf:type <http://dbpedia.org/class/yago/WikicatPresidentsOfTheUnitedStates>  . 
}

In natural language, we can translate this query as: find all presidents, where a president is defined as being in the class PresidentsOfTheUnitedStates.




Find all first ladies:
The first line of code tells us that any first lady must be a spouse of a president. We then define what a president is, by saying that they must be an instance of the class of presidents.

SELECT ?first_lady
WHERE {
  ?first_lady <http://dbpedia.org/ontology/spouse> ?prez .
  ?prez rdf:type <http://dbpedia.org/class/yago/WikicatPresidentsOfTheUnitedStates>  . 
}

Let’s simplify this query a little by using prefixes. We can then reuse these prefixes in later searches and this makes performing queries easier and faster.

PREFIX class: <http://dbpedia.org/class/yago/>
PREFIX onto: <http://dbpedia.org/ontology/>
SELECT ?first_lady
WHERE {
  ?first_lady onto:spouse ?prez .
  ?prez rdf:type class:WikicatPresidentsOfTheUnitedStates . 
}

*** ADDING ANOTHER EXAMPLE ***




3. Tim Berners-Lee's FOAF information available at http://dig.csail.mit.edu/2008/webdav/timbl/foaf.rdf

PREFIX foaf:  <http://xmlns.com/foaf/0.1/>
SELECT ?name
WHERE {
    ?person foaf:name ?name .
}
Note: ‘?’  -> variable (can be )



4. Find only 5 people from Berners-Lee's list who has homepage

SELECT *
WHERE {
    ?person foaf:name ?name .
    ?person foaf:homepage ?home
}

Finding entity addresses/URLs
	This section will explain how to come up with the URLs shown in the examples. If the desired entity is a Wikipedia article, the URL <http://dbpedia.org/resource/ARTICLE_NAME> will usually be the corresponding DBPedia resource. 
Quick Summary

Variables are preceded by a question mark (?var).

PREFIX serve as aliases for longer and repetitive urls. These are optional but time saving. 

SELECT works just as it does in SQL. Simply select the variables you wish to return. The star symbol returns all variables (*).

WHERE is where we build queries. Queries are built using triples that relate subjects to different objects via a predicate. The predicate relates the subject to an object. The First Lady is a spouse of the president. These triples can be changed with each other. Remember: island-bridge-island.

DISTINCT only returns unique objects.

ORDER BY and LIMIT serve as query modifiers. They can change the order and limit the results of the query respectively.

FILTER allows us to narrow the results using boolean expressions.

There are many more keywords that allow us to enhance our queries these can be found in the official SPARQL specification:
https://www.w3.org/TR/sparql11-query/



