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

In this project, we set out to uncover how cast diversity influences both the financial and critical success of films—and the results might surprise you.<br>
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

Once ethnicities have been categorized into larger groups (16 in total), we can begin defining diversity. Since the focus is on the diversity of the cast rather than the representation of minority groups, the country of production does not need to be taken into account.<br>
A straightforward way to calculate diversity is to divide the number of ethnicities represented by the number of actors in the cast. However, this method is limited. For instance, in a movie with nine actors and three ethnicities, this approach would assign the same diversity score to a distribution of ethnicities such as (3,3,3) and (1,1,7).<br>
To refine this measure, we incorporate the concept of entropy. Entropy accounts for the distribution of ethnicities within a cast. The basic formula for entropy is:
{: .text-justify}
<p><b><em>S = Σ p<sub>i</sub> · ln(p<sub>i</sub>)</em></b>, 
where <em>p<sub>i</sub></em> represents the proportion of the cast belonging to a particular ethnicity.</p>
To avoid obtaining a value of zero when all actors belong to a single ethnicity, we add 1 to the entropy calculation. This ensures a more meaningful result when combining this measure with our initial diversity definition.<br>
Finally, to balance the limitations of both methods, we multiply the entropy value by the initial diversity measure. This final diversity coefficient better captures the nuances of cast diversity, penalizing films with fewer actors while rewarding those with a more even distribution of ethnicities.
{: .text-justify}

{% include diversity_histogram_essai.html %}

### Reflecting on the Limitations of Our Definition

Firstly, the dataset includes movies with varying numbers of actors. Films with only one actor cannot be included in the diversity calculation, as it would not provide meaningful insights into cast diversity. For a more comprehensive analysis, one could also consider whether an actor belongs to a minority group, adding another dimension to the evaluation.<br>
Secondly, the diversity coefficient relies on the previously defined ethnic groups. Any changes to the characteristics of these groups—such as their size, number, or composition—will directly impact the diversity factor. This highlights the importance of consistent and well-defined groupings when conducting such analyses.
{: .text-justify}

{% include average_diversity_per_year.html %}

Cool! Diversity in movie casts has been steadily increasing over time as mindsets evolve and inclusivity becomes a greater priority. But is this shift driven by success? Let’s dive deeper and find out…
{: .text-justify}

## Movie Success Criteria

{% include ingredients_of_success.html %}

## The Cruel Truth of Data

Let us now dive into the results of our data analysis and see how diversity aligns with the overall definition of success we established. Is cast diversity more prominent in successful movies, or do less successful films lead the way? Let’s uncover the patterns and insights hidden in the data.
{: .text-justify}

### Overall Success

##### Do Successful and Unsuccessful Movies Have Different Diversity Scores?

{% include diversity_success.html %}

The mean diversity score for successful movies is 0.51 and for less successful movies 0.60. The diversity score seems to be lower for successful movies. But is this difference truly significant? To determine this, we will perform a t-test, a statistical method used to compare the means of two groups and evaluate whether the observed differences are likely due to chance. In this case, we will compare the diversity scores of successful movies (using the overall success) versus less successful movies. This test will help us establish whether there is a statistically significant relationship. Let’s dive in!
{: .text-justify}

{% include t_test_Overall_success.html %}

The significance threshold for the p-value is set at 0.05. Based on our analysis, the difference in diversity scores between successful and less successful films is statistically significant. The test statistic is -9, indicating a strong inverse relationship: the less diverse the cast, the more likely the movie is to be successful. This result challenges the assumption that diversity directly correlates with success and highlights a complex dynamic worth further exploration.
{: .text-justify}

##### Is Diversity Score Correlated with Movie Success?

Claiming that the diversity score and a movie's success are correlated implies that the two variables tend to change together in a systematic way. To investigate this relationship, we use two key metrics: Pearson and Spearman correlation coefficients. Both metrics measure the strength and direction of the relationship between two variables. The correlation coefficient can range from -1 to 1, where a value close to 1 indicates a strong positive relationship, meaning that as one variable increases, the other does as well. A value close to 0 suggests no meaningful relationship, with the variables changing independently of each other. These metrics provide valuable insights into the connection between diversity and success.
{: .text-justify}

TABLEAU ....

The correlation coefficient is nearly zero, revealing that diversity scores and a movie's success march to their own beats. There’s no clear link between the two—diversity doesn’t seem to influence success, and success doesn’t seem to hinge on diversity.
{: .text-justify}

### Let's dig in...

Let us now dive deeper and analyze each key criterion of success—box office revenue, award nominations, and user ratings—individually. By examining these factors separately, we aim to uncover their specific relationships with diversity and determine if any one of them carries a greater influence or impact. We will apply the same methodology used in the overall success analysis.
{: .text-justify}

#### Box Office

Recall: For this part of the story the dataset is reduced to films where box office revenue is available, meaning we are working with 1'370 movies.
{: .text-justify}

{% include diversity_box_office.html %}

The mean diversity score for movies with high box revenue is 0.57 and for lower box office revenue 0.60. Here again diversity appears higher for films with lower box office revenue. Is this difference significant? Let’s investigate:
{: .text-justify}

{% include t_test_Box_office_revenue.html %}

TABLEAU CORRELATION A COTE

For the t-test, with a significance threshold of 0.05, the p-value reveals that the difference in diversity scores between the two groups is not statistically significant. For pearson, the value is very close to 0 indicating that the two variables are not correlated. This result brings nuance on our previous conclusion about the role of diversity in this context, leaving the question open for further exploration.
{: .text-justify}

As the dataset in this case is smaller, we decided to take the analysis a step further by performing propensity score matching. Several factors contribute to a film's success, and the diversity of a cast alone cannot be assumed to be the driving factor. To better isolate the effect of cast diversity on success, we performed paired matching using propensity score matching. This approach allows us to compare films with similar probabilities of having a high diversity score, specifically those above a threshold of 0.95. This threshold was selected as it represents the third quartile of the diversity score distribution, focusing the analysis on films with the most diverse casts.<br>
The propensity scores were calculated using logistic regression, incorporating variables such as the film's runtime, release year, number of languages present, and the number of countries involved in its production. By controlling for these confounding factors, this method helps us better understand the potential relationship between cast diversity and film success.<br>
We calculated the Average Treatment Effect (ATE) to assess how the "treatment"—in this case, having a diverse cast—affects box office revenue compared to a control group. The results indicate that the diversity of a cast increases box office revenue by an average of <b>$1.57 million</b>.<br>
While this might seem promising, it is worth considering the broader context. With an average box office revenue of $47 million, the increase of $1.57 million represents a relatively small change, suggesting that the impact of diversity, though present, may not be particularly significant in financial terms.
{: .text-justify}

Therefore, when focusing solely on the effect of diversity on box office success, the statistics point toward an insignificant connection.
{: .text-justify}

#### Ratings

{% include diversity_ratings.html %}

The mean diversity score for movies with high ratings is 0.50 and for movies with low ratings 0.60. The diversity score seems to be lower for high rated movies. But is this difference really significant? Let’s conduct a t-test:
{: .text-justify}

{% include t_test_Ratings.html %}

TABLEAU CORRELATION A COTE

The difference in diversity scores between high-rated and low-rated films appears to be statistically significant, with a test statistic of -10. This indicates that movies with less diverse casts tend to receive higher ratings. However, the correlation coefficient is nearly zero, suggesting that there is no consistent or meaningful relationship between diversity scores and movie ratings.<br>
This highlights a complex dynamic where diversity may influence ratings in specific cases but does not establish a clear overall trend.
{: .text-justify}

#### Nominations

{% include diversity_nominations.html %}

The mean diversity score for movies nominated is 0.64 and for movies not nominated 0.57. The diversity score seems higher for films nominated. But is this difference really significant? Let’s conduct a t-test. Results from the t-test are presented in the table below.
{: .text-justify}

{% include t_test_Nomination.html %}

TABLEAU CORRELATION

The difference in diversity scores between nominated and non-nominated films appears significant, with a test statistic of 6, indicating that films with more diverse casts are more likely to be nominated. However, consistent with our earlier findings, the correlation coefficient is nearly zero, suggesting that there is no strong or systematic relationship between diversity scores and nominations overall. This aligns with the pattern we've observed: while diversity may have an influence in certain cases, it does not establish a clear or consistent trend.
{: .text-justify}

## Conclusion

<p align="center">
<img src="assets/img/omar_sy_triste.gif" alt=""/>
</p>

meme de omar sy triste 

## References


