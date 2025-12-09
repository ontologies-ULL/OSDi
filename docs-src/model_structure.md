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

-----

-----


-----

## 6\. Interventions and Strategy Implementation

### 6.1. Defining Strategies and Technologies

A **Strategy** (`osdi:Strategy`) defines a course of action. [cite_start]**ScreeningStrategy** (`osdi:ScreeningStrategy`) is the intervention being assessed [cite: 6][cite_start], compared to the **No Screening Strategy** (clinical detection)[cite: 61]. Strategies use **HealthTechnology** items (e.g., `osdi:Test` or `osdi:Drug`) via `osdi:usesHealthTechnology`.

**TTL Example (Screening Strategy):**

```turtle
osdi:ScreeningStrategy_BD
    a osdi:ScreeningStrategy ;
    rdfs:label "Newborn screening for biotinidase deficiency"@en ;
    # Technologies used in the strategy: tests and treatment
    osdi:usesTest osdi:InitialScreeningTest_BD ,
                  osdi:ConfirmatoryTest_BD ;
    osdi:usesTreatment osdi:BiotinTherapy .
```

### 6.2. Modeling Intervention Effects (Modifiers)

The benefit of early detection and treatment is modeled using **ModifierParameter** (`osdi:ModifierParameter`). [cite_start]This parameter typically represents a **Relative Risk** and modifies the progression probabilities of the untreated disease under the intervention strategy [cite: 122-123]. For BD, the base case assumes a Relative Risk of 0 for all clinical symptoms if detected and treated early.

**TTL Example (Effect of Screening/Treatment):**

```turtle
osdi:MOD_CognitiveDeficit_ProfoundBD_ScreenedTreated
    a owl:NamedIndividual , osdi:ModifierParameter , osdi:Parameter ;
    rdfs:label "Relative risk modifier for cognitive deficits in screened and treated profound BD"@en ;
    osdi:hasDataItemType osdi:DI_RelativeRisk ;
    osdi:hasExpectedValue "0.0"^^xsd:double ; # Assumed Relative Risk (RR) of 0
    # This modifier changes the baseline probability of cognitive deficits:
    osdi:modifiesParameter osdi:PROG_CognitiveDeficit_ProfoundBD_ClinicallyDiagnosed .

osdi:ScreeningStrategy_BD
    # The strategy involves applying the modifier
    osdi:involvesModification osdi:MOD_CognitiveDeficit_ProfoundBD_ScreenedTreated .
```

```
```