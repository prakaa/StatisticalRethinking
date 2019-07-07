---
title: Chapter 1 Notes
tags: [Notebooks/Statistical Rethinking, Notebooks/Tutorial]
created: '2019-07-07T05:17:43.776Z'
modified: '2019-07-07T11:52:53.659Z'
attachments: [decision_tree.png]
---

# Chapter 1 Notes

## Introduction

- Engineering vs. pre-canned paths. Figure below demonstrates commonly-used decision tree. Often, people do not understand underlying structure, the models embodied by the procedures

  ![decision_tree](@attachment/decision_tree.png)

- Need to understand two things:
  
  1. How statistical models relate to hypotheses and mechanisms of interest

  2. How a model processes information

## Epistemological Approach

- Often believed that the objective of statistical inference is to reject a null hypothesis ($H_0$)

- Karl Popper argues that science advances by falsifying hypotheses

> **Null hypothesis testing**: Null hypothessis significance testing is not falsificationist since NHST falsifies a null hypothesis, not the research one

Two reasons deductive falsification is impossible when it comes to science:

### Hypotheses are not models
  - Falsifying a hypothesis involves working with a model. If all models are false, then what does it mean to falsify a model?
  - Furthermore, if we have derived a model from a hypothesis and falsified suh a model we cannot necessarily falsify the hypothesis
  - Example: population biology
    - Debates over what is the *neutral* model for evolution - i.e. what causes the vast majority of evolution
    - Some proposed it was from mutation and drift rather than natural selection
    - Example in figure below. Neutral hypothesis is $H_0$ and natural selection matters is $H_1$
    - $H_0$ can be mapped to process models $P_{0A}$ and $P_{0B}$ based on population size and structure reaching steady state. Same for $H_1$
    - Statistical models are mapped to process models. $P_{0A}$, for example, produces a *power law* distribution of allele frequencies. But so does $P_{1B}$
  
  - So any statistical model can be mapped to more than one process model, any hypothesis can correspond to more than one process model and as such, any statistical model can correspond to more than one hypothesis

    >A statistical model often corresponds to many different process models as they rely upon members of the *exponential family* of distributions. These include normal, binomial and Poisson and are typically *maximum entropy* distributions that are often found in nature. They can explain phenomena without us having to understand their mechanisms

  - Rejecting null hypothesis - can't conclude if selection really matters since other neutral models predict different distributiond

  - Fail to reject - can't conclude evolution is neutral because some selection models predict the same distribution

  ![models](@attachment/hypotheses_and_models.png)

  - Another issue is that is there one neutral model to test against? Even if there were one, so many mimic the one neutral model

  #### Solution
  **Search for different description of evidence under which multiple process models look different**

  ### Measurement matters

  - Falsification simply requires some observation D to conclude H is true. If D cannot be found, we can conclude H is false
    - e.g. colour of swans - before Europeans arrived in Australia it was believed that all swans were white
  - However:
    1. Observations are prone to error
      - Finding disconfirming evidence is complicate by the difficulties of observation
    2. Hypotheses are often quantitative and consider degrees of existence rather than discrete values
      - e.g. 80% of swans are white. Most hypotheses are continuous rather than discrete
    3. Falsification is consensual in that the scientific community argues about what some evidence means, rather than some logical falsification that eliminates a hypothesis as some history books might suggest happened

  ## Statistical Engineering

  ### Bayesian Data Analysis
  - "Bayesian" denotes a particular interpretation of probability - the number of ways something can happen based on our assumptions
  - In contrast, the *frequentist* approach requires that probability be linked to countable events and ther frequencies in very large samples (i.e. population)
  - Uncertainty is based on resampling - if we repeated the measurement, the data as a whole should have a pattern to it. This resampling is never done and doesn't make sense when you have a specific set of evidence that you are looking at
    - However in the case of experiments where you can repeat useful measurements it's a useful device for describing uncertainty
  - Parameters and models cannot have probability distributions, only measurements can (*sampling distribution*)
  - Galileo - primitive telescope revealed blurred image of Saturn. Uncertainty in the shape of the planet and repeated measurements would not yield a different result. 
    - Frequentist approach would struggle
    - Bayesian inference can proceed - deterministic 'noise' from measurement can be modeled using probability
      - Image reconstruction and processing dominated by Bayesian algorithms
  - Linear regression can have same outcomes from two methods
    - Bayesian approach doesn't justify inference with imagined resampling
    - *Randomness* is treated as a property of information, not the world
      - Is anything really random in the world or do we use this term to describe uncertainty when we have incomplete knowledge?
  - **Main advantage of Bayesian data analysis is that most people find it more intuitive, to the point that people interpret non-Bayesian results in Bayesian terms**
  - Bayesian methods are computationally expensive and hence have become popular recently. Larger data sets will require alternatives or approximations

  ### Multilevel Models
  - Models contain parameters, but what are these based on?
    - Placeholder for a missing model
    - However now we have a model with multiple levels of uncertainty, with each feeding into the next
  - Four reasons to use multilevel models, which apply when there are groups or clusters of measurements that may differ from each other:
    1. Adjust estimate for repeat sampling - when more than one observation arises from the same individual, location, time
    2. Adjust estimate for imbalance in sampling- when something is sampled more than something else
    3. Study variation - multilevel models explicitly model uncertainty
    4. To avoid averaging, whih can remove variation and give false confidence (e.g. regression analysis). Can still used averages to make predictions but preserve uncertainty in a multi-level model 
  - Even well-controlled experiments involve interaction with unmeasured aspects of a group or cluster, leading to variation in results. Muti-level models attempt to quantify the extent of this variation
  - However, fitting and interpreting multi-level models can be harder

  ### Model Comparison and Information Criteria
  - Information criteria allow us to compare models based upon expected predictive accuracy
    - e.g. Akaike information criterion
    - build a model of prediction task and use that model to estimate performance of each model you wish to compare
    - IC depend on certain assumptions
    - based on Information Theory
  - Two primary benefits from IC:
    1. IC provides estimate for predictive accuracy rather than just fit, which means it can tell us if a model is overfitted
    2.  Allow for comparison of multiple non-null models, such as the evolution debate models
  - Young compared to multilevel models and Bayesian data analysis

  ## The End Goal
  **Instead of choosing various black-box tools for NHST, learn to build and analyse multiple non-null models of natural phenomena**
    
 
