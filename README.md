# Encouraging COVID-19 Vaccination in the US
## Understanding vaccine hesitancy

**Author**: Jessica Miles

<img src="./images/pexels-cottonbro-3952241.jpg" width=80%>

This repository applies machine learning models to [Household Pulse Survey results published by the Census Bureau](https://www.census.gov/programs-surveys/household-pulse-survey/datasets.html) to determine the primary predictors of vaccine hesitancy and optimism in the US. The purpose is to understand common population characteristics of those who are hesitant so that federal, state, and local governing bodies and groups can develop effective strategies to encourage vaccination.

## The challenge: Declining COVID-19 vaccination rate

- State and local governing bodies around the US are asking: how can we encourage more people to get vaccinated sooner?
- Percentage of vaccinated residents is a key metric used to decide when and how to lift pandemic restrictions 
- Vaccines have proven effective at protecting people from severe illness from COVID-19, which we of course want fewer Americans to suffer from
- Vaccine doses administered per day initially increased as more people became eligible, but has been declining since mid-April. All adults were eligible in all states as of April 19th, 2021.
- Only 61% of adults have received at least one dose as of May 25, so there are definitely holdouts.
- The percentage of vaccinated people to reach herd immunity varies per disease, and isn't known for COVID-19 yet. For measles it was 95%; for polio it was 80%. Our goal is herd immunity for COVID-19, and 61% is probably not enough yet.

### Vaccine Doses Administered Over Time
<img src="./images/vax_rate_may25_2021.png" width=90%>

Chart source: [CDC](https://covid.cdc.gov/covid-data-tracker/#vaccination-trends)

## Objective of this analysis
To determine whether data-driven insights can reveal strategies to overcome hesitancy and encourage vaccination. Currently, some states are starting to offer perks such as cash or food vouchers or automatic entrance into a lottery, but it's not clear if offering financial incentives will be the best approach.

In this analysis, I answer the questions:
- What, if any, are common characteristics among the vaccine hesitant?
- Do hesitant people with different characteristics give different reasons for their hesitancy?

## Methods
- Trained logistic regression and random models to predict vaccine hesitancy or optimism
- Tested several different feature selection methods from scikit-learn to reduce dimensionality, such as VarianceThreshold, and SelectKBest using three different classification algorithms. Combined these results with applying Lasso regularization to initial Logistic Regression model and selecting top predictors from initial Random Forest models by building a voting system to select features for final models that at least 2 methods agreed should be kept.
- Analyzed top predictors of hesitancy to understand their significance, combined with patterns in hesitancy reasons within population subgroups.

## Sample Data Summary

- [Household Pulse Survey results](https://www.census.gov/programs-surveys/household-pulse-survey/datasets.html)  published by the Census Bureau
  - Random statistical samples from all 50 states and top 15 metro areas
  - Adults 18 years or older
  - Used data from survey conducted March 3 to March 15, 2021 (week 26)
  - Public microdata sample of ~78,000 responses
- Survey questions included:
  - How life had changed for the household during the pandemic
  - Vaccine doses received, intent to be vaccinated, and reasons why not unless definitely intended to be vaccinated
  - Information such as family size, housing situation, state or metro area, employment status, income, age, race and ethnicity

  See data dictionary and survey technical information in the [data](./data) folder.

### Breakdown of vaccine sentiment in survey sample

I considered respondents to be "optimistic" if they indicated they definitely or probably DID intend to be vaccinated.

Respondents were "hesitant" if they indicated they probably did not or definitely did not intend to be vaccinated.

According to this definition, about 88% of respondents were optimistic, and 12% were hesitant in my mid-March sample data set.

<img src="./images/vis1.png" width=50%>

The data I used was only a sample of the entire survey results. At the time of the analysis, the mid-March dataset was the latest available microdata file, but the Census Bureau had published more recent aggregate statistics. I compared the vaccine outlook statistics for my sample dataset (Mar 15) to the entire survey dataset for the same time period, and found that my sample actually skewed slightly more optimistic (88%) compared to the entire survey, which was 83% optimistic.

<img src="./images/all_surveys_outlook.png" width=90%>

However, I also determined that using the same criteria to define hesitant and optimistic, more recent datasets didn't show significant change in the split. Respondents surveyed in the most recent wave, ending May 10th, were still about 18% hesitant.


### Hesitancy reasons (multi-choice)
<img src="./images/vis2.png" width=90%>

Looking at all respondents, the most common reason cited for hesitancy was being "concerned about side effects," with 50% of respondents giving this as at least one reason. Those who said they planned to "wait and see if it is safe" are the next largest group, with 40% of respondents listing this as a reason.

Though there certain were people who indicated lack of trust in the vaccines and/or government, it's clear that the hesitant population does not consist solely of conspiracy-minded individuals.

<img src="./images/vis3.png" width=90%>

Similarly, if a respondent answered "Don't believe I need it," they were also asked to indicate a sub-reason why. Over half of respondents listed "Not in a high risk group" as a reason. 40% still indicated they "don't believe COVID is a serious illness," but again the majority of people do not seem to be citing a conspiracy theory.

## Results

### Top Predictors of Hesitancy

Top predictors of hesitancy with odds over 1.5:

| Characteristic | Odds of Hesitancy |
|:-|:-|
| Household residence is a boat, RV, or van | 2.48 |
| Greater number of individuals under 18 in the household | 2.29 |
| Greater level of difficulty meeting household expenses in the past 7 days | 2.24 |
| Greater level of household food insufficiency in the past 7 days | 1.80 |
| Household residence is a mobile home | 1.73 |
| Respondent did not use public transportation such as bus, rail, or ride-share before the pandemic, so transportation did not change | 1.51 |
***
### Hesitant Subgroups

Although these are the top predictors of hesitancy and we can see how percentage of hesitant respondents increases in these groups, distributions show that MOST hesitant categories represent a fairly small portion of the sample population.

Percentages of hesitant respondents living in mobile homes, boats, RVs and vans are more than double the hesitant percentages of respondents in houses and apartments. However, only 1,900 respondents lived in a mobile home, and less than 300 lived in a boat, RV, or van.

<p float="left">
    <img src="./images/vis10.png" width=40%>
    <img src="./images/vis4.png" width=49%>
</p>

The more children residing in a household, the greater the percentage of sample respondents who reported hesitancy. However, the majority of responding households reported having any children residing at home (households with children living elsewhere, such as grown children, would report 0).

<p float="left">
    <img src="./images/vis11.png" width=40%>
    <img src="./images/vis5.png" width=49%>
</p>

The more difficult is has been for the household to meet expenses in the prior 7 days, the greater percentage of hesitant respondents.

More than twice as many "Very difficult" households were hesitant than "Not at all difficult", although concern about vaccine cost was one of the lowest reported reasons in the overall sample.

<p float="left">
    <img src="./images/vis12.png" width=40%>
    <img src="./images/vis6.png" width=49%>
</p>

***

### Top Hesitancy Reasons per Subgroup

Although some reasons had different percentages for different subgroups, the top reasons were often the same and in the same order. The top 5 reasons of the most hesitant subgroups also tended to match the top 5 of the overall hesitant population, with the exception of food insufficiency.

<img src="./images/vis7.png" width=90%>
<img src="./images/vis8.png" width=90%>

Number 5 reason for people in  the "Very difficult" subgroup is Don't know if it will work, which is the number 6 reason overall. However, this is a minor difference.
<img src="./images/vis9.png" width=90%>

***

### Top Predictors of Optimism

Top predictors of optimism with odds over 1.5:

| Description of Question | Odds of Optimism |
|:-|:-|
| Greater age in years | 6.06 |
| Higher level of education | 2.62 |
| Respondent identified as Asian versus other races/ethnicities| 2.22 |
| Household is in the San Francisco-Oakland-Berkeley, CA Metro Area | 2.18 |
| Members of the household had avoided eating at restaurants in the prior 7 days | 2.03 |
| Higher pre-tax income level | 1.89 |
| Members of the household had taken fewer trips to stores because of the pandemic in the prior 7 days | 1.88 |
| At least one adult in the household substituted some or all of their typical in-person work for telework | 1.51 |

There is a statistically significant difference in mean and median ages between the hesitant and the opsimistic. The hesitant group is generally younger, with a median age of 46 compared to 57 for optimistic.

<img src="./images/vis13.png" width=90%>

Households in the lowest pre-tax income range (under $25k) are hesitant at twice the rate of those in the average income range $75 to 100k).

<p float="left">
    <img src="./images/vis14.png" width=40%>
    <img src="./images/vis15.png" width=49%>
</p>

***

# Summary of Results: Understanding the Hesitant Population

## Hesitancy Trends

Individually, no single one of the top predictors of hesitancy stands out as especially meaningful. When we look at characteristics of the hesitant population, it was the rarest categories that had the highest percentage of hesitancy. Even if it were feasible to create targeted campaigns for people with over 3 children, or people who live in mobile homes, or people who have had the most difficulty meeting expenses, such approaches would be focusing on a fairly small segment of the overall population.

However, what if we consider these characteristics together, and also with the understanding that the top reason for hesitancy across the board is a concern about side effects? 

Heads of household with more children to care for, or who struggle to make ends meet in terms of expenses and food, may not feel they can risk experiencing side effects from the vaccine. If a household's primary income earner or child care provider experiences moderate to severe side effects, it could mean having to miss work or not being able to meet household responsibilities for up to several days.

Given the characteristics of the most hesitant groups, **I believe a large group of people are actually worried about experiencing side effects they and their families wouldn't be able to afford.** This theory also considers the top predictors of vaccine optimism, which outline a situation on the opposite end of the economic spectrum: higher level of education, higher income, and more likely to have been able to work remotely. This demographic may be more financially secure and more likely to be knowledge-workers who are able to work from home, so may feel they are better able to afford missing work or household responsibilities if they experience side effects.

It's also worth noting that the predictors I attempted to engineer to represent political leanings (household being in a "Red" versus "Blue" state, or being in a metropolitan area, which tend to be Blue) didn't make it into the top predictors of hesitancy or optimism. It's possible we would see different results if we had data on the specific household's political affiliations, but this data did not support the idea that vaccine hesitancy is largely a partisan issue.

## Two Groups: The Cautious and the Mistrustful

I not only reviewed the top predictors of hesitancy, but also reviewed the reasons given among the "probably won't" group versus the "definitely won't" group.

<img src="./images/hes_reasons_outlook.png" width=90%>

<img src="./images/hes_reasonsb_outlook.png" width=90%>

The "probably won't" group seems more concerned with the timing (want to wait longer and see if it's safe), whether they really need it right away (maybe other people need it more, or they aren't at very high risk), and willing to continue using other precautions such as masks. This group may have some concerns about the science and the speed at which the vaccines were developed, but in general seems to simply be cautious, and may come around with time.

The "definitely won't" group has a higher percentage of people who are mistrustful of these specific vaccines (or not believing vaccines are helpful in general), are mistrustful of the government, or don't believe the reports that COVID is a serious illness. This group may be more difficult to convince even if they could be targeted using demographics, since they have strong feelings that rational arguments might not easily dispel.

There is about the same percentage of people in both groups who are concerned with side effects, and aren't sure if the vaccine will work.


# Recommendations:

## 1. Generate data to educate the public about the true risk of moderate to severe side effects

Currently, the top result of Googling "COVID vaccine side effects" is [this CDC webpage](https://www.cdc.gov/coronavirus/2019-ncov/vaccines/expect/after.html). Although factually accurate, and I believe genuinely designed to educate the public on what to expect, the warnings can seem scary for a reader. Without knowing how likely they are to experience side effects, many people may assume the "worst case scenario", especially if most of the anecdotal stories they've heard are about people who DID experience severe effects.

If we had more available data on the likelihood of experiencing moderate to severe side effects, it would help people for whom that is a primary concern make a more informed assessment of their risk of missing work or being unable to meet household responsibilities such as child care. It's unclear whether data exists to help people understand the risk of side effects; if it does exist, it is not widely cited.

## 2. Make sure employees know they will be supported if they need to miss work due to side effects

If employers were not allowed to penalize their employees who have to miss work to keep a vaccine appointment or due to vaccine side effects, this might help  people feel less vulnerable.

## 3. Carry on with current campaigns offering perks and cash

The second most common reason for hesitancy was waiting to see if it was safe. This survey was conducted in early March, so by the time of this writing in late May, some people may already have been convinced. However, the existing campaigns offering money or perks such as food coupons may help sway the hesitant sooner, especially since they tended to be groups with lower income, greater difficulty meeting expenses, and greater food insufficiency.

Also, since only 61% of adults have been vaccinated at this point (compared to the 82% of people who said they were optimistic in the last survey from May 10) it's clear there are still people who actually do intend to get vaccinated, but just need the right motivation to get it done sooner. The existing campaigns may help convince people who are optimistic, but have held off for other reasons. 

## Limitations & Next Steps

- Respondents were surveyed in early March. More recent surveys may reveal different trends and importance in predictors. Continuing analysis on newly available data may provide a more accurate picture, as well as help us understand how opinions about vaccines change.

- Some groups are over- or under-represented based on who actually responded to the survey

In this survey, based on comparing population percentages to 2020 census statistics:
- White respondents are over-represented
- Asian respondents are slightly under-represented
- Black and Hispanic/Latinx respondents are far under-represented

***

### For further information
Please review the narrative of my analysis in [my jupyter notebook](./index.ipynb) or review my [presentation](./presentation.pdf)

For any additional questions, please contact jess.c.miles@gmail.com.

***

## Repository Structure:

```
├── README.md               <- The top-level README for reviewers of this project.
├── index.ipynb             <- narrative documentation of analysis in jupyter notebook
├── presentation.pdf        <- pdf version of project presentation
└── images
    └── images, both sourced externally and generated from  code
└── data
    └── Public microdata file from Census bureau, data dictionary, and technical survey documentation
└── dstools
    └── Python functions used in the notebook
```
