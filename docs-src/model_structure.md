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

## 4\. Population and Epidemiology

### 4.1. Defining Population Subgroups

[cite_start]The **Population** (`osdi:Population`) class defines the target birth cohort for Spain [cite: 194] and its relevant subgroups (e.g., cases with profound BD) using the `osdi:isSubpopulationOf` property.

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

### 4.2. Epidemiological Parameters

[cite_start]**EpidemiologicalParameter** (`osdi:EpidemiologicalParameter`) captures population-level statistics like incidence, estimated from the Galicia NBS program [cite: 113-114]. The parameter is linked to the population it describes using `osdi:isParameterOf`.

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

-----

## 5\. Parameters, Costs, and Utilities

All quantifiable values are modeled as **Parameter** (`osdi:Parameter`) instances, differentiated by their **Parameter Nature** (e.g., `osdi:DeterministicParameter`) and **Data Item Type** (`osdi:DataItemType`).

### 5.1. Progression Probabilities

[cite_start]These are estimates of the probability of an outcome in untreated cases, based on meta-analysis of literature[cite: 132, 134].

**TTL Example (Probability of Seizures):**

```turtle
osdi:PROG_Seizures_ProfoundBD_ClinicallyDiagnosed
    a owl:NamedIndividual , osdi:DeterministicParameter , osdi:Parameter ;
    rdfs:label "Probability of seizures in clinically diagnosed profound BD"@en ;
    osdi:hasDataItemType osdi:DI_Proportion ; # Data item type: Proportion/Probability
    [cite_start]osdi:hasExpectedValue "0.564"^^xsd:double ; [cite: 128]
    osdi:isParameterOf osdi:BD_Seizures .
```

### 5.2. Costs

[cite_start]**Cost** (`osdi:Cost`) parameters are expressed in 2013 US dollars [cite: 55] and linked to the resource (`osdi:HealthTechnology`) or adverse event (`osdi:DiseaseProgression`) they apply to.

**TTL Example (Unit Cost):**

```turtle
osdi:COST_ScreeningTest_BD_PerTest
    a owl:NamedIndividual , osdi:DeterministicParameter , osdi:Parameter ;
    rdfs:label "Unit cost per BD screening test"@en ;
    osdi:hasDataItemType osdi:DI_UnitCost ;
    osdi:hasExpectedValue "1.23"^^xsd:double ; # [cite_start]Cost in 2013 US dollars [cite: 143]
    osdi:isParameterOf osdi:InitialScreeningTest_BD .
```

### 5.3. Utilities/Disutilities

[cite_start]**Utility** (`osdi:Utility`) models health-related quality of life (QoL) measures (QALYs)[cite: 52, 53]. [cite_start]If it represents a decrement in QoL, the `osdi:isDisutility` property is set to `true`[cite: 181].

**TTL Example (Disutility for Seizures):**

```turtle
osdi:UTIL_MildSeizureDisorder
    a owl:NamedIndividual , osdi:Utility ;
    rdfs:label "Utility decrement for mild seizure disorder"@en ;
    osdi:hasDataItemType osdi:DI_Utility ;
    [cite_start]osdi:hasExpectedValue "0.040"^^xsd:double ; [cite: 161]
    osdi:isDisutility true ; # This is a QALY decrement (disutility)
    osdi:isParameterOf osdi:BD_Seizures .
```

### 5.4. Calculated Parameters

**CalculatedParameter** (`osdi:CalculatedParameter`) models values derived from an explicit formula involving other parameters. The example illustrates an annual cost calculation for follow-up.

**TTL Example (Annual Cost Calculation):**

```turtle
osdi:CALC_AnnualCost_TreatedBD_NoComplications
    a owl:NamedIndividual , osdi:CalculatedParameter , osdi:Parameter ;
    rdfs:label "Annual cost of treated BD patient without complications"@en ;
    osdi:hasDataItemType osdi:DI_UnitCost ;
    osdi:hasExpressionValue "=COST_Biotin_Pack_5mg40Tablets*(365/30) + COST_SpecialistVisit*4 + COST_Audiometry + COST_VisualAcuityTest"^^xsd:string ;
    osdi:hasExpressionLanguage osdi:Exp_Excel .
```

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