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
