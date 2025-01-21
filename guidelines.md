---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

# TREC 2025 Tip-of-the-Tongue (ToT) Track

Welcome to the guidelines for the upcoming 2025 edition of the TREC ToT track!

# Guidelines

## Important dates
* **May (tentative):** Release corpus and training queries
* **August (tentative):** Release test queries
* **September (tentative):** Deadline for submitting runs

## Registration

Organizations wishing to participate in TREC 2025 should submit an application.
Participants in previous TRECs who wish to participate in TREC 2025 must submit a new application.

To apply, use the <a href="http://ir.nist.gov/evalbase" target="_blank">new Evalbase web app</a>.
First you will need to create an account and profile, then you can register a participating organization from the main Evalbase page.

Any questions about conference participation should be sent to the general TREC email address, trec (at) nist.gov.

## Task definition

In terms of input and output, the ToT known-item identification task is relatively straightforward—given an input TOT request, output a ranked list of items.

More details coming soon!


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

