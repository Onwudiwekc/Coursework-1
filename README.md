Mauna Loa's co2 concentration levels
---
# Section 1: The aim of this project

The aim of this project is to explore a time series using Meta’s Prophet forecasting system.

## What is Mauna Loa?

Mauna Loa, located on the Big Island of Hawaii, is the world's largest active shield volcano by volume and area, comprising over half the island. Standing 13,681 feet (4,170 m) above sea level, it rises over 30,000 feet from the ocean floor. It is highly active, with its most recent eruption occurring in November 2022.

![](images/Mauna Loa.jpg)

Now that we've caught up to speed with the beautiful but terrifying gem known as Mauna Loa. Lets get into the knitty gritty nerdy stuff and look at the numbers...

# Code

As mentioned before, the aim is to create a time series displaying how the atmospheric conditions in Mauna Loa have changed over time. We do this using the CO2 dataset.

The CO2 dataset contains monthly CO2 concentration values from 1959 to 1997, the data is expressed in parts per million. 

We start by importing a forecasting system known as **Prophet**

```{r echo=FALSE}
library(prophet)
```
 
Then we use the command **data** to load specified data sets, in this case the dataset is known as CO2.

```{r echo=FALSE}
data(co2)
```

We then use the command **time** to create a vector of times at which a time series was sampled.
Conveniently, we can split up the messy data into neat monthly timeframes using **yearmon**.

```{r echo=FALSE}
time(co2)
time_values = time(co2)
library(zoo)
dates = as.Date(as.yearmon(time_values))
```

Now, we create a dataframe using the command **data.frame**. Then we use the forecaster prohpet to extrapolate some future data for 8 months ahead of the end date of the CO2 dataset.

For a quick sidenote, we assume there is not any daily or weekly seasonality here.

```{r echo=FALSE}
WeatherDataframe = data.frame(ds = dates, y = as.numeric(co2))
WeatherForecaster = prophet(WeatherDataframe)
FutureDataFrame = make_future_dataframe(WeatherForecaster, 8, freq = "month", include_history = TRUE)
Prediction = predict(WeatherForecaster, FutureDataFrame)
```

# **Time Series Analysis: CO₂ Concentration at Mauna Loa**

Finally, we plot our data as a time series to show the changes

```{r}
plot(WeatherForecaster, Prediction)
```

## Long-term trend

The most prominent feature of the time series is a strong upward trend in atmospheric CO₂ concentrations over the observed period. At the beginning of the series in the late 1950s, CO₂ levels are approximately 315 ppm, while by the late 1990s they approach 365–370 ppm.

This steady increase indicates that atmospheric CO₂ has been accumulating over time. The increase appears approximately linear to slightly accelerating, suggesting that the rate of emissions into the atmosphere has been persistent and potentially increasing. To take a few guesses, this change in atmospheric CO2 levels is likely due to:

-Fossil fuel combustion
-Volcanic activity
-Deforestation and land-use change in the surrounding area

The overall increase of roughly 50 ppm over about four decades represents a substantial change in atmospheric composition and highlights the long-term impact of human and volcanic activity on the global carbon cycle.

## Seasonal Pattern

There is a clear and consistent seasonal cycle, visible as repeating peaks and troughs each year. These oscillations occur roughly once every 12 months, indicating strong annual seasonality.The seasonal fluctuations are caused mainly by biological processes.

Spring and Summer:
During the growing season, plants absorb CO₂ through photosynthesis, causing atmospheric CO₂ concentrations to decline.

Autumn and Winter:
As plants die back and photosynthesis decreases, respiration and decomposition release CO₂ back into the atmosphere, causing concentrations to rise.

This creates the characteristic saw-tooth pattern observed in the graph.

## Variability and noise

Aside from the dominant trend and seasonal cycle, the data shows minimal irregular fluctuations or random noise. The observations appear smooth and consistent, indicating:

-High measurement reliability
-Stable monitoring procedures
-Limited short-term disturbances affecting the data

Any noise is negligible relative to the magnitude of the trend and seasonal variation.

## An overview

The data has a persistent upward trajectory in CO₂ concentration over time. For the seasonality, it has a strong annual seasonal cycle with regular peaks and troughs.In terms of noise, there are mall random fluctuations around the trend-seasonal structure.

Because of the strong trend, the series is non-stationary, meaning its mean changes over time. 
