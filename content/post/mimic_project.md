---
title: "Dashboard of Mimic Dataset Exploratory Analysis"
date: 2020-11-21T12:23:10+08:00
draft: False
---

"[MIMIC](https://mimic.physionet.org/) is an openly available dataset developed by the MIT Lab for Computational Physiology, comprising deidentified health data associated with ~60,000 intensive care unit admissions. It includes demographics, vital signs, laboratory tests, medications, and more." (MIMIC official website). There are tons of experimental research on this dataset. This project aims at building an interactive dashboard of the ICU admission data to provide insight into demographics/disease information and co-occurrence of various diseases. The dashboard repository can be found [here](https://github.com/joekrinke15/MIMIC-Analysis). Check the published version [here](https://share.streamlit.io/joekrinke15/mimic-analysis/Dashboard/dashboard.py). (Strealit has trouble caching the data efficiently, so the better option is to run the App locally. ) Co-author of this project are Alena Kalodzitsa, Felipe Buchbinder, and Joe Krinke from Duke MIDS.

## Overview
The dataset contains over 60,000 de-identified patient records collected from 2001-2012. Ages are shifted to protect patient identity. The tables we used for analysis are *admission*, *patient* and *diagnosis*. The questions we are trying to answer are:  
- What are the general trends of different types of diseases in the given time span?  
- What is the relationship of demographics information and diagnosis, do certain types of people tend to get certain disease?  
- Which diseases tend to happen together?  

Our target audience are hospital administrators and physicians, as well as general people who care about their health. They could use the dashboard to investigate preventative care options and get an overview of the distribution of various disease types. Note that all the conclusions are developed from this dataset only, which might have bias.

## Project Flowchart

The flowchart of this project can be summarized as below:  

![fig](/Flowchart.png)

We began our project by getting the approval from [PhysioNet](https://physionet.org/) for access to the MIMIC dataset. Then we launched our data on AWS stack. The data were stored in S3 bucket and can be queried using Athena. Then we passed and processed the data into Streamlit application for further visualizations and modeling.  


## Dashboard Structure
The dashboard have 5 components: general trends, disease to demographics, demographics to disease, a market basket analysis, and a co-occurrence analysis.  

### General trends
This tab provides an overview of the data we have. In MIMIC dataset, every admission record can be linked to patients’ birthday, gender, ethnicity and other important demographics information. It is helpful to explore whether there are certain types of people who are vulnerable for some diseases. By selecting the number of diseases we want to investigate, the page shows the most common **n** diseases as well as the age, admission locations, gender and ethnicity distributions.  

![fig](/generaltrends.png)

### Disease to Demographics  
This tab provides insight into the demographics analysis of different disease. By selecting the disease of interest, we can see the age, ethnicity and other demographics information distribution.  
![fig](/diseasetodemo.png)

### Demographics to disease
This tab serves as the other side of the previous tab. It enables users to select the demographics input to see the corresponding most likely illness.  

![fig](/demotodisease.png)

After selecting the ethnicity, age range and gender, the first plot will show the 10 most popular diseases for the group, which is counted by the total number of occurrences of that disease in the diagnosis dataset. For example, if I select the ethnicity to be White, gender to be female and the age group as 50 to 75, we can see that the most frequent disease is hypertension NOS, which is high blood pressure. Interestingly, it is also the riskiest disease for males between 50 to 75 years old as well. The most typical disease for females of other ethnicities between 50 to 75 years old is CHF, congestive heart failure. However this disease is also related to hypertension. So our general suggestion for males and females between 50 to 75 years old is to to get annual checkups to lower your risk for heart health problems, including high blood pressure (hypertension), coronary artery disease, and valve conditions.  

The left plot shows the distribution of the length of the hospital stay for the group of patients we selected. We found that there is not much difference from *staylength* across different age group nor gender. However, with regard to ethnicity, Asian patients seem to have spent less time in hospital compared to other ethnicity group.  

Generally, using this tab, you can find the riskiest diseases for any combination of different demographics information, and we can adjust our living habit accordingly to prevent ourselves from getting these diseases.  

### Market Basket Analysis

This tab aims at exploring the inter relationship between diseases. We can see the combinations of diseases that have the highest **lift** values. The lift value indicates if the odds of two diseases happening together is higher than if the diseases occurred independently. Specifically, high lift value indicate a strong relationship between two disease types.  
![fig](/mktbsk.png)

### Co-occurrence Analysis  

Co-occurrence analysis is generally used to provide a graphic visualization of potential relationships between entities, which are different types of diseases in our case. By abstracting entities into Nodes, and using Links to show the relationship between entities, as well as quantifying the network structure Index with nodes and connections as components, it is possible to find complex systems under a unified framework. Choosing the disease of interest, we can see its relationship with other ones, which displays a clustering type of feature.

![fig](/cooccur.png)

## Insights

The interesting finds of this analysis are displayed as follows:  

- Hypertension (high blood pressure) presents in majority of ICU patients and disproportionately affects African-Americans.  
- Asian patients are underrepresented in this hospital. Specifically, Asians account for 2% in dataset vs. 7.5% in Boston area. They also tend to stay shorter times in hospital compared to other ethnicities.    
- Most patients under 25 are infants or women having children.  
- Most people don’t come in through the Emergency Room. Many are infants or come from physician referrals.  

Thank you so much for checking out our project. We look forward to incorporating more features in the dashboard in the future.  
