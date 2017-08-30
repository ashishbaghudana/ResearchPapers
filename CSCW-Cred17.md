# A Parsimonious Language Model of Social Media Credibility Across Disparate Events

## Questions

* Fundamental: What drove the authors to engineer features by themselves, for instance, subjectivity or modality? Would use of NLP-based techniques, such as dependency parsing, or analyzing sentence structure help in adding features and hopefully accuracy to the model?
* Classification: The method for classification is logistic regression. Would other algorithms give higher accuracy?
* Features: Some of the features seem counter-intuitive (see below for an explanation). What was the philosophy while choosing these as the features and did their expectations about the features correspond to the results?

## Observations

### Construction of CREDBANK

* The paper attempts to build a statistical model on the credibility of ~66 million tweets over 1377 event streams.
* The credibility scheme was a 5-point scale where mechanical turks marked the tweet as -2 (Certainly inaccurate) to +2 (Certainly accurate).
* To average out the ratings, calculate `P(ca) = probability of being certainly accurate = (number of certainly accurate annotations/total number of annotations)`
* Eventually, use a table based approach to level give ordinal ratings to each tweet

|          Credibility | Probability Range  |
|---------------------:|--------------------|
|  Perfect Credibility | 1 >= P(ca) >= 0.9  |
|     High Credibility | 0.9 > P(ca) >= 0.8 |
| Moderate Credibility | 0.8 > P(ca) >= 0.6 |
|      Low Credibility | 0.6 > P(ca) >= 0.0 |

### Method

* Identified **15 linguistic measures** as potential predictors of credibility
* The features (described in brief) below:
  * **Modality:** The presence of _should, sure_ &rightarrow; strong claim whereas _may, possibly_ &rightarrow; speculation. Use Roser Sauri's [TO-READ] [list of words](https://www.aaai.org/Papers/FLAIRS/2006/Flairs06-065.pdf) on event modality.
  * **Subjectivity** of text based on OpinionFinder's subjectivity lexion
  * Presence of **hedges** &rightarrow; makes text more fuzzy and difficult to understand &rightarrow; low credibility
  * **Evidentiality**: Generally words that convey factuality of information
  * Higher the number of **negations** (no, neither, nor) &rightarrow; conveys higher credibility
    * _Really?_ Recent trends on Twitter tend to show very polarized viewpoints about politics and both tend to use strong negations.
  * Increased number of conjuctions and exclusions &rightarrow; more reasoned argument &rightarrow; more credible
  * Presence of **anxiety** words
  * Positive and negative emotion
    * _How were these words used to identify credibility?_
  * **Boosters and Capitalization** indicate higher credibility
    * _The presence of capitalization can often also be used to force an argument. Capitalization is the Internet equivalent of shouting the other person down_
  * **Quotation marks** tend to indicate higher credibility
  * Inclusion of **questions** indicates higher credibility
  * Hashtags might indicate rumors and therefore less credible
* Used logistic regression based classifier with a set of `controls = {tweet count, reply count, retweet count, tweet length, reply length, retweet length, tweet word count, reply word count, retweet word count}`

### Results

* Baseline - random guess - should give accuracy (F1) of ~25%.
* 75/25 split for train/test
* 10-fold cross validation
* Level-1 Weight 0.25 accuracy at ~53.6%.
* Level-1 Weight 0.50 accuracy at ~64.9%.
* Level-2 Weight 0.25,0.50 accuracy at ~67.8%.

### Discussion

* Average retweet length was the only control variable within the top 100 predictors for high credibility.
* Average reply length was within 200 predictors for high credibility.
* Number of retweets was in the top 50 predictors for low credibility.
  * Table 7: How often do these words occur in the corpus? For instance, if _vibrant_ occurs only 20 times and of these 17 tweets are credible, then it indicates a sense of high credibility. However, if _radiant_ occurs over 1000 times and of these 60 are credible, it indicates a sense of low credibility.
