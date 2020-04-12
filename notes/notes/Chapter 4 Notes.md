# Chapter 4 Notes

## Geocentrism and Regression
  - Geocentric models are accurate with regards to planetary motion despite being wrong
  - Regression too can be descriptively accurate but mechanistically wrong
    - General model of approximation

## Regression
  - Model mean and variance of a Gaussian measure

## Why normal distributions are everywhere in stats?
  ### Easy to calculate with
  - Additive
  ### Common in nature
  - Ontological perspective
    - Results from processes which add fluctuations
    - Fluctuations dampen one another, resulting in symmetrical distribution
    - This means no information left about underlying process, except mean and variance
      - Different processes can produce same distribution
  - Gaussians typically result from:
    - Addition
    - Products of small deviations (approximately addition)
    - Logarithms of products (addition)
  ### Very conservative assumption 
  - Epistemological perspective
    - if you know mean and variance, then you should only use Gaussian model
      - good spread of values
      - basically assumes the least
    - Nature like maximum entropy distributions

## Linear models
  - include classic t-tests, single regression, multiple regressions, ANOVA - all procedures

  ### Language for linear models - example
  - $y_i \sim Normal(\mu_i,\sigma)$
  - $\mu_i = \beta x_i$
    - $\beta \sim Normal(0.10)$
    - $\sigma \sim Exponential(1)$
    - $x \sim Normal(0,1)$
  - all variables have distributions
  - distributional assumptions allow us to add these into the model

  ### Height example
  - $h \sim Normal(\mu,\sigma)$
  - Add priors
    - $\mu \sim Normal(178, 20)$
      - McElreath height as mean
    - $\sigma \sim Uniform(0,50)$
  - Use prior predictive distribAU
ution simulation to simulate outcomes and assess choice of priors
    - if we change $\sigma$ to 100, then prior predictive has heights that are negative (physically impossible) and heights above the world's tallest person
      - this information should be used to shape the priors
    - more relevant for more complex models
  - Posterior distribution - 2D grid
    - $\mu$ vs $\sigma$
    - $P(h|\mu, \sigma) \times P(\mu) \times P(\sigma)$
      - multiply conditional probability of height for $\mu$ and $\sigma$ at that point in 2D grid times prior of $\mu$ and prior of $\sigma$
      - where $P(\bf{h}|\mu, \sigma)$ is a probability for a dataset (i.e. $\bf{h}$ is a vector of data), then $P(\bf{h}|\mu, \sigma) = \prod_i^{\mu, \sigma}{P(h_i)}$ given that each data point is an independent event
        - if we log these, we can rewrite this as a sum
  - Draw samples
    - can take cross-section of $\mu$ and $\sigma$ 
    - right skew in variance parameters, as larger values of $\sigma$ are consistent with data than smaller values

  ## Quadratic Approximations
  - Assert posterior is a Gaussian for every dimension/parameter
      - Require covariance matrix for multidimensional Gaussian
  - Can estimate with:
    1. MAP or peak of posterior
        - Gradient descent algorithm
    2. Standard deviations & correlationss of parameters (covariance matrix)

  - With flat priors, same as conventional maximum likelihood estimation

  ### quap (R)
  - formula list imitates mathematical definition of model
    - e.g.
      ```
      flist <- alist(
        height ~ dnorm(mu, sigma),
        mu ~ dnorm(178, 20),
        sigma ~ dunif(0,50)
      )
        ```
    - translates into log-probability of combinations of parameters, which is then passed to a gradient descent algorithm
    - returns list of means and covariance matrix
  
  - `quap` is a scaffold - full specification of the model
  - same as penalised maximum likelihood
  - for generalised linear models (non-Gaussian outcomes), not a good predictor
  
    #### Julia
    1. We can use Stan to produce samples (presumably uses MCMC and returns chains) and then use quadratic approximation using one of the following methods:
      1. Stan Optimize (slow, unstable)
      2. Particle Approximation (basically sample mean and stddev)
      3. QUAP from rethinking package, which uses kde and then finds MAP
      4. Maximum Likelihood Estimation fit, which is just Bayesian Inference with a uniform prior

    2. Manually write log likelihood function and use gradient descent to calculate MAP
      - Looks something like this:
        * function for log likelihood
        * optimise to find MAP
        ```
        obs = df2[:, :height]

        # x is a vector composed of [mu, sigma]
        function loglik(x)
          ll = 0.0
          ll += log(pdf(Normal(178, 20), x[1]))
          ll += log(pdf(Uniform(0, 50), x[2]))
          ll += sum(log.(pdf.(Normal(x[1], x[2]), obs)))
          -ll
        end

        # ### snippet 4.29

        # Start values

        lower = [0.0, 0.0]
        upper = [250.0, 50.0]
        x0 = [170.0, 10.0]

        inner_optimizer = GradientDescent()

        @show res = optimize(loglik, lower, upper, x0, Fminbox(inner_optimizer))

        ```
## Regression - Height and Weight

### First Pass
- Linear model of the mean, $\mu$
- Likelihood -> $h_i \sim Normal(\mu_i, \sigma)$, where i is the row/n dimensional data point
- Linear model -> $\mu_i = \alpha + \beta(x_i - \bar{x})$
  - $\mu_i$ is the mean on row i
  - $\alpha$ is mean when $x_i - \bar{x}=0$, "intercept" -> population mean for height
  - $\beta$, slope 
  - $x_i$ predictor (weight) on row i
  - $\bar{x}$ is th mean predictor (mean weight)
    - $(x_i-\bar{x})$ is called centring variable

- Alpha prior -> $\alpha \sim Normal(178,20)$ (height of McElreath)
- Beta prior -> $\beta \sim Normal(0,10)$
  - mean=0 means prior expectation is that there may be no relationship
- Sigma prior -> $\sigma \sim Uniform(0,50)$

#### Prior predictive distribution
  - Simulate lines for priors
    - use min and max of weights and create a grid
    - draw $\mu_i$ lines for all weight values
    - relationship can be absurdly positive (greater than world's tallest person) or negative (negatve heights)
  - Log-Normal distribution
    - when you log numbers, close to Normal distribution, with mean and standard deviation of log defined
    - always positive
    - $beta \sim Log-Normal(0,1$)





  

