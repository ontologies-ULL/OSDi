
In general, individuals of OSDi are expected to be defined in a separate file. All examples use Turtle (TTL) syntax and assume the following prefixes are declared:

```turtle
@prefix osdi: <https://w3id.org/ontologies-ULL/OSDi#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
```

The file with the individuals should import a specific version of **OSDi** (its version IRI, e.g., `{{ config.extra.osdi }}`), and the namespace should be something like `https://w3id.org/ontologies-ULL/OSDi/individuals/<name>`.

```turtle
<https://w3id.org/ontologies-ULL/OSDi/individuals/BD> rdf:type owl:Ontology ;
    rdfs:label "OSDi BD individuals"@en ;
    rdfs:comment "OSDi individuals for newborn screening of Biotinidase Deficiency, version {{ config.extra.version }}. "@en ;
    owl:imports <{{ config.extra.osdi }}> .
```

Remember that all the components of **OSDi**  (classes, properties and common named individuals) are [documented online]({{ config.extra.osdi }}).