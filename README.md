# PRO_HACK2020
Team05 solution to the PROCHACK 2020 contest

(Finished 2nd on the public leaderbord and 18th on the private leaderboard.)

Our approach to provide more information to the model was to add time related features. For all the features, we added exponential moving averages of past feature values from the same galaxy.
We also ended up adding future values of the features as they can be predictive of the current trend and can be constructed for the test set as well. This is not completely logical in a realistic set-up but it apparently improved the score so we went for it.
The latest features we added were past targets for the same galaxy. The only issue here is that the test points for a given galaxy lie in consecutive years and therefore the target at year-1 feature isnt available for most test points
What we ended up doing was using the last available target as a feature. We trained a model on it. Then used the evaluation of test set to updated the target related features to hopefully more accurate ones. We also added finance inspired time series indicators, hoping our model can understand some trend in the evolution of the target and its relation with the trend of the features.
The procedure is repeated 3-4 times and we then fit a model on the final features.


As for the optimisation part, we first used scipy.optimize on our predictions, as we thought the constraint was too hard to meet. The method didnt even give optimal solutions.
Later on we found out the optimal solution was handing 100 to the first 500 and 0 to the rest (and the constraint was always satisfied with our prediction) .
Of course this is true if the ranking of our predictions was accurate, and this method yields huge loss in score in case of a ranking mistake.
We therefore used this allocation method on multiple predictions (from different models or from individual trees in a random forest) and averaged the allocations.
we hoped this would give an accurate sense of uncertainty.
Unfortunetaly, we couldn't produce a neural net that gave a proper validation error, probably due to the huge number of features and low number of points. So we couldn't try fitting the data on log likelihood to predict a mean and std of prediction. We also wouldve loved trying dropout as a measure of uncertainty, leaving it activated during evaluation.
So we resorted to ditributing the allocation manually in a linear fashion to account for uncertainty as it produced a better leaderboard score.
The final improvement we made was averaging these manual allocations accross supposedly independant predictions.
