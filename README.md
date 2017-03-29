# semantic-web

Tutorial for semantic web, DBWiki, and SPARQL

Gerard Puhalla,
William Greer,
Sayeed Siddiqui,
Allie Subtenly,
Chen Cai

# Overview

Semantic Web, in a broad sense, refers to the idea of structuring web pages so that computers are better able to read and process the data in these web pages. At present, the concept has not been fulfilled in its entirety, although millions of web pages implement it in some way. In addition, many companies from tech giants to small startups utilize the Semantic Web in some way.

Typically, data on these web pages is stored in RDF (Resource Description Framework) format. Data is linked together so that the relation between any two objects is clearly defined. SPARQL is a query language similar to SQL that is used to retrieve data stored in RDF format from databases, and is recognized as an important technology of the Semantic Web.

DBPedia is a project that strives to structure knowledge in Wikipedia, especially in its infoboxes and category hierarchy. Essentially, entities are constructed from articles and relations from links between them.

The intent of this tutorial is to teach the reader how to construct a SPARQL query, as well as how to interpret the data returned by the query, starting with simple queries and progressing in complexity. 

# Writing a Query

There are a few key parts to a SPARQL query. First in SPARQL, variables are denoted with a ? before the variable name. An example would be ?x or ?person. 

When building a SPARQL query, you must use certain keywords to form your request. The first key word is SELECT. SELECT allows you to pick the value or values that are to be returned. Additionally, SELECT \* can be used to select all of the output. 

The next keyword WHERE, is used to specify requirements and make connections to return the values you want. It is in the WHERE section that you include code to specify the attributes of the result you are seeking. In the WHERE block, lines are built based on the RDF format, and so each line contains a first object, a relation, and a second object. Either of the objects or the relation can be replaced by a variable to further define your query. A period must be used at the end of each line to use multiple lines in a WHERE block.
```		
	SELECT ?thing
	WHERE{
	<http://dbpedia.org/resource/Barack_Obama> ?relationship ?thing
	}
```	
There are several optional parameters you can add to a query as well.
The LIMIT keyword allows you to limit the number of outputs returned by the query. For example, writing a query with LIMIT 10 would limit the output to 10 entries.

The DISTINCT keyword eliminates any duplicates that may occur from a certain object in the query.

The ORDER BY keyword allows you to arrange the order in which the output of the query is returned. An example would be using ORDER BY DESC(?list) to order ?list in descending order.

The PREFIX keyword can be used to represent long url's in beginning as variable, allowing you to reuse a url many times without typing out the entire string.
```	    
	PREFIX ex: <http://example.com/exampleOntology#>
	SELECT ?capital
	WHERE {
	    ?capital ?x  ex:Barack_Obama
	}
```

# Examples  

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

Let’s reuse our prefixes, but for a completely different query. In this query we will incorporate one of our modifiers ORDER BY. We’ll sort countries by their population. To achieve this we will do what we did before with presidents and their wives, but this time do it with countries and their populations. Then we’ll use ORDER BY using the population to sort our countries.

    PREFIX class: <http://dbpedia.org/class/yago/>
    PREFIX onto: <http://dbpedia.org/ontology/>
    SELECT ?country
    WHERE {
      ?country onto:populationTotal ?total .
      ?country rdf:type class:WikicatCountries . 
    } ORDER BY ?total

But let’s say we want to know the 10 largest countries in the world (according to DBpedia). Then we need to modify the above query with the modifier LIMIT and specify in ORDER BY that we want our results in decending order.

    PREFIX class: <http://dbpedia.org/class/yago/>
    PREFIX onto: <http://dbpedia.org/ontology/>
    SELECT ?country
    WHERE {
      ?country onto:populationTotal ?total .
      ?country rdf:type class:WikicatCountries . 
    } ORDER BY DESC(?total)
    LIMIT 10

## Tips on finding entities and relationships

This section will explain how to come up with the URLs shown in the examples. If the desired entity is a Wikipedia article, the URL <http://dbpedia.org/resource/ARTICLE_NAME> will usually be the corresponding DBPedia resource. To find a relationship, use a query like:

    SELECT ?property ?hasValue ?isValueOf
    WHERE {
      { <http://dbpedia.org/resource/ENTITY> ?property ?hasValue }
      UNION
      { ?isValueOf ?property <http://dbpedia.org/resource/ENTITY> }
    }

This query will find all bridges to the entity or from it, thereby giving you all of its relationships. You can then see what their URLs are, to use them in future queries. This query can also be used to find the URL of an entity: if you run it on a closely-related entity, you'll see the desired one come up in the results.

# Quick Summary

Variables are preceded by a question mark (?var).

PREFIX serve as aliases for longer and repetitive urls. These are optional but time saving. 

SELECT works just as it does in SQL. Simply select the variables you wish to return. The star symbol returns all variables (*).

WHERE is where we build queries. Queries are built using triples that relate subjects to different objects via a predicate. The predicate relates the subject to an object. The First Lady is a spouse of the president. These triples can be changed with each other. Remember: island-bridge-island.

DISTINCT only returns unique objects.

ORDER BY and LIMIT serve as query modifiers. They can change the order and limit the results of the query respectively.

FILTER allows us to narrow the results using boolean expressions.

There are many more keywords that allow us to enhance our queries these can be found in the official SPARQL specification:
https://www.w3.org/TR/sparql11-query/



