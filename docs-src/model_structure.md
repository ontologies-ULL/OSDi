## 3. The Model Itself

The **Model** class (`osdi:Model`) is the central container for the entire HTA simulation. A user may create different models using the same individuals from the ontology. The most important property is 

The BD analysis is structured as a decision tree model.

| OSDi Class | Description | BD Example (`osdi:BD_DecisionTreeModel`) |
| :--- | :--- | :--- |
| `osdi:Model` | The HTA simulation container | `a osdi:DecisionTreeModel` |
| `osdi:hasStrategy` | Links to the interventions being compared | `osdi:ScreeningStrategy_BD`, `osdi:NoScreeningStrategy_BD` |
| `osdi:modelsDisease` | Links to the primary disease under study | `osdi:BiotinidaseDeficiency` |

**TTL Example:**

```turtle
osdi:BD_DecisionTreeModel
    a osdi:DecisionTreeModel ;
    rdfs:label "Decision tree model for newborn screening of biotinidase deficiency"@en ;
    osdi:hasDescription """
A deterministic decision tree model comparing two strategies:
(1) newborn screening for biotinidase deficiency with confirmatory testing and biotin treatment,
and (2) no newborn screening (clinical detection only). 
"""@en ;
    osdi:hasStrategy osdi:ScreeningStrategy_BD ,
                     osdi:NoScreeningStrategy_BD ;
    osdi:modelsDisease osdi:BiotinidaseDeficiency .
```
