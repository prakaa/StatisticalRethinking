# Chapter 3 Notes

## Sampling from the posterior
- Bayes theorem medical test example
  - can take probability distribution, sample from that and generate counts, around which we have better intuition (natural frequencies)
- Incredibly useful to sample
  - Visualise uncertainty
  - Compute confidence intervals
  - Simulate observations
- MCMC produces only samples
- Easy to think about samples

### Recipe
1. Compute or approximate posterior
2. Sample with replacement from posterior
3. Compute stuff from samples

## Computing stuff
- Typically we construct intervals
  - Posterior is the Bayesian estimate, but we often need to summarise or communicate the shape of an interval
  - How much posterior probability within an interval
  - What values return 80-90% posterior probability ('confidence interval')
  - What estimate of the parameter maximises posterior probability ('point estimates')
- Intervals and point estimates
  - Intervals of defined boundary - how much mass? (e.g. between $p$=0.25 and $p$=0.5)
    - calculate proportion of samples between these values 
  - Intervals of defined mass - which values? (e.g. lower, middle 80%)
    - treat samples as data and use quantiles (e.g. middle 50% is computed using quantile for 0.25 and 0.75)
### Percentile intervals (PIs)
  - Equal area in each tail (e.g. 50% - each tail contains 25% mass)
  - Good for symmetric distribution, may omit highest probability
  - Language around intervals
  - "Confidence interval"
    - non-Bayesian, misnomer
  - "Credible interval"
    - values are only credible if you trust data and model
  - "Compatability interval"
    - interval has values which are most compatible with data and model

### Highest posterior density interval (HDPI)
  - Narrowest interval containing mass specified (e.g. 50% HDPI contains highest density interval with 50% mass)

### Point estimates
  - Generally don't want to compute point estimates
    - Entire posterior contains more information
    - "Best" point depends upon purpose
  - Highest posterior probability is called Maximum A Posteriori
    - using samples, you need to take the mode
  - Can use loss function, which tells you cost associated with using any point estimate. Different loss functions lead to different point estimates
    - whatever the loss function is, typically weighted by the posterior probability of the potential parameter value
    - when loss is proportional to distance of your decision from the true value a.k.a absolute loss (i.e. $loss\ \alpha\ |decision - true|$), then median of posterior minmises loss
    - when loss is proportional to square of distance (quadratic loss), posterior mean is the point estimate
    - when distribution is symmetrical and normal-looking, mean and median are same point
  - Calculating loss function
      - True value is unknown
      - So loss can be calculated using posterior probability weighted difference
        - for one decision, `loss=sum(posterior * abs(decision - p_grid))`
        - for every possible decision (loss curve) 
        ```
        apply(p_grid, lambda x: sum(posterior*abs(x - p_grid)))
    ```
        - find point which minimises loss

## Predictive checks
  - Bayesian models are *generative* i.e. they are capable of simulating observations
  - Can use samples from posterior to simulate observations
      - called Dummy data
- Prior predictive distribution
- Posterior predictive distribution
  - Allows us to propagae parameter uncertainty
  - Example
    - Based on different values of $p$, we can generate different sampling distributions that show how likely a certain number of successes would be given a particular value of $p$
    - We want something that weights these and merges them to give a single dataset -  this is the posterior probability distribution
    - When using samples, we use the values of the samples as the values of $p$ to generate appropriate results in the model
  		![Icon](../attachments/merging_results.png) 
    - Random Binomial -> 10,000 points, 9 tosses, samples from posterior distribution (probability of success from 0 to 1 -  $p$)
    - Simulated model predictions consistent with observed data
      - spread from predictions - arise from binomial process
    - Model can fail in other aspects of prediction
      - number of switches from W to L not predicted correctly by model
  - Gets harder with more complicated models
