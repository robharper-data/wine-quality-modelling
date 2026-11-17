# Wine quality: regression, classification and clustering

Can a wine's quality score be predicted from laboratory measurements? This project applies three standard techniques to that question (linear regression, k-nearest-neighbours classification and k-means clustering) and looks closely at how parameter choices change the results.

Completed in 2023 during my Master of Data Science at RMIT University.

## Data

A variant of the [UCI Wine Quality dataset](https://archive.ics.uci.edu/dataset/186/wine+quality) (Cortez et al., 2009): eleven physicochemical measurements per wine, such as acidity, residual sugar, density and alcohol, plus a quality score from 0 to 10 given by tasters.

The full file I worked from (4,781 rows, with missing and malformed values added for the exercise) is not mine to redistribute, so it is not included. This repository contains:

- `data/wine_quality_sample.csv`: the random sample used for all modelling (600 rows drawn with a fixed seed, 599 after removing one with malformed values)
- `wine_quality_modelling.ipynb`: the full notebook, with its original outputs

Section 1.1 needs the full file to re-run. Everything after it uses the sample.

## Method and findings

**1. Regression: alcohol and density**

- A simple linear regression of alcohol on density gives `alcohol = -338.45 x density + 346.94`.
- Correlation is -0.77, R-squared 0.60 and RMSE 0.77: denser wines tend to have less alcohol, and density alone explains about 60% of the variation.
- Higher-quality wines tend to have more alcohol (boxplot in section 1.3).

**2. Classification: k-nearest neighbours**

- Predicting the exact quality score from the eleven measurements.
- Tried k from 2 to 11. Accuracy ranged from 0.38 to 0.43, best at k = 9 or 10. Switching to Manhattan distance at k = 10 gave 0.44.
- Varying the share of data held out for testing from 20% to 80% moved accuracy between 0.39 and 0.50, with the best result at a 40% test share.
- Evaluated with a confusion matrix plus precision, recall and F1.

**3. Clustering: k-means**

- Silhouette scores for k from 2 to 9 were highest at k = 2 (0.49) and fell steadily after that; the elbow plot agreed.
- The two clusters do not correspond to quality. Their mean quality scores are 5.68 and 5.97, and every quality level is split across both.

**Overall:** these measurements carry real but limited signal about taster-assigned quality. Neither a distance-based classifier nor unsupervised clustering separates quality levels well.

## Limitations

- Quality scores are heavily imbalanced: most wines score 5 or 6 and only two score 3, so the rare classes are effectively never predicted.
- Features were not scaled. k-NN and k-means are both distance-based, so high-magnitude features such as total sulfur dioxide dominate. Standardising first is the obvious next step.
- Train/test results come from single splits, not cross-validation, so differences of a few points between settings are within noise.
- The outputs in the notebook are from the original 2023 run and have not been regenerated.

## Running it

```
pip install -r requirements.txt
jupyter notebook wine_quality_modelling.ipynb
```
