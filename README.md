It is well-established that junk food is usually lacking in nutrition, but does the same hold true for home-made foods? This project by Beomsuk Seo and Ethan Lin for DSC 80 at UCSD aims to answer that, and is a continuation of our previous project: https://ethanslin.github.io/Comfort-Food-Bias-Analysis/

# Introduction

We are attempting to predict the Nutritional Score (a system we established in <a href = 'https://ethanslin.github.io/Comfort-Food-Bias-Analysis/'>our previous project</a>) using a multi-class classification model. 

The response variable we used is Nutritional Score, since it is a strong indicator of the nutritional value of a food item, supported by parameters set by the FDA.

Our model was evaluated using R^2, since it tells us how well our model can predict the Nutritional Score in percentage terms, which is easier to understand than an abstract, extraordinarily large number, like RMSE.

For our fairness analysis, when we wished to use binary classification instead of multiclass classification, we instead evaluated our model using precision. Precision is used when false positives are more costly than false negatives. In our case, a false positive would be an unhealthy food mislabelled as a healthy food, which is more harmful than the alternative. Hence, we chose to use precision to evaluate our model.

The features we used to produce our multi-class classification model are strictly those provided by the author themselves upon publication. No data gathered post-publication, such as ratings and reviews, are factored in, so all features we are using are available at time of prediction.

# Baseline Model

For our baseline model, we used three features: `Calories`, `minutes`, and `n_steps`. 

`Calories` is a quantitative feature that we did not encode, while `minutes` and `n_steps` were quantitative features that we had converted into nominal features through binning. We then used OneHotEncoding to prepare it for our model.

The classifier we used was a DecisionTreeClassifier with a max_depth of 25. We chose DecisionTreeClassifier because we were most familiar with it.

Our model performed fairly well, with an R^2 test score of 0.7422. In general, an R^2 value greater than 0.5 is OK, which our model greatly exceeded. In addition, our `Calories` feature is generally agreed upon in the food health scientific community to be a good indicator of how healthy a food item is. This explains why it produces a sufficient model such as this. However, we can do better than this.

# Final Model

For our final model, we added two new features. Instead of simply passing the `Calories` feature into our model as before, we applied QuantileTransformer on `Calories`, as well as a new quantitative feature: `Sugar`.

QuantileTransformer reduces the impact of outliers, of which there are many in both `Calories` and `Sugar`. It does this by transforming these features to follow a uniform or normal distribution, allowing our model to be less biased towards extreme values, thereby producing a more accurate model.

We once again chose to use DecisionTreeClassifier. This time, we also used GridSearchCV to find the ideal hyperparameter: max_depth. We found that the max_depth that performed the best is 25, so we used it for our final model.

Our model performed very well. with an R^2 test score of 0.8472, which is a marked improvement of 0.1050. With this R^2 value of nearly 0.9, our model is a strong predictor of the Nutritional Score of a food item.

We have included a confusion matrix to visualize our model's performance.

<iframe src="assets/confusionmatrix.html" width=600 height=450 frameBorder=0></iframe>
