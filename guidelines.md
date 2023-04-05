---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

# Guidelines

## Timelines (tentative)
* April: Release corpus and training queries
* July: Release test queries
* Early August: Deadline for submitting runs

## Registration

To participate in TREC please pre-register at the following website: <a href="https://ir.nist.gov/trecsubmit.open/application.html" target="_blank">https://ir.nist.gov/trecsubmit.open/application.html</a>.

## Task definition

In terms of input and output, the movie identification task is relatively straightforward—given an input TOT request, output a ranked list of movies.  Each movie must be identified by its Wikipedia page id and the correct movie should be ranked as high as possible.  For each query, runs should return a ranked list of 1000 Wikipedia page ids.  Runs will be evaluated using IR metrics that are appropriate for IR tasks with one relevant document, such as discounted gain and reciprocal rank.

## Datasets

Participating groups will be given the following resources for training, development, and testing.

### Corpora

**Wikipedia Corpus:** Subset of Wikipedia pages directly or indirectly associated with (relevant sub-classes of) the "audiovisual works" category (231,852 pages). This corpus contains the relevant movie for all TOT queries in the training, development, and test sets described below. An example document is described below. Each page also has the following fields associated with it, which participants can utilize, say, to augment data: 
- **Wikidata:** Wikidata IDs for all pages in the Wikipedia Corpus.
- **IMDb:** Internet Movie Database (IMDb) IDs for pages in the Wikipedia Corpus. Note: Some pages in the Wikipedia Corpus do not have an IMDb ID.

Each document in the corpus will be described by the following fields:
- **doc_id**: The primary identifier, the Wikipedia page ID.
- **page_title**: Wikipedia page title.
- **text**: Parsed text from page_source.
- **wikidata_id**: ID of corresponding entry in Wikidata. Participants can use this to gather additional data (e.g., cast), or to link to external sources like IMDb.
- **wikidata_classes**: List of tuples, each tuple is (Wikidata ID, Wikidata Name), corresponding to the class of the Wikidata entity—e.g., ["Q11424","film"].
- **sections**: A dictionary containing extracted top-level headings and corresponding parsed text.
- **infoboxes**: A list of dictionaries, each containing parsed infoboxes.
- **page_source**: Wikepedia page source, in <a href="https://en.wikipedia.org/wiki/Help:Wikitext" target="_blank">WikiText</a> format. Participants can use this for additional processing—e.g., using a <a href="https://www.mediawiki.org/wiki/Alternative_parsers" target="_blank">parser</a> like <a href="https://mwparserfromhell.readthedocs.io/en/latest/" target="_blank">mwparserfromhell</a>.

### Queries

Queries (or topics in TREC lingo) are sourced from two distinct sources hereby referred to simply as Source-1 and Source-2. Participating groups will be given a JSONL file consisting of a mixture of Source-1 and Source-2 queries for training, development, and test. For the Source-1 subset, sentence-level annotations will also be distributed. Participating groups are encouraged to leverage these codes however they want. This might include treating sentences associated with specific codes differently (e.g., ignoring or down-weighing sentences that convey uncertainty). Note that due to data sharing limitations, the title and text of the Source-2 subset will not be distributed. Participants can download the title and text using a script distributed with the data.

Participants will be provided the following query sets as part of this year's track.
- Train: 150 Source-1 queries and 9000 Source-2 queries.
- Dev: 150 Source-1 queries and 1000 Source-2 queries.
- Test: 150 Source-1 queries and 1000 Source-2 queries.

### Use of external information

You are generally allowed to use external information while developing your runs. When you submit your runs, please fill in a form listing what resources you used.
This could include an external corpus such as TMDB or a pretrained model (e.g. word embeddings, BERT). Additionally,
- Participating groups  are **PERMITTED** and encouraged to gather movie data from multiple sources. We are providing tools to leverage data from Wikipedia, Wikidata, IMDB, and TMDB. Groups are encouraged to explore other resources on their own. Pages from different sources can often be resolved using the IMDB ID (e.g., Wikipedia pages often contain the movie’s IMDB ID).
- Participating groups are **PERMITTED** and encouraged to leverage the qualitative codes associated with the Source-1 train, dev, and test sets. When submitting a run, groups may be asked to explain whether and how the qualitative codes were used during training and/or testing.

## Submission and evaluation

We will be following a similar format as the ones used by most TREC submissions, which is repeated below. White space is used to separate columns. The width of the columns in the format is not important, but it is important to have exactly six columns per line with at least one space between the columns.

```text
1 Q0 pid1    1 2.73 runid1
1 Q0 pid2    2 2.71 runid1
1 Q0 pid3    3 2.61 runid1
1 Q0 pid4    4 2.05 runid1
1 Q0 pid5    5 1.89 runid1
```

, where:

* The first column is the query (topic) ID.
* The second column is currently unused and should always be "Q0".
* The third column is the official identifier of the retrieved document.
* The fourth column is the rank the document is retrieved.
* The fifth column is the score (integer or floating point) that generated the ranking. This score must be in descending (non-increasing) order.
* The sixth column is the ID of the run you are submitting.

The main type of TREC submission is _automatic_, which means there was not manual intervention in running the test queries. This means you should not adjust your runs, rewrite the query, retrain your model, or make any other sorts of manual adjustments after you see the test queries. The ideal case is that you only look at the test queries to check that they ran properly (i.e. no bugs) then you submit your automatic runs. However, if you want to have a human in the loop for your run, or do anything else that uses the test queries to adjust your model or ranking, you can mark your run as _manual_. Manual runs are interesting, and we may learn a lot, but these are distinct from our main scenario which is a system that responds to unseen queries automatically.

Runs will be evaluated using metrics appropriate for retrieval scenarios with one relevant document. In particular, **our primary evaluation metric for this year's track will be discounted gain (DG)** but we may also compute other metrics such as reciprocal rank (RR) and succes@k.
