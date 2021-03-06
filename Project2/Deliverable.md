# Project 2: Generating synthetic populations 

## Q1    
This DHS report is for Sierra Leone 2013
- Data collection took place from 29 June 2016 to 4 August 2016
- Based on a nationally representative sample of 13,006 households and 16,500 completed interviews of women, the main objectives of the 2013 SLDHS were to provide up-to-date information on fertility and childhood mortality levels; fertility preferences; awareness, approval, and use of family planning methods; maternal and child health; knowledge and attitudes towards HIV/AIDS and other sexually transmitted infections (STI); and prevalence of HIV/AIDS. All women age 15-49 who slept in the selected households the night before the survey were eligible for the survey. The survey results are representative for the country as a whole, for the urban and rural areas separately, for each of the four geographical regions, and for each of the 14 administrative districts.
- Apart from the women’s survey, a survey among men was conducted in one of every two households selected for the women’s survey. All men age 15-59 who slept in the households selected for the men’s survey were interviewed with the Man’s Questionnaire. All eligible men age 15-59 and all eligible women age 15-49 in the households selected for the men's survey were eligible for HIV testing.
- The sample for the 2013 SLDHS was a stratified sample selected in two stages from the 2004 census frame. Stratification was achieved by separating each district into urban and rural areas. The West Urban Area has only urban areas. In total, 27 sampling strata had been constructed. Samples had been selected independently in each stratum, by a two-stage selection process. By sorting the sampling frame according to administrative orders and by using a probability proportional to size selection at the first stage’s sampling, an implicit stratification and proportional allocation would have been achieved at each of the administrative levels.
- Questionnaires - Household, Women, and Men Questionnaires**
- The columns that were analyzed from the dataset were the household id (hhid), units(hv004), weights(hv005), size(hv009), sex (hv104_01 - 104_90), age(hv105_01 - hv105_90), education(hv106_01 - 106_90), and wealth(hv270)

![dataset](cbind.png)

## Q2

### Expanding survey data to persons
Sum of weights for the hhs is the same as number of rows as expected cos the survey design was set at the unit of the household, but when we disaggregated (pivoted) from household down to the persons within the households, the weights change that is why the weight of persons does not match the rows of the number of persons because we are validating the number of persons from the larger population. During the survey the enumerators may have had an idea of how many people may be in the house, but they did not know exactly. The number of households may have been less or much more than they may have estimated when they were doing the survey. So when we pivoted from households to persons we compromised the inherent accuracy of the raw data itself and got 74835.12. This then messed with the weights of the observations that is why they don’t match the number of rows for persons. So using the inherent design of the survey we would have expected to produce 74835 persons, while the raw data itself produced 75299. A 1.2 % survey of all the persons from the population of Sierra Leone were selected to include in the survey and we got this by dividing the number of rows in the person column by the total population of Sierra Leone.

Below is a picture of spatially locating the households in Sierra Leone. The unit of this is still household and not persons. This image is from when I was initially just focusing on households

![not_pivoted_pop_spread](sle.png)

The above plot may not be the most accurate. It’s a randomly generated point of where various households will be located based on the density of the population. Obviously the higher the population density in an area the higher the chance of more points being generated in that area. However if we wanted to be more accurate, we could have requested GPS data from DHS to improve spatial accuracy because it would have given us some central point in our data to better estimate the number of households in a certain range. So if we had that, we could have taken the center point of the cluster and based on whether if was rural or urban, we could have taken that radius and overlaid it over our population density and further improved the accuracy, so instead of using the whole country we could have set the window to spatially locate the households to the radius of the GPS data

### Trying to spatially locate adm0 households and expand households to persons

When I tried to pivot my data and spatially locate it at adm 0, I faced a bunch of computational problems. My R studio kept having to restart as it could not process that much data. This was because I was trying to get R to process almost a million observations (998296 observations to be exact) and spatially locate them over a very wide area.

## Q3

After analyzing my data, I realized unlike most people in the class, my data had been tailored towards adm2, so that is what I used.

### Trying to spatially locate adm2
For adm2, I subsetted Moyamba from the data to work on as that had been my area of interest for Sierra Leone in project 1. The average number of households in Moyamba is 47386. The weight of the household size in Moyamba was 722.5517 - what the design projected the number of households to be. It’s saying we are anticipating approximately 723 households. The number of rows was 847 - in reality this is the number of households

### Geom_density plot interpretation
```
adm2_sampP <- slice_sample(hhs, n = moyamba_hhs_n, replace = TRUE)
adm2_sampP1 <- slice_sample(moyamba_hhs, n = moyamba_hhs_n, replace = TRUE)
```
adm2_samP - drawing household observations for Moyamba from the whole dataset

adm2_samP1 - drawing household observations from Moyamba itself
The above lines of code was used to generate the density plot below

![density_plot](geom_density.png)

As you can tell, the raw dhs data has a much greater distribution and heterogeneity. If you subset to just the Moyamba observations, what happens if you're suppressing the heterogeneity cos the sample size is too small. What may end up happening is you may end up overprediction some elements of the data and underprediction some. So you make the decision to use the household sample data instead of the Moyamba because there aren’t that much observations in Moyamba as to the raw DHS data, so while it may not be exactly where we are in terms of adm2, it’s a better representation in the small subset

### Expand households to persons
I expanded my households to person by pivoting the following columns in my data: gender, age, and education.
I then plotted the expanded data and got this
![moymaba_spatial_expansion](moyamba.png)
Also, using the inherent design of the survey we would have expected to produce 47302 persons, while the raw data itself produced 47386. The calculated weighted error was 0.001765266

## Q4
*Raw data output*
![raw](raw.png)
*Scaled data, so each value would reflect the distance from the mean in units of standard deviation*
![scale](scale.png)
*Normalize brings data to the 0 to 1 scale by subtracting the minimum and dividing by the maximum of all observations. This preserves the shape of each variable’s distribution while making them easily comparable on the same “scale”*
![normal](normal.png)
*An alternative to normalize is the percentize function. This is similar to ranking the variables, but instead of keeping the rank values, divide them by the maximal rank. This is done by using the ecdf of the variables on their own values, bringing each value to its empirical percentile. The benefit of the percentize function is that each value has a relatively clear interpretation, it is the percent of observations with that value or below it*
![percentize](percent.png)

Heat maps with a lot of color represent variation in our data. Same color means no variation. That’s why percentize is better because it breaks it down even further, so you can easily analyze and see which columns of your data are doing a much better job

To test the accuracy of my synthetically generated data, I ran an ML algorithm on education and got surprising results. When compared to a randomly generated synthetic population that describes the demographic attributes of households and persons, it does a pretty okay job of closely approximating reality, especailly for the education variable where accuracy is approximately 74% <br />
![accuracy](accuracy.png)

![roc_curves](roc_curve.png) <br />
Roc curves - 0, 1, and 2 are 3 of my better predictors for eductaion. For a worthless test, my line would lie on the diagonal dotted line. The closer an ROC curve is to the upper left corner, the more efficient is the test. From our 3 better predictors you can tell all cut-offs the true positive rate (sensitivity)  is higher and the false positive rate (specificity) since the curves are closer to the upper left corner. 

To improve model accuracy even further, the GPS data would have really come in handy to help narrow things down
