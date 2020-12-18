---
title: Applied Data Analysis (ADA) - extensionn of the paper `friendship and mobility user movement in location-based social networks` 
---
# **I - Discretization Testing:**

In this part, we will test the performance of the home localization method proposed by the paper. We will compute the
distance between localized homes, and the known homes of users. We will use a different dataset from the one used in the
paper, as there were not labeled check-ins.`

## *1 - True Home Location:*

We first start by selecting the homes of users with low variance. This is done by computing the standard deviation and mean of latitude and longitude of each user's check-ins labeled as Home. Then, we construct two points by respectively substracting and adding the standard deviation from the mean of the latitude and longitude coordinates. If this distance is smaller than 100m, we can be confident about the home location.

## *2 - Discretization:*

The discretization method proposed by the paper has the following steps:

- We discretize the world into 25km-side squares.
- For each user, find the square with the most check-ins. Then, the home location is set as the average of the
  corresponding check-in locations in the most frequented square.

We use this method to find home locations of the users of the new dataset.

## *3 - Testing:*

We start by computing the distance between the true home location, and the home location provided by discretization.
Then, we visualize the distribution of distances.

<p style="text-align:center;"><img src="assets/part1/discretization_test.png" style="width: 50%"/></p>

We see that the distribution follows a power law that can be approximated by:
<p style="text-align:center;"><img src="assets/part1/powerlaw.jpeg"/></p>

This indicates that there are users whose predicted home location is far by orders of magnitude from the original actual
home location.

Moreover, if we take a look at the CDF, we see that to achieve high accuracy, we need to tolerate large distances in the
order of thousands of kilometers.

<p style="text-align:center;"><img src="assets/part1/discretization_test_CDF.png" style="width: 50%"/></p>

In fact, the paper claims that their method reaches 85% accuracy through manual verification. Nevertheless, we see that
this does not apply to Foursquare dataset. Indeed, we need to tolerate distances up to 8'305km, which is larger than Europe.

In the following section, we will try to use Machine Learning methods to find the homes of users, and we will compare our
solution to the method proposed by the paper in terms of performance on the same dataset.

# **Home locations predictions**

## *1 - Method:*

## *i - Feature engineering:*

In order to predict users' home location, we based our work on the paper `Fine-scale prediction of people's home location using social media footprints` by H.Kavak et al. They used DBSCAN unsupervised density-based clustring algorithm which creates clusters of points that can be connected to each other within a given radius. The clustering is performed on each user's check-ins. This is equivalent to the discretization approach done in Friendship and Mobility paper. 

Later, the authors created the following mobilitiy features for each cluster:

- Checkin Ratio (CR): It's the ratio of check-ins per cluster
- Checkin During Midnight (MR): In this feature, we consider only the checkins between 00 am and 5 am and compute the checkin ratio per cluster
- Last Checkin of the day (EDR): We consider only the last check-in of the day happeninig befor 3 am and compute the ratio over the cluster. This feature suggests that the last checkin of the day is most likely coming from home.
- Last Checkin with inactive midnight: Similarely to the previous feature with the exception that we drop the last checkin if it is after midnight
- PageRank and ReversePageRank:  These features measure the importance of cluster based on the time made between each transition.

After creating these feature for each cluster, we label each cluster with the most home tags as a home cluster, whereas the remaining one are labeled as not home.

## *ii - Classification:*

- Now that we have labeled clusters, we can use a classifier to predict the home location. While authors of the paper `Fine-scale prediction of people's home location using social media footprints` only use a Sector Vector Machine Classifier SVMC, we buildt three different models and compared them one to another on two different datasets.
- After visualizing the distribution of the location of check-ins for the Foursquare, Gowalla and Brightkite datasets, we noticed that most of the check-ins in Gowalla and Brightkite occur in the USA while they are more distributed over the world for the Foursquare dataset. 
- Knowing this, we chose to compare models on the dataset with check-ins only in USA, and on the whole dataset.
We build a SVMC model, a Neural Network NN and a random forest classifier and compared their scores. The SVMC was build with a linear activation function, while the Neural Network consisted of 5 hidden layers. Each hidden layer was followed by a dropout to avoid overfitting.
- After running the models on the two different sets, we found the following resluts :
<p style="text-align:center;"><img src="assets/part3/rami.png" style="width: 70%"/></p>
- These results show that the f1 score is overall better when we only consider check-ins made in one same country. This can be explained by the fact that the social behaviour is different around the world. Working on just a country reduces the number of parameters to be taken into account and therefore lets the models perform better. We can make the hypothesis that if we restrained our work to just some states (New York or Chicago for example), our f1 score would become even better.
- Finally, the SVMC performed better than other models even if the activation function was just linear. More time and computational power would have let us create and perform better models

## *iii - Prediction testing:*


<p style="text-align:center;"><img src="assets/part1/prediction_test.png" style="width: 50%"/></p>

<p style="text-align:center;"><img src="assets/part1/prediction_test_CDF.png" style="width: 50%"/></p>

# **III - Checkin Patterns Between Friends:**

In this part, we seek to find some meeting patterns between friends. We say two friends have met if they checked in the
same place with at most one-hour difference.  
We will do three tasks in this part:

- Firstly, we will try to find the distribution of check-ins as a function of the distance from home and compare two
  cases: the case where users check-in with a friend, and the general case where users check in not taking into account
  friends.
- Secondly, we will try to find place check-in patterns: we will study the places where friends meet, and how likely
  some friends are to meet in a certain place depending on the whether it s a work day or weekend day. Then, we will
  study the distribution of check-ins in places depending on the time of the day (day or night).

## *1 - Checkin Patterns and Ditribution:*

We begin by getting the probability of distribution for two different datasets: dataset where we take into account all
the check-ins and dataset where we only study check-ins made with friends. In both cases, we only study users who
checked in at least once in their homes and assume their home is located at the average of check-ins labeled with
home `Home (private)`. We plot (loglog) the distribution for both datasets as a function from the distance from home and
try to describe the case where a user only moves to meet friends with a function of the type:
<p style="text-align:center;"><img src="assets/part3/latex-selim.jpg" style="width: 10%; height: 10%"/></p>
We now move to the visualisation
<p style="text-align:center;"><img src="assets/part3/img1-selim.png" style="width: 90%; height: 80%"/></p>

From the plot above, we can draw some conclusions:

- After fitting the curves, we get the approximated equtions for the probability distribution of the number of checkis
  as a function of the distance from home:

<p style="text-align:center;"><img src="assets/part3/latex0-selim.jpg" style="width: 40%; height: 40%"/></p>

- We notice a change in the slope at a distance of approximately 20km distance from home. This behavior is similar to
  the one described in the paper 'Friendship and Mobility: User Movement In Location-Based Social Networks' (figure 1)
  .  
  However:
    - The distance from home where the shift happened is different (20km here vs 100km in the paper)
    - The slope are different than the ones described in the paper. In fact, while the slope is smaller for small
      distances in the paper (-1.9 < -0.9), the behaviour is different in our study (-0.44 > -1.3)
- Finally, we notice in the plots that whether a user visited a friend or not does not make much differences in the
  overall behavior of check-ins. We make the hypothesis that the behavior is the same and we test it. The null
  hypothesis is that friends don't have any influence on a user's movement.  
  After confirming that our data distribution is not normal (using a Kolmogorov Smirnov test), we use a Wilcoxon test to
  check whether friends have influence or not on the movement of a people. The result is interesting: having found a
  p-value of 0.0, we strongly reject the hypothesis and we conclude that even if the general behavior seems the same,
  the quantitative results show the contary.

## *2 - Place Check-in Patterns:*

- For the two remaining parts, we will only be working with the dataset containing only check-ins with friends. We make
  this choice because the goal of the paper is to predict mobility and see the influence of friends.
- To be able to find place patterns, we categorize our dataset:
    - places into different types: `Eat`, `Study`, `Drink`, `Culture`, `Home`, `Move`, `Consume`, `Work`, `Entertain`
      , `Sport`
    - day of the week into two types: `Working days`, `Week end day`
    - time of the day: `day` (between 5h and 17h) and `night` (between 17h and 5h)
- Then, we will study the probability that people meet in different places as a function of:
    - Time of the day
    - Day of the week

### i - Day of the Week Check-in Patterns

<p style="text-align:center;"><img src="assets/part3/img2-selim.png" style="width: 80%; height: 40%"/></p>

- After computing the probability for checking in a certain place as a function of the day type, we conclude that people
  are the most likely to be studying. This observation can be explained by the fact that students are the most likely to
  use sociaal media, so the most number of check-ins can be found among students.

- Then, to be able to draw further conclusions, we do some processing to our results:
    - Get probability/day:
        - Divide probabilities that happened in work day by 5
        - Divide probabilities that happened in week end day by 2
    - Get ratio of results found
    - The final equation we find for each place is:

<p style="text-align:center;"><img src="assets/part3/latex-selim1.jpg" style="width: 40%; height: 40%"/></p>

- In the end:
    - If this difference is positive: people are more likely to check-in in the place in a working day
    - If this difference is negative: people are more likely to check-in in the place in a week end day
    - The absolute value gives us the magnitude of the absolute ratio

<p style="text-align:center;"><img src="assets/part3/img3-selim.png" style="width: 100%; height: 100%"/></p>

- After observing the figure, we conclude that people tend to meet their friends more in work or study places during the
  week. This can be explained by the fact that people usually have their coworkers and classmates as friends on social
  media. Studying or working is part of people's obligations and these are task are generally proceeded during the week
- However, when it comes to free time (week end for most of people), people choose to meet their friends in diverting
  places (every other category that doesn't involve working or studying). Specifically, people are the most likely to go
  out in weekend to have drinks or to entertainement places.
- Finally, people tend to spend their day working and studying, and then spend their evening and night in diverting
  places (eating, having drinks.

### ii - Period of the Day Check-in Patterns:

- Now we move to studying the times of the day friends are the most likely to meet.

<p style="text-align:center;"><img src="assets/part3/img4-selim.png" style="width: 90%; height: 80%"/></p>

- We first notice that:
    - The biggest probability of checking in with friends during day occurs during studying
    - The biggest probability of checking in with friends during night occurs during night.
- Then, we do a similar processing work than the one we did in the previous part: we compute the ratio of check-ins in
  day or at night.  
  We use the equation below to compute the ratios:

<p style="text-align:center;"><img src="assets/part3/latex2-selim.jpg" style="width: 40%; height: 40%"/></p>

<p style="text-align:center;"><img src="assets/part3/img5-selim.png" style="width: 90%; height: 80%"/></p>

- We can see that friends are more likely to meet during the day to study or work. Moreover, they are more likely to
  meet at night to have night, even if the difference is small.
- We can also conlude that most check-ins happen to be during the day and that people tend less to check-in at night.
