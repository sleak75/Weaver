##################################
An ontology for a software project
##################################

Using Turtle_ for its human-readability, with help from `this blog post`_

.. _Turtle: https://www.w3.org/TR/turtle
.. _`this blog post`: http://pedroszekely.blogspot.com/2013/06/writing-ontologies-in-turtle.html

RDF/Turtle quick start:

- triples of subject-predicate-object, each component is a URI or a literal
  (but predicates are always URIs)
- URIs can be abbreviated via prefixes: how the prefix should be expanded is
  described in an @prefix statement
- statements end with a period (.). a comma after the object indicates that
  the next element is aother object (the subject and predicate are the same),
  and a semicolon indicates that the next 2 elements are predicate and object
  for the same subject

First define some prefixes (including a base, empty one) to make the rest of 
this document more readable:

.. code-block:: turtle

  @prefix : <http://example.com/owl/weaver/> .
    # empty prefix for this ontology. Eventually example.com should be replaced
    # with the github location of this file
  @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
  @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
    # rdf and rdfs have basic vocabulary: most important are rdf:Property and
    # rdf:type (abbreviated 'a'). Key things of type rdf:Property are 
    # rdfs:domain, rdfs:range, rdfs:subClassOf.
  @prefix owl: <http://www.w3.org/2002/07/owl#> .
    # owl provides a formal description logic, more constrained that rdf. (Use 
    # owl:Class instead of rdfs:Class) owl also adds unionOf, sameAs, etc

.. end

some more text

back to rst
----------




