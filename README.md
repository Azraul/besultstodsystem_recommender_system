# besultstodsystem_recommender_system

[Teacher notebook](https://observablehq.com/@sandravizmad/final-project "observable.com")

## Evaluation
The max. points for this assignment will be 65, spilt into the three parts as followed:

### I Part - 28 points

Criteria for the evaluation will be accuracy, complexity and presentation of the work. All analysis and code has to be correct and clean with short comment lines to help navigating the logic. Complexity also matters, as trying out an advanced idea is more challenging and greater workload than a simple repetition of shown examples. Last but not least the presentation of your work should have a clear flow, explanation and interpretation of the results. If you tired something that didn't work, please also present this work as it shows your workflow and in general can be very interesting.

### II Part - 21 points

In this part of the assignments all that matters is how well you can explain a complex approach. Please use a step by step presentation meaning first showing the general idea (for example the conclusion of the paper) followed by walking through the model in conjunction with the code.

### III Part - 16 points

In the last part I evaluate how amplified and detailed your judgement regarding ethical challenges in the RS for music streaming platform like Spotify is presented.


# I. Part - Build RS using Spotify data
This first part of the final project is all about hands-on. In order to build your own RS using Spotify data you need to ...

## Understand RS deeply
Therefore use the information provided during the course in the corresponding notebook, the additional information specific to Spotify you can find below as well as the presented python example and of course any information you search on your own.

### Overview Spotify
Spotify is with around 200 million users and 50 million songs in 2018 the world's largest independent music streaming platform. It offers a free, ad-supported option and a paid, ad-free version at $9.99 (US) per month. You can get Spotify + Hulu for $12.99 a month (again, US). Other plans (family and student deals are common) are available, but they differ by market. Spotify pays royalties to musicians (it actually contracts a third party to handle this) based on the number of plays their songs receive. Typically, this is between $0.006 and $0.0084 per play. No wonder songs are getting shorter and catchier. Reasons for this success are recommendations and personalization. Discover Weekly is the company's premier recommender system. Spotify has a clear and unwavering focus on using engagement data to understand, and then provide, what their customers crave. It estimates that 60% of the time listeners are in a “closed” mindset when they are on the app; they know what they want to listen to and they just need to find it. The remaining 40% of time is spent in an “open” mindset. In this mindset, users put in less effort, they scroll less, they skip tracks more, and they click less on the artist for further information. Essentially, they are receptive to new ideas, but in a passive yet impatient state.

Spotify’s lasting attraction lies in how it caters to that 40% “open” mindset.

Let's get in the mood by reading/interacting with some fancy visual essays using Spotify.

    - How Bad Is Your Spotify?
    - Crowdsourcing the Definition of "Punk"
    - The most timeless song of all time

### RS "Discover Weekly"
In July 2015, Spotify launched its Discover Weekly playlist. As clearly apparent by its self-evident title, Discover Weekly is an algorithm-generated playlist that is released every Monday, bringing listeners up to two hours of custom, curated music recommendations. This service makes an incisive case study in how rethinking recommendation algorithms profoundly changes people's path to novelty.

![flowchart](/media/readme/discover_weekly.png)

Spotify also offers other customized recommendations in Daily Mixes, Release Radar and Recommended suggestions of playlists or artists. According to Spotify, by 2016 Discover Weekly had “reached nearly 5 billion tracks streamed” since its launch, a clear sign of its success as an algorithmic product offering.

“Music isn’t like news, where it’s what happened five minutes ago or even 10 seconds ago that matters. With music, a song from the 1960s could be as relevant to someone today as the latest Ke$ha song.” — Daniel Ek

Different algorithms address different features, attributes and elements in the computational pursuit of predictably pleasant surprise.

### Collaborative filtering (nearest neighbor)
The primary aim of recommendation algorithms is to analyze user data in order to provide personalized recommendations.

Machine learning algorithms in RS are typically classified under two main categories content-based and collaborative filtering. Traditionally, Spotify has relied primarily on collaborative filtering approaches for their recommendations. This works well for Spotify, as it revolves around the strategy of determining user preference from historical behavioral data patterns.

In terms of Spotify, Discover Weekly and other playlists are created using collaborative filtering, based on the user’s listening history, in tandem with songs enjoyed by users who seem to have a similar history, hence finding users that are similar to each other based on listening history and recommends songs that "Spotify music lovers like you" liked.

Additionally, Spotify uses “Taste Analysis Data” to establish a Taste Profile. This technology, developed by Echo Nest, groups the music users frequently listen to into clusters and not genres, as the human categorization of music is largely subjective.

Examples of this are evident in Spotify’s Discover Weekly and Daily Mix playlists suggestions. Clustering algorithms like Spotify’s group data based on their similarities. Services like Spotify can cluster songs, genres and even playlist tones, in order to train machine learning algorithms to predict preferences and future listening patterns.

An example of this is if two users listen to the same sets of songs or artists, their tastes are likely to align.

Christopher Johnson, former Director of Data Science at Spotify, explains that Content Based strategy relies on analyzing factors and demographics that are directly associated with the user or product, such as the age and sex of the user or a song genre or period, such as music in the 70’s or 80’s.

Recommendation systems that are based on Collaborative Filtering take consumer behavior data and utilize it to predict future behavior. This consumer behavior leaves a trail of data, generated through implicit and explicit feedback.

Unlike Netflix, which from its nascence used a 5 star point rating system, Spotify relied primarily on implicit feedback to train their algorithm. Examples of user data based on implicit feedback can be playing a song on repeat or skipping it entirely after the first 10 seconds. User data is also gleaned from explicit feedback, such as the heart button on Discover Weekly, or songs that were liked that automatically save in the library and “Liked from Radio” playlist.

An example of the myriad other ways in which collaborative filtering and recommendation algorithms work in different approaches is evident in the diagram below. Spotify uses an amalgamation of 4 approaches – Attribute based, CF (item by item), CF (user similarity) and Model based.

Spotify further analyzes and applies user data by using a matrix decomposition method, which is also known as matrix factorization. The approach of matrix factorization aims to find answers by ‘decomposing’ (hence the term matrix decomposition) the data into two separate segments (Alpaydin, 2016). The first segment defines the user in terms of marked factors, each of which is weighted differently. The second segment maps between factors and products, which in the Spotify universe are songs, artists, albums and genres, thus defining a factor in terms of the products offered. In his book, Alpaydin provides an example to further elaborate on this as applied to movies. In the following diagram (see Figure 4) each customer has only watched a small percentage of the movies and the overall movies have only been watched by a small percentage of the customers. Based on these assumptions, the learning algorithm needs to be able to generalize and predict successfully.

Through the acquisition of Echo Nest, a Boston based start-up company, Spotify’s algorithm was able to be advanced through acoustic analysis which allows music on the application to be classified based on several aural factors. Echo Nest is also designed to crawl the internet for music related digital media in order to find actionable and quantifiable data for Spotify’s recommendation algorithm (Prey). The design of Echo Nest relies upon distributed cognition across social media posts, blogs, and music reviews, as well as natural language processing to identify key words and phrases which allow for the derivation of similarities between songs. Distributed cognition in Echo Nest also enables collaborative filtering because the identification of key words and phrases indicates similarities in cognitive processes (Holland). The design of Echo Nest enables the semantic, tempo and even danceability analysis of the songs within Spotify’s corpus. This in-depth assessment is integral to the distinction between rock and Christian rock, as well as other genres of music which may have similar tempos and structures but vastly different content and listeners (Prey).

### Natural language processing
Spotify’s engineers, who are wonderfully open about their work, state on a regular basis that natural language processing is more effective on music than one might assume. They can turn playlists into text documents and analyze how lyrical patterns relate to each other. Similar to Google’s NLP algorithms, Spotify identifies the co-location of individual terms and uses this to predict the meaning of phrases. In the example below, we see the score certain terms receive in relation to the stimulus ‘Abba’. ‘Dancing Queen’ has a slightly higher score than ‘Mamma Mia’, almost certainly due to the latter’s ubiquity in Italian pop music. Abba is likely to be described as ‘perky’ and ‘nonviolent’, although I’ve seen wedding dancefloors that offer evidence to the contrary.

### Outlier/anomaly detection
By definition, outliers are extreme values that dramatically deviate from other observations. They can have special relevance to recommenders. In the case of Spotify, outliers detection determines if a particular instance, that is listening to a song, is part of a normal behavior pattern or not, and if so it is a mistake or not. With outliers there is potential for a playlist experiment.

As a by-product of this training, Spotify is smart enough to recognize and distinguish outliers. Alpaydin explains this as another application area of machine learning, termed outlier detection, “where the aim this time is to find instances that do not obey the general rule—those are the exceptions that are informative in certain contexts.”

### Deep learning neural network (CNN)
In previous years, Spotify encountered a “cold start problem” – when no prior behavioral or user data was available it was unable to use its existing CF model trained algorithms.

Consequently, faced with a “cold start”, Spotify found themselves inept at providing recommendations for brand new artists or old or unpopular music.

In order to navigate this, Spotify harnessed convolutional neural networks – known as CNNs or ConvNet – the same deep neural network technology used in facial recognition software. In the case of Spotify, the CNN has been trained within the set paradigms of audio, conducting a raw audio data analysis instead of examining pixels. The audio frames pass through the convolutional layers of the neural network architecture resulting in a “global temporal pooling layer”, the computation of learned features throughout the course of a single track.

### Ressources
[How Spotify recommends your new favorite artists](https://towardsdatascience.com/how-spotify-recommends-your-new-favorite-artist-8c1850512af0)
[Music To My Ears: De-Blackboxing Spotify’s Recommendation Engine](https://blogs.commons.georgetown.edu/cctp-607-spring2019/2019/05/06/music-to-my-ears-de-blackboxing-spotifys-recommendation-algorithm/)
[In class notebook on recommender systems](https://observablehq.com/@sandravizmad/recommender-systems)

## Choose a dataset
You can find different options below and you are free to choose your dataset for this part, the only condition is that it is a Spotify dataset. Working with a "smaller" dataset from Kaggle or creating your own one using the API, or even working with a very "big" dataset as the one from the challenge isn't the same workload, which I will clearly take into account when evaluating your work.

### Kaggle
On Kaggle you can find a huge range of Spotify dataset, please check out this link.

### Spotify API
Spotify has a web API that developers can use to retrieve audio features and metadata about songs such as the song’s popularity, tempo, loudness, key, and the year in which it was released.
![dataset](/media/readme/dataset.png)

We can use this data to build music recommendation systems that recommend songs to users based on both the audio features and the metadata of the songs that they have listened to.

### Challenge dataset
Moreover you are also free to use the dataset from part II of the final project, which includes playlists.

## Cleaning & EDA
Before building anything using data, you need to understand the data: structure, patterns, outliers etc. in this process you might even consider transforming your data as presented in the example below (normalizing). Moreover it is necessary to check on some simple relationship in your data. You can use different visualizations like histograms, scatter plots, correlation matrix etc.)

Please check out these two notebooks for inspiration, even though you can of course use Python code for your EDA.

### Tidy data
"Clean" data is a deceptive fiction. In reality, we must regularly wrangle, cajole, prepare, and format data for downstream use. This notebook explores how to transform messy datasets into a more usable tidy format. This notebook is a translation of Hadley Wickham's Tidy Data (Chapter 12, R for Data Science) to JavaScript using Arquero.

[Original](https://observablehq.com/@uwdata/tidy-data-in-javascript?collection=@uwdata/arquero)
[Spotify data](https://observablehq.com/@sandravizmad/spotify-genres-arquero)

### Plot visualizations
Exploratory data analysis (EDA) is a very important step which takes place after feature engineering and acquiring data and it should be done before any modeling. This is because it is very important for a data scientist to be able to understand the nature of the data without making assumptions. The purpose of EDA is to use summary statistics and visualizations to better understand data, and find clues about the tendencies of the data, its quality and to formulate assumptions and the hypothesis of our analysis. EDA is NOT about making fancy visualizations or even aesthetically pleasing ones, the goal is to try and answer questions with data. Your goal should be to be able to create a figure which someone can look at in a couple of seconds and understand what is going on. If not, the visualization is too complicated (or fancy) and something similar should be used. EDA is also very iterative since we first make assumptions based on our first exploratory visualizations, then build some models. We then make visualizations of the model results and tune our models.

[Visual data science using R](https://slides.com/sandravizmad/rggplot2)
[Spotify data](https://observablehq.com/@sandravizmad/spotify-data)

## Build the RS
As we learnt in class there is not ONE way to build a RS, therefore I ask you to experiment with different options. In any case start with something basic like the "most popular" option and then create at least one collaborative filtering and one content based filtering RS.

### Recommender in python
As a starting point have a look at the example below you can also find under this link

The dataset used for this exercise contains over 175,000 songs with over 19 features grouped by artist, year and genre. (please check the file attachment of the notebook.)


# II Part - Explain an advanced RS
In this second part you are asked to select one submission to the challenge and explain their approach as well as the code.

### Overview
In 2018 Spotify sponsored the RecSys Challenge 2018, which introduced the Million Playlist Dataset (MPD) to the research community. The challenge focused on music recommendation, specifically the challenge of automatic playlist continuation. By suggesting appropriate songs to add to a playlist, a RS can increase user engagement by making playlist creation easier, as well as extending listening beyond the end of existing playlists.

Sampled from the over 4 billion public playlists on Spotify, this dataset of 1 million playlists consist of over 2 million unique tracks by nearly 300,000 artists, and represents the largest public dataset of music playlists in the world. The dataset includes public playlists created by US Spotify users between January 2010 and November 2017.

![spotify-million](/media/readme/million.png)

In September 2020, Spotify re-released the dataset as an open-ended challenge on AIcrowd.com. The dataset can now be downloaded by registered participants from the Resources page.

Each playlist in the MPD contains a playlist title, the track list (including track IDs and metadata), and other metadata fields (last edit time, number of playlist edits, and more).

All data is anonymized to protect user privacy. Playlists are sampled with some randomization, are manually filtered for playlist quality and to remove offensive content, and have some dithering and fictitious tracks added to them. As such, the dataset is not representative of the true distribution of playlists on the Spotify platform, and must not be interpreted as such in any research or analysis performed on the dataset.

## Data
The challenge set consists of a single JSON dictionary with three fields:

-   date - the date the challenge set was generated. This should be "2018-01-16 08:47:28.198015"
-   version - the version of the challenge set. This should be "v1"
-   playlists - an array of 10,000 incomplete playlists. Each element in this array contains the following fields:
    -   pid - the playlist ID
    -   name - (optional) - the name of the playlist. For some challenge playlists, the name will be missing.
    -   num_holdouts - the number of tracks that have been omitted from the playlist
    -   tracks - a (possibly empty) array of tracks that are in the playlist. Each element of this array contains the following fields:
        -   pos - the position of the track in the playlist (zero offset)
        -   track_name - the name of the track
        -   track_uri - the Spotify URI of the track
        -   artist_name - the name of the primary artist of the track
        -   artist_uri - the Spotify URI of the primary artist of the track
        -   album_name - the name of the album that the track is on
        -   album_uri -- the Spotify URI of the album that the track is on
        -   duration_ms - the duration of the track in milliseconds
    -   num_samples the number of tracks included in the playlist
    -   num_tracks - the total number of tracks in the playlist.

## Challenge
The goal of the challenge is to develop a system for the task of automatic playlist continuation. Given a set of playlist features, participants' systems shall generate a list of recommended tracks that can be added to that playlist, thereby "continuing" the playlist. We define the task formally as follows:

-   Input A user-created playlist, represented by: Playlist metadata (see the dataset README).

    K seed tracks: a list of K tracks in the playlist, where K can equal 0, 1, 5, 10, 25, or 100.

-   Output A list of 500 recommended candidate tracks, ordered by relevance in decreasing order.

## Submissions
Please choose one of the following submissions.

Efficient K-NN for Playlist Continuation Domokos M. Kelen, Dániel Berecz, Ferenc Béres, András A. Benczúr.
[Paper](https://dl.acm.org/doi/10.1145/3267471.3267477) | [Code](https://github.com/proto-n/recsys-challenge-2018)

Effective Nearest-Neighbor Music Recommendations Malte Ludewig, Iman Kamehkhosh, Nick Landia, Dietmar Jannach.
[Paper](https://dl.acm.org/doi/10.1145/3267471.3267474) | [Code](https://github.com/rn5l/rsc18)

Automatic Music Playlist Continuation via Neighbor-based Collaborative Filtering and Discriminative Reweighting/Reranking Lin Zhu, Bowen He, Mengxin Ji, Cheng Ju, Yihong Chen.
[Paper](https://research.atspotify.com/) | [Code](https://research.atspotify.com/)

Efficient similarity based methods for the playlist continuation task Guglielmo Faggioli, Mirko Polato, Fabio Aiolli.
[Paper](https://dl.acm.org/doi/10.1145/3267471.3267481) | [Code](https://github.com/LauraBowenHe/Recsys-Spotify-2018-challenge)

A hybrid two-stage recommender system for automatic playlist continuation Vasiliy Rubtsov, Mikhail Kamenshikov, Ilya Valyaev, Vasiliy Leksin, Dmitry I. Ignatov.
[Paper](https://dl.acm.org/doi/10.1145/3267471.3267488) | [Code](https://github.com/VasiliyRubtsov/recsys2018)

Artist-driven layering and user’s behaviour impact on recommendations in a playlist continuation scenario, Sebastiano Antenucci, Simone Boglio, Emanuele Chioso, Ervin Dervishaj, Shuwen Kang, Tommaso Scarlatti, Maurizio Ferrari Dacrema.
[Paper](https://observablehq.com/@sandravizmad/Artist-driven%20layering%20and%20user's%20behaviour%20impact%20on%20recommendations%20in%20a%20playlist%20continuation%20scenario) | [Code](https://github.com/tmscarla/spotify-recsys-challenge) | [Presentation](https://www.slideshare.net/EmanueleChioso/recsys-challenge-2018-creamy-fireflies-artistdriven-layering-and-users-behaviour-impact-on-titolo-presentazione-recommendations-in-a-playlist-continuation-scenario)

MMCF: Multimodal Collaborative Filtering for Automatic Playlist Continuation Hojin Yang, Yoonki Jeong, Minjin Choi, Jongwuk Lee.
[Paper](https://dl.acm.org/doi/10.1145/3267471.3267482) | [Code](https://github.com/hojinYang/spotify_recSys_challenge_2018) | [Presentation](https://www.slideshare.net/HojinYang3/mmcf-multimodal-collaborative-filtering-for-automatic-playlist-conitnuation)

Two-stage Model for Automatic Playlist Continuation at Scale, Maksims Volkovs, Himanshu Rai, Zhaoyue Cheng, Ga Wu, Yichao Lu, Scott Sanner.
[Paper](https://dl.acm.org/doi/10.1145/3267471.3267480) | [Code](https://github.com/layer6ai-labs/RecSys2018)

### Resources
[Research Spotify](https://research.atspotify.com/)
[ACM RecSyS Challenge 2018](https://www.recsyschallenge.com/2018/)
[Spotify Million Playlist Dataset Challenge](https://www.aicrowd.com/challenges/spotify-million-playlist-dataset-challenge)


# III Part - Evaluate ethical challenges
In this last part I want you to reflect, discuss and evaluate ethical challenges regarding the Spotify RS.

## Sexism on Spotify
-   What is your view on "Sexism on Spotify"?
-   How do you evaluate the "Smirnoff Equalizer on Spotify"?

Please read through the resources below and base your response also on the discussion in class related to algorithmic biases.

[Reflecting on Spotify’s Recommender System.](https://songdata.ca/2019/10/01/reflecting-on-spotifys-recommender-system/)
[Sexism on Spotify](https://thebaffler.com/latest/discover-weakly-pelly)
[Smirnoff Equalizer tool on Spotify – gimmick or meaningful?](https://idmmag.com/news/smirnoff-equalizer-tool-spotify/)

[Recommender Systems and their ethical challenges.](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3378581)
Silvia Milano, Mariarosaria Taddeo, Luciano Floridi
University of Oxford - Oxford Internet Institute
2019

## Strategic Recommendation Bias
Make a thought experiment regarding the "Strategic Recommendation Bias" presented in the paper below.

Can you imagine this becoming a real problem which will be unnoticed by users?
What could be the possible risk and harm for users and artists?
What is your suggestion to address this problem?
[Streaming Platform and Strategic Recommendation Bias](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3338744)
Marc Bourreau.
Telecom ParisTech.
Germain Gaudin.
University of Freiburg - College of Economics and Behavioral Sciences.
2018