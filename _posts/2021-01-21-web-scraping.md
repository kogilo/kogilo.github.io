---
layout: post
title: "Web Scraping in R"
subtitle: "Scraping movie data from IMDB with the rvest package."
background: "/img/posts/web-scraping/francesco-perego-vFzS4qXpAl4-unsplash.jpg"
---

## Prepare for Scraping

First, you need to add selector gadget extension in your chrome, you can install here. Just follow the instruction, then if you’ve finished install, the selector gadget would show up in here.

## Scraping with RStudio

Install the rvest & xml2 package to scraping a website using the following syntax.

## Installing Packages

```
install.packages("rvest")
install.packages("xml2")
library(rvest)
library(xml2)
```

## The webpage

![IMDb page](/img/posts/web-scraping/web_scrapingR.png)

## Scraping the data

```
web <- 'https://www.imdb.com/search/title/?count=100&release_date=2019,2019&title_type=feature'
website <- read_html(web)
website
```

## Scraping Rank

We will scraping the ranking using selector gadget, and then put the cursor there.

```
rank_data_html <- html_nodes(website,'.text-primary')
rank_data_html
rank_data <- html_text(rank_data_html)
rank_data
head(rank_data)
rank_data<-as.numeric(rank_data)
head(rank_data)
```

## Scraping Title

As previously, we will scraping the title using selector gadget, and then put the cursor there.

```
title_data_html <- html_nodes(website,'.lister-item-header a')
#Converting the title data to text
title_data <- html_text(title_data_html)
head(title_data)
```

## Scraping Runtime

We will scraping the runtime using selector gadget, and then put the cursor there.

```
runtime_data_web <- html_nodes(website,'.runtime')
runtime_data_web
runtime_data <- html_text(runtime_data_web)
head(runtime_data)
runtime_data <- gsub(" min","",runtime_data)
runtime_data
runtime_data<-as.numeric(runtime_data)
runtime_data
head(runtime_data)
```

## Scraping Genre

We will scraping the genre using selector gadget, and then put the cursor there.

```
genre_data_web <- html_nodes(website,'.genre')
genre_data_web
genre_data <- html_text(genre_data_web)
genre_data<-gsub("\n","",genre_data)
genre_data<-gsub(" ","",genre_data)
genre_data<-gsub(",.*","",genre_data)
genre_data
genre_data<-as.factor(genre_data)
head(genre_data)
```

## Scraping Rating

We will scraping the rating using selector gadget, and then put the cursor there.

```
rating_data_web <- html_nodes(website,'.ratings-imdb-rating strong')
rating_data_web
rating_data <- html_text(rating_data_web)
rating_data
rating_data<-as.numeric(rating_data)
head(rating_data)
```

## Scraping Gross

We will scraping the gross using selector gadget, and then put the cursor there.

```
gross_data_web <- html_nodes(website,'.ghost~ .text-muted+ span')
gross_data <- html_text(gross_data_web)
gross_data
gross_data<-gsub("M","",gross_data)
gross_data<-substring(gross_data,2,6)
gross_data
length(gross_data)
```

Then, filling missing entries with NA

```
for (i in c(2,4,5,14,15,17,20,23,24,25,30,32,35,36,40,41,42,50,51,52,53,54,59,60,64,67,70,71,73,74,76,78,79,80,81,82,83,84,85,86,87,90,92,93,95,98,99)){

  a<-gross_data[1:(i-1)]

  b<-gross_data[i:length(gross_data)]

  gross_data<-append(a,list("NA"))

  gross_data<-append(gross_data,b)

}
# data gross dikonversi menjadi numerik
gross_data<-as.numeric(gross_data)
length(gross_data)
summary(gross_data)
```

Combining all the lists that we’ve been scrap to form a data frame.

```
film_df <-data.frame(Rank = rank_data, Title = title_data,
                     Runtime = runtime_data,Genre = genre_data,
                     Rating = rating_data,Gross = gross_data)
str(film_df)
View(film_df)
```

## Visualization

I want to make a visualization based on which films have the longest duration according to film genre, using the following syntax.

```
library(ggplot2)
qplot(data = film_df,Runtime,fill = Genre,bins = 30)
```

From the graphic, we can see that the genre of film that has the longest duration is in Biography genre with a duration of more than 200 minutes. While the shortest duration is in the Drama, Adventure and Action.
Now I want to make a visualization based on what genre film get the highest gross value.

```
ggplot(film_df,aes(x=Runtime,y=Gross))+
  geom_point(aes(size=Rating,col=Genre))
```
