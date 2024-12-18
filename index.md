---
layout: page
title: The (un)Success of Inclusive Cinema
subtitle: Exploring the Connection Between Representation and Movie Triumph
cover-img: /assets/img/header_img.png
---

## Introduction

Movies don't just entertain—they inspire and shape how we see the world. A little girl watching a character like her become a doctor might suddenly believe she can do the same. Cinema reflects our emotions and dreams, and in doing so, it helps shape our collective imagination.
{: .text-justify}

Yet, this incredible influence comes with responsibility. Films can perpetuate harmful stereotypes or amplify fear and prejudice. Worse, a lack of representation leaves entire communities invisible, excluded from the collective narrative. Thankfully, awareness of this issue has grown over the past few decades, leading to slow but meaningful progress in diversifying casts.
{: .text-justify}

Today, even the Oscars have introduced diversity quotas for Best Picture eligibility, recognizing the importance of representation in storytelling. But beyond quotas and moral imperatives, there's a compelling question: can a diverse cast also drive cinematic success? Could the power of representation become a key motivator for filmmakers to embrace inclusivity? Let's dive into a data story to uncover the impact of diversity on movie success. 
{: .text-justify}

<p align="center">
    <img src="assets/img/great_success.jpg" alt="" width="300"/> <!-- Adjust width as needed -->
</p>

## Setting the Stage

In this project, we will dive into the provided CMU Movie Summary Corpus Dataset containing 42'306 plot summaries and metadata for films released from 1893 to 2013. To define "success" we have expanded our resources, integrating data on award nominations and audience ratings. Since our focus is on cast diversity, we exclude all films with only one character, as they do not contribute meaningfully to the diversity analysis. Our final cleaned dataset comprises 13'000 films.
{: .text-justify}

### About Our Inspiring Datasets

<div style="display: flex; align-items: center; justify-content: center;">
    <!-- Left Side: Image -->
    <div style="margin-right: 20px;">
        <img src="assets/img/video_camera.png" alt="" style="max-width: 150px; height: auto;">
    </div>
    
    <!-- Right Side: Text -->
    <div>
        <b>Our primary dataset provides key elements such as:</b>
        <ul>
            <li>Movie Name</li>
            <li>Release Year</li>
            <li>Actor Ethnicity</li>
            <li>Box Office Revenue</li>
        </ul>
    </div>
</div>


<div style="display: flex; align-items: center; justify-content: center;">
    <!-- Left Side: Text -->
    <div style="margin-right: 20px;">
        <b>To enrich our analysis, we brought in additional datasets:</b>
        <ul>
            <li>IMDb datasets for movie titles and user ratings on a 0-10 scale.</li>
            <li>Award data from globally recognized institutions like the Oscars, Golden Globes, Filmfare, and others.</li>
            <li>A mapping dataset linking Freebase IDs to Wikidata, enabling deeper insights.</li>
        </ul>
    </div>
    
    <!-- Right Side: Image -->
    <div>
        <img src="assets/img/oscar.png" alt="" style="max-width: 150px; height: auto;">
    </div>
</div>

{% include distribution_realease_date.html %}

Our dataset spans nearly a century of filmmaking, but for our analysis, we determined that the period between 1960 and 2013 offers a rich and sufficient sample of movies. Therefore, we will focus on this era for the remainder of our study.
{: .text-justify}

### What We'll Explore

In this project, we set out to uncover how cast diversity influences both the financial and critical success of films—and the results might surprise you.<br> {: .text-justify}
Does diversity impact box office performance? To find out, we will dive into the data using linear regression to pinpoint trends while accounting for factors like movie length and release year.<br>
But the intrigue does not stop at ticket sales—what about awards? Are films with more diverse casts more likely to snag nominations for prestigious accolades like the Oscars or Golden Globes? With a little help from propensity score matching, we can compare similar films and see if diversity tips the scales or not...<br>
Lastly, let’s not forget the critics and audiences, do movies with diverse casts get higher ratings on IMDb? Through logistic regression and statistical analysis, we aim to decode these patterns.<br>
Armed with these analytical tools, we are ready to explore how representation is not always a recipe for success. The data reveals a more complex story, and we are here to share it with you.<br>
{: .text-justify}

## What Makes a Cast Inclusive?

<p align="center">
<img src="assets/img/Intouchable.gif" alt=""/>
</p>

### Clustering the Ethnicities

When we first have a look at the ethnicities, we can see that there are a total of more than 350 different ethnicities, some of them still very similar (e.g. 'Austrian American' and 'Austrian Canadian' etc). We want to first simplify this ethnicity criterion before defining diversity. If we didn’t sort the ethnicities, a film with a cast of a German, Austrian and Swiss would be considered very diverse. This is however not what we want to consider diverse. It is for this reason that the ethnicities were first grouped into larger ethnic groups. This was done with the help of a Large Language Model (LLM), with checks and corrections done by hand. Doing this by hand was still possible thanks to the manageable number of ethnicities and the LLM doing the most time-consuming part.
{: .text-justify}

{% include ethnicities_piechart.html %}

### Diversity Score

Once ethnicities have been categorized into larger groups (16 in total), we can begin defining diversity. Since the focus is on the diversity of the cast rather than the representation of minority groups, the country of production does not need to be taken into account.<br> {: .text-justify}
A straightforward way to calculate diversity is to divide the number of ethnicities represented by the number of actors in the cast. However, this method is limited. For instance, in a movie with nine actors and three ethnicities, this approach would assign the same diversity score to a distribution of ethnicities such as (3,3,3) and (1,1,7).<br> {: .text-justify}
To refine this measure, we incorporate the concept of entropy. Entropy accounts for the distribution of ethnicities within a cast. The basic formula for entropy is:
{: .text-justify}
<p><b><em>S = Σ p<sub>i</sub> · ln(p<sub>i</sub>)</em></b>, 
where <em>p<sub>i</sub></em> represents the proportion of the cast belonging to a particular ethnicity.</p>
To avoid obtaining a value of zero when all actors belong to a single ethnicity, we add 1 to the entropy calculation. This ensures a more meaningful result when combining this measure with our initial diversity definition.<br> {: .text-justify}
Finally, to balance the limitations of both methods, we multiply the entropy value by the initial diversity measure. This final diversity coefficient better captures the nuances of cast diversity, penalizing films with fewer actors while rewarding those with a more even distribution of ethnicities.
{: .text-justify}

{% include diversity_histogram_essai.html %}

### Reflecting on the Limitations of Our Definition

Firstly, the dataset includes movies with varying numbers of actors. Films with only one actor cannot be included in the diversity calculation, as it would not provide meaningful insights into cast diversity. For a more comprehensive analysis, one could also consider whether an actor belongs to a minority group, adding another dimension to the evaluation.<br> {: .text-justify}
Secondly, the diversity coefficient relies on the previously defined ethnic groups. Any changes to the characteristics of these groups—such as their size, number, or composition—will directly impact the diversity factor. This highlights the importance of consistent and well-defined groupings when conducting such analyses.
{: .text-justify}

{% include average_diversity_per_year.html %}

Cool! Diversity in movie casts has been steadily increasing over time as mindsets evolve and inclusivity becomes a greater priority. But is this shift driven by success? Let’s dive deeper and find out…

## Money, Ratings, Price the Ingredients of a Successful Movie

<div class="criteria-container">
    <div class="criteria">
        <div class="criteria-icon">💰</div>
        <div class="criteria-content">
            <div class="criteria-title">Box Office Revenue</div>
            <p>To qualify as financially successful, a movie must exceed a revenue threshold of <span class="highlight">$38,119,483</span>. This value, derived from the third quartile of the revenue distribution, ensures we are focusing on the top-performing films. Anything below this is not considered a box office hit.</p>
        </div>
    </div>
    <div class="criteria">
        <div class="criteria-icon">⭐</div>
        <div class="criteria-content">
            <div class="criteria-title">User Ratings</div>
            <p>Films need an average rating of <span class="highlight">7/10</span> or higher to make the cut. This threshold, also based on the third quartile of audience ratings, highlights movies that resonate strongly with viewers.</p>
        </div>
    </div>
    <div class="criteria">
        <div class="criteria-icon">🏆</div>
        <div class="criteria-content">
            <div class="criteria-title">Award Nominations</div>
            <p>Success is not just about wins, but also recognition. We consider all films nominated for prestigious awards like the Oscars and Golden Globes, among others. By including nominees, we avoid being overly restrictive while still capturing critical recognition.</p>
        </div>
    </div>
    <div class="note">
        Together, these criteria provide a multifaceted approach to evaluating a film’s overall success—if a movie meets one or more of these benchmarks, it earns its place as a standout. However, only <span class="highlight">1,370 films</span> in our dataset include box office revenue data. To minimize information loss and ensure a comprehensive analysis, we utilized the full dataset of <span class="highlight">13,000 films</span> when focusing on overall success, particularly for ratings and nominations. For analyses specifically involving box office revenue, we limited our focus to the subset of <span class="highlight">1,370 films</span> with recorded values. This approach ensures we make the most of the available data for each success criterion while transparently addressing the limitations posed by missing box office information for the majority of the dataset.
    </div>
</div>

## The Cruel Truth of Data

Let us now dive into the results of our data analysis and see how diversity aligns with the overall definition of success we established. Is cast diversity more prominent in successful movies, or do less successful films lead the way? Let’s uncover the patterns and insights hidden in the data.
{: .text-justify}

### Overall Success

#### Do Successful and Unsuccessful Movies Have Different Diversity Scores?

{% include diversity_success.html %}

The mean diversity score for successful movies is 0.51 and for less successful movies 0.60. The diversity score seems to be lower for successful movies. But is this difference truly significant? To determine this, we will perform a t-test, a statistical method used to compare the means of two groups and evaluate whether the observed differences are likely due to chance. In this case, we will compare the diversity scores of successful movies (using the overall success) versus less successful movies. This test will help us establish whether there is a statistically significant relationship. Let’s dive in!
{: .text-justify}

{% include t_test_Overall_success.html %}

The significance threshold for the p-value is set at 0.05. Based on our analysis, the difference in diversity scores between successful and less successful films is statistically significant. The test statistic is -9, indicating a strong inverse relationship: the less diverse the cast, the more likely the movie is to be successful. This result challenges the assumption that diversity directly correlates with success and highlights a complex dynamic worth further exploration.

#### Is Diversity Score Correlated with Movie Success?

Claiming that the diversity score and a movie's success are correlated implies that the two variables tend to change together in a systematic way. To investigate this relationship, we use two key metrics: Pearson and Spearman correlation coefficients. Both metrics measure the strength and direction of the relationship between two variables. The correlation coefficient can range from -1 to 1, where a value close to 1 indicates a strong positive relationship, meaning that as one variable increases, the other does as well. A value close to 0 suggests no meaningful relationship, with the variables changing independently of each other. These metrics provide valuable insights into the connection between diversity and success.

TABLEAU ....

The correlation coefficient is nearly zero, revealing that diversity scores and a movie's success march to their own beats. There’s no clear link between the two—diversity doesn’t seem to influence success, and success doesn’t seem to hinge on diversity.

#### Using Propensity Score Matching to Isolate Diversity's Role in Film Success

Several factors contribute to a film's success, and the diversity of a cast alone cannot be assumed to be the driving factor. To better isolate the effect of cast diversity on success, we performed paired matching using a method called propensity score matching. This approach allows us to compare films with similar probabilities of having a high diversity score, specifically those above a threshold of 0.95. This threshold was selected as it represents the third quartile of the diversity score distribution, focusing the analysis on films with the most diverse casts.<br>
The propensity scores were calculated using logistic regression, incorporating variables such as the film's runtime, release year, number of languages present, and the number of countries involved in its production. By controlling for these confounding factors, this method helps us better understand the potential relationship between cast diversity and film success.

propensity...

### Let's dig in...
p-value tsble 

#### Box Office

{% include diversity_box_office.html %}

{% include t_test_Box_office_revenue.html %}

Propensity Score + pearson ou spearman

#### Nominations

{% include diversity_nominations.html %}

{% include t_test_Nomination.html %}

Propensity Score + pearson ou spearman

#### Ratings

{% include diversity_ratings.html %}

{% include t_test_Ratings.html %}

Propensity Score + pearson ou spearman

## Conclusion

<p align="center">
<img src="assets/img/omar_sy_triste.gif" alt=""/>
</p>

meme de omar sy triste 

## References


