Within each model, you must define one or more **Interventions** ([`osdi:Intervention`]({{ config.extra.osdi }}#Intervention)) to be assessed for cost-effectiveness. In the typical scenario for one intervention versus a comparator, the assessed intervention will be identified by setting [`osdi:isAssessedIntervention`]({{ config.extra.osdi }}#isAssessedIntervention) to `true`.

![Intervention](img/osdi_intervention.png)

**TTL Example (Interventions for comparing NBS versus clinical detection):**

```turtle
osdi:BD_InterventionNoScreening 
    a owl:NamedIndividual , osdi:DiagnosisIntervention ;
    osdi:hasDescription "No screening, i.e., clinical detection, of BD" ;
    osdi:isAssessedIntervention "false"^^xsd:boolean .

osdi:BD_InterventionScreening 
    a owl:NamedIndividual ,  osdi:ScreeningIntervention ;
    osdi:hasCost osdi:BD_CostScreening ;
    osdi:hasSensitivity osdi:BD_ScreeningSensitivity ;
    osdi:hasSpecificity osdi:BD_ScreeningSpecificity ;
    osdi:involvesModification osdi:BD_InterventionScreeningEffect ;
    osdi:hasDescription "Basic screening intervention for BD" ;
    osdi:isAssessedIntervention "true"^^xsd:boolean .
```

**Interventions** may involve costs and disutilities, but they should have some kind of effect on the progression of the disease or the use of resources. Hence, **Interventions** involve modifications ([`osdi:involvesModification`]({{ config.extra.osdi }}#involvesModification)) that are defined as **Modifier Parameters** ([`osdi:ModifierParameter`]({{ config.extra.osdi }}#ModifierParameter)). A **Modifier Parameter** is a special type of parameter that includes a property to indicate the parameter (or parameters) whose value is modifying ([`osdi:modifies`]({{ config.extra.osdi }}#modifies)).

For example, according to the reference case, the effect of the screening should be to prevent the appearance of the manifestations related to profound BD. You can also use relative risks or mean differences to model such effects. 

**TTL Example (Effect of the screening):**

```turtle
osdi:BD_InterventionScreeningEffect 
    a owl:NamedIndividual ,  osdi:ModifierParameter , osdi:DeterministicParameter ;
    osdi:hasDataItemType osdi:DI_Continuous_Variable ;
    osdi:modifies osdi:BD_Proportion_Seizures_PBD , ... ;
    osdi:hasDescription "Sets the probabilities of all the manifestations to 0" ;
    osdi:hasExpectedValue "0.0"^^xsd:double .
```

