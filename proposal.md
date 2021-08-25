# Project Proposal
## Classification of Aggressive Driving Behavior from Smartphone Data




### Question:
What aspects (features) of a driver's behavior are most important for identifying (classifying) aggressive driving behavior?


### Data description:
[UAH-DriveSet](http://www.robesafe.uah.es/personal/eduardo.romera/uah-driveset/) is a public collection of data captured by a drive monitoring app. The data are collected for various drivers in different driving environments, with varying driving styles (**classes**: normal, drowsy, aggressive). The **features** recorded for each driver are scores for:
- acceleration
- braking
- turning
- weaving (between lanes)
- drifting (within lane)
- speeding
- following distance


This dataset tries to facilitate progress in the field of driving analysis by providing a large amount of variables that were captured and processed by all the sensors and capabilities of a smartphone during independent driving tests. The application was run on 6 different drivers and vehicles, performing 3 different behaviors (normal, drowsy and aggressive) on two types of roads (motorway and secondary road), resulting in more than 500 minutes of naturalistic driving with its associated raw data and additional semantic information, together with the video recordings of the trips. You can download the dataset on the Download Section.


### Tools:
A majority of the relevant data is available via direct download; however, to scrape the associated hourly weather data, the `requests` and `BeautifulSoup` packages in Python will be utilized, along with Selenium and ChromeDriver to allow each Weather Underground page to fully load before scraping.

The scraped data will be stored in a `.csv` file, with rows corresponding for each scraped date-time. Both the downloaded data and the scraped data will be read in to python and manipulated using the `pandas` package. The `pandas` package will be used for initial exploratory data analysis and feature engineering.

The classification models in `scikit-learn` will be used to build, validate, and test baseline and further expanded/refined models. If we find that the target class is imbalanced in the dataset, we will also weight the classes or resample the data.

We will use the matplotlib package to create visualizations of the resulting model metrics and classification results.

### MVP:

The minimal viable product (MVP) for this project will likely be a few, simple, baseline classification models (e.g., logit, KNN, decision tree) including just a few features (e.g., acceleration and braking scores, following distance), so that we can begin to understand what the most important features will be in classifying driving behavior.
