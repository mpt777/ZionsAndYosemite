# Battle of the Parks
#### Marshal Taylor


## Introduction:
I love National Parks, and I wanted to see if there was any real difference in popularity between two of the most famous parks. The best way to measure this would be to model the number of visitors. Is there a difference in the mean number of visitors for Zions (Utah) and Yosemite (California) National Parks. 

## Methods:
The data was obtained directly from the United States National Park Service, as they make all of their visitor data publicly available on their website. Their data was downloaded in a CSV format, and cleaned within R (To download the data, please visit https://irma.nps.gov/STATS/, find the park of interest, and select Recreation Visits By Month (1979 - Current Calendar Year) ). Given that the data is a visitor count by month I am using a Poisson Distribution to model the likelihood. A Poisson Distribution is assumed to be valid if a few assumptions are met.
1) The number of occurrences  must be any non-negative integer. K is modeling the number of visitors to each of these parks. The number of visitors is an integer, and cannot be negative.
2) Occurrences must be independent. Since people often visit parks in groups, this assumption may be violated, as one person entering the park may lead to a temporary increase in the probability of a visitor entering directly after them. Overall, I think this assumption is still met.
3) The rate of occurrence must remain constant. This assumption is violated as there is seasonality within the data. There are more visitors during the summer months for each of these parks than there are during the winter months. 
Although our model violated assumption 3, I will proceed using the Poisson Distribution with the caveat that the analysis will be a rough approximation over the whole year, and we will be losing seasonality insight.

The only parameter for a Poisson Distribution is an average rate of occurrence, which I am assuming to be 22026 visitors per month for each of the parks. With a poisson, we can find the mean number of visitors for any given lambda, or the average number of visitors. With the Poisson Distribution as the likelihood model, I found it appropriate to use a Gamma Distribution as the prior as they are conjugate. Since the gamma is continuous, I transformed the visitor data on the log scale to keep numbers reasonable. The prior shape parameter is 60 and our rate is 10 (~ 22026 when exponentiated).

## Results:
### Summary Statistics:

The log mean number of visitors for Zions in 2019 is 12.66425 per month (~ 374022.3 non-log), and the log standard deviation is 0.6638583 (~190688.5). The log mean number of visitors in Yosemite in 2019 is 12.62138 (~368571.8) and the log standard deviation is 0.6837923 (~222282.5). Based on these simple summary statistics, it’s easy to see that these parks are quite similar in terms of visitors with Zions having a slightly higher mean average number of visitors, while Yosemite has a higher standard deviation between the months. I assume Yosemite has a greater dip in attendance during the winter months than Zions has, due to Zions staying warmer throughout the whole year.

### Priors and Posteriors:

The 2019 data for Zions pushed the prior distribution farther to the right. This signifies that there were more visitors to the park than we priorly predicted. The 95% confidence interval for the posterior had the bounds [10.08259 12.90994] (~ [203828.0 204205.5]). There is a 95% probability that the true (unknown) estimate for the mean log visitors for Zions lies within the given interval, given the evidence provided by the observed data.

![zions](https://user-images.githubusercontent.com/70606376/113515534-501e5280-9532-11eb-87c0-b6459d644a66.png)

The 2019 data for Yosemite also pushed the prior distribution farther to the right, just like the Zions data. The 95% confidence interval for the posterior had the bounds [10.06065, 12.88511] (~ [200856.4, 201231.1]). There is a 95% probability that the true (unknown) estimate for the mean log visitors for Yosemite lies within the given interval, given the evidence provided by the observed data.

![yosemite](https://user-images.githubusercontent.com/70606376/113515535-50b6e900-9532-11eb-8bd5-2dfe03358134.png)

When the posteriors are plotted together, there is almost a perfect overlap between the two parks. Zions and Yosemite almost have the same number of visitors each year, and the log scale shrinks the”visible” difference in visitors to almost nothing.

![together](https://user-images.githubusercontent.com/70606376/113515533-501e5280-9532-11eb-9a5b-69d8ee61ab15.png)

### Monte Carlo Approximations:

Given that the domain of an exponential function is all positive numbers having, it was necessary to transform the data back to their original scale (Everything from this point on will NOT be on the log scale, and instead on the linear scale). I assumed Zions to be the more popular park, so the distribution for the difference in visitors between these two parks is a monte carlo approximation of Zions - Yosemite. The mean difference was 2973.207 visitors, and the 95% confidence interval of this distribution had the bounds [2708.315 3238.829]. Since the confidence interval does not include 0, we can conclude that there is any difference in the mean number of visitors between Zions and Yosemite, with Zions having ever so slightly more visitors!

![difference](https://user-images.githubusercontent.com/70606376/113515532-501e5280-9532-11eb-9134-23ec2e4ac4b1.png)

## Conclusion:

Even though Yosemite and Zions are as different as any two parks could be, parks both receive a very similar distribution of visitors. When looking at the 2019 data, we can conclude that Zions was the slightly more popular park, as it had a mean difference in visitors of 2973.207 using the monte carlo methods. 

Although the Poisson-Gamma modelled the data well, there still was no element of seasonality to the analysis. To better capture the seasonal trends of the data, other machine learning tactics could be used (such as a SARIMA regression model) to better fit the data. The prior distribution in the analysis was the same for each of the parks. With more time, I could’ve found a prior better suited for each of the distributions.

Naturally, I was curious to see what would happen if more data was included in the analysis. I quickly added the last 10 years of visitor data to the posterior distributions (2020 was omitted due to covid) to see the effect it would have in our analysis. When years 2010-2019 were added to the analysis, Yosemite was the more popular park (in terms of visitors). This suggests that Zions has become more popular over the years relative to Yosemite, leaving Yosemite more popular when the last 10 years are accounted for, but less popular when the analysis only includes 2019 (Zions is more popular when the starting years for the analysis are 2016:2019, and Yosemite is more popular when the starting year is 2010:2015).

The distribution for the difference in visitors between these two parks is a monte carlo approximation of Yosemite - Zions. The mean difference was 42377.19 visitors, and the 95% confidence interval of this distribution had the bounds [42245.31, 42510.34]. Again, the interval does not contain 0, so we conclude that Yosemite has more visitors per month.
![difference2](https://user-images.githubusercontent.com/70606376/113515531-501e5280-9532-11eb-9525-9361bfe26059.png)

To better understand the rise in popularity for Zions National Park, a moving average regression model can be used. This will allow the model to better fit the trend of popularity that we’ve been seeing in the data. More analysis will need to be done.
