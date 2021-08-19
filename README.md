# GSoC 2021 Qualia Project
## Qualia Relation Extraction and Annotation Tools
The goal of this project was to  develop a set of tools allowing enrichment of FrameNet-BR DB with qualia relations by means of transforming data from existing data sources, such as BabelNet, VerbNet, ConceptNet. I chose to focus on BabelNet specifically due to its multilinguality and (relatively) approachable API.

### Introduction
...

### Components
To reach the stated objective I created three self-contained tools:
- BabelNet Extractor
- FrameNet Sense Mapper
- Qualia Annotation UI Prototype

#### BabelNet Extractor
The first component of this project is a [tool](https://github.com/slowwavesleep/BabelNetExtractor) to extract data required for the project
from local BabelNet indices using the provided Java API. It is written in Scala.

The single purpose of this tool is to query the BabelNet Java API with lexical units found in FrameNet-BR (just the English nouns at the moment) and store returned candidate synsents (their ids, definitions, edges) in files. It may be extended with additional languages and parts of speech.

#### FrameNet Sense Mapper
[The second component](https://github.com/slowwavesleep/FnSenseMapper) is responsible for matching lexical units with the most relevant candidate synsent using cosine similarity between FrameNet and BabelNet definitions. Embeddings from the LaBSE model were used (although that's easy enough to change).

Candidates synsets are ranked according to their sentence similarity to the corresponding FrameNet definitions, synsets with similarity lower than 0.3 are filtered out, as well as candidates with names that, when compared to a correspodning lexical unit name, have FuzzyWuzzy partial ratio below 90. Top 3 (assuming that there are as many after filtering) synset candidates are returned for each lexical unit. The specific values were selected empirically and are modifiable.

#### Qualia Annotation UI Prototype
The purpose [the third component](https://github.com/slowwavesleep/QualiaAnnotationUI) is to facilitate human annotation of data produced by the previous component. It allows a user to go through relation candidates and mark them as acceptable/not acceptable. This is essentially a single-user local app. The data is persisted locally. More work is needed to use it as a fully-fledged web app that is integrated into existing infrastructure.
