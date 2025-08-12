### COVID-19 Data Analysis
###  Aim
To check if there is any relationship between coronavirus spread in a country and how happy people are living in that country.

###  Steps
### 1. Importing the Libraries

     Imported libraries such as pandas,numpy,matplotlib and seaborn
     
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### 2. Import the Dataset
```
dataset = pd.read_csv("Covid19_confirmed_dataset.csv")
```

### 3. Deleting the Unnecessary Column

```
df = dataset.drop(["Lat", "Long"], axis=1, inplace=True)
```
### 4. Aggregate the Values by Country

```
corona_aggregate = dataset.groupby("Country/Region").sum()
```

### 5. Visualize the Data for Some Countries

```
corona_aggregate.loc["China"].plot()
```
Used .loc to plot and find the trend.
Similarly plotted for Spain and India.

### 6. Calculate a Good Measure Representing Spread

```
corona_aggregate.loc["China"][3:5].plot()
corona_aggregate.loc["China"].diff().plot()
```
Used slicing to check spread in the first few days.
Calculated first derivative to find rate of spread.

### 7. Finding Maximum Infection Rate for All Countries

```

countries = list(corona_aggregate.index)
max_infection_rates = []
for c in countries:
    max_infection_rates.append(corona_aggregate.loc[c].diff().max())
```

### 8. New DataFrame with Max Infection Rates

```
corona_aggregate["Max_infection_rate"] = max_infection_rates
```

### 9. Import Happiness Report and deleting unnecessary column

```
happiness_report = pd.read_csv("worldwide_happiness_report.csv")
unnecessary = ["Overall rank", "Score", "Generosity", "Perceptions of corruption"]
happiness_report.drop(unnecessary, axis=1, inplace=True)
```

### 10. Set Index as Country

```
happiness_report.set_index("Country or region", inplace=True)
```

### 11. Join the Two Datasets

```
data = corona_data.join(happiness_report, how="inner")
```

### 12. Correlation Analysis

```
data.corr()
```

Positive → both increase
Negative → one increases, other decreases
Zero → no relationship

we can observe that weak corr between GDP and max infection rate, very low or negligible corr between freedom and infection,weak corr between social support and infection rate.
and strong corr between GDP and healthy life ,GDP and social support also have good corr.

### 13. Visualization
GDP per Capita vs Max Infection Rate
```
x = data["GDP per capita"]
y = data["Max_infection_rate"]
sns.scatterplot(x=x, y=np.log(y))
sns.regplot(x=x,y=np.log(y))
```
similarly for

~~Social Support vs Max Infection Rate

~~Healthy Life Expectancy vs Max Infection Rate

~~Freedom vs Max Infection Rate

### 14. Conclusion

Countries with higher GDP per capita, better health, and stronger social support tend to have slightly higher maximum COVID-19 infection rates,
though these relationships are weak. 








