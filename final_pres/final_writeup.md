# Project Write-Up
## Classification of Aggressive Driving Behavior from Smartphone Data


### Abstract

The goal of this project was to classify driving behaviors as normal or abnormal (here, meaning either aggressive or drowsy driving), with the ultimate objective of implementing this model in a real-world, semi-autonomous vehicle. I utilized data from the [UAH-DriveSet](http://www.robesafe.uah.es/personal/eduardo.romera/uah-driveset/) to build a Random Forest classifier; the model is tuned to optimize the F2-score (weighting recall more heavily than precision), and attains F2 = 0.9925. After finalizing the model, I built an interactive Jupyter Binder to understand how the model classifications change with each feature from the data set.


### Design

The near-instantaneous analysis of driver behavior is an important part of the function and safety of semi-autonomous vehicles (i.e., vehicles with advanced driver assist systems). For example, many semi-autonomous vehicles are capable of warning/correcting the driver if they are drifting from their lane or if the person ahead of them has suddenly slowed their speed. Therefore, using data from built-in car sensors&mdash;or, in this case, from a smartphone app&mdash;to also quickly classify driving behaviors as aggressive (and to then warn drivers about those behaviors) will further improve the safety of such vehicles, and may lead to increased confidence in high- and fully-autonomous vehicles.

In this project, we seek to determine what aspects of a driver's behavior are most important for identifying aggressive driving behavior.

### Data

The UAH-DriveSet provides scores (i.e., features) for acceleration, braking, turning, weaving (between lanes), drifting (within lane), speeding, and following distance, as well as the driving style that a driver was simulating (normal vs. abnormal; i.e., the target) during each drive. Drives are recorded for a range of driving styles (normal vs. abnormal, including aggressive and drowsy), in a variety of environments (day vs. night, on highways vs. secondary roads).


I have also scraped weather conditions for the date, time, and location of each recorded drive, from Weather Underground. A multitude of weather data are scraped; however, the drives collected in this dataset were recorded only at times when there was no precipitation, when the temperature was above freezing, and when conditions were fair (i.e., low winds, low cloud cover). Therefore, on the whole, weather conditions do not vary significantly between drives. The only parameter scraped from Weather Underground that varies significantly between the various drives in the dataset is whether it is day or night.


### Algorithms

#### Cleaning & EDA
Given the NASA Landsat 8 image, the New York City Census block shapefile is used as reference to extract spectral radiances, and derive the median temperature within each Census block; outliers are replaced with the 1st and 99th percentile temperature values. From the observed temperature variations over the land area, a "heat index" in the range of 1 to 10 is calculated and assigned to each Census block, 10 being a land area with higher than median heat, 1 with lower than median heat.

Given the SQL database containing MTA turnstile data, I created a new table within that database with the available geolocation information. Using SQLAlchemy in Python, these tables are joined (on the `booth`/`C/A` and `unit`) so that each turnstile now also has an associated latitude and longitude. From the database containing all turnstile data for the year 2018, only data collected during summer months (i.e., between 06/01/2018 and 08/31/2018) were selected for this analysis.

I then calculated the time passed (in seconds) and the change in the turnstile `entries` counts between each reading; again, readings occur roughly every four hours. Here, there are two peculiarities in the data: (1) some turnstiles are counting backwards and (2) turnstiles appear to reset, leading to apparent increases in `entries` on the order of 10<sup>5</sup>-10<sup>7</sup> riders over just a few hours. To deal with these, I (1) always take the absolute value of the number of entries between measurements and (2) set an upper limit of 3 entries per turnstile per second. The later of these allows for a dynamic upper-limit to be set for each observation, depending on the time between measurements, rather than setting a single upper-limit.

#### Aggregation
The cleaned MTA data are then aggregated by station and linename, such that the net entries over the observed three month period can be derived. From the net entries, a "crowd index" in the range of 1 to 10 is calculated for each station, 10 being the most crowded, 1 being the least.

The MTA and heat data are then joined together based on the spatial location of each station.

By combining the derived "heat index" and "crowd index" for each station, I calculate a "risk index" (again, scaled from 1 to 10, with 10 being high risk) for heat-illness at each station.

#### Visualization


### Tools
- Selenium, and BeautifulSoup for web scraping
- Pandas and Numpy for data analysis and exploration
- Scikit-learn for building, tuning, training, and testing the various baseline and final models
- Matplotlib and Seaborn for plotting and visualizations
- IPywidgets and Binder for publicly available, interactive visualizations

### Communication

In addition to the slides and visuals presented here, the interactive Binder notebook will be included in a forthcoming blog post.
