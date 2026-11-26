# Changes in OSDi

## OSDi 1.3

OSDi 1.3 makes the ontology able to describe the strategies, outcomes and side effects of the interventions of a health economic evaluation, and fixes the semantics of what an intervention targets. All the changes have been implemented and validated with decision trees in JaDES (jades-hta): 104 test models whose expected results were computed by hand, 34 malformed models that must be rejected for the right reason, and a first validation example of a published study, Evans et al. (1997), whose cost-effectiveness ratio JaDES reproduces (CAD 29 363 per QALY, against 29 366 in the article). A complete reasoner (HermiT) finds OSDi 1.3 and all the test and example ontologies consistent.

### What is new

- **Target of an intervention** (`hasIntervention` / `isInterventionOf`, now between `HealthCondition` and `Intervention`): every intervention has exactly one target, the disease or an element of its progression. Time 0 is the moment when the intervention is applied: every patient presents the target, and the intervention is active during the whole simulation. With an acute target, every patient starts with an episode. Compared interventions have the same target.
- **Strategies of an intervention** are declared as effects that replace the strategies of a health condition (`StrategyReplacementEffect`, `hasReplacementStrategy`). `Intervention` no longer declares `hasStrategy`.
- **Effect magnitudes**: the data item types of the magnitude and of the affected parameter define how an effect applies (relative risks and hazard ratios on the equivalent rate, odds ratios on the odds, differences, replacements). New `DI_HazardRatio` and `DI_OddsRatio`, with their STATO references.
- **Strategy outcomes** (`StrategyOutcome`, with `TreatmentOutcome` — `SuccessOutcome`, `FailureOutcome` — and `DetectionOutcome` — positive, negative, inconclusive): mutually exclusive outcomes with their risk, evaluation time, next strategy, payoffs, effects and resolution of the health condition. `failsIf` (now also towards a health condition), `followedOnSuccessBy`, `followedOnFailureBy` and `resolvesOnSuccess` are short forms. Relapses after the response are the failure of a follow-up strategy.
- **Adverse events** (`AdverseEvent`, `hasAdverseEvent`, `isOccurrenceOf`): problems caused by a strategy, with a mandatory risk per application, one-time payoffs, and effects such as death or the failure of the strategy.
- **Detection**: `detects`, `hasSensitivity`, `hasSpecificity` and `hasTemporaryThreshold` belong to detection strategies, not to interventions. The disease detected by a detection intervention is its target.
- **Payoffs per episode**: one-time utilities are QALY losses (a negative loss is a gain); the costs and utilities of an acute manifestation apply per episode.
- New currency `Currency_CanadianDollar`, and clarified comments of `hasTimeHorizon`, `hasFrequency`, `hasDuration`, `appliesOneTime`, `isDisutility` and `hasProportionWithinGroup`.
- Every class and property has its comment in English and Spanish, with some spelling fixes; `GroupableModelItemWithProportion` gets its own label and comment.

### Individuals and examples

- `individuals/PBD.ttl` and `individuals/T1DM.ttl` import OSDi 1.3 and follow its changes (strategies as replacement effects, detection in the strategies, the scale of the gamma distributions). The IRI of `T1DM.ttl` is fixed (`https://w3id.org/ontologies-ULL/OSDi/individuals/T1DM`), and `individuals/catalog-v001.xml` resolves OSDi 1.3 with the copy at the root of the repository.
- New `examples/`: validation examples of published studies, each one with its ontology, the expected results and the decisions of representation (see `examples/README.md`). The first one is Evans et al. (1997), `examples/evans1997-migraine`.

### Breaking changes

- `hasStrategy` on an intervention: use a `StrategyReplacementEffect`.
- `detects`, `hasSensitivity`, `hasSpecificity` and `hasTemporaryThreshold` on an intervention: move them to its detection strategy.
- The scale of a gamma distribution is now `hasThetaParameter` (mean = shape × scale); OSDi 1.2 called it `hasLambdaParameter`, although lambda usually denotes the rate. Rename the property in your gamma distributions; the value does not change. `hasLambdaParameter` remains the rate of an exponential distribution and the mean of a Poisson distribution.
- An intervention shared by the models of different diseases: since it has exactly one target, a reasoner infers that the diseases are the same individual. Give each model its own interventions.

### What the discrete event simulation of JaDES does not implement yet

JaDES rejects (as unsupported) or ignores some features of OSDi 1.3 in discrete event simulations; decision trees support them:

- strategy outcomes (`hasStrategyOutcome`, `failsIf`), adverse events and guidelines in the strategies of a model (rejected), and therefore relapses after the response;
- detection: the sensitivity and specificity of the detection strategy of each arm are read, but no tests are scheduled, so detection costs are not applied and there is no clinical diagnosis;
- developments and combination rules of progressions (developments are not simulated; a development as a target is rejected);
- the instant death of the progressions, and more than one cost or utility of the same kind in a health condition or an intervention (only the first one is used).

### Documented divergences between decision trees and discrete event simulations

- **Acute target**: a decision tree evaluates a single episode, the one at time 0, and ignores the frequency of the episodes; a simulation also simulates the next episodes, and applies the one-time payoffs of the intervention with each of them.
- **Acute manifestations**: a tree counts one episode or, with a frequency, multiplies the one-time payoffs by the expected number of episodes; a simulation counts every episode.
- **Progressions without risk** are always present in their context in a tree; in a simulation, only through an initial proportion. Trees do not support initial proportions.
- **Effects**: a simulation only applies the effects caused by an intervention; a tree also applies those of the disease and its progressions.
- **Time horizon**: a tree uses it as declared; a simulation rounds it to whole years.
- `Pathway` is only supported in simulations.
