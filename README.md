# Applied Data Analysis (ADA)

## Data Story:

Our datastory can be found [here](https://theazouz.github.io/CS401-Extension-of-Friendship-and-mobility/).

## Group work for P4:

In this work, we test the discretization of the assigned paper to find home locations. Then, we explore machine learning
methods to find home location of users. Finally, we try to find friends meetup patterns (spatial and temporal)

## Overview of files:

- `data/`: contains the data and the dumps from feature engineering.
- `models/`: contains the pre-trained ML models.
- `home_location_test.ipynb`: notebook where we test the discretization, the chosen ML model (SVM) and the effect of our
  predictions on distances between friends.
- `feature_engineering.ipynb`: notebook where we perform our feature engineering.
- `home_location_prediction.ipynb`: notebook where we construct the ML models, fit them and compare them to choose a
  model to use.
- `friends_checkin_patterns.ipynb`: notebook where we study friends meet up patterns (spatial and temporal)
- `metrics.py`: contains some metrics used for the model fitting.

## Data:

Some of our data is not provided in the repository because of its large size. It can be
found [here](https://drive.google.com/drive/folders/1POXvWQgKXmzLTpDJli-bqfKlldvg4i4n). These files are to be downloaded
and places under `data/`.

The data was pre-processed. Our data is from Foursquare. The raw files are `dataset_WWW_Checkins_anonymized.txt`
and `raw_POIs.txt`. They were merged to create `foursquare_checkin_data.csv.zip`.

## Dependencies:

Our code uses the standard Conda packages. In addition, we use [pandarellel](https://pypi.org/project/pandarallel/),
[networkX](https://networkx.org/), [sklearn](https://scikit-learn.org/stable/), [keras](https://keras.io/)
, [scipy](https://www.scipy.org/) and [imblearn](https://pypi.org/project/imblearn/)

## Group Members Contribution:

Each member focused mainly on:

- Ayman: testing the methods.
- Selim: patterns in friends meetups and data pre-processing
- Rami: Explore machine learning methods There was also a lot of contribution in one another's notebooks to make the
  work evenly spread between team members.