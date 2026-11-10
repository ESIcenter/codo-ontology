# CODO: Cognitive Operations Detection Ontology

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24086889.svg)](https://doi.org/10.5281/zenodo.24086889)

CODO is an OWL 2 ontology for knowledge-graph representation of disinformation campaigns, influence operations and cognitive warfare. It connects content, actors, targets, operations, consequences, countermeasures and evidence in one model, and aligns with DISARM, FIMI, STIX 2.1, MITRE ATT&CK, ABCDE, ENISA, AMAS, RESIST and ETSI TR 104 159.

CODO separates persistent domain entities from contextual relations and from analytical or evidentiary constructs. Relationships that carry role, confidence, time or evidence are modelled as qualified relations (`ActorParticipation`, `ActorRelationship`, `TargetingRelation`, `Assertion` and its specialisations), so that attribution, targeting and effect can be represented as contested or probabilistic claims rather than unconditional facts.

## Files

| File | Content |
|---|---|
| `CODO-v1.0.ttl` | The ontology: OWL 2 in RDF Turtle, self-contained (no `owl:imports`). |
| `CODO-v1.0-competency-queries.rq` | Six SPARQL competency queries, as described in the accompanying publication. |
| `CODO-v1.0-test-graph.ttl` | A small fictitious population that exercises the six queries, with the expected result for each. |

## Namespaces

| Prefix | Namespace | Content |
|---|---|---|
| `dis:` | `https://ontology.cogops.eu/codo#` | CODO classes and properties |
| `disarm:` | `https://ontology.cogops.eu/codo/disarm#` | DISARM v1.6 alignment |

Ontology IRI: `https://ontology.cogops.eu/codo` · Version IRI: `https://ontology.cogops.eu/codo/1.0.0`

## Structure

CODO is organised in five modules. Every top-level class records its module through the annotation `dis:belongsToModule`.

| Module | Content |
|---|---|
| A. Information and Meaning | ContentItem, Claim, Narrative, Topic, Channel |
| B. Actors and Targeting | Actor (nine intrinsic classes), Account, OperationalAsset, ActorParticipation, ActorRelationship, TargetingRelation, AudienceSegment |
| C. Operations | Campaign, Incident, Activity, Objective, Motivation, ManipulationTechnique, Infrastructure |
| D. Consequences and Response | Effect (ImmediateEffect, Outcome, Impact, Harm), Vulnerability, Countermeasure, DetectionEvent |
| E. Evidence, Assessment and Context | Assertion and its specialisations, Evidence, Event, Location, AffectiveProfile, SemanticCluster, SimilarityRelation |

| Element | Count |
|---|---|
| Classes | 187 |
| Datatype properties | 472 |
| Object properties | 252 |
| Annotation properties | 11 |
| DISARM phases, tactics, techniques, sub-techniques, countermeasures | 4, 16, 76, 12, 10 |

The DISARM individuals are a representative subset of DISARM v1.6, which defines 103 techniques, 288 sub-techniques and 140 countermeasures.

## Using CODO

Open `CODO-v1.0.ttl` in an ontology editor such as Protégé, or load it with any RDF library. The example below uses Python and rdflib to load the ontology and the test graph, and runs the six competency queries:

```python
import re
import rdflib

g = rdflib.Graph()
g.parse("CODO-v1.0.ttl", format="turtle")
g.parse("CODO-v1.0-test-graph.ttl", format="turtle")

queries = re.split(r"^#### ", open("CODO-v1.0-competency-queries.rq").read(), flags=re.M)[1:]
for q in queries:
    title, body = q.split("\n", 1)
    print(title, "->", len(list(g.query(body))), "row(s)")
```

Expected result: 1, 2, 1, 1, 1 and 6 rows. Without an OWL reasoner, the symmetric property `dis:coordinatedWith` matches only in the asserted direction; with a reasoner, the first query returns each pair in both directions.

## Citation

The accompanying article, "CODO: An OWL Ontology for Knowledge-Graph Representation of Disinformation Campaigns and Cognitive Warfare" by T. Tagarev and G. Sharkov, is under review. Until it is published, please cite this release:

Tagarev, T.; Sharkov, G. *CODO: Cognitive Operations Detection Ontology*, version 1.0.0. Zenodo, 2026. https://doi.org/10.5281/zenodo.24086889

See also `CITATION.cff`.

## Licence

CODO is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). You may share and adapt it for any purpose, provided you give appropriate credit.

## Authors

Todor Tagarev and George Sharkov.
