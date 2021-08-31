# Minimum Viable Product
## Classification of Aggressive Driving Behavior from Smartphone Data

The goal of this project is to classify driving behaviors as normal or not normal (e.g., aggressive or drowsy-driving) based on data from smartphone GPS and inertial sensors (accelerometers and gyroscopes).

To do this, I am using the [UAH-DriveSet](http://www.robesafe.uah.es/personal/eduardo.romera/uah-driveset/); the dataset is made up of a series of 15-20 minute drives performed by various drivers in different driving environments, with each drive simulating a series of different behaviors: normal, drowsy, and aggressive driving. Here, drowsy and aggressive driving are grouped into a single "non-normal" driving class. The processed dataset provides scores calculated in 60 second windows of each drive (scaled from 0 to 100) for e.g., acceleration, braking, and weaving. I have also scraped weather data from Weather Underground&mdash;including temperature, wind speed, and day/night driving conditions&mdash;for each drive, because weather conditions can have an impact _how_ non-normally someone can drive.  However, the drives collected in this dataset were recorded only at times when there was no precipitation, when the temperature was above freezing, and when conditions were fair (i.e., low winds, low cloud cover); therefore, the only parameter scraped from WUnderground that varies significantly between the different drives in the dataset is whether it is day or night.


To begin building a classification model, I create baseline models with just the acceleration, braking, and speeding scores as features, using KNN, logistic regression, decision tree, and Random Forest. The KNN, decision tree, and Random Forest models (all have F2 > 0.91) perform marginally better than the logistic regression model (F2 ~ 0.85), so I have dropped the logit model from further consideration.

In expanding and refining these three models (KNN, decision tree, Random Forest), I have added all drive specific features (scores for acceleration, braking, turning, weaving, drifting, speeding, and following), as well as a dummy variable for day- vs. night-time driving. For each of these models, I run GridSearchCV over:
- KNN: _k_ from 3 to 100, with a step size of 2
- DT: _max_depth_ from 5 to 20; _min_samples_split_ from 2 to 10; _min_samples_leaf_ from 1 to 5; _criterion_ as entropy or gini
- RF: _max_depth_ from 5 to 20; _criterion_ as entropy or gini; _max_features_ as auto, sqrt, or log2

The figure below shows the ROC curve (based on the validation set) for the best estimator for each model type. Note, the _x_- and _y_-axes are formatted to show the upper left corner of the typical ROC curve; in this project, the AUC is large (approaching 1) for all three models, so to see the slight differences between the model, we need to zoom into this part of the plot.

<p float="left" align="center">
  <img src="roc_curves_base.png" width="500" />
</p>

These results show that the Random Forest model (AUC = 0.9867, F2 = 0.9834) performs very well on the validation set; I have dropped the KNN and decision tree models from further consideration. As a next step, I plan to test a gradient boosted tree model (e.g., XGBoost) to compare with the Random Forest model.
