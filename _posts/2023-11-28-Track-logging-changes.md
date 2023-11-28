---
layout: post
title: Track logging changes
#subtitle: An essential part of Resources Evaluation. GSLIB Cell Based Method.
tags: [EDA]
bigimg: /img/per010rz.jpg
share-img: /img/abstract_bg_cuda.png.PNG
---

Exploration projects collect information over the years. The information changes due to incoming data, change of personal, among other factors. Changes in the data must be justified and well documented, it requires a sound database and personal to provide insightful details. A simple case is to understand the impact of new logged intervals between two consecutive years using tabular data. This post refers to these years as prior and posterior. It lists basic considerations to compare to year to date files of categorical logs.

Two data tables are compared, the year-to-date prior, and posterior file.  ID is the column of drillcores ID's, Domain is the categorical column of the interpretations. The header below is the format of the logging table. 

| ID | From |  To | Domain |
| --- | ---- | --- | --- |

Additionally, a table with the  drilling year and optionally sample type is required.

```
| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |
```


1. Delete leading and trailing blanks in drill core ID's
2. Use either lower or upper case in the ID columns
3. Filter out null values of domain column.

The join clauses in the post are applied on the set of keys  ID, the start (from) and end (to) of the interval. After performing a joining clause, the interpretations from the prior remains in the column Domain_prior, and in Domain_posterior for the posterior interpretations. The steps tag the intervals using the keywords: unchanged, changed and new on a column Tag.

1. Perform a prior left outer join posterior (table 1), and tag as unchanged to equal interpretations in Domain_prior and Domain_posterior. This accounts for logged intervals in the prior and posterior tables that remain unchanged.

2. On table 1, tag intervals where Domain_prior and Domain_posterior are different, with changed. This step accounts for the intervals logged in the prior file, different in the posterior. Differences come from reinterpreting the prior (at any previous year) or deleting prior interpretations (absent in posterior).

3. Perform a prior right outer join posterior (table 2). Tag as new to null values in the column Domain_prior, and filter out rows where the Tag column is null. This labels interpreted intervals in the posterior file that were inexistent (null) in the prior file.  This new data comes from the same drilling year of the posterior, or from interpreting old drilling (at any previous year) logged in the year of the posterior.

4. Concatenate the rows of table 1 and 2 into a batch table with the tagging. Add a drilling year and optionally sample type column to the batch by mapping the ID's.

The tags: unchanged, changed and new summarizes the change of information.  The length variation suffices in some cases, however is not complete. New logging data results from targeting mineralized zones, the justification of drilling and reinterpretation must be consistent to the variation of metal content. Proceed to generate a table by splitting the intervals of the logging interval table and the assays table containing the elements of interest. Apply the next steps to the new split batch file.

-  (a) Generate the statistics by domain_prior.

-  (b) Generate the statistics by domain_prior of the batch where Tag is changed. This is discounted from the prior due to re-interpretations.

-  (c) Get the statistics by domain_posterior where Tag is changed. Filter the rows where domain_posterior is null. This is the additioned data in the posterior that comes from reinterpreted data of the prior. It does not account for deletion of data in the posterior.

-  (d) Get the statistics of the batch where Tag is new.

-  (e) Get the statistics by domain of the entire batch (less or equal than the year of the posterior file).

The relation a - b + c + d = e holds for the length.