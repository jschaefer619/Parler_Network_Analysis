# Introduction

On January 6th, 2021, a group of right-wing Insurrectionists stormed the US Capitol. The Insurrectionists were able to breach the US Captiol Building itself, leading to the evacuation of the Senate and House Chambers. Five people died and over 140 were injured (https://www.theguardian.com/us-news/2021/jan/08/capitol-attack-police-officer-five-deaths).

The role of the right wing social media website Parler was instrumental in the planning and publicity of the Insurrection. Many Parler users expressed pro-trump, violent sentiment on the Platform. In response to Parler's culpability in this event, Amazon Web Services dropped support for managing parler's cloud infrastrucure. This led to a month long disruption in Parler's service, though the social media provider is not back online.

# Purpose of this Project

Though Parler may be back online, virtually all of the posts from the Riots are now no longer on the Parler Platform. This is a shame - these posts provide critical insight to a traumatic national event. Fortunately, savvy, self-proclaimed online hacktivists scraped the parler website for text and videos during the Insurrection. The videos from Parler have been coupled with open source facial identification software to hold the insurrectionists accountable.

This project uses the textual information from Parler to accomplish two things:

1) Compile an cleaned, accessable dataset of the Parler textual data
2) Build a online network analysis tool to allow users to critically engage with the conversations on Parler

## Close Reading

In this project, I will provide some visualizations that allows users to engage with the corpus as a whole. However, I do beleive that many users have already had the oppurtinuity to engage with the voices on Parler through an abstracted, mediated manner. No doubt, many people reading this have already read many of the excellent articles and think pieces by people far more knowledgeable than myself. So I hope to accomplish something unique with this project: allow users to engage with the texts of the Insurrectionist firsthand, in a manner that emulates their networked strucure. I hope to remove some of the abstraction and mediation from this Parler data - and I do hope this will be insightful in some way. Users should be warned: the content that will be viewed in this way does contain strong language and hate speech.

# The Data

This data was collected from DDos secrets, a Nonprofit group dedicated to make information public and accessible. Note that DDOsSecrets states that this a partial dataset from the capitol beaches. But from my hands on experience working with the data, it is certianly sufficient enough for some insight into the insurrection.  Let's breifly walk through my data and processing and cleaning methods.

## Data Processing

I began by downloading the .torrent seed shared by DDos Secrets. Once decompressed, the downloaded information was ~23,500 html webpages (Note that the cloud computing service I used to unzip the downloaded torrent file did crash after unizipping for some time, so there may be more information - will update this page soon to reflect these changes). Though these webpages did not follow an exact uniform format, virtually all of these pages were user pages: the page contained information about a user, including their handle, their screen name, and then their most trecent posts or posts they have recently 'echoed' (retweeted).

Then, I used the BeautifulSoup4 four library to extract relevant information from these html files. BeatuiflSoup4 is a useful library for parsing data from HTML and XML files. Regular expressions were also used to extract some detailed information, and to clean the dataset. Pandas was also used to effectively manage the transition from raw data to a compatible .csv format.

## The Dataset itself, explained

Notice that there are two shared .csv files, the 'master' csv file, and the 'deduped' csv file. The master file contains all of the posts shared, while the 'deduped' removes all duplicate posts within the dataset (posts that have the same text and are by the same author)

Let's walk through the each column in the .csv dataset.

Each column represent scraped post.

### user_page

The handle of the user who's timeline was scraped in the data cleaning process.

### author_username

The handle of the author of the post. If this author's username matches the "user_page" handle, then this was an original post (the user who's timeline was scrapped and the author of the post are the same user). If the author's username and the "user_page" handle are different, then the "user_page" user 'echoed' this post, or interacted with in in some way for this post (that was not their own) to appear on their timeline.

### author_name

The screename of the author of the post.

### impression count

This a summary metric used to measure engagement on a post. Though I do not know the exact calculations that Parler uses to compute impressions, it is some kind of composite combination of views, comments, upvotes, and echoes of a post. Many posts in either did not have impressions data or extracting it was nontrivial. I used "-1" to denote that a post has no impressions data. A future goal will be to rexamine the impressions data extraction process to collect more engagement data.

### 











