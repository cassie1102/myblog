---
title: "Malaria Visualizations"
date: 2020-09-24T11:55:10+08:00
draft: False
---

As one of the most severe diseases in current world, the World Health Organization (WHO) considers nearly half the world’s population is at risk from Malaria. Today I will create 3 visualizations for Malaria using the dataset provided by [Tidytuesday](https://github.com/rfordatascience/tidytuesday/tree/master/data/2018/2018-11-13).

The datasets I used to create the visualizations are as follows:  

`malaria_inc.csv` - Malaria incidence by country for all ages across the world across time.    
`malaria_deaths.csv` - Malaria deaths by country for all ages across the world and time.  
`malaria_deaths_age.csv` - Malaria deaths by age across the world and time.  

## 1. Top 10 countries with Malaria Incidence from 2000 to 2015

![fig 1](/fig1.png)

The first picture shows the 10 countries that have the most confirmed cases of Malaria across 15 years. It is a stacked area plot, which every colored chunk represents the incidence of one country from 2000 to 2015 (The incidence is calculated per 1,000 population at risk). We can see that generally there is a downtrend over the 10 countries of confirmed cases. And the number decreased faster in recent years.

## 2. Malaria deaths across the world from 1990 to 2016

The second visualization is an interactive animated image produced by `plotly.express`, **check it [here](/plotly/fig2.html).** (I did not find a decent way to embed the plot directly on webpage).

it shows the death rate from malaria by country from 1990 to 2016, measured as the number of deaths per 100,000 individuals. We can see that Africa is most affected by Malaria across time. But Africa achieved substantial progress in fighting the disease over time.

## 3. Malaria deaths by age group from 1990 to 2015

![fig 3](/fig3.png)

Figure 3 categories the annual number of deaths worldwide by age group. We can see that the most vulnerable age group to malaria deaths are children under five years old - they account for the majority of the deaths caused by Malaria. Additionally, deaths rate from malaria tends to decrease with age. But deaths in elder groups (50 and older) seems to be increasing over the time, which calls for attention.


The complete script to generate these images can be found [here](https://github.com/cassie1102/mini-project/blob/master/Malaria%20Visual.ipynb).
