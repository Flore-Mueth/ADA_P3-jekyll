---
layout: page
title: The Success of Inclusive Cinema
subtitle: Exploring the Connection Between Representation and Movie Triumph
cover-img: /assets/img/header_img.png
---

## Introduction

Movies don't just entertain—they inspire and shape how we see the world. A little girl watching a character like her become a doctor might suddenly believe she can do the same. Cinema reflects our emotions and dreams, and in doing so, it helps shape our collective imagination.

Yet, this incredible influence comes with responsibility. Films can perpetuate harmful stereotypes or amplify fear and prejudice. Worse, a lack of representation leaves entire communities invisible, excluded from the collective narrative. Thankfully, awareness of this issue has grown over the past few decades, leading to slow but meaningful progress in diversifying casts.

Today, even the Oscars have introduced diversity quotas for Best Picture eligibility, recognizing the importance of representation in storytelling. But beyond quotas and moral imperatives, there's a compelling question: can a diverse cast also drive cinematic success? Could the power of representation become a key motivator for filmmakers to embrace inclusivity? Let's dive into a data story to uncover the impact of diversity on movie success.

## Setting the Stage: What We're Exploring and How

In this project, we will dive into the provided CMU Movie Summary Corpus Dataset containing 42,306 plot summaries and metadata for films released from 1893 to 2013. To define "success," we have expanded our resources, integrating data on award nominations and audience ratings.

### About the Data

<div style="display: flex; align-items: center; justify-content: center;">
    <!-- Left Side: Image -->
    <div style="margin-right: 20px;">
        <img src="assets/img/video_camera.png" alt="" style="max-width: 150px; height: auto;">
    </div>
    
    <!-- Right Side: Text -->
    <div>
        <h6>Our primary dataset provides key elements such as:</h6>
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
        <h6>To enrich our analysis, we brought in additional datasets:</h6>
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

### What We'll Explore

In this project, we aim to address some significant questions about how cast diversity influences both financial and critical success in the film industry:

- Does cast diversity impact box office performance?

To explore this, we will analyze the relationship between cast diversity and box office revenue using methods like linear regression analysis to identify trends and correlations. We will also account for other influencing factors such as budget and genre to isolate the effect of diversity.

- Are films with diverse casts more likely to receive award nominations?

By leveraging data on prestigious awards such as the Oscars, Golden Globes, and other internationally recognized accolades, we will employ propensity score matching. This method allows us to compare films with similar characteristics, except for their cast diversity, to determine whether diversity correlates with a higher likelihood of earning nominations.

- Do films with diverse casts earn higher audience and critic ratings?

Using IMDb user ratings and critical reviews, we will examine whether diversity in casts influences public and professional evaluations. Through techniques like logistic regression and statistical tests, we aim to uncover patterns that highlight the impact of representation on reception.
By applying these robust analytical methods, we will delve deeper into the data to uncover insights about how diversity shapes a film's financial success and critical acclaim.

## What Makes a Cast Inclusive?

<p align="center">
<img src="assets/img/Intouchable.gif" alt=""/>
</p>

### Clustering the Ethnicities

When we first have a look at the ethnicities, we can see that there are a total of more than 350 different ethnicities, some of them still very similar (e.g. 'Austrian American' and 'Austrian Canadian' etc). We want to first simplify this ethnicity criterion before defining diversity. If we didn’t sort the ethnicities, a film with a cast of a German, Austrian and Swiss would be considered very diverse. This is however not what we want to consider diverse. It is for this reason that the ethnicities were first grouped into larger ethnic groups. This was done with the help of a Large Language Model (LLM), with checks and corrections done by hand. Doing this by hand was still possible thanks to the manageable number of ethnicities and the LLM doing the most time-consuming part.

<div style="display: flex; align-items: center; justify-content: center;">
    <!-- Left Side: Image -->
    <div style="margin-right: 20px;">
        <img src="assets/img/camembert1.png" alt="" style="width: 150px; height: 150px; object-fit: cover;">
    </div>
    
    <!-- Right Side: Image -->
    <div>
        <img src="assets/img/camembert2.png" alt="" style="width: 150px; height: 150px; object-fit: cover;">
    </div>
</div>



### Diversity Score

Once ethnicities have been categorized into larger groups (16 in total), we can begin defining diversity. Since the focus is on the diversity of the cast rather than the representation of minority groups, the country of production does not need to be taken into account.

A straightforward way to calculate diversity is to divide the number of ethnicities represented by the number of actors in the cast. However, this method is limited. For instance, in a movie with nine actors and three ethnicities, this approach would assign the same diversity score to a distribution of ethnicities such as (3,3,3) and (1,1,7).
To refine this measure, we incorporate the concept of entropy. Entropy accounts for the distribution of ethnicities within a cast. The basic formula for entropy is:

<p>S = &Sigma; p<sub>i</sub> &middot; ln(p<sub>i</sub>)</p>
where p<sub>i represents the proportion of the cast belonging to a particular ethnicity.

To avoid obtaining a value of zero when all actors belong to a single ethnicity, we add 1 to the entropy calculation. This ensures a more meaningful result when combining this measure with our initial diversity definition.
Finally, to balance the limitations of both methods, we multiply the entropy value by the initial diversity measure. This final diversity coefficient better captures the nuances of cast diversity, penalizing films with fewer actors while rewarding those with a more even distribution of ethnicities.

Plot diversity over time + comments

### Reflecting on the Limitations of Our Definition

Firstly, the dataset includes movies with varying numbers of actors. Films with only one actor cannot be included in the diversity calculation, as it would not provide meaningful insights into cast diversity. For a more comprehensive analysis, one could also consider whether an actor belongs to a minority group, adding another dimension to the evaluation.

Secondly, the diversity coefficient relies on the previously defined ethnic groups. Any changes to the characteristics of these groups—such as their size, number, or composition—will directly impact the diversity factor. This highlights the importance of consistent and well-defined groupings when conducting such analyses.

## How to Measure the Success of a Movie?

### Succes Score, price, box office, public

### Set threshold (explore diff th)

## The Cruel Truth

### Linear Regression

### 


