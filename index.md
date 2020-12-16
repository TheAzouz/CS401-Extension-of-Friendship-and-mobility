## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/TheAzouz/Project_ADA.github.io/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

# **Header 1**  
# **Header 2**  
# **III) Checkin patterns between friends:**  
In this part, we seek to finding some meeting patterns between friends. We say two friends have met if they checked in the same place with at most one hour difference.  
We will do three tasks in this part : 
- Firstly, we will try to find the distribution of checkins as a function of the distance from home and compare two cases : case where users checkin with a friend a the general case where users check in not taking into account friends.
- Secondly, we will try to find place checkin patterns : we will study the places where friends meet, and how likely some friends are to meet in a certain place depending on the whether it s a work day or week end day. Then, we will study the distribution of checkins in places depending on the time of the day (day or night).  

## *1) Checkin patterns and ditribution :*  

We begin by getting the probability of distribution for two different datasets : dataset where we take into account all the checkins and dataset where we only study checkins made with friends. 
In both cases, we only study users who checked in at least once in their homes and assume their home is located at the average of checkins labeled with home `Home (private)`. 
We plot (loglog) the distribution for both datasets as a function from the distance from home and try to describe the case where a user only moves to meet friends with a function of the type : $ax^b$
#################   GRAPH1   ###########################   
From the plot above, we can draw some conclusions :
- After fitting the curves, we get the approximated equtions for the probability distribution of the number of checkis as a function of the distance from home:
$$ P(x)=
\begin{cases}
    0.1e^{-0.44x} & \text{if x<20 km}\\
    0.35e^{-1.3x} & \text{otherwise}
\end{cases}
$$
- We notice a change in the slope at a distance of approximately 20km distance from home. This behavior is similar to the one described in the paper 'Friendship and Mobility: User Movement In Location-Based Social Networks' (figure 1).  
However : 
  - The distance from home where the shift happened is different (20km here vs 100km in the paper)  
  - The slope are different than the ones described in the paper. In fact, while the slope is smaller for small distances in the paper (-1.9 < -0.9), the behaviour is different in our study (-0.44 > -1.3)
- Finally, we notice in the plots that whether a user visited a friend or not does not make much differences in the overall behavior of checkins. 
We make the hypothesis that the behavior is the same and we test it. The null hypothesis is that friends don't have any influence on a user's movement.  
After confirming that our data distribution is not normal (using a Kolmogorov Smirnov test), we use a Wilcoxon test to check whether friends have influence or not on the movement of a people. The result is interesting : having found a p-value of 0.0, we strongly reject the hypothesis and we conclude that even if the general behavior seems the same, the quantitative results show the contary.

## *2) Place checkin patterns :*  

- For the two remaining parts, we will only be working with the dataset containing only checkins with friends. We make this choice because the goal of the paper is to predict mobility and see the influence of friends.
- To be able to find place patterns, we categorize our dataset:
    - places into different types : `Eat` ,`Study` , `Drink`,`Culture`,`Home`, `Move`,`Consume`,`Work`,`Entertain`,`Sport`  
    - day of the week into two types : `Working days`,`Week end day`
    - time of the day : `day` (between 5h and 17h): ,`night` (between 17h and 5h)
- Then, we will study the probability that people meet in different places as a function of:
    - Time of the day
    - Day of the week  
### i) Day of the week checkin patterns  
#################### graph 2 ########################  
- After computing the probability for checking in a certain place as a function of the day type, we conclude that people are the most likely to be studying. 
This observation can be explained by the fact that students are the most likely to use sociaal media, so the most number of checkins can be found among students.

- Then, to be able to draw further conclusions, we do some processing to our results:
    - Get probability/day :
        -Divide probabilities that happened in work day by 5
        -Divide probabilities that happened in week end day by 2
    - Get ratio of results found
    - The final equation we find for each place is :
    $$
    final ratio = \frac{\frac{P_{workday}}{5}-\frac{P_{workend}}{2}}{\frac{P_{weekday}}{5}}
    $$
- In the end:  
    - If this difference is positive : people are more likely to checkin in the place in a working day  
    - If this difference is negative : people are more likely to checkin in the place in a week end day  
    - The absolute value gives us the magnitude of the absolute ratio  
####################################   GRAPH 3 #############################################    
- After observing the figure, we conclude that people tend to meet their friends more in work or study places during the week. This can be explained by the fact that people usually have their coworkers and classmates as friends on social media. Studying or working is part of people's obligations and these are task are generally proceeded during the week
- However, when it comes to free time (week end for most of people), people choose to meet their friends in diverting places (every other category that doesn't involve working or studying). Specifically, people are the most likely to go out in weekend to have drinks or to entertainement places.
- Finally, people tend to spend their day working and studying, and then spend their evening and night in diverting places (eating, having drinks.  
### ii) Period of the day checkin patterns:  
- Now we move to studying the times of the day friends are the most likely to meet.  
####################################   GRAPH 4 #############################################  
- We first notice that :  
    - The biggest probability of checking in with friends during day occurs during studying
    - The biggest probability of checking in with friends during night occurs during night.
- Then, we do a similar processing work than the one we did in the previous part : we compute the ratio of checkins in day or at night.  
We use the equation below to compute the ratios:
$$
final ratio = \frac{P_{daycheckin}-P_{nightcheckin}}{P_{daycheckin}}
$$
################################## GRAPH 5 ##########################################  
- We can see that friends are more likely to meet during the day to study or work. Moreover, they are more likely to meet at night to have night, even if the difference is small.
- We can also conlude that most checkins happen to be during the day and that people tend less to checkin at night. 


**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)


For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/TheAzouz/Project_ADA.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
