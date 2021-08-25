# Project Proposal
## Classification of Aggressive Driving Behavior from Smartphone Data

### Question & Background:
The near-instantaneous analysis of driver behavior is an important part of the function and safety of semi-autonomous vehicles (i.e., vehicles with advanced driver assist systems). For example, many semi-autonomous vehicles are capable of warning/correcting the driver if they are drifting from their lane or if the person ahead of them has suddenly slowed their speed. Therefore, using data from built-in car sensors&mdash;or, in this case, from a smartphone app&mdash;to also quickly classify driving behaviors as aggressive (and to then inhibit those behaviors) will further improve the safety of such vehicles, and may lead to increased confidence in high- and fully-autonomous vehicles.

In this project, we seek to **determine what aspects (features) of a driver's behavior are most important for identifying (classifying) aggressive driving behavior**.


### Data description:
[UAH-DriveSet](http://www.robesafe.uah.es/personal/eduardo.romera/uah-driveset/) is a public collection of driving data captured by a drive monitoring app, and comprises more than 30,000 observations [(Romera et al. 2016)](http://www.robesafe.uah.es/personal/eduardo.romera/pdfs/Romera16itsc.pdf). The data include both raw videos (these may be used for a deep learning project later in the bootcamp), as well as processed `.csv` files scoring and classifying each drive in the data set. The data are collected for various drivers in different driving environments, with varying driving styles (**classes**: non-aggressive/negative class, aggressive/positive class). The **features** recorded for each driver are scores for:
- acceleration
- braking
- turning
- weaving (between lanes)
- drifting (within lane)
- speeding
- following distance

Weather conditions&mdash;specifically, precipitation (e.g., is it raining/snowing?), temperature (e.g., is it below freezing/potentially icy?), and wind speed (e.g., are wind speeds high/dangerous?)&mdash;have an impact on whether a specific driving maneuver is classified as non-aggressive/aggressive. Because we have the date, time, and location of each recorded drive, we can obtain information on the weather conditions for each drive from Weather Underground, which retains historical daily and hourly observations of weather conditions for most major airports across the globe. The following **features** will be scraped from WUnderground for each drive in the data set:
- temperature
- wind speed
- wind gust
- precipitation
- weather condition (e.g., clear, cloudy, foggy, light/heavy snow, etc.)
- day/twilight/night


### Tools:
A majority of the relevant data is available via direct download; however, to scrape the associated hourly weather data, the `requests` and `BeautifulSoup` packages in Python will be utilized, along with Selenium and ChromeDriver to allow each Weather Underground page to fully load before scraping.

The scraped data will be stored in a `.csv` file, with rows corresponding for each scraped date-time. Both the downloaded data and the scraped data will be read in to python and manipulated using the `pandas` package. The `pandas` package will be used for initial exploratory data analysis and feature engineering.

The classification models in `scikit-learn` will be used to build, validate, and test baseline and further expanded/refined models. If we find that the target class is imbalanced in the data set, we will also weight the classes or resample the data. In building these models, for driver safety, we will want to minimize computation time, as well as the number of false negative classifications (i.e., classifying a behavior as non-aggressive when it is).

We will use the matplotlib package to create visualizations of the resulting model metrics and classification results.


### MVP:

The minimal viable product (MVP) for this project will likely be a few, simple, baseline classification models (e.g., logit, KNN, decision tree) including just a few features (e.g., acceleration and braking scores, following distance), so that we can begin to understand what the most important features will be in classifying driving behavior.
