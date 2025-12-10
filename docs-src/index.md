# 🩺 Tutorial for the Ontology for the Simulation of Diseases (OSDi)

The **Ontology for the Simulation of Diseases (OSDi)** provides a structured and semantic framework for health technology assessment and simulation modeling. The ontology offers classes and properties to define all the key elements required to perform a cost-effectiveness analysis, i.e., diseases and their progression, populations, interventions, costs and utilities. In this tutorial, valid for version 1.0 of the ontology, we overview its main features and provide samples of use.

**OSDi** serves as an interoperability framework for a number of use cases: 

1. Extract information from a data source using an LLM
2. Automatically generate models (using [JaDES-HTA](https://github.com/JaDES-ULL/JaDES-HTA))
3. Infer new knowledge from the available information

The tutorial is intended for users familiar with ontologies and health economic models. If you are not familiar with ontologies, we frankly recommend starting with the [Pizza tutorial](https://www.michaeldebellis.com/post/new-protege-pizza-tutorial). All the examples are expressed in Turtle format.

After giving some details on the reference case, we will walk through:

1. [Some tips before starting](before_start.md)
2. [Explanation of the reference case](reference_case.md)
3. Disease modeling:
    - [Disease](disease.md)
    - [Progression](progression.md)
4. [Parameters](parameter.md)
5. [Characterizing the risk using pathways](pathway.md)
6. [Population affected](population.md)
7. [Interventions and their effects](intervention.md)
8. [Decision model](model_structure.md)
9. [Modeling costs in detail](resource_usage.md)

