# Clustering on Word Vectors using Truncated SVD #

Semantic clustering of text using K-means on custom-built contextual Word Co-occurence matrix, decomposed using SVD. Truncated length of word vectors are estimated by plotting explained variance of decomposed matrix.

# Clustering on Truncated SVD Word Occurence Matrix (Part I) #

## Amazon Fine Food Review Dataset ##

Data Source: https://www.kaggle.com/snap/amazon-fine-food-reviews

The Amazon Fine Food Reviews dataset consists of reviews of fine foods from Amazon. <br/>
Number of reviews                   : 568,454  <br/>
Number of users                     : 256,059  <br/>
Number of products                  : 74,258  <br/>
Timespan: Oct 1999                  : Oct 2012  <br/>
Number of Attributes/Columns in data: 10 <br/>

### Attribute Information ###
1. Id <br/>
2. ProductId - unique identifier for the product <br/>
3. UserId - unqiue identifier for the user <br/>
4. ProfileName <br/>
5. HelpfulnessNumerator - number of users who found the review helpful <br/>
6. HelpfulnessDenominator - number of users who indicated whether they found the review helpful or not <br/>
7. Score - rating between 1 and 5 <br/>
8. Time - timestamp for the review <br/>
9. Summary - brief summary of the review <br/>
10. Text - text of the review <br/>

## Objective ##

The code below would **clean the review text from html tags and punctuations and write it as a new column in the database and write it to disk**. This is further taken up in Part 2 to find accuracy of 10-fold cross validation KNN on vectorized input data, for each of the 4 featurizations, namely BoW, tf-IDF, W2V, tf-IDF weighted W2V.

## Significant Points ##

1. **Duplication of reviews** are found with same userid and timestamp (Cleaned).
2. Found discrepancy issues with HelpfulnessDenominator (Cleaned).
3. final.sqlite db is to be **used for further processing** such as Text to Vector operations.
4. The preprocessing step is one time effort but the training & visualization steps require multiple runs. Hence, it is prudent to make reprocessing step independant, to avoid multiple runs.

# Clustering on Truncated SVD Word Occurence Matrix (Part II) #

## Data Source: ##

**The preprocessing step has produced final.sqlite file after doing the data preparation & cleaning**. The review text is now devoid of punctuations, HTML markups and stop words.

## Objective: ##

To **find clusters of semantically related words** from Amazon reviews using contextual Word Co-occurence matrix. **Co-occurence Matrix is factor decomposed using SVD, truncated with an estimated K on the basis of maximum explained variance**.

## Steps ##

1. Found **Top Features based on TF-IDF featurization**.

2. Created **Word Co-Occurence Matrix with neighbourhood = 5**

3. Word Co-Occurence Matrix **Decomposition done using SVD. Found matrix, U**.

4. Found the best value of **’k’, based on explained variance of matrix, U** (same as in PCA).

5. Done **TruncatedSVD on U to find Word Vectors** (Reduced U to ’k’ components)

6. **Ran K-means Clustering** on StandardizedWord Vectors to find clusters.

7. Took one word, found the cluster to which it belongs & found the most similar words using cosine similarity metric.

8. Draw word cloud based on cosine similarity. Do step (g) & (h) for couple of words

9. Analyze the word vector clusters obtained.

## Custom Defined Functions ##

3 user defined functions are written to

1. Elbow Method to find K

2. Analyze the Clusters function

3. Generate Similiarity Word Clouds

## Word Co-Occurence Matrix Decomposition using SVD & K Estimation ##

<p align="center">
    <img src="https://github.com/AdroitAnandAI/Clustering-on-Word-Vectors-using-Truncated-SVD/blob/master/images/5.PNG">
</p>

## Estimate K for K-means Clustering ##

<p align="center">
    <img src="https://github.com/AdroitAnandAI/Clustering-on-Word-Vectors-using-Truncated-SVD/blob/master/images/7.PNG">
</p>

## Word Clouds using Cosine Similarity ##

<p align="center">
    <img src="https://github.com/AdroitAnandAI/Clustering-on-Word-Vectors-using-Truncated-SVD/blob/master/images/9.PNG">
</p>

**The words meaningfully similar to ’food’ in cluster 15 are irregular, wholefood, brunch, bitterish, grind, robusto, dogfoodanalyst, dinner, cocunut etc. Thus 15th cluster contains food and food related words in general.**

<p align="center">
    <img src="https://github.com/AdroitAnandAI/Clustering-on-Word-Vectors-using-Truncated-SVD/blob/master/images/9.2.PNG">
</p>

**The words meaningfully similar to ’alkalin’ in cluster 16 are nonalcohol, stimuli, vitamin, needl, lecithin, technivorm, latent, microbiolog, benzocain, sunburn etc. Thus 16th cluster contains chemcial and medicine related words.**

<p align="center">
    <img src="https://github.com/AdroitAnandAI/Clustering-on-Word-Vectors-using-Truncated-SVD/blob/master/images/9.3.PNG">
</p>

**The words meaningfully similar to ’gourmet’ in cluster 1 are pillsburi, chia, blueberri, dragon-fruit, naturopath, maker, healthi, masterpiec, halal, semi, reput, fast, fatti, pilchard etc. Thus, 1st cluster contains gourmet and food/health related words.**

# Observations #

1. There are clusters where semantic relation could be noticed. For instance, **words hypoglecemia and hysterectomi are grouped together (both are medical words)**.

2. The words grouped together with word, **’alkalin’ in cluster 16** are nonalcohol, stimuli, vitamin, needl, lecithin, technivorm, latent, microbiolog, benzocain, sunburn etc. Thus **16th cluster contains chemical and medicine related words**.

3. The words meaningfully similar to **’food’** in cluster 15 are irregular, wholefood, brunch, bitterish, grind, robusto, dogfoodanalyst, dinner, cocunut etc. Thus **15th cluster contains food and food related words in general**.

4. The words along with **’gourmet’** in cluster 1 are pillsburi, chia, blueberri, dragonfruit, naturopath, maker, healthi, masterpiec, halal, semi, reput, fast, fatti, pilchard etc. Thus, **1st cluster contains gourmet and food/health related words**.

5. Thus, using **factor decomposedWord Co-occurence matrix, semantically related words are clustered** from Amazon reviews.
