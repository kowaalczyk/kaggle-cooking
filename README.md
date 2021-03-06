# Kaggle cooking competition
Submission and experiemnts for in-class cooking competition.

### Development setup
Setup environment:
```
conda create -n kaggle-cooking python=3.7.1 numpy pandas tqdm scikit-learn spacy torch keras
source activate kaggle-cooking
```
Pull all git submodules:
```
git pull --recurse-submodules
```
and compile `fastText` if you're going to use it:
```
source setup.sh
```
(sourcing it will also create alias for the library).

## Baseline (`baseline.ipynb`)
Combined all words from all ingedients into one space-separated strings and vectorized using Tf-Idf vectorizer,
which generated the best results on CV. Submission was generated by taking mode of out-of-fold 
predictions for each sample of the test set and yielded 0.80 score on public LB.

That score was never matched on public LB, even though some experiments achieved similar or slightly 
better results on CV.


## Top submission (`positional-features.ipynb`)
The intuition was, that there might be meaningful information to extract from position of particular
ingredients on the list - order appears to be random, but could well be related to the order of ingredient
usage or their amount in a given recipe.

To check this hypothesis, I extracted the most popular ingredients and added features related not only to their 
presence in the dataset, but also their position (treated as an ordinal feature).

This improved CV score slightly, but did not result in any improvement on the public LB. 
On the private LB however, this approach scored the best. 

Unfortunately, after not seeing improvement on LB I moved away from the idea - I should've trusted my CV scores 
more and work from this model, rather than trying to create something else from the baseline.


## Other experiments

### `fast-text.ipynb`
Was an attempt to see how a complete black-box text classification library would handle this task.
Unsurprisingly, possibly due to lack of text structure and domain-specific vocabulary it performed
worse than baseline LGBM. 

The ability to use ngrams to capture positional relationships between pairs of features also 
seemed to be an interesting idea, but it increased feature space way too much, preventing 
the model from doing anything useful.

### `vectorization.ipynb`
Contains my experiments related to vectorization - trying to eliminate vectors appearing only once in entire set. 

I wanted to combine it with positional-based features, which would possibly be a great way to reduce 
the feature space and allow for more positional features. Unfortunately, I could not find enough time to do this.

### `pytorch.ipynb`, `small-classes.ipynb`, `nlp.ipynb`
These are experiments I did not managed to finish due to time pressure from other subjects:
- trying to run a pytorch CNN classifier
- training binary classifiers / classifiers working on a smaller set of classes to distinguish them better
- attempting to preprocess words using `spaCy` nlp library, 
  which did not seem to improve word selection for the model

In retrospective, I should've put more effort into feature selection rather than 
trying to experiment with weird nlp heuristics about word meaning. Trying too much different experiments was also not the greatest idea do do right before the exam session.
