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

In terms of input and output, the movie identification task is relatively straightforward—given an input TOT request, output a ranked list of movies.  Each movie must be identified by its Wikipedia page id and the correct movie should be ranked as high as possible.  For each query, runs should return a ranked list of 1000 Wikipedia page ids.  Runs will be evaluated using IR metrics that are appropriate for IR tasks with one relevant document, such as discounted cumulative gain, reciprocal rank, and success@k.

## Datasets

Participating groups will be given the following resources for training, development, and testing.

### Corpora

**Wikipedia Corpus:** Subset of Wikipedia pages directly or indirectly associated with (relevant sub-classes of) the "audiovisual works" category (231,852 pages). This corpus contains the relevant movie for all TOT queries in the training, development, and test sets described below. An [example document](#example-document) is described below. Each page also has the following fields associated with it, which participants can utilize, say, to augment data: 
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

Queries (or topics in TREC lingo) are sourced from two distinct sources: the <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">MS-TOT dataset</a> and the <a href="https://dl.acm.org/doi/abs/10.1145/3488560.3498421" target="_blank">Reddit-TOMT dataset</a>. Participating groups will be given a JSONL file consisting of a mixture of MS-TOT and Reddit-TOMT queries for training, development, and test. An [example query](#example-query) is described below. For the MS-TOT subset, sentence-level annotations will also be distributed. Participating groups are encouraged to leverage these codes however they want. This might include treating sentences associated with specific codes differently (e.g., ignoring or down-weighing sentences that convey uncertainty). Note that due to data sharing limitations, the title and text of the Reddit-TOMT subset will not be distributed. Participants can download the title and text using a script distributed with the data.

[//]: # (Participants will be provided the following query sets as part of this year's track.)
[//]: # (- Train: 150 MS-TOT queries and 9000 Reddit-TOMT queries.)
[//]: # (- Dev: 150 MS-TOT queries and 1000 Reddit-TOMT queries.)
[//]: # (- Test: 150 MS-TOT queries and 1000 Reddit-TOMT queries.)

### Use of external information

You are generally allowed to use external information while developing your runs. When you submit your runs, please fill in a form listing what resources you used.
This could include an external corpus such as TMDB or a pretrained model (e.g. word embeddings, BERT). Additionally,
- Participating groups  are **PERMITTED** and encouraged to gather movie data from multiple sources. We are distributing identifiers for several data sources i.e., Wikipedia, Wikidata and IMDb (if available). Groups are encouraged to explore other resources on their own. Pages from different sources can often be resolved using the IMDB ID (e.g., Wikipedia pages often contain the movie’s IMDB ID).
- Participating groups are **PERMITTED** and encouraged to leverage the qualitative codes associated with the MS-TOT train, dev, and test sets. When submitting a run, groups may be asked to explain whether and how the qualitative codes were used during training and/or testing.
- Participating groups are **PROHIBITED** from using any data from the following websites that are not already included in the train/dev sets described above. **If you do so, you will be training and/or hyperparameter tuning using test data.**
    - <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification</a>
    - <a href="https://irememberthismovie.com/" target="_blank">iRememberThisMovie.com</a>
  

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

The main type of TREC submission is *automatic*, which means there was not manual intervention in running the test queries. This means you should not adjust your runs, rewrite the query, retrain your model, or make any other sorts of manual adjustments after you see the test queries. The ideal case is that you only look at the test queries to check that they ran properly (i.e. no bugs) then you submit your automatic runs. However, if you want to have a human in the loop for your run, or do anything else that uses the test queries to adjust your model or ranking, you can mark your run as *manual*. Manual runs are interesting, and we may learn a lot, but these are distinct from our main scenario which is a system that responds to unseen queries automatically.

Runs will be evaluated using metrics appropriate for retrieval scenarios with one relevant document. In particular, **our primary evaluation metric for this year's track will be discounted cumulative gain (DCG)** but we may also compute other metrics such as reciprocal rank (RR) and success@k.

## Examples

### Example Document

<details><summary>Document</summary>
```json
{
  "page_title": "Actrius",
  "page_source": "{{Use dmy dates|date=March 2021}}\n{{Infobox film\n| name           = Actresses\n| image          = Actrius film poster.jpg\n| alt            = \n| caption        = Catalan language film poster\n| native_name      = ([[Catalan language|Catalan]]: '''''Actrius''''')\n| director       = [[Ventura Pons]]\n| producer       = Ventura Pons\n| writer         = [[Josep Maria Benet i Jornet]]\n| screenplay     = Ventura Pons\n| story          = \n| based_on       = {{based on|(stage play) ''E.R.''|Josep Maria Benet i Jornet}}\n| starring       = {{ubl|[[N\u00faria Espert]]|[[Rosa Maria Sard\u00e0]]|[[Anna Lizaran]]|[[Merc\u00e8 Pons]]}}\n| narrator       = <!-- or: |narrators = -->\n| music          = Carles Cases\n| cinematography = Tom\u00e0s Pladevall\n| editing        = Pere Abadal\n| production_companies = {{ubl|[[Canal+|Canal+ Espa\u00f1a]]|Els Films de la Rambla S.A.|[[Generalitat de Catalunya|Generalitat de Catalunya - Departament de Cultura]]|[[Televisi\u00f3n Espa\u00f1ola]]}}\n| distributor    = [[Buena Vista International]]\n| released       = {{film date|df=yes|1997|1|17|[[Spain]]}}\n| runtime        = 100 minutes\n| country        = Spain\n| language       = Catalan\n| budget         = \n| gross          = <!--(please use condensed and rounded values, e.g. \"\u00a311.6 million\" not \"\u00a311,586,221\")-->\n}}\n\n'''''Actresses''''' ([[Catalan language|Catalan]]: '''''Actrius''''') is a 1997 [[Catalan language]] Spanish drama film produced and directed by [[Ventura Pons]] and based on the award-winning stage play ''E.R.'' by [[Josep Maria Benet i Jornet]]. The film has no male actors, with all roles played by females.<ref name=\"El Pais\">{{cite news|last1=Torres|first1=Rosanna|title='E. R', de Benet i Jornet, es llevada al cine y al teatro|url=http://elpais.com/diario/1996/10/15/cultura/845330405_850215.html|access-date=2015-12-21|lang=es|work=[[El Pa\u00eds]]|date=1996-10-15|archive-url=https://web.archive.org/web/20121014165502/http://elpais.com/diario/1996/10/15/cultura/845330405_850215.html|archive-date=2012-10-14}}</ref> The film was produced in 1996. <ref>{{cite web |title=Actrius |url=https://www.bcncatfilmcommission.com/en/films/actrius|lang=en|website=Barcelona Film Commission |access-date=2021-05-13|archive-url=https://web.archive.org/web/20210513150105/https://www.bcncatfilmcommission.com/en/films/actrius|archive-date=2021-05-13|url-status=live}}</ref>\n\n== Synopsis ==\nIn order to prepare herself to play a role commemorating the life of legendary actress Empar Ribera, young actress ([[Merc\u00e8 Pons]]) interviews three established actresses who had been the Ribera's pupils: the international diva Gl\u00f2ria Marc ([[N\u00faria Espert]]), the television star Assumpta Roca ([[Rosa Maria Sard\u00e0]]), and dubbing director Maria Caminal ([[Anna Lizaran]]).<ref name=SFF>{{cite web | url=http://www.stockholmfilmfestival.se/en/festival/1997/film/actrius | title=Actrius |lang=en|publisher=[[Stockholm International Film Festival]] |date=1997 | access-date=2015-12-21|archive-url=https://web.archive.org/web/20151222175333/http://www.stockholmfilmfestival.se/en/festival/1997/film/actrius|archive-date=2015-12-22|url-status=dead }}</ref>\n\n== Cast ==\n* [[N\u00faria Espert]] as Gl\u00f2ria Marc\n* [[Rosa Maria Sard\u00e0]] as Assumpta Roca\n* [[Anna Lizaran]] as Maria Caminal\n* [[Merc\u00e8 Pons]] as Estudiant\n\n== Recognition ==\n\n=== Screenings ===\n''Actrius'' screened in 2001 at the [[Grauman's Egyptian Theatre]] in an [[American Cinematheque]] retrospective of the works of its director. The film had first screened at the same location in 1998.<ref name=\"LA Times\">{{cite news|last1=THomas|first1=Kevin|title=Sometimes, the World Gets in the Way|url=http://articles.latimes.com/2001/mar/01/entertainment/ca-31570|lang=en|access-date=2015-12-21|newspaper=[[Los Angeles Times]]|date=2001-03-01|archive-url=https://web.archive.org/web/20121221084433/http://articles.latimes.com/2001/mar/01/entertainment/ca-31570|archive-date=2012-12-21|url-status=live}}</ref> It was also shown at the 1997 [[Stockholm International Film Festival]].<ref name=SFF />\n\n=== Reception ===\nIn ''Movie - Film - Review'', Christopher Tookey wrote that though the actresses were \"competent in roles that may have some reference to their own careers\", the film \"is visually unimaginative, never escapes its stage origins, and is almost totally lacking in revelation or surprising incident\".<ref name=Tookey/> Noting that there were \"occasional, refreshing moments of intergenerational bitchiness\", they did not \"justify comparisons to ''[[All About Eve]]''\", and were \"insufficiently different to deserve critical parallels with ''[[Rashomon]]''\".<ref name=Tookey/> He also wrote that ''[[The Guardian]]'' called the film a \"slow, stuffy chamber-piece\", and that ''[[The Evening Standard]]'' stated the film's \"best moments exhibit the bitchy tantrums seething beneath the threesome's composed veneers\".<ref name=Tookey>{{cite web|last1=Tookey|first1=Chris|title=review: Actresses / Actrius / Actrices|url=http://www.movie-film-review.com/devFilm.asp?ID=12423|lang=en|publisher=Movie - Film - Review|access-date=2015-12-21|archive-url=https://web.archive.org/web/20120501211137/http://www.movie-film-review.com/devFilm.asp?ID=12423|archive-date=2012-05-01|url-status=dead}}</ref> [[MRQE]] wrote \"This cinematic adaptation of a theatrical work is true to the original, but does not stray far from a theatrical rendering of the story.\"<ref name=MRQE>{{cite web|last1=staff|title=Actrius (1997)|url=http://www.mrqe.com/movie_reviews/actrius-m100030469|lang=en|publisher=[[MRQE]]|access-date=2015-12-21|archive-url=https://web.archive.org/web/20151222150358/http://www.mrqe.com/movie_reviews/actrius-m100030469|archive-date=2015-12-22}}</ref>\n\n=== Awards and nominations ===\n* 1997, won 'Best Catalan Film' at [[Butaca Awards]] for [[Ventura Pons]]\n* 1997, won 'Best Catalan Film Actress' at Butaca Awards, shared by [[N\u00faria Espert]], [[Rosa Maria Sard\u00e0]], [[Anna Lizaran]], and [[Merc\u00e8 Pons]]\n* 1998, nominated for 'Best Screenplay' at [[Goya Awards]], shared by [[Josep Maria Benet i Jornet]] and Ventura Pons\n\n== References ==\n{{reflist}}\n\n== External links ==\n* {{IMDb title|0115462|Actresses}}\n* {{official website|https://web.archive.org/web/20090217140746/http://venturapons.com/filmografia/actrices.html}} [[Wayback Machine|as archived 17 February 2009]] (Spanish)\n\n[[Category:1997 films]]\n[[Category:1997 drama films]]\n[[Category:Catalan-language films]]\n[[Category:Films set in Barcelona]]\n[[Category:Films directed by Ventura Pons]]\n[[Category:Spanish drama films]]\n[[Category:1990s Spanish films]]",
  "wikidata_id": "Q2823770",
  "wikidata_classes": [
    [
      "Q11424",
      "film"
    ]
  ],
  "text": "Actresses (Catalan: Actrius) is a 1997 Catalan language Spanish drama film produced and directed by Ventura Pons and based on the award-winning stage play \"E.R.\" by Josep Maria Benet i Jornet. The film has no male actors, with all roles played by females. The film was produced in 1996. \nSynopsis.\nIn order to prepare herself to play a role commemorating the life of legendary actress Empar Ribera, young actress (Merc\u00e8 Pons) interviews three established actresses who had been the Ribera's pupils: the international diva Gl\u00f2ria Marc (N\u00faria Espert), the television star Assumpta Roca (Rosa Maria Sard\u00e0), and dubbing director Maria Caminal (Anna Lizaran).\nRecognition.\nScreenings.\n\"Actrius\" screened in 2001 at the Grauman's Egyptian Theatre in an American Cinematheque retrospective of the works of its director. The film had first screened at the same location in 1998. It was also shown at the 1997 Stockholm International Film Festival.\nReception.\nIn \"Movie - Film - Review\", Christopher Tookey wrote that though the actresses were \"competent in roles that may have some reference to their own careers\", the film \"is visually unimaginative, never escapes its stage origins, and is almost totally lacking in revelation or surprising incident\". Noting that there were \"occasional, refreshing moments of intergenerational bitchiness\", they did not \"justify comparisons to \"All About Eve\"\", and were \"insufficiently different to deserve critical parallels with \"Rashomon\"\". He also wrote that \"The Guardian\" called the film a \"slow, stuffy chamber-piece\", and that \"The Evening Standard\" stated the film's \"best moments exhibit the bitchy tantrums seething beneath the threesome's composed veneers\". MRQE wrote \"This cinematic adaptation of a theatrical work is true to the original, but does not stray far from a theatrical rendering of the story.\"",
  "sections": {
    "abstract": "Actresses (Catalan: Actrius) is a 1997 Catalan language Spanish drama film produced and directed by Ventura Pons and based on the award-winning stage play \"E.R.\" by Josep Maria Benet i Jornet. The film has no male actors, with all roles played by females. The film was produced in 1996. ",
    "synopsis": "Synopsis.\nIn order to prepare herself to play a role commemorating the life of legendary actress Empar Ribera, young actress (Merc\u00e8 Pons) interviews three established actresses who had been the Ribera's pupils: the international diva Gl\u00f2ria Marc (N\u00faria Espert), the television star Assumpta Roca (Rosa Maria Sard\u00e0), and dubbing director Maria Caminal (Anna Lizaran).",
    "recognition": "Recognition.\nScreenings.\n\"Actrius\" screened in 2001 at the Grauman's Egyptian Theatre in an American Cinematheque retrospective of the works of its director. The film had first screened at the same location in 1998. It was also shown at the 1997 Stockholm International Film Festival.\nReception.\nIn \"Movie - Film - Review\", Christopher Tookey wrote that though the actresses were \"competent in roles that may have some reference to their own careers\", the film \"is visually unimaginative, never escapes its stage origins, and is almost totally lacking in revelation or surprising incident\". Noting that there were \"occasional, refreshing moments of intergenerational bitchiness\", they did not \"justify comparisons to \"All About Eve\"\", and were \"insufficiently different to deserve critical parallels with \"Rashomon\"\". He also wrote that \"The Guardian\" called the film a \"slow, stuffy chamber-piece\", and that \"The Evening Standard\" stated the film's \"best moments exhibit the bitchy tantrums seething beneath the threesome's composed veneers\". MRQE wrote \"This cinematic adaptation of a theatrical work is true to the original, but does not stray far from a theatrical rendering of the story.\"",
    "screenings": "Screenings.\n\"Actrius\" screened in 2001 at the Grauman's Egyptian Theatre in an American Cinematheque retrospective of the works of its director. The film had first screened at the same location in 1998. It was also shown at the 1997 Stockholm International Film Festival.",
    "reception": "Reception.\nIn \"Movie - Film - Review\", Christopher Tookey wrote that though the actresses were \"competent in roles that may have some reference to their own careers\", the film \"is visually unimaginative, never escapes its stage origins, and is almost totally lacking in revelation or surprising incident\". Noting that there were \"occasional, refreshing moments of intergenerational bitchiness\", they did not \"justify comparisons to \"All About Eve\"\", and were \"insufficiently different to deserve critical parallels with \"Rashomon\"\". He also wrote that \"The Guardian\" called the film a \"slow, stuffy chamber-piece\", and that \"The Evening Standard\" stated the film's \"best moments exhibit the bitchy tantrums seething beneath the threesome's composed veneers\". MRQE wrote \"This cinematic adaptation of a theatrical work is true to the original, but does not stray far from a theatrical rendering of the story.\""
  },
  "infoboxes": [
    {
      "name": "film",
      "params": {
        "name": "Actresses",
        "image": "Actrius film poster.jpg",
        "alt": "",
        "caption": "Catalan language film poster",
        "native_name": "([[Catalan language|Catalan]]: '''''Actrius''''')",
        "director": "[[Ventura Pons]]",
        "producer": "Ventura Pons",
        "writer": "[[Josep Maria Benet i Jornet]]",
        "screenplay": "Ventura Pons",
        "story": "",
        "based_on": "{{based on|(stage play) ''E.R.''|Josep Maria Benet i Jornet}}",
        "starring": "{{ubl|[[N\u00faria Espert]]|[[Rosa Maria Sard\u00e0]]|[[Anna Lizaran]]|[[Merc\u00e8 Pons]]}}",
        "narrator": "<!-- or: |narrators = -->",
        "music": "Carles Cases",
        "cinematography": "Tom\u00e0s Pladevall",
        "editing": "Pere Abadal",
        "production_companies": "{{ubl|[[Canal+|Canal+ Espa\u00f1a]]|Els Films de la Rambla S.A.|[[Generalitat de Catalunya|Generalitat de Catalunya - Departament de Cultura]]|[[Televisi\u00f3n Espa\u00f1ola]]}}",
        "distributor": "[[Buena Vista International]]",
        "released": "{{film date|df=yes|1997|1|17|[[Spain]]}}",
        "runtime": "100 minutes",
        "country": "Spain",
        "language": "Catalan",
        "budget": "",
        "gross": "<!--(please use condensed and rounded values, e.g. \"\u00a311.6 million\" not \"\u00a311,586,221\")-->"
      }
    }
  ],
  "doc_id": "330"
}
```
</details>

### Example query

**Note:**  `sentence_annotations` is `null` for queries with `source:'R-TOMT'`

<details><summary>Query</summary>
```json
{
  "id": "763",
  "url": "https://irememberthismovie.com/super-rare-surreal-dystopian-masterpiece/",
  "domain": "movie",
  "source": "MS-TOT",
  "title": "Super Rare Surreal Dystopian Masterpiece",
  "text": "Very rare movie that is scifi/dystopian/experimental/surreal. It\u2019s like Stalker meets el Topo meets Holy Mountain meets Alphaville meets Delicatessen meets Hard to be a God, like Kurosawa, Tarkovsky, and Lynch had a kid together. It was color, possibly Russian, and I don\u2019t really remember the decade but want to say 60s or 70s, though could easily be more recent. It is VERY rare, there is only one crappy partial print of it, and that is what the youtube version is from. Lot of wide shots in a surreal wilderness, winter settings, strange bleeding saturation in some shots. Crazy costumes. Seriously one of the strangest films I\u2019ve ever seen and my favorite films are strange/weird ones. If you\u2019ve ever seen what you\u2019re thinking of on a \u201cbest weird movies\u201d or \u201cyou\u2019ve never seen this!\u201d list, that\u2019s NOT it. I don\u2019t think this film even has a cult following of ten people. It\u2019s an actual rare gem. Have been looking through selections at 366 Weird Movies and not found it yet (btw the way most of those titles are exactly the kind of not-actually-rare movies this film is definitely not).",
  "wikipedia_id": "16742289",
  "wikipedia_url": "https://en.wikipedia.org/wiki/On_the_Silver_Globe_(film)",
  "wikidata_id": "Q1988165",
  "imdb_url": "https://www.imdb.com/title/tt0093593",
  "sentence_annotations": [
    {
      "id": 1,
      "text": "Very rare movie that is scifi/dystopian/experimental/surreal.",
      "labels": {
        "opinion": false,
        "emotion": false,
        "hedging": true,
        "social": false,
        "comparison_relative": false,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": true,
          "release_date": false,
          "location_type": false,
          "origin_language": false,
          "origin_movie": false,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": true,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 2,
      "text": "It\u2019s like Stalker meets el Topo meets Holy Mountain meets Alphaville meets Delicatessen meets Hard to be a God, like Kurosawa, Tarkovsky, and Lynch had a kid together.",
      "labels": {
        "opinion": false,
        "emotion": false,
        "hedging": false,
        "social": false,
        "comparison_relative": true,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": false,
          "release_date": false,
          "location_type": false,
          "origin_language": false,
          "origin_movie": false,
          "person_real": true,
          "plot": false,
          "quote": false,
          "category": false,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 3,
      "text": "It was color, possibly Russian, and I don\u2019t really remember the decade but want to say 60s or 70s, though could easily be more recent.",
      "labels": {
        "opinion": false,
        "emotion": false,
        "hedging": true,
        "social": false,
        "comparison_relative": false,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": true,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": false,
          "release_date": true,
          "location_type": false,
          "origin_language": false,
          "origin_movie": true,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": false,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 4,
      "text": "It is VERY rare, there is only one crappy partial print of it, and that is what the youtube version is from.",
      "labels": {
        "opinion": false,
        "emotion": false,
        "hedging": false,
        "social": false,
        "comparison_relative": false,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": false,
          "release_date": false,
          "location_type": false,
          "origin_language": false,
          "origin_movie": false,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": false,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 5,
      "text": "Lot of wide shots in a surreal wilderness, winter settings, strange bleeding saturation in some shots.",
      "labels": {
        "opinion": false,
        "emotion": false,
        "hedging": false,
        "social": false,
        "comparison_relative": false,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": true,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": false,
          "release_date": false,
          "location_type": true,
          "origin_language": false,
          "origin_movie": false,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": false,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 6,
      "text": "Crazy costumes.",
      "labels": {
        "opinion": false,
        "emotion": false,
        "hedging": false,
        "social": false,
        "comparison_relative": false,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": true,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": false,
          "release_date": false,
          "location_type": false,
          "origin_language": false,
          "origin_movie": false,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": false,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 7,
      "text": "Seriously one of the strangest films I\u2019ve ever seen and my favorite films are strange/weird ones.",
      "labels": {
        "opinion": true,
        "emotion": false,
        "hedging": false,
        "social": false,
        "comparison_relative": false,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": true,
          "release_date": false,
          "location_type": false,
          "origin_language": false,
          "origin_movie": false,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": true,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 8,
      "text": "If you\u2019ve ever seen what you\u2019re thinking of on a \u201cbest weird movies\u201d or \u201cyou\u2019ve never seen this!\u201d",
      "labels": {
        "opinion": false,
        "emotion": false,
        "hedging": false,
        "social": true,
        "comparison_relative": false,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": false,
          "release_date": false,
          "location_type": false,
          "origin_language": false,
          "origin_movie": false,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": false,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 9,
      "text": "list, that\u2019s NOT it.",
      "labels": {
        "opinion": false,
        "emotion": false,
        "hedging": false,
        "social": false,
        "comparison_relative": false,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": false,
          "release_date": false,
          "location_type": false,
          "origin_language": false,
          "origin_movie": false,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": false,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 10,
      "text": "I don\u2019t think this film even has a cult following of ten people.",
      "labels": {
        "opinion": true,
        "emotion": false,
        "hedging": true,
        "social": false,
        "comparison_relative": false,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": false,
          "release_date": false,
          "location_type": false,
          "origin_language": false,
          "origin_movie": false,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": true,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 11,
      "text": "It\u2019s an actual rare gem.",
      "labels": {
        "opinion": false,
        "emotion": false,
        "hedging": false,
        "social": false,
        "comparison_relative": false,
        "search": false,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": false,
          "release_date": false,
          "location_type": false,
          "origin_language": false,
          "origin_movie": false,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": false,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    },
    {
      "id": 12,
      "text": "Have been looking through selections at 366 Weird Movies and not found it yet (btw the way most of those titles are exactly the kind of not-actually-rare movies this film is definitely not).",
      "labels": {
        "opinion": false,
        "emotion": false,
        "hedging": false,
        "social": false,
        "comparison_relative": false,
        "search": true,
        "movie": {
          "music_compare": false,
          "genre_audience": false,
          "production_camera_angle": false,
          "timeframe_singular": false,
          "origin_actor": false,
          "object": false,
          "location_specific": false,
          "person_fictional": false,
          "production_visual": false,
          "scene": false,
          "negation": false,
          "production_audio": false,
          "character": false,
          "genre_traditional_tone": false,
          "release_date": false,
          "location_type": false,
          "origin_language": false,
          "origin_movie": false,
          "person_real": false,
          "plot": false,
          "quote": false,
          "category": false,
          "music_specific": false,
          "timeframe_plural": false
        },
        "context": {
          "cross_media": false,
          "physical_medium": false,
          "situational_count": false,
          "physical_user_location": false,
          "situational_evidence": false,
          "situational_witness": false,
          "temporal": false
        }
      }
    }
  ]
}
```
</details>