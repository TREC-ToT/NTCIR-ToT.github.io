---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

# TREC 2024 Tip-of-the-Tongue (ToT) Track

**Overview paper:** <a href="https://trec.nist.gov/pubs/trec33/papers/Overview_tot.pdf" target="_blank">trec.nist.gov/pubs/trec33/papers/Overview_tot.pdf</a></br>
**Overview presentation:** <a href="https://docs.google.com/presentation/d/1hieEN2lJ27492TFI4fa6wkQ8DPlVlBtlS1PU90neAyk" target="_blank">https://docs.google.com/presentation/d/1hieEN2lJ27492TFI4...</a></br>
**Track proceedings:** <a href="https://trec.nist.gov/pubs/trec33/xref.html#tot" target="_blank">trec.nist.gov/pubs/trec33/xref.html#tot</a>

**Note:** You are viewing the guidelines for the 2024 edition of the TREC ToT track.
Please see the <a href="guidelines">latest guidelines</a> for the upcoming / latest edition of the track.

# Guidelines

**IMPORTANT ANNOUNCEMENT**: The deadline for submitting runs have been **extended to October 4th**.

## Important dates
* **May 16th:** Release corpus and training queries
* **August 26th:** Release test queries
* **October 4th:** Deadline for submitting runs (**Extended**, previously September 23rd)

## Registration

Organizations wishing to participate in TREC 2024 should submit an application.
Participants in previous TRECs who wish to participate in TREC 2024 must submit a new application.

To apply, use the <a href="http://ir.nist.gov/evalbase" target="_blank">new Evalbase web app</a>.
First you will need to create an account and profile, then you can register a participating organization from the main Evalbase page.

Any questions about conference participation should be sent to the general TREC email address, trec (at) nist.gov.

## Task definition

In terms of input and output, the ToT known-item identification task is relatively straightforward—given an input TOT request, output a ranked list of items.
This year, in addition to Movies we are adding two new domains: Landmarks and Celebrities to the track.
So, each item can be either a movie, a landmark, or a celebrity and must be identified by its Wikipedia page id and the correct item should be ranked as high as possible.
For each query, runs should return a ranked list of 1000 Wikipedia page ids.
Runs will be evaluated using IR metrics that are appropriate for IR tasks with one relevant document, such as discounted cumulative gain, reciprocal rank, and success@k.

## Datasets

This year’s track will have a larger corpus to account for multiple domains. The format for the topics / documents have also been modified. The data is hosted in Zenodo and can be downloaded [here](https://zenodo.org/records/11185090). See [Corpora](#corpora) and [Queries](#queries) for a description of the files  (train/dev folders also include qrel files in the TREC format).

| Description                                   | Link             | # entries| 
|-----------------------------------------------|------------------|----------|
| corpus, train, dev1, dev2 (single zip) | [TREC-ToT.zip](https://zenodo.org/api/records/11185090/files-archive) | - |
| corpus (JSONL)                           | [corpus.jsonl.zip](https://zenodo.org/records/11185090/files/corpus.jsonl.zip?download=1) | 3,185,450 |
| train queries (JSONL) & qrel.txt                      | [train.zip](https://zenodo.org/records/11185090/files/train-2024.zip?download=1) | 150 |
| dev1 queries (JSONL) & qrel.txt ('23 dev set)  | [dev1.zip](https://zenodo.org/records/11185090/files/dev1-2024.zip?download=1)           | 150 |
| dev2 queries (JSONL) & qrel.txt ('23 test set) | [dev2.zip](https://zenodo.org/records/11185090/files/dev2-2024.zip?download=1)           | 150 |
| test queries (JSONL)    | [test-2024.zip](https://zenodo.org/records/13370657/files/test-2024.zip?download=1) | 600 |

Note: In our train and dev sets this year all queries come only from the movie domain while our test queries will include a combination of movies, landmarks, and celebrities.

### Corpora

Wikipedia Corpus: Subset of Wikipedia pages directly or indirectly associated with (relevant sub-classes of) the several categories relating to the domains mentioned in the task definition.. This corpus contains the relevant movie for all ToT queries in the training, development, and test sets described below. 
Each document in the corpus will be described by the following fields:

- **doc_id**: the primary identifier, the Wikipedia page ID.
- **title**: wikipedia page title.
- **text**: (full) text of the wikipedia page. 
- **wikidata_id**: ID of corresponding entry in Wikidata. Participants can use this to gather additional data (e.g., cast), or to link to external sources like IMDb.
- **sections**: a list of dictionaries containing the title, start and end positions of extracted top-level headings and corresponding parsed text.
  - **sections.start**: starting (character)  position in the text field
  - **sections.end**: end position in the text field
  - **sections.section**: title of the section


An example document is described below. 

#### Example Document

```json
{
  "doc_id": "846",
  "text": "The Museum of Work (\"Arbetets museum\") is a museum located in Norrk\u00f6ping, Sweden. The museum is located in the \"Strykj\u00e4rn\" (Clothes iron), a former weaving mill in the old industrial area on the Motala str\u00f6m river in the city centre of Norrk\u00f6ping. The former textile factory Holmens Bruk operated in the building from 1917 to 1962.\nThe museum documents work and everyday life by collecting personal stories about people's professional lives from both the past and the present. The museum's archive contain material from memory collections and documentation projects.\nSince 2009, the museum also houses the EWK \u2014 Center for Political Illustration Art, which is based on work of the satirist Ewert Karlsson (1918 \u2014 2004). For decades he was frequently published in the Swedish tabloid, \"Aftonbladet\". \n\nOverview.\nThe museum is a national central museum with the task of preserving and telling about work and everyday life. It has, among other things, exhibitions on the terms and conditions of the work and the history of the industrial society. The museum is also known to highlight gender perspective in their exhibitions. \nThe work museum documents work and everyday life by collecting personal stories, including people's professional life from both the past and present. In the museum's archive, there is a rich material of memory collections and documentation projects \u2014 over 2600 interviews, stories and photodocumentations have been collected since the museum opened.\nThe museum is also a support for the country's approximately 1,500 working life museums that are old workplaces preserved to convey their history. \n\nExhibitions.\nThe Museum of Work shows exhibitions going on over several years, but also shorter exhibitions \u2014 including several photo exhibitions on themes that can be linked to work and everyday life. \nThe history of Alva.\nThe history of Alva Karlsson is the only exhibition in the museum that is permanent. The exhibition connects to the museum's building and its history as part of the textile industry in Norrk\u00f6ping. Alva worked as a rollers between the years 1927 \u2014 1962.\nIndustriland.\nOne of the museum long-term exhibitions is Industriland \u2014 when Sweden became modern, the exhibition was in 2007 \u2014 2013 and consisted of an ongoing bond with various objects that were somehow significant both for working life and everyday during the period 1930 \u2014 1980. The exhibition also consisted of presentations of the working life museums in Sweden and a number of rooms with themes such as: leisure, world, living and consumption.\nFramtidsland (Future country).\nIn 2014, the exhibition was inaugurated that takes by where Industriland ends: Future country. It is an exhibition that investigates what a sustainable society is will be part of the museum's exhibitions until 2019. The exhibition consists of materials that are designed based on conversations between young people and researchers around Sweden. The exhibition addresses themes such as work, environment and everyday life. A tour version of the exhibition is given in the locations Falun, Kristianstad and \u00d6rebro. \n\nThe history of Alva.\nThe history of Alva Karlsson is the only exhibition in the museum that is permanent. The exhibition connects to the museum's building and its history as part of the textile industry in Norrk\u00f6ping. Alva worked as a rollers between the years 1927 \u2014 1962. \n\nIndustriland.\nOne of the museum long-term exhibitions is Industriland \u2014 when Sweden became modern, the exhibition was in 2007 \u2014 2013 and consisted of an ongoing bond with various objects that were somehow significant both for working life and everyday during the period 1930 \u2014 1980. The exhibition also consisted of presentations of the working life museums in Sweden and a number of rooms with themes such as: leisure, world, living and consumption. \n\nFramtidsland (Future country).\nIn 2014, the exhibition was inaugurated that takes by where Industriland ends: Future country. It is an exhibition that investigates what a sustainable society is will be part of the museum's exhibitions until 2019. The exhibition consists of materials that are designed based on conversations between young people and researchers around Sweden. The exhibition addresses themes such as work, environment and everyday life. A tour version of the exhibition is given in the locations Falun, Kristianstad and \u00d6rebro. \n\nEWK \u2014 The Center for Political Illustration Art.\nSince 2009, the Museum also houses EWK \u2014 center for political illustration art. The museum preserves, develops and conveys the political illustrator Ewert Karlsson's production. The museum also holds theme exhibitions with national and international political illustrators with the aim of highlighting and strengthening the political art.",
  "sections": [
	{
  	"start": 0,
  	"end": 798,
  	"section": "Abstract"
	},
	{
  	"start": 798,
  	"end": 1620,
  	"section": "Overview"
	},
	{
  	"start": 1620,
  	"end": 3095,
  	"section": "Exhibitions"
	},
	{
  	"start": 3095,
  	"end": 3371,
  	"section": "The history of Alva"
	},
	{
  	"start": 3371,
  	"end": 3824,
  	"section": "Industriland"
	},
	{
  	"start": 3824,
  	"end": 4371,
  	"section": "Framtidsland (Future country)"
	},
	{
  	"start": 4371,
  	"end": 4761,
  	"section": "EWK \u2014 The Center for Political Illustration Art"
	}
  ],
  "title": "Museum of Work",
  "wikidata_id": "Q6941060"
}
```


### Queries

Participating groups will be given a JSONL file consisting of a random sample of queries (or topics in TREC lingo) each for training, two development sets, and the test set. There are two development sets, dev1 (the previous year’s dev set) and dev2 (previous year’s test set).  The query format for this year is different from last year's format, with two fields: **query_id** and **query**. An example query is described below.

```json
{
  "query_id": "763",
  "query": "Super Rare Surreal Dystopian Masterpiece .\n Very rare movie that is scifi/dystopian/experimental/surreal. It\u2019s like Stalker meets el Topo meets Holy Mountain meets Alphaville meets Delicatessen meets Hard to be a God, like Kurosawa, Tarkovsky, and Lynch had a kid together. It was color, possibly Russian, and I don\u2019t really remember the decade but want to say 60s or 70s, though could easily be more recent. It is VERY rare, there is only one crappy partial print of it, and that is what the youtube version is from. Lot of wide shots in a surreal wilderness, winter settings, strange bleeding saturation in some shots. Crazy costumes. Seriously one of the strangest films I\u2019ve ever seen and my favorite films are strange/weird ones. If you\u2019ve ever seen what you\u2019re thinking of on a \u201cbest weird movies\u201d or \u201cyou\u2019ve never seen this!\u201d list, that\u2019s NOT it. I don\u2019t think this film even has a cult following of ten people. It\u2019s an actual rare gem. Have been looking through selections at 366 Weird Movies and not found it yet (btw the way most of those titles are exactly the kind of not-actually-rare movies this film is definitely not)."
}

```

### Use of external information

You are generally allowed to use external information while developing your runs. When you submit your runs, please fill in a form listing what resources you used.
This could include an external corpus such as TMDB or a pretrained model (e.g. word embeddings, BERT). Additionally,
- Participating groups  are **PERMITTED** and encouraged to gather movie data from multiple sources. We are distributing identifiers for several data sources i.e., Wikipedia, Wikidata and IMDb (if available). Groups are encouraged to explore other resources on their own. Pages from different sources can often be resolved using the IMDB ID (e.g., Wikipedia pages often contain the movie’s IMDB ID).
- Participating groups are **PROHIBITED** from using any data from the following websites that are not already included in the train/dev sets described above. **If you do so, you will be training and/or hyperparameter tuning using test data.**
    - <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification</a>
    - <a href="https://irememberthismovie.com/" target="_blank">iRememberThisMovie.com</a>
 
### Training data documentation and declaration
 
The emerging practices of pretraining large language models on web-scale datasets and community sharing of these pretrained models for downstream training / model development creates challenges in tracking what training data have been used for particular run submissions. Given that some of our test queries are sampled from the <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">MS-TOT dataset</a> that has been publicly available online since 2021, there is a possibility that this data may have been used for training models that are employed in submitted runs. To ensure that we get robust scientific conclusions and insights from this year's track, we are requesting all participants to document *all* training data that were employed in the preparation of a given run to the best of your knowledge (including any data used for pretraining models that you are building on top of). In addition, we are also requesting participants to declare if they are 100% confident that no data from <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification</a> or <a href="https://irememberthismovie.com/" target="_blank">iRememberThisMovie.com</a> was used for training. We recognize that not all pretrained models publicly document their training data in details. So, please mark that declaration to be true *only* if you are 100% confident about all training data used in preparation of your run and can guarantee that it does not include any data from the sources mentioned. We do not discourage submissions where there may be some uncertainty about the training data but we want to be aware of this at the time of analyzing the evaluated results.

## Baselines

We’ve released two baselines, BM2 and  a dense retriever, in <a href="https://github.com/TREC-ToT/bench/" target="_blank">this</a> repository. Additional baselines may be released subsequently. A summary of baseline results, along with corresponding run files are provided below. The results correspond to the dev2 split:

| Benchmark            | Runfiles | NDCG@10 | NDCG@1000 |  MRR@1000 |R@1000  |
|----------------------|----------|----------|-----------------|-------|----|
| <a href="https://github.com/TREC-ToT/bench/blob/main/BM25.md" target="_blank">BM25</a> (k1=1, b=1.0) |  <a href="https://github.com/TREC-ToT/bench/tree/main/runs/bm25" target="_blank">runs</a> | 0.0657  |0.1033| 0.0590 | 0.3600|
| <a href="https://github.com/TREC-ToT/bench/blob/main/DENSE.md" target="_blank">Dense Retrieval (SBERT)</a> (DR) |  <a href="https://github.com/TREC-ToT/bench/tree/main/runs/DR" target="_blank">runs</a> | 0.1040 | 0.1665   | 0.0901  | 0.5600| 



## Submission and evaluation

**Submission form:** <a href="https://ir.nist.gov/evalbase/conf/trec-2024/trec2024-tot-main/submit" target="_blank">https://ir.nist.gov/evalbase/conf/trec-2024/trec2024-tot-main/submit</a> (You must <a href="#registration">register as a participant</a> to submit a run).

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

The main type of TREC submission is *automatic*, which means there was not manual intervention in running the test queries. This means you should not adjust your runs, rewrite the query, retrain your model, or make any other sorts of manual adjustments after you see the test queries. The ideal case is that you only look at the test queries to check that they ran properly (i.e. no bugs) then you submit your automatic runs. However, if you want to have a human in the loop for your run, or do anything else that uses the test queries to adjust your model or ranking, you can mark your run as *manual*. Manual runs are interesting, and we may learn a lot, but these are distinct from our main scenario which is a system that responds to unseen queries automatically.

Runs will be evaluated using metrics appropriate for retrieval scenarios with one relevant document. In particular, **our primary evaluation metric for this year's track will be discounted cumulative gain (DCG)** but we may also compute other metrics such as reciprocal rank (RR) and success@k.
