---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

# TREC 2025 Tip-of-the-Tongue (ToT) Track

Welcome to the guidelines for the upcoming 2025 edition of the TREC ToT track!

# Guidelines

## Important dates
* **May 9th:** Release corpus and training queries
* **Early July (tentative):** Release test queries
* **August (tentative):** Deadline for submitting runs

## Registration

Organizations wishing to participate in TREC 2025 should submit an application.
Participants in previous TRECs who wish to participate in TREC 2025 must submit a new application.

To apply, use the <a href="http://ir.nist.gov/evalbase" target="_blank">new Evalbase web app</a>.
First you will need to create an account and profile, then you can register a participating organization from the main Evalbase page.

Any questions about conference participation should be sent to the general TREC email address, trec (at) nist.gov.

## Task definition

In terms of input and output, the ToT known-item identification task is relatively straightforward—given an input ToT request, output a ranked list of items.
In previous years, our focus was on specific domains like movies, landmarks, and celebrities.
This year, the retrieval task will be open domain.
So, each item can be any entity and must be identified by its Wikipedia page id and the correct item should be ranked as high as possible.
For each query, runs should return a ranked list of 1000 Wikipedia page ids.
Runs will be evaluated using IR metrics that are appropriate for IR tasks with one relevant document, such as discounted cumulative gain, reciprocal rank, and success@k.

## Datasets
This year’s track will have a larger corpus to account for the open domain setting.
The format for the topics / documents have also been modified.
The data is hosted in Zenodo and can be downloaded <a href="https://zenodo.org/records/15356599" target="_blank">here</a>.
The corpus can also be accessed via <a href="[https://github.com/mam10eks/ir_datasets/tree/trec-tot-2025](https://github.com/allenai/ir_datasets)" target="_blank">IR-Dataset</a> and <a href="https://huggingface.co/datasets/wikimedia/wikipedia" target="_blank">Hugging Face Datasets</a>.
See [Corpora](#corpora) and [Queries](#queries) for a description of the files and additional access information.

| Description                                   | Link             | # entries| md5sum |
|-----------------------------------------------|------------------|----------|--------|
| corpus (JSONL)                                | [trec-tot-2025-corpus.jsonl.gz](https://zenodo.org/records/15356599/files/trec-tot-2025-corpus.jsonl.gz) | 6,407,814 | a2c82398aa86df6a68c8706b9b462bf2 |
| corpus offsets (JSONL)                                | [trec-tot-2025-offsets.jsonl.gz](https://zenodo.org/records/15356599/files/trec-tot-2025-offsets.jsonl.gz) | 6,407,814 | 00678e3155d962bb244e034e6401b79b |
| train queries                                 | [train-2025-queries.jsonl](https://zenodo.org/records/15356599/files/train-2025-queries.jsonl)      | 143 | 288b7707b4e897f7447aac2cc2f613be |
| train qrels                                   | [train-2025-qrel.txt](https://zenodo.org/records/15356599/files/train-2025-qrel.txt)  | 143 | 10a3c727fc5806ec4510f7a071b57cd7 |
| dev1 queries ('23 dev set)                    | [dev1-2025-queries.jsonl](https://zenodo.org/records/15356599/files/dev1-2025-queries.jsonl)       | 142 | b87c2f51d058de844e258a69b02e70fc |
| dev1 qrels                                    | [dev1-2025-qrel.txt](https://zenodo.org/records/15356599/files/dev1-2025-qrel.txt)   | 142 | 0c913ce8b5b287c73a6dfac662971e82 |
| dev2 queries ('23 test set)                   | [dev2-2025-queries.jsonl](https://zenodo.org/records/15356599/files/dev2-2025-queries.jsonl)       | 143 | b174a128a255e92d0d54b76465d596b5 |
| dev2 qrels                                    | [dev2-2025-qrel.txt](https://zenodo.org/records/15356599/files/dev2-2025-qrel.txt)   | 143 | 4548eb41e639905384aa017c69129bfc |
| dev3 queries ('24 test set)                   | [dev3-2025-queries.jsonl](https://zenodo.org/records/15356599/files/dev3-2025-queries.jsonl)       | 536 | 259c11645694a3c5230b66c7852d4d80 |
| dev3 qrels                                    | [dev3-2025-qrel.txt](https://zenodo.org/records/15356599/files/dev3-2025-qrel.txt)   | 536 | 48ab0d24a5946861546e54064238477f |
| test queries                                  | coming soon!         |  |  |

Note: In our train / dev1 / dev2 sets all queries come only from the movie domain while dev3 queries include a combination of movies, landmarks, and celebrities.
The test queries for this year can be from any arbitrary domain.
Also, the dev sets this year, which are based on dev and test sets from previous years of the track, may be missing some queries because the corresponding relevant documents are not in our corpus this year.

### Corpora

Wikipedia Corpus: Subset of Wikipedia pages containing the relevant items for all ToT queries in the training, development, and test sets described below.
Each document in the corpus will be described by the following fields:

- **doc_id**: the primary identifier, the Wikipedia page ID.
- **url**: the wikipedia page URL.
- **title**: wikipedia page title.
- **text**: (full) text of the wikipedia page.

An example document is described below.

#### Example Document

```json

{
  "id": "846",
  "url": "https://en.wikipedia.org/wiki/Museum%20of%20Work",
  "title": "Museum of Work",
  "text": "The Museum of Work (Arbetets museum) is a museum located in Norrköping, Sweden. The museum is located in the Strykjärn (Clothes iron), a former weaving mill in the old industrial area on the Motala ström river in the city centre of Norrköping. The former textile factory Holmens Bruk (sv) operated in the building from 1917 to 1962.\n\nThe museum documents work and everyday life by collecting personal stories about people's professional lives from both the past and the present. The museum's archive contain material from memory collections and documentation projects.\n\nSince 2009, the museum also houses the EWK – Center for Political Illustration Art, which is based on work of the satirist Ewert Karlsson (1918–2004). For decades he was frequently published in the Swedish tabloid, Aftonbladet.\n\nOverview \nThe museum is a national central museum with the task of preserving and telling about work and everyday life. It has, among other things, exhibitions on the terms and conditions of the work and the history of the industrial society. The museum is also known to highlight gender perspective in their exhibitions.\n\nThe work museum documents work and everyday life by collecting personal stories, including people's professional life from both the past and present. In the museum's archive, there is a rich material of memory collections and documentation projects – over 2600 interviews, stories and photodocumentations have been collected since the museum opened.\n\nThe museum is also a support for the country's approximately 1,500 working life museums that are old workplaces preserved to convey their history.\n\nExhibitions \nThe Museum of Work shows exhibitions going on over several years, but also shorter exhibitions – including several photo exhibitions on themes that can be linked to work and everyday life.\n\nThe history of Alva \nThe history of Alva Karlsson is the only exhibition in the museum that is permanent. The exhibition connects to the museum's building and its history as part of the textile industry in Norrköping. Alva worked as a rollers between the years 1927 – 1962.\n\nIndustriland \nOne of the museum long-term exhibitions is Industriland – when Sweden became modern, the exhibition was in 2007–2013 and consisted of an ongoing bond with various objects that were somehow significant both for working life and everyday during the period 1930–1980. The exhibition also consisted of presentations of the working life museums in Sweden and a number of rooms with themes such as: leisure, world, living and consumption.\n\nFramtidsland (Future country) \nIn 2014, the exhibition was inaugurated that takes by where Industriland ends: Future country. It is an exhibition that investigates what a sustainable society is will be part of the museum's exhibitions until 2019. The exhibition consists of materials that are designed based on conversations between young people and researchers around Sweden. The exhibition addresses themes such as work, environment and everyday life. A tour version of the exhibition is given in the locations Falun, Kristianstad and Örebro.\n\nEWK – The Center for Political Illustration Art \nSince 2009, the Museum also houses EWK – center for political illustration art. The museum preserves, develops and conveys the political illustrator Ewert Karlsson's production. The museum also holds theme exhibitions with national and international political illustrators with the aim of highlighting and strengthening the political art.\n\nSee also\n List of museums in Sweden\n Culture of Sweden\n\nReferences\n\nExternal links\n  Arbetetsmuseum Official site\n\nMuseums in Östergötland County\nBuildings and structures in Norrköping\nIndustry museums in Sweden\nCultural heritage of Sweden"
}

```

This year, the corpus is also available via <a href="https://huggingface.co/datasets/wikimedia/wikipedia" target="_blank">Hugging Face Datasets</a> and <a href="https://github.com/mam10eks/ir_datasets/tree/trec-tot-2025" target="_blank">IR-Dataset</a>.

#### Access corpora through IR-Datasets

Please install the ir_datasets datasets package `pip install ir_datasets`, and then run the following sample code.

```python
import ir_datasets
dataset = ir_datasets.load("trec-tot/2025/train")

for query in dataset.queries_iter():
    print(query)
    break

for doc in dataset.docs_iter():
    print(doc.doc_id)
    break
```

#### Access corpora through Hugging Face

Please install the Hugging Face datasets package using `pip install datasets`, and then run the following sample code.

```python
from datasets import load_dataset

ds = load_dataset("wikimedia/wikipedia", "20231101.en")
print(ds['train'][134])
```

### Queries

Participating groups will be given a JSONL file consisting of a random sample of queries (or topics in TREC lingo) each for training, three development sets, and the test set. There are three development sets, dev1 (TREC 2023 dev set), dev2 (TREC 2023 test set), and dev3 (TREC 2024 test set).
As noted earlier, some queries in these dev sets may be missing compared to their source dev / test sets from previous years because the corresponding relevant documents are not in our corpus this year.
The query format for this year has two fields: **query_id** and **query**. An example query is described below.

```json
{
  "query_id": "763",
  "query": "Super Rare Surreal Dystopian Masterpiece .\n Very rare movie that is scifi/dystopian/experimental/surreal. It\u2019s like Stalker meets el Topo meets Holy Mountain meets Alphaville meets Delicatessen meets Hard to be a God, like Kurosawa, Tarkovsky, and Lynch had a kid together. It was color, possibly Russian, and I don\u2019t really remember the decade but want to say 60s or 70s, though could easily be more recent. It is VERY rare, there is only one crappy partial print of it, and that is what the youtube version is from. Lot of wide shots in a surreal wilderness, winter settings, strange bleeding saturation in some shots. Crazy costumes. Seriously one of the strangest films I\u2019ve ever seen and my favorite films are strange/weird ones. If you\u2019ve ever seen what you\u2019re thinking of on a \u201cbest weird movies\u201d or \u201cyou\u2019ve never seen this!\u201d list, that\u2019s NOT it. I don\u2019t think this film even has a cult following of ten people. It\u2019s an actual rare gem. Have been looking through selections at 366 Weird Movies and not found it yet (btw the way most of those titles are exactly the kind of not-actually-rare movies this film is definitely not)."
}

```

### Use of external information

You are generally **PERMITTED** to use external information while developing your runs. When you submit your runs, please fill in a form listing what resources you used.
This could include an external corpus such as TMDB or a pretrained model (e.g. word embeddings, BERT). Additionally,

Participating groups are **PROHIBITED** from using any data from the following websites that are not already included in the train/dev sets described above. **If you do so, you will be training and/or hyperparameter tuning using test data.**
- <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification</a>
- <a href="https://irememberthismovie.com/" target="_blank">iRememberThisMovie.com</a>

### Training data documentation and declaration
 
The emerging practices of pretraining large language models on web-scale datasets and community sharing of these pretrained models for downstream training / model development creates challenges in tracking what training data have been used for particular run submissions. Given that some of our test queries are sampled from the <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">MS-ToT dataset</a> that has been publicly available online since 2021, there is a possibility that this data may have been used for training models that are employed in submitted runs. To ensure that we get robust scientific conclusions and insights from this year's track, we are requesting all participants to document *all* training data that were employed in the preparation of a given run to the best of your knowledge (including any data used for pretraining models that you are building on top of). In addition, we are also requesting participants to declare if they are 100% confident that no data from <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification</a> or <a href="https://irememberthismovie.com/" target="_blank">iRememberThisMovie.com</a> was used for training. We recognize that not all pretrained models publicly document their training data in details. So, please mark that declaration to be true *only* if you are 100% confident about all training data used in preparation of your run and can guarantee that it does not include any data from the sources mentioned. We do not discourage submissions where there may be some uncertainty about the training data but we want to be aware of this at the time of analyzing the evaluated results.

## Baselines

We have prepared a set of baselines implemented against ir_datasets here: [https://github.com/TREC-ToT/bench](https://github.com/TREC-ToT/bench).
The run files are [publicly available](https://github.com/TREC-ToT/bench/tree/main/trec25) so that you can directly re-use them.
The indices of our baselines are publicly available as well, so that you can easily modify the baselines for your own submission here: [https://github.com/TREC-ToT/bench/tree/main/trec25#prepared-indices](https://github.com/TREC-ToT/bench/tree/main/trec25#prepared-indices).
    
## Submission and evaluation

**Submission form:** Coming soon! (You must <a href="#registration">register as a participant</a> to submit a run).

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

