
The **Population** ([`osdi:Population`]({{ config.extra.osdi }}#Population)) class defines the target birth cohort for Spain and its relevant subgroups (e.g., cases with profound BD) using the [`osdi:isSubpopulationOf`]({{ config.extra.osdi }}#isSubpopulationOf) property.

**TTL Example:**

```turtle
osdi:BD_BirthCohortPopulation
    a osdi:Population ;
    rdfs:label "Birth cohort targeted by BD screening model"@en .

osdi:BD_ProfoundCasesPopulation
    a osdi:Population ;
    rdfs:label "Newborns with profound biotinidase deficiency"@en ;
    osdi:isSubpopulationOf osdi:BD_BirthCohortPopulation .
```

**EpidemiologicalParameter** ([`osdi:EpidemiologicalParameter`]({{ config.extra.osdi }}#EpidemiologicalParameter)) captures population-level statistics like incidence, estimated from the Galicia NBS program. The parameter is linked to the population it describes using [`osdi:isParameterOf`]({{ config.extra.osdi }}#isParameterOf).

**TTL Example (Incidence):**

```turtle
osdi:EPI_Incidence_ProfoundBD_Galicia
    a owl:NamedIndividual , osdi:DeterministicParameter , osdi:EpidemiologicalParameter ;
    rdfs:label "Incidence of profound BD (Galicia NBS program)"@en ;
    osdi:hasDataItemType osdi:DI_Incidence ; # Epidemiological data item type
    osdi:hasExpectedValue 0.0000147885 ;  # [cite_start]Value: 1 in 67 620 [cite: 128]
    osdi:hasSource "Galicia NBS program data; Vallejo-Torres et al. 2015..."^^xsd:string ;
    osdi:isParameterOf osdi:BD_BirthCohortPopulation .
```
