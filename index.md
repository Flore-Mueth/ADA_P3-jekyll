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

In this project, we will dive into the provided CMU Movie Summary Corpus Dataset containing 42'306 plot summaries and metadata for films released from 1893 to 2013. To define "success" we have expanded our resources, integrating data on award nominations and audience ratings. Since our focus is on cast diversity, we exclude all films with only one character, as they do not contribute meaningfully to the diversity analysis. Our final cleaned dataset comprises 10'000 films.
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

<div style="text-align: center; font-size: 20%; line-height: 1; display: inline-block;">
    {% include distribution_realease_date.html %}
</div>

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

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include ethnicities_piechart.html %}
</div>

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include ethnic_groups_piechart.html %}
</div>

### Diversity Score

Once ethnicities have been categorized into larger groups (18 in total), we can begin defining diversity. Since the focus is on the diversity of the cast rather than the representation of minority groups, the country of production does not need to be taken into account.<br>
A straightforward way to calculate diversity is to divide the number of ethnicities represented by the number of actors in the cast. However, this method is limited. For instance, in a movie with nine actors and three ethnicities, this approach would assign the same diversity score to a distribution of ethnicities such as (3,3,3) and (1,1,7).<br>
To refine this measure, we incorporate the concept of entropy. Entropy accounts for the distribution of ethnicities within a cast. The basic formula for entropy is:
{: .text-justify}
<p><b><em>S = Σ p<sub>i</sub> · ln(p<sub>i</sub>)</em></b>, 
where <em>p<sub>i</sub></em> represents the proportion of the cast belonging to a particular ethnicity.</p>
To avoid obtaining a value of zero when all actors belong to a single ethnicity, we add 1 to the entropy calculation. This ensures a more meaningful result when combining this measure with our initial diversity definition.<br>
Finally, to balance the limitations of both methods, we multiply the entropy value by the initial diversity measure. This final diversity coefficient better captures the nuances of cast diversity, penalizing films with fewer actors while rewarding those with a more even distribution of ethnicities.
{: .text-justify}

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include diversity_histogram_essai.html %}
</div>

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include average_diversity_per_year.html %}
</div>

Cool! Diversity in movie casts has been steadily increasing over time as mindsets evolve and inclusivity becomes a greater priority. But is this shift driven by success? Let’s dive deeper and find out…
{: .text-justify}

### Reflecting on the Limitations of Our Definition

Firstly, the dataset includes movies with varying numbers of actors. Films with only one actor cannot be included in the diversity calculation, as it would not provide meaningful insights into cast diversity. For a more comprehensive analysis, one could also consider whether an actor belongs to a minority group, adding another dimension to the evaluation.<br>
Secondly, the diversity coefficient relies on the previously defined ethnic groups. Any changes to the characteristics of these groups—such as their size, number, or composition—will directly impact the diversity factor. This highlights the importance of consistent and well-defined groupings when conducting such analyses.
{: .text-justify}

## Movie Success Criteria

{% include ingredients_of_success.html %}

## The Truth of Data

Let us now dive into the results of our data analysis and see how diversity aligns with the overall definition of success we established. Is cast diversity more prominent in successful movies, or do less successful films lead the way? Let’s uncover the patterns and insights hidden in the data.
{: .text-justify}

### Overall Success

#### Do Successful and Unsuccessful Movies Have Different Diversity Scores?

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include diversity_success.html %}
</div>

The mean diversity score for successful movies is 2.54 and for less successful movies 2.29. The diversity score seems to be higher for successful movies. We can notice that the standard deviation on this plot is very important. This is due to the large number of data we are working with.<br>
But is this difference truly significant? To determine this, we will perform a t-test, a statistical method used to compare the means of two groups and evaluate whether the observed differences are likely due to chance. In this case, we will compare the diversity scores of successful movies (using the overall success) versus less successful movies. This test will help us establish whether there is a statistically significant relationship. Let’s dive in!
{: .text-justify}

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include t_test_Overall_success.html %}
</div>

The significance threshold for the p-value is set at 0.05. Based on our analysis, the difference in diversity scores between successful and less successful films is statistically significant. The test statistic is 7.9, indicating a positive relationship: the more diverse the cast, the more likely the movie is to be successful. This finding suggests that diversity may play a role in contributing to a film’s success, underscoring its potential as a valuable factor in the industry!
{: .text-justify}

#### Is Diversity Score Correlated with Movie Success?

Claiming that the diversity score and a movie's success are correlated implies that the two variables tend to change together in a systematic way. To investigate this relationship, we use two key metrics: Pearson and Spearman correlation coefficients. Both metrics measure the strength and direction of the relationship between two variables. The correlation coefficient can range from -1 to 1, where a value close to 1 indicates a strong positive relationship, meaning that as one variable increases, the other does as well. A value close to 0 suggests no meaningful relationship, with the variables changing independently of each other. Pearson assumes a linear relation between two continuous variables normally distributed. Therefore, we decided to use Pearson for continuous variables (box office revenue and ratings) but not for binary variables like success and nominations. Instead we used Spearman. These metrics provide valuable insights into the connection between diversity and success.
{: .text-justify}

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include corr_Success_Spearman.html %}
</div>

The correlation coefficient is nearly zero, revealing that diversity scores and a movie's success march to their own beats. However, this tells a different story, suggesting that while successful movies might have slightly higher diversity scores on average, this does not translate to a consistent relationship between the two variables.
{: .text-justify}

<b> The t-test and the spearman correlation are consistent with each other in the way that they are showing significant but small relations. That is very typical from studies with large dataset: even small effect can become significant.
</b>

### Let's dig in...

Let us now dive deeper and analyze each key criterion of success—box office revenue, award nominations, and user ratings—individually. By examining these factors separately, we aim to uncover their specific relationships with diversity and determine if any one of them carries a greater influence or impact. We will apply the same methodology used in the overall success analysis.
{: .text-justify}

#### <span class="criteria-icon">💰</span> Box Office

Recall: For this part of the story the dataset is reduced to films where box office revenue is available, meaning we are working with 4'726 movies.
{: .text-justify}

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include diversity_box_office.html %}
</div>

The mean diversity score for movies with high box revenue is 3.21 and for lower box office revenue 2.75. Here again diversity appears higher for films with higher box office revenue. Is this difference significant? Let’s investigate:
{: .text-justify}

<div style="display: flex; justify-content: space-around; align-items: flex-start; gap: 20px;">
    <div style="text-align: center; font-size: 80%; line-height: 1.2;">
        {% include t_test_Box_office_revenue.html %}
    </div>

    <div style="text-align: center; font-size: 80%; line-height: 1.2;">
        {% include corr_Movie_box_office_revenue_Pearson.html %}
    </div>
</div>

For the t-test, with a significance threshold of 0.05, the p-value reveals that the difference in diversity scores between the two groups is statistically significant. For pearson, the value is very close to 0 indicating that the two variables are not correlated. This result brings the same conclusion about the role of diversity in this context, there is a small (0.149), but significant (p < 0.05) positive correlation between box office revenue and diversity score.
{: .text-justify}

As the dataset in this case is smaller, we decided to take the analysis a step further by performing propensity score matching. Several factors contribute to a film's success, and the diversity of a cast alone cannot be assumed to be the driving factor. To better isolate the effect of cast diversity on success, we performed paired matching using propensity score matching. This approach allows us to compare films with similar probabilities of having a high diversity score, specifically those above a threshold of 4 (third quantile of the diversity score), focusing the analysis on films with the most diverse casts.
{: .text-justify}

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include propensity_score_matching.html %}
</div>

The plot reveals that most movies in the control group (those with lower diversity scores) cluster in the lower range of box office revenue. In contrast, the treated group (films with higher diversity scores) is associated with higher box office revenue.<br>
The average treatment effect (ATE) determines how the treatment (having a diverse cast) changes the outcome (box office revenue). We find a value of $52 millions, this average treatment effect seems way too high. We have a few hypotheses to explain this very high ATE. Maybe the criteria we matched on were not good criteria. We matched on the number of countries but then it would mean that a film with countries India and Sri Lanka can be matched with a movie with countries France and the United States just because they have the same number of countries. Maybe some other criteria could have been matched on and we overlooked them. Maybe matching on movie genres could have been a good idea. More importantly, describing the data for box office revenue we have a very high standard deviation and the data is skewed.
{: .text-justify}

#### <span class="criteria-icon">⭐</span> Ratings

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include diversity_ratings.html %}
</div>

The mean diversity score for movies with high ratings is 2.2 and for movies with low ratings 2.4. The diversity score seems to be lower for high rated movies... Disappointing... But is this difference really significant? Let’s conduct a t-test and pearson correlation:
{: .text-justify}

<div style="display: flex; justify-content: space-around; align-items: flex-start; gap: 20px;">
    <div style="text-align: center; font-size: 80%; line-height: 1.2;">
        {% include t_test_Ratings.html %}
    </div>

    <div style="text-align: center; font-size: 80%; line-height: 1.2;">
        {% include corr_Ratings_Pearson.html %}
    </div>
</div>

The difference in diversity scores between high-rated and low-rated films appears to be statistically significant, with a test statistic of -6. This indicates that movies with less diverse casts tend to receive higher ratings. However, the correlation coefficient is nearly zero, suggesting that there is no consistent or meaningful relationship.<br>
This highlights a complex dynamic where diversity may influence ratings in specific cases but does not establish a clear overall trend.
{: .text-justify}

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include propensity_score_matching_ratings.html %}
</div>

The ATE is 0.11 out of ten, we can consider that the diversity of the cast doesn't have an effect on the ratings. Compared to the one we got for the box office revenue, it makes more sense. It can be explained by the fact that the standard deviation is not very high in this case.

#### <span class="criteria-icon">🏆</span> Nominations

<div style="text-align: center; font-size: 80%; line-height: 1.2;">
    {% include diversity_nominations.html %}
</div>

The mean diversity score for movies nominated is 2.43 and for movies not nominated 2.36. Unlike ratings, nominations appear to favor more diverse casts, though the difference is minor. Let's see:
{: .text-justify}

<div style="display: flex; justify-content: space-around; align-items: flex-start; gap: 20px;">
    <div style="text-align: center; font-size: 80%; line-height: 1.2;">
        {% include t_test_Nomination.html %}
    </div>

    <div style="text-align: center; font-size: 80%; line-height: 1.2;">
        {% include corr_Nomination_Spearman.html %}
    </div>
</div>

The difference in diversity scores between nominated and non-nominated films appears significant, with a p-value of 0.042 (<0.05), indicating that films with more diverse casts are more nominated. However, consistent with our earlier findings, the correlation coefficient is nearly zero, suggesting that there is no strong or systematic relationship between diversity scores and nominations overall. This aligns with the pattern we've observed: while diversity may have an influence in certain cases, it does not establish a clear or consistent trend.<br>
Most of our results appear significant but with small relations. That is very typical from analysis with large dataset: even small effect can become significant.
{: .text-justify}

## Conclusion

<p align="center">
<img src="assets/img/omar_sy_triste.gif" alt=""/>
</p>

The relationship between cast diversity and movie success paints a complex picture. Films with more diverse casts often shine at the box office and during awards season, as nominations appear linked to higher diversity. However, high ratings tend to favor less diverse casts, and overall, correlation coefficients reveal no strong or consistent link between diversity and success metrics.<br>
This suggests that directors may not be easily convinced to prioritize diversity, as its impact on a film’s success isn’t direct or significant. But Hollywood, take note: even if diversity isn’t the guaranteed star of the show, the industry still has work to do to ensure diversity becomes a more prominent and lasting part of the narrative.
{: .text-justify}

## References

<ul>
    <li>
        <a href="https://socialsciences.ucla.edu/wp-content/uploads/2024/06/UCLA-Hollywood-Diversity-Report-2024-Film-Streaming-5-23-2024.pdf" target="_blank">
            UCLA Social Sciences. <i>Hollywood Diversity Report 2024: Film and Streaming</i>.
        </a>
    </li>
</ul>
