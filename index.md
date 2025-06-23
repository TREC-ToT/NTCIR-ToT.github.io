---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

# Overview

Have you ever tried re-finding a movie that you watched as a child but cannot remember its name? Perhaps you vaguely remember the plot or a few scenes from the movie.  You might also remember things about the context in which you watched the movie.  For example, you might remember that you watched the movie in the 1980s on television.  You also remember that the movie made you feel melancholy.  Unfortunately, none of the things you remember would enable you to re-find the movie using existing information retrieval (IR) tools.

Tip-of-the-tongue (ToT) known-item retrieval is defined as "an item identification task in which the searcher has previously experienced an item but cannot recall a reliable identifier" [<a href="https://dl.acm.org/doi/abs/10.1145/3406522.3446021" target="_blank">1</a>] (i.e., "It's on the tip of my tongue...").  Current IR systems are not well-equipped to address ToT information needs.  As evidence, a wide range of community Q&A sites have emerged to help people resolve their ToT information needs with the help of other people—e.g.,
- <a href="https://www.reddit.com/r/tipofmytongue" target="_blank">https://www.reddit.com/r/tipofmytongue</a>
- <a href="https://irememberthismovie.com" target="_blank">https://irememberthismovie.com</a>
- <a href="https://www.watzatsong.com/en" target="_blank">https://www.watzatsong.com/en</a>
- <a href="https://www.goodreads.com/group/show/185-what-s-the-name-of-that-book" target="_blank">https://www.goodreads.com/group/show/185-what-s-the-name-of-that-book</a>
- <a href="https://scifi.stackexchange.com/questions/tagged/story-identification" target="_blank">https://scifi.stackexchange.com/questions/tagged/story-identification</a>

Presumably, people who post to these sites either failed to resolve their needs using existing IR tools or did not even try.  

ToT information requests (or "queries") are verbose and contain a wide-range of complex phenomena.  Importantly, such phenomena are influenced by: (1) what we  remember (i.e., long-term memory)  and (2) how we naturally convey our memories to others.  Complex phenomena include contextual memories, comparisons, mentions of uncertainty, false memories, and mentions of exclusion criteria, to name a few.  The example below shows some of these complex phenomena.

<img src="img/example.png" style="height:158px">

**Correct answer**: <a href="https://www.imdb.com/title/tt0058230/" target="_blank">The Incredible Mr. Limpet</a>

The TREC ToT track aims to develop IR systems that can successfully resolve ToT information needs.  Progress in this area will likely benefit other IR systems that must deal with memory assistance, such as personal information management (PIM) systems (e.g., email re-finding).

The 2023 TREC ToT Track will focus on movie identification—a task that is broadly relatable and has been the focus of recent research [<a href="https://dl.acm.org/doi/abs/10.1145/3406522.3446021" target="_blank">1</a>, <a href="https://dl.acm.org/doi/abs/10.1145/3465336.3475096" target="_blank">2</a>, <a href="https://dl.acm.org/doi/abs/10.1145/3488560.3498421" target="_blank">3</a>, <a href="https://ceur-ws.org/Vol-3366/paper-03.pdf" target="_blank">4</a>, <a href="https://aclanthology.org/2023.findings-emnlp.367/" target="_blank">5</a>, <a href="https://vbn.aau.dk/en/publications/exploring-the-zero-shot-known-item-retrieval-capabilities-of-llms" target="_blank">6</a>, <a href="https://arxiv.org/abs/2503.19193" target="_blank">7</a>, <a href="https://arxiv.org/abs/2502.17776" target="_blank">8</a>].


## Motivations and challenges

**Beyond keyword search.** ToT queries are natural-language, verbose, and complex requiring a deeper understanding of language and contextual information and potentially more complicated search strategies including multi-hop reasoning.

**Handling inaccuracies.** Searchers in ToT states frequently express uncertainty and suffer from False memories.

**Futuristic.** Novel task. Emergent search interfaces, such as conversational search, may make it easier to express such information needs in the near-future.

See <a href="https://ils.unc.edu/~jarguell/TREC-TOT.pdf">this presentation</a> for more ideas about motivations and challenges.

# Organizers

<p>
<div style="width: 100%; overflow: hidden;">
  <div style="float: left;">
    <img src="img/arguello.jpg" alt="Avatar" style="height:130px;border-radius:50%">
  </div>
  <div style="margin-left: 200px;">
    <b>Jaime Arguello</b>
    <br/>University of North Carolina
    <br/>Chapel Hill, North Carolina, USA
    <p>
        <a href="https://ils.unc.edu/~jarguell/" class="fa fa-home" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.linkedin.com/in/jaime-arguello-5b417572/" class="fab fa-linkedin" style="font-size:24px;color:grey" target="_blank"></a>
    </p>
  </div>
</div>
</p>
<p>
<div style="width: 100%; overflow: hidden;">
  <div style="float: left;">
    <img src="img/diaz.jpg" alt="Avatar" style="height:130px;border-radius:50%">
  </div>
  <div style="margin-left: 200px;">
    <b>Fernando Diaz</b>
    <br/>Carnegie Mellon University and Google
    <br/>Pennsylvania, United States
    <p>
        <a href="https://841.io/" class="fa fa-home" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://bsky.app/profile/841io.bsky.social" class="fab fa-bluesky" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://twitter.com/841io" class="fab fa-twitter" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://mastodon.social/@841io@sigmoid.social" class="fab fa-mastodon" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.linkedin.com/in/fernando-diaz-b6b9023/" class="fab fa-linkedin" style="font-size:24px;color:grey" target="_blank"></a>
    </p>
  </div>
</div>
</p>
<p>
<div style="width: 100%; overflow: hidden;">
  <div style="float: left;">
    <img src="img/kim.jpg" alt="Avatar" style="height:130px;border-radius:50%">
  </div>
  <div style="margin-left: 200px;">
    <b>To Eun Kim</b>
    <br/>Carnegie Mellon University
    <br/>Pennsylvania, United States
    <p>
        <a href="https://kimdanny.github.io/" class="fa fa-home" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://bsky.app/profile/teknology.bsky.social" class="fab fa-bluesky" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://x.com/TEKnologyy" class="fab fa-twitter" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.linkedin.com/in/danny-toeun-kim/" class="fab fa-linkedin" style="font-size:24px;color:grey" target="_blank"></a>
    </p>
  </div>
</div>
</p>
<p>
<div style="width: 100%; overflow: hidden;">
  <div style="float: left;">
    <img src="img/frobe.jpg" alt="Avatar" style="height:130px;border-radius:50%">
  </div>
  <div style="margin-left: 200px;">
    <b>Maik Fröbe</b>
    <br/>Friedrich-Schiller-Universität Jena
    <br/>Jena, Germany
    <p>
        <a href="https://www.fmi.uni-jena.de/4653/maik-froebe" class="fa fa-home" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://bsky.app/profile/maik-froebe.bsky.social" class="fab fa-bluesky" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://mastodon.social/@maik_froebe@idf.social" class="fab fa-mastodon" style="font-size:24px;color:grey" target="_blank"></a>
    </p>
  </div>
</div>
</p>
<p>
<div style="width: 100%; overflow: hidden;">
  <div style="float: left;">
    <img src="img/mitra.jpg" alt="Avatar" style="height:130px;border-radius:50%">
  </div>
  <div style="margin-left: 200px;">
    <b>Bhaskar Mitra</b>
    <br/>Montreal, Canada
    <p>
        <a href="https://bhaskar-mitra.github.io/" class="fa fa-home" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://bsky.app/profile/bmitra.bsky.social" class="fab fa-bluesky" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://twitter.com/UnderdogGeek" class="fab fa-twitter" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://mastodon.social/@bmitra" class="fab fa-mastodon" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.linkedin.com/in/bhaskarmitra/" class="fab fa-linkedin" style="font-size:24px;color:grey" target="_blank"></a>
    </p>
  </div>
</div>
</p>

# Past organizers

<p>
<div style="width: 100%; overflow: hidden;">
  <div style="float: left;">
    <img src="img/bhargav.jpg" alt="Avatar" style="height:130px;border-radius:50%">
  </div>
  <div style="margin-left: 200px;">
    <b>Samarth Bhargav</b>
    <br/>University of Amsterdam
    <br/>Amsterdam, The Netherlands
    <p>
        <a href="http://samarthbhargav.github.io/" class="fa fa-home" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://twitter.com/samarthbhargav" class="fab fa-twitter" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.linkedin.com/in/samarthbhargav/" class="fab fa-linkedin" style="font-size:24px;color:grey" target="_blank"></a>
    </p>
  </div>
</div>
</p>
<p>
<div style="width: 100%; overflow: hidden;">
  <div style="float: left;">
    <img src="img/kanoulas.jpg" alt="Avatar" style="height:130px;border-radius:50%">
  </div>
  <div style="margin-left: 200px;">
    <b>Evangelos Kanoulas</b>
    <br/>University of Amsterdam
    <br/>Amsterdam, The Netherlands
    <p>
        <a href="https://staff.fnwi.uva.nl/e.kanoulas/" class="fa fa-home" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://twitter.com/ekanou" class="fab fa-twitter" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://mastodon.social/@ekanou@mas.to" class="fab fa-mastodon" style="font-size:24px;color:grey" target="_blank"></a>
        &nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.linkedin.com/in/ekanou/" class="fab fa-linkedin" style="font-size:24px;color:grey" target="_blank"></a>
    </p>
  </div>
</div>
</p>
