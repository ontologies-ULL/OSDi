## 3. Disease and Progression

We will start by describing how to represent a disease and its progression using **OSDi**. We will just define the elements but not the parameters that characterize the progression and risks.

### 3.1. Defining the Disease

![Disease classes](img/osdi_disease.png)

The **Disease** (`osdi:Disease`) class is specialized using subtypes like `osdi:InheritedDisease` or `osdi:RareDisease`. The automated generation of models is expected to use this information in the future to infer the best model paradigm and structure based on this and other characteristics. 

BD is modeled as an inherited and rare disease using `osdi:InheritedDisease` and `osdi:RareDisease`. 

**TTL Example:**

```turtle
osdi:BiotinidaseDeficiency
    a osdi:InheritedDisease ,
      osdi:RareDisease ;
    rdfs:label "Biotinidase deficiency"@en ,
               "Deficiencia de biotinidasa"@es ;
    osdi:hasDescription """
Inherited metabolic disorder caused by deficiency of the biotinidase enzyme.
The decision model distinguishes profound and partial biotinidase deficiency,
each with its own untreated reference development.
"""@en ;
    osdi:hasSource "10.1542/peds.2014-3399"^^xsd:string .
```

### 3.2. Modeling Disease Progression

![Disease classes](img/osdi_disease_progression.png)

**DiseaseProgression** (`osdi:DiseaseProgression`) models how the disease develops. Its subclasses allow for different level of detail and characterize the different ways in which the disease signs and symptoms occur:

  * **Development** (`osdi:Development`): A specific progression pathway, such as the natural history without screening. This is also useful when little detail is provided on the specific manifestations of a disease (e.g. you just need to model mild and severe *developments*)
  * **Manifestation** (`osdi:Manifestation`): Specific symptoms categorized as **Acute** (`osdi:AcuteManifestation`) (e.g., seizures, hypotonia, skin problems) or **Chronic** (`osdi:ChronicManifestation`) (e.g., cognitive deficits, optic atrophy, hearing loss).
  * **Stage** (`osdi:Stage`): Phases in the progression of a disease, such as those used to describe an oncological condition.

All of these subclasses can be combined to create a detail view of the disease. Hence, you may define **Stages** or **Manifestations** specific to certain **Development**, and also **Manifestations** specific to certain **Stage**. For more complex structures, **OSDi** defines **Disease Progression Sets** (`osdi:DiseaseProgressionSet`). These sets include: 
  * **CoexistentDiseaseProgressionSet** (`osdi:CoexistentDiseaseProgressionSet`) describes a set of progressions (manifestations, stages...) that may occur simultaneously (e.g. retinopathy and nephropathy in diabetes).
  * **AlternativeDiseaseProgressionSet** (`osdi:AlternativeDiseaseProgressionSet`) describes a set of progressions (manifestations, stages...) that are mutually exclusive, i.e., they can not occur simultaneously (e.g. mild and severe development of a disease).
  * **SequentialDiseaseProgressionSet** (`osdi:SequentialDiseaseProgressionSet`) describes a set of progressions (manifestations, stages...) that appear sequentially. Unlike **AlternativeDiseaseProgressionSet**, where progressions may appear in any order, in this case each new progressions replaces the former.

The two forms for BD (profound and partial) can be treated in many different ways in a model: we could define two different populations for each of the forms, and we could even consider them as different diseases. However, in this tutorial, we will consider them as **Developments**.

**TTL Example (Developments for forms of DB):**

```turtle
osdi:ProfoundBD_Development
    a osdi:Development ;
    rdfs:label "Reference development of profound BD without screening"@en ;
    # Links to sets of acute and chronic manifestations, and death
    osdi:hasDiseaseProgression osdi:BD_AcuteManifestationsSet ,
                               osdi:BD_ChronicManifestationsSet ,
                               osdi:BD_DiseaseRelatedDeath .

osdi:BD_AcuteManifestationsSet
    a osdi:CoexistentDiseaseProgressionSet ;
    rdfs:label "Acute manifestations of untreated profound BD"@en ;
    # Contains individual manifestations that may occur simultaneously
    osdi:hasDiseaseProgression osdi:BD_Seizures ,
                               osdi:BD_Hypotonia ,
                               osdi:BD_SkinProblems .
```
