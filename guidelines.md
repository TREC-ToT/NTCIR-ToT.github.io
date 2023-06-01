---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

# Guidelines

## Important dates
* **May 18th:** Release corpus and training queries
* **July 24th:** Release test queries
* **August 31st:** Deadline for submitting runs

## Registration

To participate in TREC please pre-register at the following website: <a href="https://ir.nist.gov/trecsubmit.open/application.html" target="_blank">https://ir.nist.gov/trecsubmit.open/application.html</a>.

## Task definition

In terms of input and output, the movie identification task is relatively straightforward—given an input TOT request, output a ranked list of movies.  Each movie must be identified by its Wikipedia page id and the correct movie should be ranked as high as possible.  For each query, runs should return a ranked list of 1000 Wikipedia page ids.  Runs will be evaluated using IR metrics that are appropriate for IR tasks with one relevant document, such as discounted cumulative gain, reciprocal rank, and success@k.

## Datasets

Data can be downloaded using the links below. Participants can download the files in a single zip file (TREC-ToT.zip) or download each file (corpus.jsonl.zip, train.zip, dev.zip) separately. See [Corpora](#corpora) and [Queries](#queries) for a description of the files (train/dev folders also include qrel files in the TREC format). 

| Description                                   | Link             | MD5 Hash |
|-----------------------------------------------|------------------|----------|
| corpus, train and dev set (single zip) | [TREC-ToT.zip](https://surfdrive.surf.nl/files/index.php/s/FaEK4xc6Xp2JcAJ/download)     |   `f84fe82cb80e3ee1072576c8d6c4a417`       |
| corpus (JSONL)                           | [corpus.jsonl.zip](https://surfdrive.surf.nl/files/index.php/s/sWLysSXtFRLz8R1/download) |  `3bdf2941d256c88d1fbfd9dbc483f43d`        |
| train queries (JSONL) & qrel.txt                      | [train.zip](https://surfdrive.surf.nl/files/index.php/s/KJFwx32qMT7Tinh/download)        |  `f5e7e07e409f214b5a67af7e23b231c7`        |
| dev queries (JSONL) & qrel.txt                        | [dev.zip](https://surfdrive.surf.nl/files/index.php/s/F1mUvAW06gn7OdP/download)          |  `40ef6eace079a40f23237005db1b0f1a` |
| test queries & qrel.txt                       | Release: July                | -        |



### Corpora

**Wikipedia Corpus:** Subset of Wikipedia pages directly or indirectly associated with (relevant sub-classes of) the "audiovisual works" category (231,852 pages). This corpus contains the relevant movie for all TOT queries in the training, development, and test sets described below. Each page also has the following fields associated with it, which participants can utilize, say, to augment data: 
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

An example document is described below. 

#### Example Document

```json
{
   "page_title":"Actrius",
   "page_source": "{% raw %}{{Use dmy dates|date=March 2021}}\n{{Infobox film\n| name           = Actresses\n| image          = Actrius film poster.jpg\n| alt            = \n| caption        = Catalan language film poster\n| native_name      = ([[Catalan language|Catalan]]: '''''Actrius''''')\n| director       = [[Ventura Pons]]\n| producer       = Ventura Pons\n| writer         = [[Josep Maria Benet i Jornet]]\n| screenplay     = Ventura Pons\n| story          = \n| based_on       = {{based on|(stage play) ''E.R.''|Josep Maria Benet i Jornet}}\n| starring       = {{ubl|[[N\u00faria Espert]]|[[Rosa Maria Sard\u00e0]]|[[Anna Lizaran]]|[[Merc\u00e8 Pons]]}}\n| narrator       = <!-- or: |narrators = -->\n| music          = Carles Cases\n| cinematography = Tom\u00e0s Pladevall\n| editing        = Pere Abadal\n| production_companies = {{ubl|[[Canal+|Canal+ Espa\u00f1a]]|Els Films de la Rambla S.A.|[[Generalitat de Catalunya|Generalitat de Catalunya - Departament de Cultura]]|[[Televisi\u00f3n Espa\u00f1ola]]}}\n| distributor    = [[Buena Vista International]]\n| released       = {{film date|df=yes|1997|1|17|[[Spain]]}}\n| runtime        = 100 minutes\n| country        = Spain\n| language       = Catalan\n| budget         = \n| gross          = <!--(please use condensed and rounded values, e.g. \"\u00a311.6 million\" not \"\u00a311,586,221\")-->\n}}\n\n'''''Actresses''''' ([[Catalan language|Catalan]]: '''''Actrius''''') is a 1997 [[Catalan language]] Spanish drama film produced and directed by [[Ventura Pons]] and based on the award-winning stage play ''E.R.'' by [[Josep Maria Benet i Jornet]]. The film has no male actors, with all roles played by females.<ref name=\"El Pais\">{{cite news|last1=Torres|first1=Rosanna|title='E. R', de Benet i Jornet, es llevada al cine y al teatro|url=http://elpais.com/diario/1996/10/15/cultura/845330405_850215.html|access-date=2015-12-21|lang=es|work=[[El Pa\u00eds]]|date=1996-10-15|archive-url=https://web.archive.org/web/20121014165502/http://elpais.com/diario/1996/10/15/cultura/845330405_850215.html|archive-date=2012-10-14}}</ref> The film was produced in 1996. <ref>{{cite web |title=Actrius |url=https://www.bcncatfilmcommission.com/en/films/actrius|lang=en|website=Barcelona Film Commission |access-date=2021-05-13|archive-url=https://web.archive.org/web/20210513150105/https://www.bcncatfilmcommission.com/en/films/actrius|archive-date=2021-05-13|url-status=live}}</ref>\n\n== Synopsis ==\nIn order to prepare herself to play a role commemorating the life of legendary actress Empar Ribera, young actress ([[Merc\u00e8 Pons]]) interviews three established actresses who had been the Ribera's pupils: the international diva Gl\u00f2ria Marc ([[N\u00faria Espert]]), the television star Assumpta Roca ([[Rosa Maria Sard\u00e0]]), and dubbing director Maria Caminal ([[Anna Lizaran]]).<ref name=SFF>{{cite web | url=http://www.stockholmfilmfestival.se/en/festival/1997/film/actrius | title=Actrius |lang=en|publisher=[[Stockholm International Film Festival]] |date=1997 | access-date=2015-12-21|archive-url=https://web.archive.org/web/20151222175333/http://www.stockholmfilmfestival.se/en/festival/1997/film/actrius|archive-date=2015-12-22|url-status=dead }}</ref>\n\n== Cast ==\n* [[N\u00faria Espert]] as Gl\u00f2ria Marc\n* [[Rosa Maria Sard\u00e0]] as Assumpta Roca\n* [[Anna Lizaran]] as Maria Caminal\n* [[Merc\u00e8 Pons]] as Estudiant\n\n== Recognition ==\n\n=== Screenings ===\n''Actrius'' screened in 2001 at the [[Grauman's Egyptian Theatre]] in an [[American Cinematheque]] retrospective of the works of its director. The film had first screened at the same location in 1998.<ref name=\"LA Times\">{{cite news|last1=THomas|first1=Kevin|title=Sometimes, the World Gets in the Way|url=http://articles.latimes.com/2001/mar/01/entertainment/ca-31570|lang=en|access-date=2015-12-21|newspaper=[[Los Angeles Times]]|date=2001-03-01|archive-url=https://web.archive.org/web/20121221084433/http://articles.latimes.com/2001/mar/01/entertainment/ca-31570|archive-date=2012-12-21|url-status=live}}</ref> It was also shown at the 1997 [[Stockholm International Film Festival]].<ref name=SFF />\n\n=== Reception ===\nIn ''Movie - Film - Review'', Christopher Tookey wrote that though the actresses were \"competent in roles that may have some reference to their own careers\", the film \"is visually unimaginative, never escapes its stage origins, and is almost totally lacking in revelation or surprising incident\".<ref name=Tookey/> Noting that there were \"occasional, refreshing moments of intergenerational bitchiness\", they did not \"justify comparisons to ''[[All About Eve]]''\", and were \"insufficiently different to deserve critical parallels with ''[[Rashomon]]''\".<ref name=Tookey/> He also wrote that ''[[The Guardian]]'' called the film a \"slow, stuffy chamber-piece\", and that ''[[The Evening Standard]]'' stated the film's \"best moments exhibit the bitchy tantrums seething beneath the threesome's composed veneers\".<ref name=Tookey>{{cite web|last1=Tookey|first1=Chris|title=review: Actresses / Actrius / Actrices|url=http://www.movie-film-review.com/devFilm.asp?ID=12423|lang=en|publisher=Movie - Film - Review|access-date=2015-12-21|archive-url=https://web.archive.org/web/20120501211137/http://www.movie-film-review.com/devFilm.asp?ID=12423|archive-date=2012-05-01|url-status=dead}}</ref> [[MRQE]] wrote \"This cinematic adaptation of a theatrical work is true to the original, but does not stray far from a theatrical rendering of the story.\"<ref name=MRQE>{{cite web|last1=staff|title=Actrius (1997)|url=http://www.mrqe.com/movie_reviews/actrius-m100030469|lang=en|publisher=[[MRQE]]|access-date=2015-12-21|archive-url=https://web.archive.org/web/20151222150358/http://www.mrqe.com/movie_reviews/actrius-m100030469|archive-date=2015-12-22}}</ref>\n\n=== Awards and nominations ===\n* 1997, won 'Best Catalan Film' at [[Butaca Awards]] for [[Ventura Pons]]\n* 1997, won 'Best Catalan Film Actress' at Butaca Awards, shared by [[N\u00faria Espert]], [[Rosa Maria Sard\u00e0]], [[Anna Lizaran]], and [[Merc\u00e8 Pons]]\n* 1998, nominated for 'Best Screenplay' at [[Goya Awards]], shared by [[Josep Maria Benet i Jornet]] and Ventura Pons\n\n== References ==\n{{reflist}}\n\n== External links ==\n* {{IMDb title|0115462|Actresses}}\n* {{official website|https://web.archive.org/web/20090217140746/http://venturapons.com/filmografia/actrices.html}} [[Wayback Machine|as archived 17 February 2009]] (Spanish)\n\n[[Category:1997 films]]\n[[Category:1997 drama films]]\n[[Category:Catalan-language films]]\n[[Category:Films set in Barcelona]]\n[[Category:Films directed by Ventura Pons]]\n[[Category:Spanish drama films]]\n[[Category:1990s Spanish films]]{% endraw %}" , 
   "wikidata_id":"Q2823770",
   "wikidata_classes":[
      [
         "Q11424",
         "film"
      ]
   ],
   "text":"Actresses (Catalan: Actrius) is a 1997 Catalan language Spanish drama film produced and directed by Ventura Pons and based on the award-winning stage play \"E.R.\" by Josep Maria Benet i Jornet. The film has no male actors, with all roles played by females. The film was produced in 1996. \nSynopsis.\nIn order to prepare herself to play a role commemorating the life of legendary actress Empar Ribera, young actress (Merc\u00e8 Pons) interviews three established actresses who had been the Ribera's pupils: the international diva Gl\u00f2ria Marc (N\u00faria Espert), the television star Assumpta Roca (Rosa Maria Sard\u00e0), and dubbing director Maria Caminal (Anna Lizaran).\nRecognition.\nScreenings.\n\"Actrius\" screened in 2001 at the Grauman's Egyptian Theatre in an American Cinematheque retrospective of the works of its director. The film had first screened at the same location in 1998. It was also shown at the 1997 Stockholm International Film Festival.\nReception.\nIn \"Movie - Film - Review\", Christopher Tookey wrote that though the actresses were \"competent in roles that may have some reference to their own careers\", the film \"is visually unimaginative, never escapes its stage origins, and is almost totally lacking in revelation or surprising incident\". Noting that there were \"occasional, refreshing moments of intergenerational bitchiness\", they did not \"justify comparisons to \"All About Eve\"\", and were \"insufficiently different to deserve critical parallels with \"Rashomon\"\". He also wrote that \"The Guardian\" called the film a \"slow, stuffy chamber-piece\", and that \"The Evening Standard\" stated the film's \"best moments exhibit the bitchy tantrums seething beneath the threesome's composed veneers\". MRQE wrote \"This cinematic adaptation of a theatrical work is true to the original, but does not stray far from a theatrical rendering of the story.\"",
   "sections":{
      "abstract":"Actresses (Catalan: Actrius) is a 1997 Catalan language Spanish drama film produced and directed by Ventura Pons and based on the award-winning stage play \"E.R.\" by Josep Maria Benet i Jornet. The film has no male actors, with all roles played by females. The film was produced in 1996. ",
      "synopsis":"Synopsis.\nIn order to prepare herself to play a role commemorating the life of legendary actress Empar Ribera, young actress (Merc\u00e8 Pons) interviews three established actresses who had been the Ribera's pupils: the international diva Gl\u00f2ria Marc (N\u00faria Espert), the television star Assumpta Roca (Rosa Maria Sard\u00e0), and dubbing director Maria Caminal (Anna Lizaran).",
      "recognition":"Recognition.\nScreenings.\n\"Actrius\" screened in 2001 at the Grauman's Egyptian Theatre in an American Cinematheque retrospective of the works of its director. The film had first screened at the same location in 1998. It was also shown at the 1997 Stockholm International Film Festival.\nReception.\nIn \"Movie - Film - Review\", Christopher Tookey wrote that though the actresses were \"competent in roles that may have some reference to their own careers\", the film \"is visually unimaginative, never escapes its stage origins, and is almost totally lacking in revelation or surprising incident\". Noting that there were \"occasional, refreshing moments of intergenerational bitchiness\", they did not \"justify comparisons to \"All About Eve\"\", and were \"insufficiently different to deserve critical parallels with \"Rashomon\"\". He also wrote that \"The Guardian\" called the film a \"slow, stuffy chamber-piece\", and that \"The Evening Standard\" stated the film's \"best moments exhibit the bitchy tantrums seething beneath the threesome's composed veneers\". MRQE wrote \"This cinematic adaptation of a theatrical work is true to the original, but does not stray far from a theatrical rendering of the story.\"",
      "screenings":"Screenings.\n\"Actrius\" screened in 2001 at the Grauman's Egyptian Theatre in an American Cinematheque retrospective of the works of its director. The film had first screened at the same location in 1998. It was also shown at the 1997 Stockholm International Film Festival.",
      "reception":"Reception.\nIn \"Movie - Film - Review\", Christopher Tookey wrote that though the actresses were \"competent in roles that may have some reference to their own careers\", the film \"is visually unimaginative, never escapes its stage origins, and is almost totally lacking in revelation or surprising incident\". Noting that there were \"occasional, refreshing moments of intergenerational bitchiness\", they did not \"justify comparisons to \"All About Eve\"\", and were \"insufficiently different to deserve critical parallels with \"Rashomon\"\". He also wrote that \"The Guardian\" called the film a \"slow, stuffy chamber-piece\", and that \"The Evening Standard\" stated the film's \"best moments exhibit the bitchy tantrums seething beneath the threesome's composed veneers\". MRQE wrote \"This cinematic adaptation of a theatrical work is true to the original, but does not stray far from a theatrical rendering of the story.\""
   },
   "infoboxes":[
      {
         "name":"film",
         "params":{
            "name":"Actresses",
            "image":"Actrius film poster.jpg",
            "alt":"",
            "caption":"Catalan language film poster",
            "native_name":"([[Catalan language|Catalan]]: '''''Actrius''''')",
            "director":"[[Ventura Pons]]",
            "producer":"Ventura Pons",
            "writer":"[[Josep Maria Benet i Jornet]]",
            "screenplay":"Ventura Pons",
            "story":"",
            "based_on": "{% raw %}{{based on|(stage play) ''E.R.''|Josep Maria Benet i Jornet}}{% endraw %}",
            "starring":"{% raw %}{{ubl|[[N\u00faria Espert]]|[[Rosa Maria Sard\u00e0]]|[[Anna Lizaran]]|[[Merc\u00e8 Pons]]}}{% endraw %}",
            "narrator":"<!-- or: |narrators = -->",
            "music":"Carles Cases",
            "cinematography":"Tom\u00e0s Pladevall",
            "editing":"Pere Abadal",
             "production_companies":"{% raw %}{{ubl|[[Canal+|Canal+ Espa\u00f1a]]|Els Films de la Rambla S.A.|[[Generalitat de Catalunya|Generalitat de Catalunya - Departament de Cultura]]|[[Televisi\u00f3n Espa\u00f1ola]]}}{% endraw %}", 
            "distributor":"[[Buena Vista International]]",
             "released":"{% raw %}{{film date|df=yes|1997|1|17|[[Spain]]}}{% endraw %}", 
            "runtime":"100 minutes",
            "country":"Spain",
            "language":"Catalan",
            "budget":"",
            "gross":"<!--(please use condensed and rounded values, e.g. \"\u00a311.6 million\" not \"\u00a311,586,221\")-->"
         }
      }
   ],
   "doc_id":"330"
}
```



### Queries

Queries (or topics in TREC lingo) are sourced from the <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">MS-TOT dataset</a>. Participating groups will be given a JSONL file consisting of a random sample of 150 MS-TOT queries each for training, development, and test. An example query is described below. Corresponding  to these queries, sentence-level annotations will also be distributed. Participating groups are encouraged to leverage these codes however they want. This might include treating sentences associated with specific codes differently (e.g., ignoring or down-weighing sentences that convey uncertainty).


#### Example query

**Note:**  Only a single sentence annotation is shown in the example below

```json
{% raw %}
{
   "id":"763",
   "url":"https://irememberthismovie.com/super-rare-surreal-dystopian-masterpiece/",
   "domain":"movie",
   "title":"Super Rare Surreal Dystopian Masterpiece",
   "text":"Very rare movie that is scifi/dystopian/experimental/surreal. It\u2019s like Stalker meets el Topo meets Holy Mountain meets Alphaville meets Delicatessen meets Hard to be a God, like Kurosawa, Tarkovsky, and Lynch had a kid together. It was color, possibly Russian, and I don\u2019t really remember the decade but want to say 60s or 70s, though could easily be more recent. It is VERY rare, there is only one crappy partial print of it, and that is what the youtube version is from. Lot of wide shots in a surreal wilderness, winter settings, strange bleeding saturation in some shots. Crazy costumes. Seriously one of the strangest films I\u2019ve ever seen and my favorite films are strange/weird ones. If you\u2019ve ever seen what you\u2019re thinking of on a \u201cbest weird movies\u201d or \u201cyou\u2019ve never seen this!\u201d list, that\u2019s NOT it. I don\u2019t think this film even has a cult following of ten people. It\u2019s an actual rare gem. Have been looking through selections at 366 Weird Movies and not found it yet (btw the way most of those titles are exactly the kind of not-actually-rare movies this film is definitely not).",
   "wikipedia_id":"16742289",
   "wikipedia_url":"https://en.wikipedia.org/wiki/On_the_Silver_Globe_(film)",
   "wikidata_id":"Q1988165",
   "imdb_url":"https://www.imdb.com/title/tt0093593",
   "sentence_annotations":[
      {
         "id":1,
         "text":"Very rare movie that is scifi/dystopian/experimental/surreal.",
         "labels":{
            "opinion":false,
            "emotion":false,
            "hedging":true,
            "social":false,
            "comparison_relative":false,
            "search":false,
            "movie":{
               "music_compare":false,
               "genre_audience":false,
               "production_camera_angle":false,
               "timeframe_singular":false,
               "origin_actor":false,
               "object":false,
               "location_specific":false,
               "person_fictional":false,
               "production_visual":false,
               "scene":false,
               "negation":false,
               "production_audio":false,
               "character":false,
               "genre_traditional_tone":true,
               "release_date":false,
               "location_type":false,
               "origin_language":false,
               "origin_movie":false,
               "person_real":false,
               "plot":false,
               "quote":false,
               "category":true,
               "music_specific":false,
               "timeframe_plural":false
            },
            "context":{
               "cross_media":false,
               "physical_medium":false,
               "situational_count":false,
               "physical_user_location":false,
               "situational_evidence":false,
               "situational_witness":false,
               "temporal":false
            }
         }
      }
   ]
}
{% endraw %}
```

</details>

#### Sentence annotations codes

- **movie:** the sentence describes something about the movie itself.  Sub-codes under this category are NOT mutually exclusive (i.e., sentences can be associated with zero, one, or several of the following sub-codes).
  - **category:**   describes the movie’s category (e.g., movie, tv movie, miniseries, etc.)
  - **character:**   describes a character in the movie.
  - **genre_audience:**  describes the movie’s target audience (e.g., for kids).
  - **genre_traditional_tone:**  describes the movie’s genre or tone (e.g., romantic comedy).
  - **location_specific:** describes a specific location in the movie (e.g., the boy lives with his mom in Arizona).
  - **location_type:**  describes a type of location in the movie (e.g., a European castle).
  - **music_compare:**  describes the movie’s soundtrack (e.g., lots of electronic music). 
  - **music_specific:**  describes a song in the movie (e.g., the main character sings “Looking for the Heart of Saturday Night”).
  - **negation:** uses negation to describe aspects of the movie in negative terms (e.g., not scary, but a bit weird). 
  - **object:**  describes a tangible object in the movie (e.g., they’re in a car that almost crashes into a beast).
  - **origin_actor:**  describes the nationality or ethnicity of actors/actresses in the movie.
  - **origin_language:**  describes languages spoken in the movie.
  - **origin_movie:**  describes the movie’s region of origin.
  - **person_fictional:**  references a fictional character (e.g., the main character looks like Indiana Jones).
  - **person_real:** references a real person (e.g., the main character looks like Harrison Ford).
  - **plot:** describes the movie’s plot.
  - **production_audio:** describes characteristics of the audio (e.g., badly dubbed).
  - **production_camera_angle:** describes camera movements (e.g., the camera suddenly cuts to the monster under the bed)
  - **production_visual:** describes the movie’s visual production (e.g., black and white).
  - **quote:** describes a quote from the movie.
  - **release_date:** describes the movie’s release date.
  - **scene:** describes a scene from the movie.
  - **timeframe_plural:** describes the passage of time in the movie (e.g., decades later, the house is believed to be haunted).
  - **timeframe_singular:** describes a time period in the movie (e.g., set in the 1920’s).
- **context:** the sentence describes something about the context in which the movie was seen. Sub-codes under this category are NOT mutually exclusive (i.e., sentences can be associated with zero, one, or several of the following sub-codes).
  - **cross_media:** describes exposure to the movie through other media (e.g., trailer, DVD cover, poster, etc.)
  - **physical_medium:** describes the physical medium through which the movie was seen (e.g., on late-night TV).
  - **physical_user_location:** describes the physical location in which the movie was seen (e.g., I watched it in film class).
  - **situational_count:** describes the number of times the movie was seen (e.g., I watched the series once a week).
  - **situational_evidence:** describes evidence used to recall contextual information (e.g., I watched it around 2006 because I watched it alongside Hard Candy).
  - **situational_witness:** describes other people who watched the movie (e.g., with my 6-year old nephew).
  - **temporal:** describes when the movie was seen (e.g., I rented it in the early 2000’s).
- **prevous_search:** the sentence describes previous attempts to re-find the movie.
- **opinion:** the sentence describes an opinion about some aspect of the movie.
- **emotion:** the sentence describes an emotional response to the movie.
- **hedging:** the sentence includes mentions of uncertainty (e.g., I think it was released in the early 2000’s).
- **social:** the sentence includes a social nicety (e.g., thanks in advance!).
- **comparison_relative:** the sentence describes something in relative terms (e.g., the movie stars someone who looks like Brad Pitt) versus absolute terms (e.g., the movie stars Brad Pitt).

### Use of external information

You are generally allowed to use external information while developing your runs. When you submit your runs, please fill in a form listing what resources you used.
This could include an external corpus such as TMDB or a pretrained model (e.g. word embeddings, BERT). Additionally,
- Participating groups  are **PERMITTED** and encouraged to gather movie data from multiple sources. We are distributing identifiers for several data sources i.e., Wikipedia, Wikidata and IMDb (if available). Groups are encouraged to explore other resources on their own. Pages from different sources can often be resolved using the IMDB ID (e.g., Wikipedia pages often contain the movie’s IMDB ID).
- Participating groups are **PERMITTED** and encouraged to leverage the qualitative codes associated with the MS-TOT train, dev, and test sets. When submitting a run, groups may be asked to explain whether and how the qualitative codes were used during training and/or testing.
- Participating groups are **PROHIBITED** from using any data from the following websites that are not already included in the train/dev sets described above. **If you do so, you will be training and/or hyperparameter tuning using test data.**
    - <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification</a>
    - <a href="https://irememberthismovie.com/" target="_blank">iRememberThisMovie.com</a>
 
### Training data documentation and declaration
 
The emerging practices of pretraining large language models on web-scale datasets and community sharing of these pretrained models for downstream training / model development creates challenges in tracking what training data have been used for particular run submissions. Given that our test queries are sampled from the <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">MS-TOT dataset</a> that has been publicly available online since 2021, there is a possibility that this data may have been used for training models that are employed in submitted runs. To ensure that we get robust scientific conclusions and insights from this year's track, we are requesting all participants to document *all* training data that were employed in the preparation of a given run to the best of your knowledge (including any data used for pretraining models that you are building on top of). In addition, we are also requesting participants to declare if they are 100% confident that no data from <a href="https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification" target="_blank">https://github.com/microsoft/Tip-of-the-Tongue-Known-Item-Retrieval-Dataset-for-Movie-Identification</a> or <a href="https://irememberthismovie.com/" target="_blank">iRememberThisMovie.com</a> was used for training. We recognize that not all pretrained models publicly document their training data in details. So, please mark that declaration to be true *only* if you are 100% confident about all training data used in preparation of your run and can guarantee that it does not include any data from the sources mentioned. We do not discourage submissions where there may be some uncertainty about the training data but we want to be aware of this at the time of analyzing the evaluated results.

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

