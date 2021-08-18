## Weeks 1-2

- Set up a local version of the FrameNet-BR DB
- Explored the structure of the FrameNet-BR DB
- Extracted the LUs for the alignment (just in English so far)

## Week 3

- Explored the BabelNet API
- Tested some queries
- Found out that it doesn't seem to be possible to extract sense definitions using the HTTP interface, meanwhile SPARQL endpoint doesn't appear to be working at all
- Obtained local indices
- Tested queries using both local indices and the remote web service using Scala
- Implemented a basic Scala-based definition extractor

## Week 4

- WIP on the BabelNet local data extractor:

  -  added reading/writing to/from `csv` format
  -  added edge extraction
  -  created a repository for this tool
  -  extracted BN synsets for nouns in English to test the algorithm
  -  TODO replace language/POS hard code

- Experimented on ranking sense candidates based on cosine similarity between sense definitions

  - Issue: based on several examples, the sense with the most similar defition is not necessarily the most obvious candidate (although, comparing the definitions "manually" (i.e. by reading them) doesn't make it obvious either in these cases)
  - Idea: keep cosine similarity as a feature and come up with a more robust mapping approach (perhaps, taking outgoing edges into account?)
  -  TODO formalize the experiments as a Python repository

## Week 5

- Extracted BN relations using [LaBSE](https://github.com/UKPLab/sentence-transformers) sentence embeddings:

  - Generated similarity score using embeddings
  - Ranked candidates in descending order
  - Filtered out candidates that aren't substrings of queried LUs
  - Took the first most similar candidate for each LU
  - Generated LU -> BN sense map
  - Extracted relations between mappable LUs (i.e. relations where both BN senses have a corresponding LU)
  - Filtered out duplicates, added defitions and words themselves for further analysis
  - See the [csv](/relations.csv)

- It's only English nouns so far
- I haven't yet tried multiple candidates for each LU
- Further discussion on whether the extracted relations resemble the desired result is due

## Weeks 6-9

- Further development and finalization of the FnSenseMapper tool

  - Added (and utilized) an option to generate multiple candidates for each LU
  - Added similarity threshold filtration
  - Added partial ratio (of distance between strings) between names of FN and BN entries
  - Added of list of BN relations that definitely can't interpreted as qualia relations as a result of further analysis

## Week 10-12
- Development of the UI tool

  - Created the annotation screen
  - Added SQLite to persist annotations
  - Added screens to view annotations and relations
  - Added an option to reset current annotations
  - Added an option to reset all data (to be able to load some other relations to annotate)
  - Add an option to annotate a specific relation by clicking in annotation/relation screens
  - Added navigation bar
