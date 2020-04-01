# Mathematical-Methods

## 1. Differential Equation
> How do you **design**
 - a boat that doesn't tip over as it bobs in the water,
 - the suspension system of a car for a smooth ride,
 - circuits that tune to the correct frequencies in a cell phone?

> How do you **model**
 - the growth of antibiotic resistant bacteria,
 - gene expression,
 - online purchasing trends?

__[The answer]:__ Differential Equations. They are the language of the models we use to describe the world around us. We will develop the mathematical tools needed to solve linear differential equations. In the case of nonlinear differential equations, we will employ graphical methods and approximation to understand solutions.


-------------------------------------------------------------------------------------------------------------------
## 2. Time Series
Time series is a dataset **collected through time**. Now, since we are doing sampling with adjacent points in time, we naturally introduce a correlation into the system, which means the classical, statistical inference might not work in this setting.  
 - Available time series from 'astsa' package in R
   - jj (Johnson and Johnson Quarterly Earnings for 84 quarters)
   - flu (Pneumonia and influenza deaths in the U.S. per 10,000 people Monthly for 11 years)
   - globtemp (Land-ocean mean temperature deviations for the years 1880-2015)
   - globtempl (Land only mean temperature deviations for the years 1880-2015)
   - star (Variable Star: The magnitude of a star taken at midnight for 600 consecutive days )  
### A. How to Describe Time Series   
__a> Stationarity__
 - In a stationary time series, there is no systematic change in **mean** (no trend), no systematic change in **variance**, and no **fluctuation**(periodic variations).. so..basically, the properties of one section of a data are much like the properties of the other sections of the data. Usually "stationarity" is a property of a stochastic process of a model, not a time series. But we say `stationary time series` if you think that it can be modeled with **`stochastic process`**...  
 - If we have a non-stationary time series, which we usually have, we basically do some `transformations` to get the stationary time series. Once we have a stationary time series, we model it and then go back and model our non-stationary time series. So we use the transformations as a middle step.
 
__b> Acv (Auto-Cov-Function & Coefficient)__
 - `COV` measures the strength of linear association(so dependence) b/w two random variables...assuming both shares same unit.
 - `Stochastic process` put a lot of random variables ![formula](https://render.githubusercontent.com/render/math?math=\X_t)(each of which follows different distribution) together and give them a sequence. In deterministic processes (for example, solution of ordinary differential equation), You start with some point and the solution will tell you exact trajectory. The stochastic process is basically opposite of that. At every step `t`, you have some randomness (a single random sampling from ![formula](https://render.githubusercontent.com/render/math?math=\X_t)), i.e. there is a certain distribution that ![formula](https://render.githubusercontent.com/render/math?math=\X_t) follows at that time stamp `t`.
 - `Time Series` can be actually a series of realizations from the stochastic process going on the back one. If we characterize time series as a realization of a **stochastic process**, let's say the realization of ![formula](https://render.githubusercontent.com/render/math?math=\X_1) is my first datapoint in the time series, and the realization of ![formula](https://render.githubusercontent.com/render/math?math=\X_2) is my second datapoint in my time series. Thus, the stochastic process might come with ensemble of realizations(curves, TS, or sampling trajectories), but now I only have one time series(curve). 
 - `Auto-Cov-Function` takes **covariance** of different elements in our TS as our one realization(curve) of the stochastic process. If you take the value from ![formula](https://render.githubusercontent.com/render/math?math=\X_t) and ![formula](https://render.githubusercontent.com/render/math?math=\X_s) and `s` and `t` might be in different locations and we'll get the cavariance of them. <img src="https://user-images.githubusercontent.com/31917400/78037218-281a7300-7363-11ea-8162-b4ce0ee4fb1f.jpg" /> The Auto-Cov-Function above will only depend on the time difference between these random variables. It doesn't matter what `t` is. The **time difference `k`** actually decides the nature...coz we assume we are working with **`stationary times series`**: "the properties of the one part of the time series is same as the properties of the other parts of the time series".
   - cov(![formula](https://render.githubusercontent.com/render/math?math=\X_1) to ![formula](https://render.githubusercontent.com/render/math?math=\X_1plus_k)) = cov(![formula](https://render.githubusercontent.com/render/math?math=\X_10) to ![formula](https://render.githubusercontent.com/render/math?math=\X_10plus_k)) = ![formula](https://render.githubusercontent.com/render/math?math=\gamma_k)
   - The thing is, we usually do not have the stochastic process, but we only have a time series, just a single realization of the stochastic process. So we have to use the TS to approximate cov ~> ![formula](https://render.githubusercontent.com/render/math?math=\C_k)
 - `Auto-Cov-Coeff` at different lags `k` is ![formula](https://render.githubusercontent.com/render/math?math=\gamma_k) = 𝐶𝑜𝑣(![formula](https://render.githubusercontent.com/render/math?math=\X_t), ![formula](https://render.githubusercontent.com/render/math?math=\X_tplus_k)) ~> ![formula](https://render.githubusercontent.com/render/math?math=\C_k) <img src="https://user-images.githubusercontent.com/31917400/78047111-e3e19f80-736f-11ea-9378-2231df175586.jpg" />
   
   - : `acf(TS_data, type='covariance')` returns a bunch of Auto-Cov-Coefficient values ![formula](https://render.githubusercontent.com/render/math?math=\C_k). 

__c> ACF (Auto-Corr-Function & Coefficient)__
 - Assuming weak stationarity (to define our TS as a Random Process), the **`auto-correlation coefficient`** between two random variables - ![formula](https://render.githubusercontent.com/render/math?math=\X_t) and ![formula](https://render.githubusercontent.com/render/math?math=\X_tplus_k) - is: <img src="https://user-images.githubusercontent.com/31917400/78048697-05dc2180-7372-11ea-816a-5c71815aa778.jpg" />   
 
   - :  `acf(TS_data)` returns a bunch of Auto-Corr-Coefficient values ![formula](https://render.githubusercontent.com/render/math?math=\gamma_k). 
   - From the chart above, we do not have much correlation between all the different lags(all values are within the significance level). Just because we generated this data as a purely random process(same "rnorm" for every RV), we cannot expect to see the correlation within different lags. 

### B. How to Model Time Series   
__a> method 01. Random Walk__
 - If assuming ![formula](https://render.githubusercontent.com/render/math?math=\X_t=) X_(t-1) + ![formula](https://render.githubusercontent.com/render/math?math=\epsilon_t) where ![formula](https://render.githubusercontent.com/render/math?math=\epsilon_t~\N(\mu,\sigma^2)), and if **![formula](https://render.githubusercontent.com/render/math?math=\X_0=0)**, then ![formula](https://render.githubusercontent.com/render/math?math=\X_1=\epsilon_1), thus: <img src="https://user-images.githubusercontent.com/31917400/78176394-51ff9280-7454-11ea-862a-6978765dfc0c.jpg" />
   ```
   X=NULL
   X[1] = 0
   
   for(i in 2:1000) {
      X[i] = X[i-1] + rnorm(1)
   }
   
   print(X)
   random_walk_test <- ts(X)
   plot(random_walk_test, main="Random Walk Example", xlab="Days", ylab=" ", lwd=2)
   ```
 - But, random walk above is not a stationary time series. It would not make sense to actually find acf of it because we define acf for stationary time series. If we plot `acf()`, it would show there is a high correlation in this data and there is no stationarity.
 - Can we remove this stupid trend in our random walk? How to turn back to the ransom process? `diff()` gives us the bunch of differences b/w each consecutive samples. if **![formula](https://render.githubusercontent.com/render/math?math=\X_0=0)**, then they are random noises that follow N(![formula](https://render.githubusercontent.com/render/math?math=\mu,\sigma^2)) that we set. 
   ```
   plot(diff(random_walk_test))
   plot(acf(diff(random_walk_test)))
   ```
   <img src="https://user-images.githubusercontent.com/31917400/78182015-46fd3000-745d-11ea-8ae7-146b02f3c641.jpg" />

__b> method 02. Moving Average Process__
 - First, identify MA. What makes the **![formula](https://render.githubusercontent.com/render/math?math=\X_t)**? We can express **![formula](https://render.githubusercontent.com/render/math?math=\X_t)** as a linear combination of the **noises** that affects it.  
   <img src="https://user-images.githubusercontent.com/31917400/78192690-86814780-7470-11ea-93f4-14b9509b81c8.jpg" /> 
 
 - 
 
 
 


































