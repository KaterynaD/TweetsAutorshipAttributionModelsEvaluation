# TweetsAutorshipAttributionModelsEvaluation
<H1>Data</H1>
The data used in the experiment are from https://www.kaggle.com/speckledpingu/RawTwitterFeeds
It is a dataset of tweets from various active scientists and personalities ranging from Donald Trump and Hillary Clinton to Neil deGrasse Tyson.
<H1>Experiment</H1>
In these notebooks I work on the question whether the author of a tweet (very short text) can be successfully identified. I try to choose the best classification method its parameters set and features

For now only binary classification was applied

<H2>Stage 1</H2>
In <a href="https://github.com/KaterynaD/TweetsAutorshipAttributionModelsEvaluation/blob/master/Autorship%2Battribution%2Bmodels%2Bevaluation.ipynb">Autorship+attribution+models+evaluation.ipynb</a> I tested several algorithms using <i>cross_val_score</i> with 10-folds and <i>GridSearchCV</i> with default 3 folds cross-validation.

LinearSVC (C=1, char 3-ngrams), SVC with linear kernel (C=1, char 3-ngams), SGDClassifier (alpha: 0.0001, char 3-ngams), Bernoulli Naive Bayes (alpha = 0.001 based on combination of character 3-ngrams, word unigrams and tokenized unigram) All 4 of them returns approximately the same accuracy - 96% (Sometimes it's less for other pairs of authors)

In farther experiments I used Bernoulli Naive Bayes classifier only because on these data all classifiers provides more or less the same results.

<H2>Stage 2</H2>

<a href="https://github.com/KaterynaD/TweetsAutorshipAttributionModelsEvaluation/blob/master/POS%2Btag%2Bvectorization.ipynb">POS+tag+vectorization.ipynb</a> is an advanced example of model evaluation with the addition of <i>POS tag vectorization</i> (based on an <i>other column</i> in the data set), using <i>FeatureUnion</i> in a pipeline and <i>GridSearchCV</i> with cross-validation splitting strategy (10 folds in a (Stratified)KFold)

There is also an example of extracting most informative features for the prediction

<H2>POS tag vectorizer</H2>

I convert the text column in the data set to the string of POS tags and replace the usual POS tag 2-3 chars abbreviations to 1 char abbrevations (e.g. 'NNP' -> 'N', 'NNPS' -> 'O') in order to use CountVectorizer with 'char' analyzer to get the most informative POS tag combinations per author

<H1>Results</H1>
From the achieved results point of view: <b>'char and stemmed word'</b> vectorizeres provides good results. The other combinations (with or without POS tag vectorizer) may or may not provide slightly better or worse results.

<b>'char'</b> analyzer provides a perfect result by itself in most cases.

<b>'stemmed word'</b> analyzer is better then the just 'word' analyzer but still worse then the 'char' for most of teh authors
But for some pairs (AdamSavage - ScottKelly) it provides better results the the char analyzer and worse then the just 'word'

<b>POS tag vectorizer</b> is not very selective by itself.

Using <b>POS tag vectorizer in combination</b> with 'char and stemmed word' does not improve the score dramatically. Its impact is more visible only for AdamSavage - ScottKelly pair

For most pairs of authors I get 90 - 96% accuracy. I experimented with 500 - 2000 rows rows data sets per author
