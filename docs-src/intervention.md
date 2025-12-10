

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
