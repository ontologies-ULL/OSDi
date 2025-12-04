
In general, individuals of OSDi are expected to be defined in a separate file. All examples use Turtle (TTL) syntax and assume the following prefixes are declared:

```turtle
@prefix osdi: https://w3id.org/ontologies-ULL/OSDi# .
@prefix owl: http://www.w3.org/2002/07/owl# .
@prefix rdfs: http://www.w3.org/2000/01/rdf-schema# .
@prefix xsd: http://www.w3.org/2001/XMLSchema# .
```

The file with the individuals should import **OSDi** and the namespace should be something like `https://w3id.org/ontologies-ULL/OSDi/individuals/<name>`.

```turtle
<https://w3id.org/ontologies-ULL/OSDi/individuals/BD> rdf:type owl:Ontology ;
    rdfs:label "OSDi BD individuals"@en ;
    rdfs:comment "OSDi individuals for newborn screening of Biotinidase Deficiency, version 1.0. "@en ;
    owl:imports <https://w3id.org/ontologies-ULL/OSDi/1.0> .
```

Remember that all the components of **OSDi**  (classes, properties and common named individuals) are [documented online](https://w3id.org/ontologies-ULL/OSDi/1.0).