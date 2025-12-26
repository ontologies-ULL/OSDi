We will continue this section by explaining the main classes available to represent the progression of a disease.

### 3.2. Modeling Disease Progression

![Disease classes](img/osdi_disease_progression.png)

**DiseaseProgressionElement** ([`osdi:DiseaseProgressionElement`]({{ config.extra.osdi }}#DiseaseProgressionElement)) models how the disease develops. It includes two main subclasses: **ClinicalProgressionElement** ([`osdi:ClinicalProgressionElement`]({{ config.extra.osdi }}#ClinicalProgressionElement)) and **ProgressionCombinationRule** ([`osdi:ProgressionCombinationRule`]({{ config.extra.osdi }}#ProgressionCombinationRule))

**ClinicalProgressionElement** ([`osdi:ClinicalProgressionElement`]({{ config.extra.osdi }}#ClinicalProgressionElement)) allow for different level of detail and characterize the different ways in which the disease signs and symptoms occur:
  * **Development** ([`osdi:Development`]({{ config.extra.osdi }}#Development)): A specific progression pathway, such as the natural history without screening. This is also useful when little detail is provided on the specific manifestations of a disease (e.g. you just need to model mild and severe *developments*)
  * **Manifestation** ([`osdi:Manifestation`]({{ config.extra.osdi }}#Manifestation)): Specific symptoms categorized as **Acute** ([`osdi:AcuteManifestation`]({{ config.extra.osdi }}#AcuteManifestation)) (e.g., seizures, hypotonia, skin problems) or **Chronic** ([`osdi:ChronicManifestation`]({{ config.extra.osdi }}#ChronicManifestation)) (e.g., cognitive deficits, optic atrophy, hearing loss).
  * **Stage** ([`osdi:Stage`]({{ config.extra.osdi }}#Stage)): Phases in the progression of a disease, such as those used to describe an oncological condition.

All of these subclasses can be combined to create a detail view of the disease. Hence, you may define **Stages** or **Manifestations** specific to certain **Development**, and also **Manifestations** specific to certain **Stage** by using **hasSubProgression** ([`osdi:hasSubProgression`]({{ config.extra.osdi }}#hasSubProgression)). 

For more complex structures, **OSDi** defines **Progression Combination Rules** ([`osdi:ProgressionCombinationRule`]({{ config.extra.osdi }}#ProgressionCombinationRule)). Each rule may affect several **Clinical Progression Elements** by means of **affectsProgressionElement** ([`osdi:affectsProgressionElement`]({{ config.extra.osdi }}#affectsProgressionElement)). These rules include: 
  * **CoexistenCombinationRule** ([`osdi:CoexistenCombinationRule`]({{ config.extra.osdi }}#CoexistenCombinationRule)) describes a set of progressions (manifestations, stages...) that may occur simultaneously (e.g. retinopathy and nephropathy in diabetes).
  * **AlternativeCombinationRule** ([`osdi:AlternativeCombinationRule`]({{ config.extra.osdi }}#AlternativeCombinationRule)) describes a set of progressions (manifestations, stages...) that are mutually exclusive, i.e., they can not occur simultaneously (e.g. mild and severe development of a disease). This set of progressions may also include a default (i.e., "none of the above") branch by setting the **hasNullProgression** ([`osdi:hasNullProgression`]({{ config.extra.osdi }}#hasNullProgression)) property to true.
  * **SequentialCombinationRule** ([`osdi:SequentialCombinationRule`]({{ config.extra.osdi }}#SequentialCombinationRule)) describes a set of progressions (manifestations, stages...) that appear sequentially. Unlike **AlternativeCombinationRule**, where progressions may appear in any order, in this case each new progressions replaces the former. The clinical progressions affected by this rule must be **Ordered Model Items** ([`osdi:OrderedModelItem`]({{ config.extra.osdi }}#OrderedModelItem)), which allows for the creation of a linked list of model items. The **SequentialCombinationRule** points to the first item by means of the **hasFirst** ([`osdi:hasFirst`]({{ config.extra.osdi }}#hasFirst)) property.

Let's start by representing the specific manifestations: seizures or skin problems are acute problems, and they are therefore, modeled as **AcuteManifestations**. Conversely, hearing loss or cognitive deficit should be treated as **ChronicManifestations**.

**TTL Example (manifestations):**

```turtle
osdi:BD_Seizures
    a osdi:AcuteManifestation ;
    rdfs:label "Seizures due to BD"@en ,
               "Crisis epilépticas debidas a DB"@es ;
    osdi:hasDescription "Acute seizures occurring in untreated or late-treated profound biotinidase deficiency, usually within the first year of life."@en .

osdi:BD_SkinProblems
    a osdi:AcuteManifestation ;
    rdfs:label "Skin problems due to BD"@en ,
               "Problemas cutáneos debidos a DB"@es ;
    osdi:hasDescription "Cutaneous manifestations (eg dermatitis, rash) associated with profound biotinidase deficiency in infancy."@en .

osdi:BD_HearingLoss
    a osdi:ChronicManifestation ;
    rdfs:label "Sensorineural hearing loss due to BD"@en ,
               "Hipoacusia neurosensorial debida a DB"@es ;
    osdi:hasDescription "Long-term sensorineural hearing loss attributed to untreated or late-treated profound biotinidase deficiency, considered an irreversible chronic complication in the model."@en .

osdi:BD_CognitiveDeficit
    a osdi:ChronicManifestation ;
    rdfs:label "Cognitive deficit due to BD"@en ,
               "Déficit cognitivo debido a DB"@es ;
    osdi:hasDescription "Long-term cognitive impairment attributed to untreated profound biotinidase deficiency."@en .
```

Since all the manifestations can occur simultaneously, they can be grouped together into a **DiseaseProgressionSet**.

**TTL Example (rule for coexistent disease progressions):**

```turtle
osdi:BD_ManifestationRule
    a osdi:CoexistenCombinationRule ;
    rdfs:label "Set of manifestations for BD"@en ,
               "Conjunto de manifestaciones para DB"@es ;
    osdi:hasDescription "Set of acute and chronic manifestations for untreated profound biotinidase deficiency. Any combination (including none) is allowed in the decision model."@en ;
    osdi:affectsDiseaseProgression osdi:BD_Seizures ;
                                    BD_SkinProblems ;
                                    BD_HearingLoss ;
                                    BD_CognitiveDeficit .
```


The two forms for BD (profound and partial) can be treated in many different ways in a model: we could define two different populations for each of the forms, and we could even consider them as different diseases. However, in this tutorial, we will consider them as **Developments**.

**TTL Example (Developments for forms of DB):**

```turtle
osdi:BD_ProfoundBD_Development
    a osdi:Development ;
    rdfs:label "Reference development of profound BD without screening"@en ;
    # Links to sets of acute and chronic manifestations, and death
    osdi:hasProgressionElement osdi:BD_ManifestationRule .
```

In general, it is also recommended to link the disease with all the progression elements.

**TTL Example (adding the progressions to the disease):**

```turtle
osdi:BD_Disease
    a osdi:InheritedDisease ,
      osdi:RareDisease ;
    rdfs:label "Biotinidase deficiency"@en ,
               "Deficiencia de biotinidasa"@es ;
    osdi:hasDescription "Inherited metabolic disorder caused by deficiency of the biotinidase enzyme. The decision model distinguishes profound and partial biotinidase deficiency, each with its own untreated reference development."@en ,
    osdi:hasProgressionElement  osdi:BD_ProfoundBD_Development ;
                                osdi:BD_ManifestationRule ;
                                osdi:BD_Seizures ;
                                osdi:BD_SkinProblems ;
                                osdi:BD_HearingLoss ;
                                osdi:BD_CognitiveDeficit .    
```

To see how to numerically characterize the risk of a manifestation or any other disease progression, please refer to the [parameters section](parameter.md#42-adding-uncertainty-to-parameters). 

To see how to create more complex patterns to characterize the disease progression, please refer to the [pathways section](pathway.md).
