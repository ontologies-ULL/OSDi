## 4. Parameters

Models require several types of parameters: probabilities, costs, utilities... Apart from the numeric value, parameters require the definition of many other properties to be appropriately used or to allow for inference. For example, the risk of suffering some kind of manifestation could be 0.3, but the utilization of this value in a model would be completely different depending on whether it is a probability, a ratio, a proportion, an odd... Besides, parameters have some kind of uncertainty associated to their values. Such uncertainty would represent variability among individuals (first-order uncertainty) or the degree of confidence on your estimates (second-order uncertainty).

Instead of using data properties to define the quantifiable values required by a model, we have created a more complex set of concepts to wrap them. Individuals representing parameters must inherit from two different classes at the same time: **Parameter** (([`osdi:Parameter`]({{ config.extra.osdi }}#Parameter))) and **Parameter Nature** (e.g., ([`osdi:DeterministicParameter`]({{ config.extra.osdi }}#DeterministicParameter))), and define a **Data Item Type** (([`osdi:DataItemType`]({{ config.extra.osdi }}#DataItemType))) by means of the ([`osdi:hasDataItemType`]({{ config.extra.osdi }}#hasDataItemType)) property.

![Parameters](img/osdi_parameter.png)

We will use a number of simple examples to illustrate how to properly define parameters.

### 4.1. Defining a simple deterministic value

In the original reference study, sensitivity of the screening test was assumed to be 100%. This is a good example of a deterministic parameter. As seen in the code below, the individual is both a **Parameter** and a **Deterministic Parameter**. There is a specific **Data Item Type** for sensitivity (([`osdi:DI_Sensitivity`]({{ config.extra.osdi }}#DI_Sensitivity))); otherwise, a more generic type might be used (([`osdi:DI_Probability`]({{ config.extra.osdi }}#DI_Probability))). It is important always to add the source of the parameter, even when it comes from an assumption. Information on the year the parameter applies to (([`osdi:hasYear`]({{ config.extra.osdi }}#hasYear))) or the geographical context (([`osdi:hasGeographicalContext`]({{ config.extra.osdi }}#hasGeographicalContext))) may be added.

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

### 4.2. Adding uncertainty to parameters

Deterministic parameters are treated as constants but, in general, we can add uncertainty by changing the nature of the parameter to first- or second- order uncertainty, ie.e. by inheriting  [`osdi:FirstOrderUncertaintyParameter`]({{ config.extra.osdi }}#FirstOrderUncertaintyParameter) or [`osdi:SecondOrderUncertaintyParameter`]({{ config.extra.osdi }}#SecondOrderUncertaintyParameter), respectively. Both natures define a [`osdi:hasUncertaintyCharacterization`]({{ config.extra.osdi }}#hasUncertaintyCharacterization) property to describe the uncertainty itself. The most straightforward approach is creating an individual based on the predefined **Probability Distribution Expressions** ([`osdi:ProbabilityDistributionExpression`]({{ config.extra.osdi }}#ProbabilityDistributionExpression)).

Let's suppose the proportion of individuals with profound BD who present seizures, as described in the reference study. Since its expected value comes from a meta-analysis of several studies, we should include second-order uncertainty when modeling this parameter. We would start by creating the individual for the parameter, that includes the expected value obtained from the meta-analysis (0.564).

```turtle
osdi:BD_Seizures_PBD
    a owl:NamedIndividual , osdi:SecondOrderUncertaintyParameter , osdi:Parameter ;
    rdfs:label "Proportion of seizures PBD"@en ;
    osdi:hasDescription "Proportion of seizures in clinically diagnosed profound BD" ;
    osdi:hasDataItemType osdi:DI_Proportion ;
    osdi:hasExpectedValue "0.564"^^xsd:double ; 
    osdi:hasUncertaintyCharacterization BD_Seizures_PBD_Uncertainty
    osdi:hasSource "Meta-analysis from several studies as described in DOI:0.1542/peds.2014-3399" .
```

The uncertainty on the expected value of the proportion of seizures can be characterized by means of a beta distribution with parameters alfa = 65 and beta = 50.

```turtle
osdi:BD_Seizures_PBD_Uncertainty
    a owl:NamedIndividual , osdi:BetaDistributionExpression ;
    rdfs:label "Uncertainty on proportion of seizures PBD"@en ;
    osdi:hasDescription "Characterization of the uncertainty on the proportion of seizures in clinically diagnosed profound BD" ;
    osdi:hasAlfaParameter "65.0"^^xsd:double ;
    osdi:hasBetaParameter "50.0"^^xsd:double .
    osdi:hasSource "Meta-analysis from several studies as described in DOI:0.1542/peds.2014-3399" .
```


### 5.2. Costs

[cite_start]**Cost** (([`osdi:Cost`]({{ config.extra.osdi }}#Cost))) parameters are expressed in 2013 US dollars [cite: 55] and linked to the resource (([`osdi:HealthTechnology`]({{ config.extra.osdi }}#HealthTechnology))) or adverse event (([`osdi:DiseaseProgression`]({{ config.extra.osdi }}#DiseaseProgression))) they apply to.

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

[cite_start]**Utility** (([`osdi:Utility`]({{ config.extra.osdi }}#Utility))) models health-related quality of life (QoL) measures (QALYs)[cite: 52, 53]. [cite_start]If it represents a decrement in QoL, the ([`osdi:isDisutility`]({{ config.extra.osdi }}#isDisutility)) property is set to `true`[cite: 181].

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

**CalculatedParameter** (([`osdi:CalculatedParameter`]({{ config.extra.osdi }}#CalculatedParameter))) models values derived from an explicit formula involving other parameters. The example illustrates an annual cost calculation for follow-up.

**TTL Example (Annual Cost Calculation):**

```turtle
osdi:CALC_AnnualCost_TreatedBD_NoComplications
    a owl:NamedIndividual , osdi:CalculatedParameter , osdi:Parameter ;
    rdfs:label "Annual cost of treated BD patient without complications"@en ;
    osdi:hasDataItemType osdi:DI_UnitCost ;
    osdi:hasExpressionValue "=COST_Biotin_Pack_5mg40Tablets*(365/30) + COST_SpecialistVisit*4 + COST_Audiometry + COST_VisualAcuityTest"^^xsd:string ;
    osdi:hasExpressionLanguage osdi:Exp_Excel .
```
