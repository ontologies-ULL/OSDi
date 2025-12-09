## 4. Parameters

Models require several types of parameters: probabilities, costs, utilities... Apart from the numeric value, parameters require the definition of many other properties to be appropriately used or to allow for inference. For example, the risk of suffering some kind of manifestation could be 0.3, but the utilization of this value in a model would be completely different depending on whether it is a probability, a ratio, a proportion, an odd... Besides, parameters have some kind of uncertainty associated to their values. Such uncertainty would represent variability among individuals (first-order uncertainty) or the degree of confidence on your estimates (second-order uncertainty).

Instead of using data properties to define the quantifiable values required by a model, we have created a more complex set of concepts to wrap them. Individuals representing parameters must inherit from two different classes at the same time: **Parameter** (`osdi:Parameter`) and **Parameter Nature** (e.g., `osdi:DeterministicParameter`), and define a **Data Item Type** (`osdi:DataItemType`) by means of the `osdi:hasDataItemType` property.

![Parameters](img/osdi_parameter.png)

We will use a number of simple examples to illustrate how to properly define parameters.

### 4.1. Simple deterministic value

In the original reference study, sensitivity of the screening test was assumed to be 100%. This is a good example of a deterministic parameter. As seen in the code below, the individual is both a **Parameter** and a **DeterministicParameter**. There is a specific **DataItemType** for sensitivity (`osdi:DI_Sensitivity`); otherwise, a more generic type might be used (`osdi:DI_Probability`). It is important always to add the source of the parameter, even when it comes from an assumption. Information on the year the parameter applies to (`osdi:hasYear`) or the geographical context (`osdi:hasGeographicalContext`) may be added.

**TTL Example (Constant sensitivity):**

```turtle
osdi:BD_ScreeningSensitivity
    a owl:NamedIndividual , osdi:DeterministicParameter , osdi:Parameter ;
    rdfs:label "Sensitivity of the screening test"@en ;
    osdi:hasDescription "Sensitivity of the screening test for BD" ;
    osdi:hasDataItemType osdi:DI_Sensitivity ;
    osdi:hasExpectedValue "1.0"^^xsd:double ; 
    osdi:hasSource "Assumption based on the estimated from Galicia" .
```


Let's suppose that we have extracted the proportion of individuals who present seizures from a study
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
