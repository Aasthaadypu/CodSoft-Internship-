import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

path = '/kaggle/input/movies-dataset/IMDb Movies India.csv'
data = pd.read_csv(path)

print("The shape of the dataset is: {} rows and {} columns".format(data.shape[0], data.shape[1]))

data.head(8)

data.isnull().sum()

data = data.rename(columns={"Data_Title": "Movies_Title"})

data.head(2)

print(data['Gross'].head(1))
data['Gross'] = data['Gross'].str.replace(',', '')
print(data['Gross'].head(1))

data['Gross'] = data['Gross'].astype('float64')
data['Gross'] = data['Gross'].replace(np.nan, 0)

data['Gross'] = data['Gross'].astype(int)

data['Gross'].dtype

data.corr()

numerical_attributes = ['Certificate', 'PosterLink', 'Released Year', 'Gross']
data[numerical_attributes].hist(figsize = (15, 6), color = 'Green', edgecolor = 'Blue', layout = (3, 4));

data.describe(include = ['O'])

top_voted = data.sort_values(['No_of_Votes'], ascending = False)

numerical_attributes = ['Certificate', 'PosterLink', 'Released Year', 'Gross']
data[numerical_attributes].hist(figsize = (15, 6), color = 'Green', edgecolor = 'Blue', layout = (3, 4));

data.describe(include = ['O'])

top_voted = data.sort_values(['No_of_Votes'], ascending = False)

fig,axs=plt.subplots(figsize=(13,4))
g=sns.barplot(x=top_voted['Movies_Title'][:7],y=top_voted['No_of_Votes'][:7], palette = 'hus1')
g.set_title("Top Voted Movies", weight = "bold")
plt.show()

fig,axs=plt.subplots(figsize=(15,5))
g=sns.barplot(x=top_voted['Movies_Title'][:7],y=top_voted['IMDB_Rating'][:7], palette = 'hus1')
g.set_title("IMDB Rating of top voted movies", weight = "bold")
plt.show()

fig,axs=plt.subplots(figsize=(15,5))
g=sns.barplot(x=top_voted['Movies_Title'][:7],y=top_voted['Meta_score'][:7], palette = 'husl')
g.set_title("Metascore of top rated movies", weight = "bold")
plt.show()

data['Gross'].head(3)

data.loc[data['Gross']==0]

import re
duration=[]
for x in data["Runtime"]:
    p=re.compile('\d+')
    j=p.findall(x)
    if len(j)==2:
        duration.append(int(j[1])-int(j[0])+1)
    else:
        duration.append(int(j[0])+1)
data['duration']=duration

data['duration'].head()

from collections import Counter
genre=[]
for x in data['Genre']:
    for y in x.split(','):
        genre.append(y.strip().lower())

count=Counter(genre)
count=count.most_common()[:10]
x,y=map(list,zip(*count))

fig,axs=plt.subplots(figsize=(20,5))
g=sns.barplot(y,x)
g.set_ylabel("Genres", weight = "bold")
g.set_title("Top Ten Genres", weight = "bold")
plt.show()






