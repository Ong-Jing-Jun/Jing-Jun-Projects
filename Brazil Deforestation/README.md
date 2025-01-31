## 1. Introduction

With deforestation being responsible for about 10% of global warming, it is imperative to put an end to it to tackle the climate crisis. (WWF). With deforestation being such a pressing issue, we aim to answer this question: **What are the predominant trends in deforestation, including the countries and activities primarily responsible for it?**

Deforestation trends will be identified using 3 plots. First, a facet wrap diverging bar plot will show deforestation trends from countries with the highest net change in forest area over time. Following our analysis of the first plot, the second plot will use a stacked area plot to examine the country that has contributed the most to deforestation and how deforestation drivers have affected it over time. Finally, using a map and time series graph, we will compare global soybean production and usage, one of the main deforestation drivers, to plot 1's deforestation trends.

In our research, we have used data from Our World in Data. The following are the data sets that we have used:

1.  The 'forest' data set is comprised of data from the years 1990, 2000, 2010, and 2015, encompassing various countries and their changes in forest area (measured in hectares) between each time period. We did not use this data set as a whole for plot 1; we only used the data for countries we were interested in.

2.  The 'brazil_loss' data set is comprised of data from 2001 to 2013 about the different drivers of deforestation and the amount of forest area lost (measured in hectares) for the activity. We used this data set as a whole to look at how the activities contribute to deforestation in Plot 2.

3.  The 'soybean_use' data set is comprised of data from 1961 to 2013 about soybean production and use in each country. We added up the allocation of soybean products in this data set to find the total soybean production for each year and country. We did not use this data set as a whole for plot 3; we extracted data associated with the time frame we were interested in.


## 2. Data Cleaning and Visualization
### 2.1 Plot 1

First, we are interested in identifying countries with the most significant net change in forest area over the years. This means that we will filter for the data of the top 5 countries for reforestation and deforestation, as well as the world\'s deforestation data, for each year that is available in the data set (1990, 2000, 2010, 2015). This lets us zoom in to countries where such phenomena are most pronounced and see the overall deforestation trend.

The data was presented using a facet wrap diverging bar plot. Each plot shows the net change in forest area in million hectares by country for each year. A facet wrap diverging bar plot allows for a clear and concise comparison of deforestation and reforestation trends and how they evolve over the years. It shows which countries are the biggest deforestation contributors quickly. To aid visualisation, we coloured the bars that show positive net change (reforestation) and negative net change (deforestation) green and red respectively.
<img width="786" alt="Screenshot 2025-01-31 at 3 25 49 PM" src="https://github.com/user-attachments/assets/985187ca-4cc8-4c2d-8fc9-83c3b650922f" />

### 2.2 Plot 2

Since Brazil has been the highest contributor to deforestation, we dive deeper into Brazil\'s deforestation activities to find out which activities are the main drivers. We have chosen a stacked area plot that shows us the area of Brazilian forest loss in hectares across the years 2001 to 2013. The x-axis represents the years, and the y-axis represents the total area of Brazilian forest loss in hectares. To aid visualisation, we have coloured the areas for the different driving factors in different colours, and included a legend that depicts their corresponding colours. We have also arranged the different driving factors according to size to further aid visualisation. This plot would enable us to easily identify the main driver of deforestation in Brazil by simply identifying the activity with the largest area.
<img width="843" alt="Screenshot 2025-01-31 at 3 26 46 PM" src="https://github.com/user-attachments/assets/d5f9d92c-8ef4-4ed9-b0d6-debea3934d2b" />

### 2.3 Plot 3

Soybean is one of the main deforestation drivers (Our World in Data). Therefore, we wish to see if the countries that produce and use more soybean indeed correspond to the countries with high deforestation rates in plot 1. To obtain the data we want for our plot, we added up the allocation of soybean products in the 'soybean_use' data set to find the total soybean production for each year and country. In the time series plot, we kept the data between 1990 to the latest year of data available 2013 to match the years for plot 1 for ease of comparison. Additionally, we added a world map that captures a point of soybean production in the different countries for the starting year 1990. The map is coloured by intensity, with darker regions representing higher soybean production. We also labelled the top 5 soybean producers. This makes the map helpful in visualising where the top soybean producers are in the world at a glance.
<img width="789" alt="Screenshot 2025-01-31 at 3 27 14 PM" src="https://github.com/user-attachments/assets/0ea200a5-9cef-4aad-bbb8-24c1593a1809" />

## 4. Discussion

Plot 1 shows us that Brazil is the country most responsible for deforestation. Brazil has the longest bar in the negative direction throughout 1990 to 2015. Myanmar and Tanzania have also consistently been one of the greatest contributors, indicated by their appearance as top countries contributing to deforestation throughout the given time period. Deforestation appears to be decreasing over the years in the top countries. Despite this, while overall deforestation has fallen since 1990, it has remained roughly stagnant from 2000 to 2015. Hence, it is likely that besides the top countries, more countries are contributing significantly to deforestation as well. We can also see that despite the net deforestation globally, there are countries that have been working on reforestation instead of contributing towards deforestation. In contrast to Brazil, China has the longest bar pointing in the positive direction throughout the years, illustrating how China\'s reforestation rates have remained consistently high through the years. India has also been one of the top contributors to reforestation in recent years, appearing throughout the graphs of 2000-2015. 

Plot 2 shows us that out of all the activities in Brazil, pastures have been the greatest contributor to deforestation. It is also significantly larger than all the other drivers of deforestation. This is reflected by the fact that the area in purple representing pasture is the largest across the years from 2001 to 2013. This suggests that deforestation rates for pastures have remained consistently high. We can also see that deforestation for pastures in Brazil peaked in 2004 and has had a generally decreasing trend since. Notably, there are large falls in hectares of deforestation from 2004-2005, and 2007-2009, due to the introduction of deforestation policies in those periods (Time).

Plot 3\'s map reveals that Brazil, Argentina, China, Japan and the Netherlands are the top soybean producers in the world. Having the darkest area, Brazil topped soybean production in 1990. Plot 3\'s time series plot further reveals that the top 3 producers remain the same throughout the years. Brazil\'s soybean production increased from 1990 to 2013, remaining in the top 3 countries for soybean production. Correspondingly, Brazil was the country with the highest deforestation rate during those years. Similarly, Argentina has also appeared in both Plots 1 and 3; it is amongst the top soybean producers and deforestation contributors. Hence, the trend of high soybean production generally corresponds with high deforestation rates. However, there seems to be a discrepancy for China. China\'s soybean production increased significantly from 1990 to 2013, and rose from second to first in soybean production. Yet, China had the world\'s highest reforestation rate during those years. This discrepancy can be explained from the fact that China has been importing raw soybeans for soybean production from places like Brazil, where raw soybeans are actually grown (OEC World). Thus, China does not have to clear land to make way for soybean plantations. Hence, we maintain that the overarching trend remains valid. Additionally, we note that although soybean production in top countries has increased, deforestation rates have fallen. This trend seems to contradict the point that soybean is one of the main deforestation drivers. This discrepancy can be explained by efficiency improvements that require less land use per unit of production (The Conversation), and the presence of other factors that drive deforestation more (for instance, pasture for Brazil as seen from Plot 2). Hence, this trend does not show the full picture, and we maintain that soybean is still one of the main drivers of deforestation.

## 5. References

<https://github.com/rfordatascience/tidytuesday/blob/master/data/2021/2021-04-06/readme.md>

[https://www.wwf.org.uk/learn/effects-of/deforestation#:\\\~:text=Trees%20absorb%20and%20store%20carbon,we%20don't%20stop%20deforestation.](https://www.wwf.org.uk/learn/effects-of/deforestation#:~:text=Trees%20absorb%20and%20store%20carbon,we%20don't%20stop%20deforestation.){.uri}

<https://ourworldindata.org/soy>

<https://oec.world/en/profile/hs/soybeans?yearSelector1=2010>

<https://time.com/amazon-rainforest-disappearing/>

<https://theconversation.com/strict-amazon-protections-made-brazilian-farmers-more-productive-new-research-shows-105789>


