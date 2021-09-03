# How To:
### Running the code in this repo

The code for this analysis is broken into multiple pieces to avoid re-running pieces that take a significant amount of time. To produce the results presented here, run the code in the following order:

- `scrape_wunderground.ipynb` &mdash; [this notebook](https://github.com/hmlewis-astro/classify_aggressive_driving/blob/main/scrape_wunderground.ipynb) scrapes data information from Weather Underground for the date and time of each drive in the UAH-DriveSet; the data are written to a csv file (`UAH-DRIVESET-weather.csv`)
- `format_data.ipynb` &mdash; [this notebook](https://github.com/hmlewis-astro/classify_aggressive_driving/blob/main/format_data.ipynb) formats the data, derives some new features, and cleans the data; some initial EDA plots are included; the data are written to a new csv file for future use (`UAH-DRIVESET-classification.csv`)
- `test_class_model.ipynb` &mdash; various baseline classification models are explored in [this notebook](https://github.com/hmlewis-astro/classify_aggressive_driving/blob/main/test_class_models.ipynb); the final model is selected, tuned, and fit to the entire training set, and saved to a pickle file (`models/rf_final.pickle`)
- `final_class_model.ipynb` &mdash; [this notebook](https://github.com/hmlewis-astro/classify_aggressive_driving/blob/main/final_class_model.ipynb) presents the final, tuned and fitted, classification model through an interactive widget; the notebook can also be accessed through Binder
    - Click to [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/hmlewis-astro/classify_aggressive_driving/HEAD?filepath=final_class_model.ipynb).
