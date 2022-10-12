# The Art of Statistical Arbitrage

tags: #statarb

- [The Art of Statistical Arbitrage](#the-art-of-statistical-arbitrage)
  - [Market Data](#market-data)
    - [Price Adjustment](#price-adjustment)
    - [Return computation](#return-computation)
  - [Forecasting](#forecasting)
    - [Data for forecasts](#data-for-forecasts)
      - [Point-in-time Data](#point-in-time-data)
      - [Security Master and Survival Bias](#security-master-and-survival-bias)
      - [Fundamental and Accounting Data](#fundamental-and-accounting-data)
      - [Analyst Estimates](#analyst-estimates)
      - [Supply chain and competition](#supply-chain-and-competition)
      - [M&A and risk arbitrage](#ma-and-risk-arbitrage)
      - [Event based](#event-based)
      - [Holdings and flows](#holdings-and-flows)
      - [News and social media](#news-and-social-media)
      - [Macroeconomic data](#macroeconomic-data)
      - [Alternative data](#alternative-data)
      - [Alpha capture](#alpha-capture)
    - [Technical forecasts](#technical-forecasts)
      - [Mean reversion](#mean-reversion)
      - [Momentum](#momentum)
      - [Statistical predictors](#statistical-predictors)
      - [Data from other asset class](#data-from-other-asset-class)
    - [Statistical Learning](#statistical-learning)
      - [Mutual information and Shannon entropy](#mutual-information-and-shannon-entropy)
      - [Likelihood and Bayes inference](#likelihood-and-bayes-inference)
      - [Mean square error and correlation](#mean-square-error-and-correlation)
      - [Weighted law of large numbers](#weighted-law-of-large-numbers)
    - [Machine Leaning](#machine-leaning)
      - [Curse of dimensionality, eigenvalue cleaning, and shrinkage](#curse-of-dimensionality-eigenvalue-cleaning-and-shrinkage)
      - [Smoothing and regularization](#smoothing-and-regularization)
      - [Boosting](#boosting)
  - [Forecast Combining](#forecast-combining)
    - [Dimensionality reductions](#dimensionality-reductions)
      - [Clustering](#clustering)
    - [Alpha Pool Management](#alpha-pool-management)
      - [Forecast development guidelines](#forecast-development-guidelines)
      - [Pnl attribution](#pnl-attribution)
  - [Risk](#risk)
  - [Trading Cost and Market Elasticity](#trading-cost-and-market-elasticity)
  - [Portfolio Construction](#portfolio-construction)
    - [Hedge allocation](#hedge-allocation)
    - [Forecast from rule-based strategy](#forecast-from-rule-based-strategy)
    - [Time effect of the trade](#time-effect-of-the-trade)
    - [Portfolio capacity](#portfolio-capacity)
    - [Kelly criterion and optimal leverage](#kelly-criterion-and-optimal-leverage)
  - [Simulation](#simulation)
  - [Some good refs books, papers, and website](#some-good-refs-books-papers-and-website)

> The successful quant funds probably feed more on imperfect decisions and execution by retail investors, pension, and mutual funds than on less advanced quant traders.

Forecasting and Portfolio Management sounds like orthogonal to each other.

( So from a software prospective, we should decouple them, ie, make those components orthogonal to each other as well. )

> Trading financial instruments for profit while controlling the risk.

> From the standpoint of a portfolio manager, price changes are made of two parts: the impact of his own portfolio and the impact of others

Philosophy:

- Why market moves?
  - Economics way (Top-down method)
    - supply and demand
    - market impact
    - fundamental information
  - Market structures (Bottom-up method)
    - microstructure
    - ample market data

Basic Component:

- Data
- Alpha
  - Alpha Combination
- Portfolio Management
  - Risk
  - Cost

## Market Data

For equity, maintain corporate actions (cax) data history is hard already. For research, cax are represented by two numbers: dividend and split.

### Price Adjustment

- Forward adjustment
- Backward adjustment

$$A_d = \prod_{d^{'} \leq d} {\frac{S_{d^{'}}}{1 - D_{d^{'}}/C_{d^{'}-1}}}$$

Then the backward adjustment can be written as:

$$A_{d}^{back} = \frac{A_d}{A_{today}}$$

### Return computation

_Simple Return_: $\mathbf{Q_d} = \mathbf{P_{d-1}} . \mathbf{R_d}$. Linear return is cross-sectional additive with position weights. SR is used in the risk factor as well.

_Log Return_: $R_{ds} = log(1 + R_{ds})$

## Forecasting

The predictability, or signal-to-noise ration $\epsilon$ is around 0.01, which is most important small parameter of quantitative finance.

### Data for forecasts

> Feature or predictors is mostly art, based on financial intuition, understanding how market works, although there are some general approaches as well.
> From feature to predictions, is mostly science, either using method of classic statistics or fancy ML.

- "Fundamental" forecasts
  - Security Master
  - Fundamental Data
  - Analyst Estimation
  - Supply chain and competition
  - Event based
  - MA
  - News
  - Earning surprise
  - Index rebalancing
  - Event dividend
  - Holdings and flows
  - News and social media
  - Macroeconomic data
  - Alternative data
  - Alpha capture
- Technical forecasts
  - Mean reversion
  - Momentum
  - Trading volume
  - Statistical predictors
  - Data from other assets

#### Point-in-time Data

Financial data must be stored point-in-time (PIT), meaning a clear labeling of each records by the time when it had become available in normal datafeed update at that point of time.

Common lookahead cases:

- Not PIT
  - Data vendor modify the history mapping, data, correcting error
- Crowding
  - Data is available to too many traders
- Survival bias
  - More harmful to long term alpha
- Large portfolio using a strategy affect the price, making simulation look better via a self-fulfilling prophecy.

#### Security Master and Survival Bias

Secmaster is used to store two things:

- Attribute, mainly for grouping and decomposition
  - GICS
  - Country
  - Index Component
- Mapping, mainly to find a consistent naming
  - Map tickers from different vendor to an internal id
  - Record when is the mapping existed, changed, or removed (? really matters? )
- List and de-list date

#### Fundamental and Accounting Data

Mainly used to produce long-term alpha, including earnings, sales, cash flow, other financials are among the primary indicators of management quality, financial health, and competitiveness.

To make fundamental analysis and forecasting more manageable, various accounting ratios are often used, and those are dimensionless (the number of coordinates in the feature space).

- earning to price, PE
- book value to price
- debt to equity
- dividend payout to income
- asset to liabilities
- profit to sales

Some more event based data:

- guidance statement
- conference call

Some risk factors:

- growth
- value
- size

#### Analyst Estimates

Analyst reports are much simpler than original company reporting and contain estimates for a subset of accounting fields such as earnings or sales per share and opinions about stock price targets, upgrades/downgrades, bond credit ratings, and so forth.

Due to the attention investor pay to analyst report, no matter their accuracy, the estimates and revision carry a predict power on return directly or via statistics such as variance of the option. (first and second order moment!)

#### Supply chain and competition

Publicly traded companies form various explicit and implicit relationship graphs affecting their business, profitability, and stock price.

#### M&A and risk arbitrage

Companies related by supply chain or competition are more likely to engage in mergers and acquisitions (M&A).
After a merger announcement, the acquirer stock usually goes down and the target is sharply up due to an acquisition premium involved in a merger offer.
quants can build models estimating the probability of a merger initiation and/or its successful completion and trade affected stocks accordingly. This type of strategies is known as risk arbitrage.

#### Event based

- News
- MA
- earning surprise
- index rebalancing
- event dividend/split
  - Or some portfolio manager made a mistake using wrong split factor ( wow ... this could really happen?? )
  - trade before or after dvd ex-date due to possible tex implications

#### Holdings and flows

- Form 13F
  - large US funds are required to file quarterly report of their long holdings.
- Financial Index Regulatory Authority (FINRA)
  - broker and dealers report aggregate short interest position by issue ( not by institutions)
- SEC rule 606
  - broker report routing information quarterly
  - Real-time order flow not on exchange order books clearly carries alpha and can be available to select customers
- SEC Form 144 and Form 4
  - insiders required to pre-announce their trades in the company and derivatives
  - existed in SEC's EDGAR system
- Index rebalancing is kind a flow as well.

#### News and social media

Some quants have explored potential time advantage by automated translation of news feeds in other languages, before the English-based media catch the news.

#### Macroeconomic data

Inflation, interest rate, GDP, unemployment, FX, commodity prices, debt, consumer confidence.

Use to detect market regime change.

#### Alternative data

- crowdsourcing
- retail credit card
- car registration
- employee benefit plan
- satellite images of crops, oil tanks, shopping mall parking lots

#### Alpha capture

Many well-researched alphas are not strong enough to beat trading costs. But when used in isolation, but a combined forecast provides enough stability and strength to drive a high-capacity portfolio.

Alpha pool

- discretionary traders
- brokers
- day traders
- or other sources

### Technical forecasts

Past prices tend to be among the best predictors for future prices.
For better technical forecasts, one may need to work with separate groups of stocks, for example, by liquidity or sector.
ETFs looks like stocks, but their price dynamic is different.
( it seems we should we should find different price dynamic for different asset classes, but HOW? )

#### Mean reversion

Stocks moves are correlated/explained with mean return of a broad equity universe.

The key is find the mean, for which sector, or group. We could also try:

- Multi-factor regression
- Different horizons
- Including other features, such as trading volumes

#### Momentum

- Price mom
- Factor mom

#### Statistical predictors

- historical stock volatility used to be a reasonable predictor of future returns for a number of years (not anymore)
- fractal dimension
- Hurst exponent
- correlation

#### Data from other asset class

Price-volume data for Corporate bonds, stock and bond options, single stock futures, CDS.

- Stock options
  - implied vol -> returns ( risk factor as well )
- CDS
  - CDS spread of corp bond

### Statistical Learning

Bayes Rule:

$$F(y|\vec x) = \frac{F(\vec x, y)}{\int F(\vec x, y) dy}$$

where,

- $F(\vec x, y)$ is a joint distribution between vector $\vec x$ and target $y$, which is unknown. We need to use points $(\vec x_i, y_i)$ to estimate this distribution.
- $\vec x$ is independent variables, aka predictors, regressors, features.
- $y$ is what we want to predict, usually a return.

In quant research, $y$ could be the expect return as well. The **forecast**:

$$f(\vec x) = \mathbb{E} (y|\vec x) = \int yF(y|\vec x)dy$$

Or we could use median of return:

$$f_{med}(\vec x) = Med(y) = \Phi^{-1}(1/2)$$

where $\Phi^{-1}$ is the inverse of:

$$\Phi(y|\vec x) = \int_{- \infty}^{y} F(y|\vec x)dy$$

Distribution-based statistical theory provides a useful framework for developing forecasting algorithms and analyzing their generalization error.

#### Mutual information and Shannon entropy

A measure of dependency of two random variables $\vec x$ and $y$, or how much information about y is gained by knowing $\vec x$, is given by _mutual information function_:

$$
\begin{equation}
    I(\vec x, y)  = H(\vec x) - H(\vec x|y) \\
                  = H(y) - H(y|\vec x) \\
                  = H(\vec x) + H(y) - H(\vec x, y)
    \tag{1}
\end{equation}
$$

Where,

$$H(\vec x) = - \int F(\vec x)logF(\vec x)d \vec x,$$

$$H(\vec x, y) = - \int F(\vec x, y)logF(\vec x, y)d \vec x dy.$$

The smaller $H$ the more information about x is available. Uniform distribution has highest entropy (hence least information), whereas Dirac delta function has the least entropy (hence max information).

MI could be used for predictor selection and dimensionality reduction. MI also captures different stock return time series pattern than Pearson correlation.

#### Likelihood and Bayes inference

Another more general distribution-based learning uses maximum likelihood (MLE) or maximum a posteriors(MAP) estimates.

Bayesian methods were used to estimate out-of-sample model performance.

#### Mean square error and correlation

For equities, one can hope to achieve a correlation of order 1%. The main target of absolute-value- or median-based losses is enhanced robustness with respect to outliers and fat-tailed data.

#### Weighted law of large numbers

The lack of strong forecasts is in part compensated by the large number of tradable securities, especially stocks, and the number of days (or perhaps seconds in the case of high frequency trading) during which a quantitative trading strategy is run.

_law of large numbers_, the variance of the average of N independent random variables decreases as the N increases.

### Machine Leaning

If you have a good rule-based strategy, chances are it can be improved by rewriting it in terms of an ML forecast with features based on the trade rules.

#### Curse of dimensionality, eigenvalue cleaning, and shrinkage

In the financial forecasting context, this generally means one must use an explicit or implicit dimensionality reduction to hope for any profitable forecast-driven portfolio.

#### Smoothing and regularization

Correcting the covariance matrix is not the only way to improve out-of-sample forecast performance.

_spline_ $f(x)$ minimizes the loss function:

$$L(f(x);\lambda) = \sum_{i}w_i[y_i - f(x_i)]^2 + \lambda \int [f''(x)]^2dx$$

_total variance denoising_:

$$L(f(x);\lambda) = \sum_{i}w_i[y_i - f(x_i)]^2 + \lambda \int |f'(x)|dx$$

#### Boosting

How to combine many different weak learners which are bit better than pure noise into a strong learner? This is boosting..

## Forecast Combining

combining forecasts by analyzing pnl time series maybe not the best idea. Forecast should focus on he best possible predictor of the returns. Portfolio is another problem.

First, we could try to minimizing the mean square predictor error of different forecasts:

$$f(\vec \alpha) = argmin Var(f(\vec \alpha) - \vec R)$$

where, $\vec R$ is the realized returns, K forecasts $f_i$ as predictors. **If we only have few predictors to combine**, this is ok. But if predictors goes larger, we face curse of dimensionality.

### Dimensionality reductions

Static diversification benefit has a limit based on correlation.

#### Clustering

What matters the most for forecast combining is the definition of similarity between two forecasts.
Clustering can be formulated as an optimization problem, such as minimization of the sum of distances from objects to cluster centroids. This kind of optimization is typically NP-hard and is solved with approximate heuristic algorithms.

### Alpha Pool Management

Smart centralized forecast combining algorithm and cost-aware portfolio optimization is perhaps the most efficient quant business model used by the more advanced quantitative funds like Renaissance Technologies.

Alpha researchers, who may not be directly involved in portfolio trading, are best provided with meaningful guidelines and feedback.

#### Forecast development guidelines

- Point-in-time data
- Horizon and scaling
  - variable horizon
- Type of the target return
  - actual return
  - market-neutral return
  - residual against a factor model
- Performance metrics
  - predict future with a horizon
  - skewness of errors
  - distribution of errors
  - sharpe ratio, turnover, factor exposure
  - sector exposure
- Measure of forecast uncertainty
  - confidence interval
- Correlation with existing forecasts
- Live raw trading data
  - ML feature may have very long research data pipeline
- Overfit handling

#### Pnl attribution

- Marginal attribution
- Regression-based attribution

## Risk

- VaR and Expected shortfall
- Factor models
  - sector and group, such as GICS
  - principal components
  - geography factor
  - style factors
    - growth
    - value
    - size
    - volatility
    - momentum
    - earnings
    - leverage
    - dividend yield
  - macro factors
    - FX
    - interest
    - commodity price

As long as the number of factors K is smaller than the stock universe, factor model provides a compressed and interpretable representation of security covariance:

$$C = Diag(\sigma ^2) + L'UL.$$

where, $\sigma_s^2$ is the residual variance of stock s, $L_i$ are the loadings for K risk factors, and $U$ is the KxK covariance matrix of factor returns, $\rho_i$:

$$\rho = argmin \sum_s w_s(R_s - \sum L_{is}\rho_i)^2$$

## Trading Cost and Market Elasticity

In an efficient market, the predictability of returns is close to the costs of trading.

At least, we need to take care of two kinds of cost:

- slippage
  - half the cost of a roundtrip trade per share.
  - depending on bid-ask spread when size is not impact market
  - $slippage = c_1 * mean\_spread + c_2$
  - other fee, such as fee/rebase, tax, fixed cost can be modelled as slippage
- impact
  - The market impact of trades is the very reason why asset prices moves
  - some trades are done by _liquidity trader_, who has to trade.

## Portfolio Construction

Systematical trading needs:

- forecast of future returns
- risk model
- trading cost model
- portfolio construction algo using above three

### Hedge allocation

Signal based allocation and then hedge against some factors.

Constrained allocation can be handled using penalties by maximizing a utility function.


### Forecast from rule-based strategy

can be handled by a multi-period optimizer.

### Time effect of the trade

### Portfolio capacity

Can we predict how the performance would scale at different portfolio sizes and determine its size limit?

### Kelly criterion and optimal leverage

Variance is an important consideration for portfolio construction, but so is also drawdown. Even for portfolio size well below its capacity, the question of the optimal risk appetite is meaningful and nontrivial.

$$\Phi^* = p - \frac{1-p}{R}$$

where, $\Phi^*$ is the optimal leverage, $p$ is the probability to win, and $R$ is the return when win. 

the Kelly criterion is an elegant way of bet or leverage sizing using a subjective choice of the betting horizon and the util- ity such as log return. 

Forecast combining methods discussed in Chapter 3 work best for the same horizon. (We could build different signals and trade them separately.)

## Simulation

Reasons that simulation does not match live trading:

- wrong cost model
- self-fulfilling prophecy due to history already affected by the portfolio
- Inadequate modeling of probabilistic execution
- not using PIT data
- survival bias
- bugs in live trading

## Some good refs books, papers, and website

- Finding Alphas: A Quantitative Approach to Building Trading Strategies
- Trades, Quotes and Prices: Financial Markets Under the Microscope
- Active Portfolio Management: A Quantitative Approach for Producing Superior Returns and Controlling Risk
- Feature Engineering and Selection A Practical Approach for Predictive Models (Max Kuhn, Kjell Johnson)
- In Search of the Origins of Financial Fluctuations The Inelastic Markets Hypothesis
- How markets slowly digest changes in supply and demand
- <https://arxiv.org/>
