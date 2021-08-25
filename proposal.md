# Project Proposal
## Classification of Aggressive Driving Behavior from Smartphone Data




### Question:



### Data description:


### Tools:
A majority of the relevant data is available via direct download; however, to scrape the associated hourly weather data, the `requests` and `BeautifulSoup` packages in Python will be utilized, along with Selenium and ChromeDriver to allow each Weather Underground page to fully load before scraping.

The scraped data will be stored in a `.csv` file, with rows corresponding for each scraped date-time. Both the downloaded data and the scraped data will be read in to python and manipulated using the `pandas` package. The `pandas` package will be used for initial exploratory data analysis and feature engineering.

The classification models in `scikit-learn` will be used to build, validate, and test baseline and further expanded/refined models. If we find that the target class is imbalanced in the dataset, we will also weight the classes or resample the data.

We will use the matplotlib package to create visualizations of the resulting model metrics and classification results.

### MVP:

The minimal viable product (MVP) for this project will likely be...
