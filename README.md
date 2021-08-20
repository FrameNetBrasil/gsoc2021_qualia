# GSoC 2021 Qualia Project
## Qualia Relation Extraction and Annotation Tools

### Introduction
FN-Br has been implementing qualia relations in its database. These relations derive from the idea of qualia structure, proposed by Pustejovsky (1995) in the Generative Lexicon (GL) theory. Basically, GL assumes that the meaning of words is structured on four generative factors, called qualia roles. Each role captures how humans understand objects and the relationships between these objects in the world, trying to provide some explanation for the linguistic behavior of lexical items. Pustejovsky (1995, p. 85) defines four qualia roles:


- Formal: values that establish what differentiates a given object within its semantic domain; it is typically the description of its basic category.
- Constitutive: values that express the relationship between a given object and its constituents or parts, such as material, weight, or characteristic parts.
- Telic: values related to information about the object’s function or purpose, such as the intention of an agent performing a given action or the object’s intrinsic function.
- Agentive: values that determine the origin of the object, such as its creator, type of origin (natural or artificial), or its initial cause.


At FN-Br, this idea was implemented by creating relations between Lexical Units (LUs). Each relation has specific semantics (e.g. part_of, used_for, used_by, is_a, etc.) and is associated with one of the four qualia roles. Because of its specific semantics, each relation is also associated with a background frame that improves and explains the semantics of the relation. Each LUs is associated with a Frame Element of this Background frame. Therefore, the use of qualia relations constitutes a “lexical ontology”, which is used in several processes, such as the disambiguation of LUs and parsing.

FN-Br currently has more than 25000 qualia relations for LUs in English and Brazilian Portuguese. However, this is a relatively small number if we take into account the total number of LUs in the database (more than 27000, including pt, en, es). These relations were created manually–which is a time-consuming and costly process.

This idea proposes the development of a pipeline that takes advantage of databases available on the Internet (lexical resources, semantic networks, ontologies, etc.) to automatically create new qualia relations. Examples of resources that can be used include: VerbNet, ConceptNet, BabelNet, Framester, among others. These resources provide semantic relations between lexical items.

A successful project should implement a (human in the loop) solution for (semi-) automatically extracting qualia relations between words in existing databases and incorporating them to FN-Br.

The goal of this project was to  develop a set of tools allowing enrichment of FrameNet-BR DB with qualia relations by means of transforming data from existing data sources, such as BabelNet, VerbNet, ConceptNet. I chose to focus on BabelNet specifically due to its multilinguality and (relatively) approachable API.


### Components
To reach the stated objective three self-contained tools were created:
- BabelNet Extractor
- FrameNet Sense Mapper
- Qualia Annotation UI Prototype

#### BabelNet Extractor
The first component of this project is a [tool](https://github.com/slowwavesleep/BabelNetExtractor) to extract data required for the project
from local BabelNet indices using the provided Java API. It is written in Scala.

The single purpose of this tool is to query the BabelNet Java API with lexical units found in FrameNet-BR (just the English nouns at the moment) and store returned candidate synsents (their ids, definitions, edges) in files. It may be extended with additional languages and parts of speech.

#### FrameNet Sense Mapper
[The second component](https://github.com/slowwavesleep/FnSenseMapper) is responsible for matching lexical units with the most relevant candidate synsent using cosine similarity between FrameNet and BabelNet definitions. Embeddings from the LaBSE model were used (although that's easy enough to change).

Candidate synsets are ranked according to their sentence similarity to the corresponding FrameNet definitions, synsets with similarity lower than 0.3 are filtered out, as well as candidates with names that, when compared to a correspodning lexical unit name, have FuzzyWuzzy partial ratio below 90. Top 3 (assuming that there are as many after filtering) synset candidates are returned for each lexical unit. The specific values were selected empirically and are modifiable.

#### Qualia Annotation UI Prototype
The purpose [the third component](https://github.com/slowwavesleep/QualiaAnnotationUI) is to facilitate human annotation of data produced by the previous component. It allows a user to go through relation candidates and mark them as acceptable/not acceptable. This is essentially a single-user local app. The data is persisted locally. More work is needed to use it as a fully-fledged web app that is integrated into existing infrastructure.
