# Minimum Viable Product
## Classification of Aggressive Driving Behavior from Smartphone Data

The goal of this project is to classify driving behaviors as normal or not normal (e.g., aggressive or drowsy-driving) based on data from smartphone GPS and inertial sensors (accelerometers and gyroscopes).

To do this, I am using the [UAH-DriveSet](http://www.robesafe.uah.es/personal/eduardo.romera/uah-driveset/); the dataset is made up of a series of 15-20 minute drives performed by various drivers in different driving environments, with each drive simulating a series of different behaviors: normal, drowsy, and aggressive driving. Here, drowsy and aggressive driving are grouped into a single "non-normal" driving class. The processed dataset provides scores calculated in 60 second windows of each drive (scaled from 0 to 100) for e.g., acceleration, braking, and weaving. I have also scraped weather data from Weather Underground&mdash;including temperature, wind speed, and day/night driving conditions&mdash;for each drive, because weather conditions can have an impact _how_ non-normally someone can drive.  However, the drives collected in this dataset were recorded only at times when there was no precipitation, when the temperature was above freezing, and when conditions were fair (i.e., low winds, low cloud cover); therefore, the only parameter scraped from WUnderground that varies significantly between the different drives in the dataset is whether it is day or night.


To begin building a classification model, I create baseline models with just the acceleration, braking, and speeding scores as features, using KNN, logistic regression, decision tree, and Random Forest. The KNN, decision tree, and Random Forest models (all have F2 > 0.91) perform marginally better than the logistic regression model (F2 ~ 0.85), so I have dropped the logit model from further consideration.

In expanding and refining these three models (KNN, decision tree, Random Forest), I have added all drive specific features (scores for acceleration, braking, turning, weaving, drifting, speeding, and following), as well as a dummy variable for day- vs. night-time driving. For each of these models, I run GridSearchCV over:
- KNN: _k_ from 3 to 100, with a step size of 2
- DT: _max_depth_ from 5 to 20; _min_samples_split_ from 2 to 10; _min_samples_leaf_ from 1 to 5; _criterion_ as entropy or gini
- RF: _max_depth_ from 5 to 20; _criterion_ as entropy or gini; _max_features_ as auto, sqrt, or log2

The next figure (center) shows the residuals, and the last figure (right) shows the q-q plot. The q-q plot indicates that the predicted values are heavy-tailed, meaning that, in it's current form, the model does not correctly summarize the underlying relationship between the selected features and the target. To improve the model, I plan to transform the monetary target and features (i.e., domestic opening weekend gross, budget) to log scale.

<p float="left" align="center">
  <img src="figures/lr_basic.png" width="320" />
  <img src="figures/lr_basic_resid.png" width="320" />
  <img src="figures/lr_basic_qq.png" width="320" />
</p>


These results show that a significant proportion of the variance is explained by the current model; however, I hope to improve the R<sup>2</sup> by (1) log-scaling some of the features, (2) incorporating the studio and movie genres as dummy variables, and (3) testing LASSO and Ridge regression models to find the most important features in the model. Because I plan to use the resulting model to _interpret_ the impact of lead actor gender and age on lifetime movie gross, I am leaning towards using Ridge regression to avoid setting any of the coefficients of interest (i.e., lead actor age or gender) equal to 0.
