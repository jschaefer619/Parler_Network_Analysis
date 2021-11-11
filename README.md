# Introduction

On January 6th, 2021, a group of right-wing Insurrectionists stormed the US Capitol. The Insurrectionists were able to breach the US Capitol Building itself, leading to the evacuation of the Senate and House Chambers. [Five people died and over 140 were injured](https://www.theguardian.com/us-news/2021/jan/08/capitol-attack-police-officer-five-deaths).

The right-wing social media website Parler was instrumental to the Insurrection - it was a major part of the commmunication infrastructure for the extremists: leading to the planning, execution, and virality of the Riots. Many Parler users expressed pro-trump, conspiratorial, and violent sentiment on the Platform. In response to Parler's culpability in this event, [Amazon Web Services dropped support for managing parler's cloud infrastructure](https://slate.com/technology/2021/02/parler-capitol-riot-proud-boys-skysilk.html). This led to a month long disruption in Parler's service, but the service is now back online.

# Purpose of this Project

Parler may be back online, but virtually all of the posts from the Riots are no longer on the Parler Platform. This is a shame - these posts provide critical insight to a traumatic national event. Fortunately, [savvy researchers and online hacktivists](https://cybernews.com/news/70tb-of-parler-users-messages-videos-and-posts-leaked-by-security-researchers/) scraped Parler websites for text and videos of the Insurrection. The videos from Parler have been [coupled with open source facial identification software](https://www.vice.com/en/article/xgz7g7/facial-recognition-parler-videos) to hold some insurrectionists accountable.

This project uses the textual information from Parler data scrapes to accomplish two things:

+ 1) Compile an cleaned, accessible dataset of the Parler text data
+ 2) Build a online network analysis tool to allow users to critically engage with the conversations on Parler

## Close Reading

A macro-level analysis is often suitable for a large corpus of data. However, I believe that many people have already had the opportunity to engage with the voices on Parler through an abstracted, mediated manner. No doubt, most reading this have already engaged with some of the excellent media coverage on this event by talented journalists and academics. So I hope to accomplish something unique with this project: allow users to engage with the Parler texts of the Insurrectionist firsthand, in a manner that partially emulates Parler's networked structure. I hope to remove some of the abstraction and mediation from this Parler data - and I hope that doing so will be insightful. Visitors should be warned: the content that will be viewed in this way does contain strong language and hate speech.

# The Data

This data was collected from [Distributed Denial of Secrets](https://ddosecrets.com/wiki/Distributed_Denial_of_Secrets), a Nonprofit group dedicated to make information public and accessible. Note that DDoS states that this a [partial dataset](https://ddosecrets.com/wiki/Parler) of the capitol riots data. But from my hands on experience working with the data, it is certainly sufficient enough for some insight into the insurrection.  Let's breifly walk through my data and processing and cleaning methods.

## Data Processing

I began by downloading the .torrent seed shared by DDoS. Once decompressed, the downloaded information was ~23,500 html webpages (Note that the cloud computing service I used to unzip the downloaded torrent file did crash after unizipping for some time, so there may be more information - will update this page soon to reflect these changes). Though these webpages did not follow an exact uniform format, virtually all of these pages were user pages: the page contained information about a user, including their handle, their screen name, and then their most recent posts or posts they have recently 'echoed' (retweeted).

Then, I used the [BeautifulSoup4](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) library to extract relevant information from these html files. BeautifulSoup4 is a useful library for parsing data from HTML and XML files. Regular expressions were also used to extract some detailed information, and to clean the dataset. Pandas was also used to effectively manage the transition from raw data to a compatible .csv format.

## The Dataset itself, explained

Go to [this website's Github Repository to view the data](https://github.com/jschaefer619/Parler_Network_Analysis). Notice that there are two shared .csv files, the 'master' csv file, and the 'deduped' csv file. The master file contains all of the posts shared, while the 'deduped' removes all duplicate posts within the dataset (posts that have the same text and are by the same author)

Let's walk through the each column in the .csv dataset.

Each column represents one scraped post.

#### user_page

The handle of the user who's timeline was scraped in the data cleaning process.

#### author_username

The handle of the author of the post. If this author's username matches the "user_page" handle, then this was an original post (the user who's timeline was scrapped and the author of the post are the same user). If the author's username and the "user_page" handle are different, then the "user_page" user 'echoed' this post, or interacted with in in some way for this post (that was not their own) to appear on their timeline.

#### author_name

The screename of the author of the post.

#### impression count

This a summary metric used to measure engagement on a post. Though I do not know the exact calculations that Parler uses to compute impressions, it is some kind of composite combination of views, comments, upvotes, and echoes of a post. Many posts in either did not have impressions data or extracting it was nontrivial. I used "-1" to denote that a post has no impressions data. A future goal will be to reexamine the impressions data extraction process to collect more engagement data.

#### timestamp

Temporal info scrapped from the Parler webpages. This data is relative and is formatted as a string which reads, for instance, as "3 days ago" or "1 week ago." From looking at the Post information, I am nearly certain that this data was scraped on January 10th, 2021. So, a post that was "4 days ago" was on the day of the Capitol Riots. Another future goal would be to further investigate the relative data, then provide a clear, machine readable timestamp.

#### body_text

The body text of the Parler post. Other user mentions, hashtags, and media links are included here.

#### path

The filename of the HTML file that this post was extracted from.

#### is_original_post

"1" if the handle of "user_page" is the same as the handle of "author_username" (This is an original post) "0" if the handles do not match (The "user_page" user echoed this post or engaged with it in some manner, but the post was not their own.)

#### links

Media links found in the body text of a post. Multiple instances are seperated with a '\n'

#### domains

The domain names of the parsed media links. Multiple instances are seperated with a '\n'

#### hashtags 

The hashtags found in the body text of a post. Multiple instances are seperated with a '\n'

#### user_mentions

Mentions of other users within the body text of a post. One challenge here is that there is no way to tell if a user is mentioning another parler user or using the "@" symbol to promote an account on another platform, or to reference some other public figure. For instance, a user may mention @BarackObama to bring attention to Barack Obama the public figure, but Barack Obama himself has no parler account. Multiple instances are seperated with a '\n'

#### body_text_cleaned

This the body text shared in the post, but with all hashtags or media links removed.


### Data Conclusions

Though there is still a considerable amount of work to be to ensure that this dataset as clean and comprehensive as possible, there is more than enough info available for data enthusiasts to dive into!

## Voyant Tools Preliminary Large Scale Analysis

It is a goal of this project to encourage users to engage with the Parler in a less abstracted a mediated way. However, some initial large-scale analysis is still useful.

I encourage visitors to go to the [VoyantTools](https://voyant-tools.org) and upload the three with three 'sub-corpuses' of this dataset that I have [linked in the repository](https://github.com/jschaefer619/Parler_Network_Analysis). The three 'sub-corpuses' are as follows: the hashtags, the cleaned body texts, and the domains.

Some qualitative I noticed during my data explorations:

+ Brietbart still has a sizable presence on Parler, but it seems that other alt-right media outlets have large audiences
+ Trump is central in the overall discourse on Parler
+ There is a large amount of Hostility Towards former VP Mike Pence for aknowledging Joe Biden's victory
+ Many users go to Parler after being banned or suspended on other social media platforms

## Why Network Analysis - An why build a tool for this?

The strengths and weakness of this corpus informed my decision to focus on Network Analysis for this project. This data was not collected through an API (leading to much effort being put - and even more needed - to clean and organize this data), so some important information is missing/obscured. But this data was collected through scraping user timelines. This scrapping method is conducive to network analysis, each post was associated with both the user that shared this post, and the author of the post.

There are many fantastic Network Analysis tools avaliable. Gephi is a a great, open-source choice. These tool are powerful ways to visualize the overall structure of a network. Below is a Gephi generated network made by one of my peers using [Twitter Data about the Capitol Riots](https://capitolriots.humspace.ucla.edu).

<img width="777" alt="example_network" src="https://user-images.githubusercontent.com/56604738/111742019-a9a13480-8844-11eb-9568-c02916baadfc.png">

This graph is visually intriguing, and points of interests can be demarcated communicate interesting findings. But there is something that is obscured by this approach. The content of the Parler posts themselves is lost in this abstraction.

This ties back to a central goal of this project: to provide a less mediated access to the events on January 6th, as seen on Parler.

Providing access to a cleaned dataset is an important start, but I beleive there is more work to be done to provide an accessible way for all to engage with these conversation.

So, my goal became: build a bespoke tool for users to visualize the conversations on Parler on the capitol riots.

### Design Tenets

+ (1) The tool must be accessible through the web with extremely little / no configuration from users
+ (2) Body Text from posts must be fully readable
+ (3) A loose network structure is to be retained: users should understand how one post is related to another


### An Interactive Prototype

[VOSviewer Online](https://app.vosviewer.com/) is a powerful and flexible way to visualize network data online. As a part of my work in [UCLA's Holocaust and Genocide Studies Research Lab](https://elts.ucla.edu/person/todd-presner/), I developed a [tool to convert spreadsheet data into a VOSviewer readable file](https://github.com/jschaefer619/VOSviewer_triplet_translator).

Here is a screenshot of an early visualization of a partial subset (~ 2000 entries) of this Parler data.

<img src="https://user-images.githubusercontent.com/56604738/141378102-f0cc5495-3b18-4ea1-ad93-c41757506f8c.png">

Users can interact with this network diagram by navigating to downloading the 'vosviewer_parler_data.json' file linked in this repository, navigating to [VOSviewer Online](https://app.vosviewer.com/), then opening the downloaded file within the VOSviewer Online. 

More details coming soon!

### Reflections

This repository was original developed as an undergraduate Capstone project for Digital Humanities.

As such, I feel as though I am engaging with a core part of the Digital Humanities Ethos - access and transparency to important data. I am proud to be a part of the Digital Humanities community - the DH community and curriculum has equipped me with the skills I needed to get to where I am today. With this project, I hope to give something back to the community.

This repository will continue to recieve updates. Thanks for reading.













