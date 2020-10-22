---
title: "Starwar API Exploration"
date: 2020-10-23T02:30:47+08:00
draft: False
---

[SWAPI](https://swapi.dev/), or the Star Wars API, is the world's first quantified and programmatically-accessible data source for all the data from the Star Wars canon universe. This post will use SWAPI to request the results of interest from the website and find the oldest character in Star Wars with the films the character is in.


### Request information

The process requires the package `requests`. After exploring the request, I found that the result is stored in different pages which can be requested with the 'next' feature in each page's result. So I wrote a for loop to enumerate all pages.

```{python}
# initial request
data = requests.get("https://swapi.dev/api/people/").json()

results = []
results.append(data['results'])

# loop over all pages
while data['next']:
    print("Getting to {}; ".format(data['next']))
    data = requests.get(data['next']).json()
    results.append(data['results'])
```

### Transform into dataframe

The original data contain the characters' information from Star Wars. It is in json format. To make it easier for filtering and examining, I transformed the data into pandas dataframe. Here are the column names of the dataframe:

['name', 'height', 'mass', 'hair_color', 'skin_color', 'eye_color', 'birth_year', 'gender', 'homeworld', 'films', 'species', 'vehicles', 'starships', 'created', 'edited', 'url', 'birth_year_num']

We are interested in the age of the characters which can be computed from the `birth_year` column. I filtered those whose `birth_year` is unknown. However the year is in 'XXBBY' format which is hard to order. By examining value counts of `birth_year`, all of the entries are followed by 'BBY'. So I wrote a function to extract the number previous to the string.


```{python}
df['birth_year_num'] = df['birth_year'].str.extract('(\d+)').astype('float64')
df.sort_values('birth_year_num', ascending=False).head(5)
```
From the ordered dataframe we know the oldest character in this dataframe is Yoda, who was born in '896BBY'. Extract the column films which stores films that Yoda appeared in. They are represented as APIs in this dataframe, so we need to request again to get the information of the films.


### Get the title of films

```{Python}
# get api of yoda films
yoda_films_api = df[df['name'] == 'Yoda']['films'].to_list()
yoda_films_api = yoda_films_api[0]

# request the films titles that yoda is in
film_titles = []
for item in yoda_films_api:
    film = requests.get(item)
    film_titles.append(film.json()['title'])
print(film_titles)
```
The films that Yoda is in are listed as follows:
['The Empire Strikes Back', 'Return of the Jedi', 'The Phantom Menace', 'Attack of the Clones', 'Revenge of the Sith']


The complete script of this project can be found [here](https://github.com/cassie1102/mini-project).
