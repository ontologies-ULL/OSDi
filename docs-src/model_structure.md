The **Model** class ([`osdi:Model`]({{ config.extra.osdi }}#Model)) is the central container for the entire HTA simulation. A user may create different models using the same individuals from the ontology. Individuals are then linked to the model by using the property [`osdi:includesModelItem`]({{ config.extra.osdi }}#includesModelItem).

![Models](img/osdi_model.png)

Currently, **OSDi** includes four modeling paradigms: decision trees ([`osdi:DecisionTreeModel`]({{ config.extra.osdi }}#DecisionTreeModel)), Markov ([`osdi:MarkovModel`]({{ config.extra.osdi }}#MarkovModel)), discrete event simulation ([`osdi:DiscreteEventSimulationModel`]({{ config.extra.osdi }}#DiscreteEventSimulationModel)) and agent-based ([`osdi:AgentBasedModel`]({{ config.extra.osdi }}#AgentBasedModel)).

Each model identifies the intervention whose value is assessed ([`osdi:hasAssessedIntervention`]({{ config.extra.osdi }}#hasAssessedIntervention)) and the interventions it is compared with ([`osdi:hasComparatorIntervention`]({{ config.extra.osdi }}#hasComparatorIntervention)). The role depends on the model: the same intervention may be a comparator in another model. All of them must have the same target (see the [interventions section](intervention.md)).

The BD analysis is structured as a decision tree model.

**TTL Example (A decision tree model for the screening of BD):**

```turtle
osdi:BD_DecisionTreeModel
    a osdi:DecisionTreeModel ;
    rdfs:label "Decision tree model for BD"@en ;
    osdi:hasDescription "Decision tree model for newborn screening of biotinidase deficiency. It compares two strategies: (1) newborn screening for biotinidase deficiency with confirmatory testing and biotin treatment, and (2) no newborn screening (clinical detection only). " ;
    osdi:hasAssessedIntervention osdi:BD_InterventionScreening ;
    osdi:hasComparatorIntervention osdi:BD_InterventionNoScreening ;
    osdi:includesModelItem osdi:BD_Disease , osdi:BD_Seizures , ... , osdi:BD_ManifestationRule , osdi:BD_ClinicalDiagnosis , ... .
```
