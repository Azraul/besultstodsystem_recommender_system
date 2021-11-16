<span style="font-size:4em; color:#FF0066"> FINAL PROJECT</span>

<span style="font-size:2em; color:#FF6666"> SPOTIFY RS - BUILD + EXPLAIN + EVALUATE </span>

<span style="font-size:2em;">By: Johan Penttinen, Fredrik Ståhl, Kristoffer Kuvaja Adolfsson</span>

# besultstodsystem_recommender_system

[Teacher notebook](https://observablehq.com/@sandravizmad/final-project "observable.com")

# Assignment Overview

Therefore the assignment will be separated into three parts. 

I Part     
Build RS in Python using Spotify data

II Part     
Explain a more advanced RS using Spotify data 

III Part     
Evaluate potential ethical challenges of RS using Spotify data

This is a group project, no group can be bigger than 3 people. The final grade will be a group grade meaning everyone in the group gets the same grade. Therefore it is in your own freedom and responsibility to share the workload. 

## Delivery 

Please create a notebook here in Observable, in which you summarize all your project results. The notebook should include a short introduction text in which you summaries your findings and present the steps (three parts) of your project. 

In the first part of the assignment you are asked to create your own RS and therefore have to provide the corresponding code and results/outputs. 
You either 

- upload the code as a file to the notebook
  
- use a [link](https://gist.githubusercontent.com/amankharwal/e35290f8f355280c8d939e381f0b43ec/raw/83b09090797abaa42e6a0baf494c8ccf901bb53b/spotify.py)
  
- or copy your code into the notebook

The summary of the whole project including part 1,2 and 3 should be in a video format with a duration of 15-20 min. You are free to use any video platform and upload the link to the video file to the notebook. In the video you should present the idea and code of your own RS, explain the advanced RS and finally evaluate the ethical challenges. For more information please check the detailed information in each part. 

## Deadline 

The link to the final notebook with all the links and material included has to be submitted to "itslearning" before the 17/11/2021 end of day (3 weeks). The workload of this project should be around 50 hours, which means approx. 22 hours for Part 1, 16 hours for Part 2, 12 hours for Part 3, these numbers are guidance only. 

## Evaluation 

The max. points for this assignment will be 65, spilt into the three parts as followed: 

I Part - 28 points

Criteria for the evaluation will be accuracy, complexity and presentation of the work. All analysis and code has to be correct and clean with short comment lines to help navigating the logic. Complexity also matters, as trying out an advanced idea is more challenging and greater workload than a simple repetition of shown examples. Last but not least the presentation of your work should have a clear flow, explanation and interpretation of the results. If you tired something that didn't work, please also present this work as it shows your workflow and in general can be very interesting. 
  
II Part - 21 points

In this part of the assignments all that matters is how well you can explain a complex approach. Please use a step by step presentation meaning first showing the general idea (for example the conclusion of the paper) followed by walking through the model in conjunction with the code. 
  
III Part - 16 points

In the last part I evaluate how amplified and detailed your judgement regarding ethical challenges in the RS for music streaming platform like Spotify is presented. 

[Teacher notebook](https://observablehq.com/@sandravizmad/final-project "observable.com")

# Assignment
## Evaluation

The max. points for this assignment will be 65, spilt into the three parts as followed:

### I Part - 28 points

Criteria for the evaluation will be accuracy, complexity and presentation of the work. All analysis and code has to be correct and clean with short comment lines to help navigating the logic. Complexity also matters, as trying out an advanced idea is more challenging and greater workload than a simple repetition of shown examples. Last but not least the presentation of your work should have a clear flow, explanation and interpretation of the results. If you tired something that didn't work, please also present this work as it shows your workflow and in general can be very interesting.

### II Part - 21 points

In this part of the assignments all that matters is how well you can explain a complex approach. Please use a step by step presentation meaning first showing the general idea (for example the conclusion of the paper) followed by walking through the model in conjunction with the code.

### III Part - 16 points

In the last part I evaluate how amplified and detailed your judgement regarding ethical challenges in the RS for music streaming platform like Spotify is presented.

# Part 1
## Spotify

Spotify was founded 2006 in Sweden and is a audio streaming service. Today Spotify is the largest streaming service provider with over 70 million tracks, 380 million active users and 170 (newsroom.spotify.com, November 2021) million subscribers. For scale, the European Union has a combined population of 444 million, Finland’s population is around 5,5 million while Germany, EU’s most populated country, has a population of 83 million people (www.worldometers.info, November 2021).

So how do you keep a user base the size of unions active and engaged everyday for over 10 years? Speculate as we might, that’s not what this is about, we are simply here to look at the worlds currently most valuable resource, a resource Spotify has massive amounts of. It is a resource called data.


### Spotify API
*See notebook: explore_spotify_api.ipynb*
“Based on simple REST principles, the Spotify Web API endpoints return JSON metadata about music artists, albums, and tracks, directly from the Spotify Data Catalogue.” (developer.spotify.com, November 2021)

We explored the Spotify API using Spotipy. “Spotipy is a lightweight Python library for the Spotify Web API. With Spotipy you get full access to all of the music data provided by the Spotify platform.” (spotipy.readthedocs.io, November 2021) 

As one of us had never used Spotify before we naturally set that person to work on trying the API, gently pushing them out of their comfort zone and into a Spotify account.

![dataframe](/media/readme/spotify_api_Return.png)

Their findings can be found in the notebook explore_spotify_api.ipynb. But shortly summarized the Spotipy library makes fetching playlists and tracks rather simple, you get tracks, album covers and features. This means we could in theory build our own dataset but as it is a rather length process, not considering the rate limits we might hit we decided to make use of other datasets to go forward with our assignment.  

## Spotify One Million Dataset Challenge
*See notebook: explore_mpd_playlists.ipynb*
We went ahead and got access to the Spotify Million Playlist Dataset Challenge (henceforth mpd) and started exploring the data to try and get a good grasp on what there was to work with. 

Compared to directly connecting to the Spotify API there was a rather large amount of information missing. However, most of the useful playlist data was there, save for perhaps the specific time when the track was added to the playlist. The mpd set also removed much redundant (for us) data like album art.
 
![spotify-mpd-logo](/media/spotify-mpd-logo.png)

The full mpd set is rather large and with no access to “super” computers we decided to limit ourselves as the theory and proof of concept can be done with a thousand playlists just as well as a million.

With the first thousand playlists we could get a good overview of tracks, artists, and albums. We could then count the occurrence of these same data points over all the playlists and get a value on popularity, we combined all of this into a metadata set. We decided to plot a few using plotly and take a closer look. You can find a more detailed look in our notebook explore_mpd_playlists.ipynb

![dataframe-top-artists](/media/top-artist-mpd.png)
 
With a popularity score for all playlists, we could look at a single playlist and do something similar. Obviously track popularity wasn’t doable as each track just appears once in each playlist and there was no review or rating system to access so we simply went with artist and album popularity.

### Basic collaborative recommender
*See notebook: explore_mpd_playlists.ipynb*
Based on these 2 factors we could get a playlists top artists and albums and from there cross reference it with the metadata to recommend the most popular tracks for each of those artists and albums. And like that we have created a collaborate recommend system.

As basic as this system is we will move forward as the mpd dataset won’t suffice if we want to work on making a content-based recommender system and as we’d rather avoid fetching all the data ourselves we will turn to Kaggle.

## Spotify Features & Content filtering
*See notebook: content_based.ipynb*
User Zaheen Hamidani over at Kaggle.com [https://www.kaggle.com/zaheenhamidani/ultimate-spotify-tracks-db] has made a dataset on Spotify features that had the essential values we could get using Spotify API, like tempo, danceability or energy and the dataset has over 232.000 tracks, it is all available for download as comma-separated values.

We decided that the best approach would be to try and combine this new dataset with the playlists we compiled earlier as we still needed playlists (read users) else what is our recommend for!

After combining the datasets, we made sure to clean, there was quite many nonsensible rows and duplicates and eventually we had about 16.000 rows and 61 columns of data (that’s close to 1 million datapoints for those interested).

Danceability over genre would for example look like this.


![plotly-scatter](/media/metadata-features-plotly.png)

### Content-based recommender system
*See notebook: content_based.ipynb*

By using a user’s playlist and applying the correct features by the songs we had available we can summarize the playlist into a feature vector. We can find similar tracks by vectoring each song in our dataset and grab those most similar and recommend them to our user.

Our user, that we explored from the mpd dataset liked artists like Justin Bieber, Ed Sheeran and Ben Rector got recommended the following tracks as a result of our content-based recommender.

![dataframe-recommendations](/media/content-based-recommendations.png)


Interesting to note is that all the recommended songs are from different artists than the ones we found most popular in this users playlist before, but the recommended genre was (based on genre for the top artists we explored earlier for this user).

Something else that could be interesting to look at is how the summarized vectors of the recommended songs compare to the user’s vector.

![vectors](/media/content-recommender-vectors.png)

We would like to conclude that this content-based recommender works rather well and could recommend tracks for our user even if they where the first on the platform or recommend tracks to our users that are completely new to Spotify without relying on others to collaboratively engaging with the track first.


## Spotify Collaborative Filtering
*See notebook: collaborative_based.ipynb*

Earlier we did a basic collaborative recommender. We want to expand upon the collaborative filtering and use similar techniques, like vectors from the content filter, this means we can go back to purely using the mpd dataset.

When collaborativly filtering you can make it user-based or, intresstingly, item-based, basing our recommender on Joel Grus systems from [Data Science from Scratch, 2nd Edtion](https://www.oreilly.com/library/view/data-science-from/9781492041122/), we have built one final recommender that can do either type on a playlist basis.

*The name Pelle comes from the finnish word clown : pelle, this is no connection or reference to any real world people happening to be named pelle.*

### User-based

The user-based approach looks at what other users have added to their playlist and compares the supplied playlist/songs with those.
Each playlist is transformed into a sparse feature vector, where the vector is `num_unique_songs` long and the respective positions represent a unique song. When *Jane Doe* adds her favourite country songs to her playlist, the playlist's vector is compared in similarity to all other existing vectors using cosine similarity. A lower value means the playlists are less similar. Then we go through each playlist’s tracks, assigning the track names to a big dictionary and += the songs with their own playlist's cosine similarity value.

Similar playlists will give much more weight to their songs, and likewise very popular songs will gain a high score, even though these songs might be in very similar playlists. The end result is a list of songs with decent chance of being similar to Jane Doe's favourites.

### Item-based

The item-based approach is quite similar to user-based, with the big difference being the playlist vectors being turned around into a list of songs, its feature vector is a long list indicating which playlist the song exists in. The matrix is `num_unique_songsrows` x `num_playlists`. Jane Doe now has a sparse feature vector for each song in her playlist but is it still the same type of logic as a user-based, where the vectors get a cosine between each other, and these similarities are added up for the tracks to produce a ranking of most similar songs.

The advantage of an item-based approach could perhaps be that if Jane adds a track of very different genre compared to what her usual listening, we can now compare the similarity of this one track to all others and get similar tracks (without having to compare her playlist as a whole).

### Assessment

There's no assessment function, but from what we've interpreted of these implementations, it would be hard to assess because mostly the popular songs are recommended, so if we removed a few songs to see if they'd get re-recommended, they probably wouldn't if they were more niche tracks. So a "qualitative" method will be used instead.

### Conclusion

The recommendations seem totally serviceable.

But there's definitely a weakness to such a *simple* system. It will prefer to recommend the most popular songs out of all the other's playlists. Say you have a hiphop playlist, but most tracks are niche or older, then you will not be recommended such tracks because the recommender does addition on each playlist's tracks, say playlist[n] has similarity score of 0.05, then every song in that playlist gets incremented with that value. This means the most popular and mainstream tracks will accrue more points. More recent and mainstream tracks will bubble up to the top, while the user's own niche will be ignored.

## Reflections
Neither content-based nor collaborative user/item-based are perfect on their own. Each system has strengths and weaknesses. Using our systems, we theorize a *switching hybrid* could be developed and function quite well, using the content-based recommender for starting and slowly building up the data required to really let the collaborative recommender get to work. A *mixed hybrid* could also work with something so simple as letting each system recommend 50/50 for example.


# Effective Nearest-Neighbour Music Recommendations
##  General Idea

- [Code](https://github.com/rn5l/rsc18)

This team's approach, hereafter referred to by their team name KAENEN, used a hybrid system of conceptually simple algorithms to produce accurate predictions. KAENEN placed 7th in the main competition and 3rd in the creative sidetrack, out of 100 teams.
Their hybrid method uses and combines variations of nearest-neighbour, matrix factorization and string matching algorithms to achieve accuracies just 5-7% lower than that of the winners of the competition.
Their approach shows that you can achieve good accuracy without having to use more complex methods such as deep learning and without specialized hardware like GPU's (which happen to be in a terrible shortage right now!).

What they generally have done is separately trained recommenders such as item-based collaborative filter(similar to what our team achieved), playlist based K nearest neighbour, title matching+matrix factorization, and then combined the results of those recommenders. The combination used depends on what the input type is; for example, the playlist name + one seed track results in a combination of title string matching and the combined weighted result of all the item-based recommenders, while an input with 2>= seed tracks will use a weighted combination of the item & playlist based recommenders. The combinations will be explained later on in this article.

For their creative track, they used the same hybrid method, but additionally retrieved song metadata from the Spotify API to aid in finding track similarities.

### Dictionary
- Session-based = Only input is a playlist name or a set of songs. Called "session" because there is no longer term information about the user to base the recommendations off on.

## Data Prep
Maybe not even needed? It's good filler though

## The Different Methods
In their final solution, KAENEN implemented 6 techniques to calculate the recommendations:
- Item-based Collaborative Filtering (item-cf)
- Session-based Nearest Neighbours (s-knn)
- IDF Extension (idf-knn)
- Matrix Factorization (als-mf)
- String Matching (string-match)
- Title Factorization (title-mf)

Which then are combined to create the final suggestions. These will be explained in the following sections.

The techniques/algorithms are all initialized and then trained separately in a simple for loop:
```py
algs = {}
mp = MostPopular()

algs['diskknn'] = Fill( KNNDisk( 1000, tf_method='ratio-s50', idf_method='log10', similarity='cosine', sim_denom_add=0, folder=FOLDER_TEST ), mp )
algs['sknn'] = Fill( SessionKNN( 2000, 0, idf_weight=1, folder=FOLDER_TEST ), mp )
algs['iknn'] = Fill( ItemKNN( 100, alpha=0.75, idf_weight=1, folder=FOLDER_TEST ), mp )
algs['implicit'] = Fill( ColdImplicit( 300, epochs=10, reg=0.08, idf_weight=1, algo='als' ), mp )
```
*Algos are initialized*

```py
for k, v in algs.items():    
    tstart = time.time()
    v.train( train, test=test )
    print( 'trained {} in {}s'.format( k, (time.time() - tstart) ) )
```
*Trained successively*

What is returned from these recommenders are huge feature vectors representing the relation between everything. This means all similarities to other tracks/playlists are computed ahead of time so as to speed up the recommendation times once test data is given as input. (Computing this probably took tens of hours)

### Item-based Collaborative Filtering (item-cf)
Item-based collaborative filtering calculates the similarity of one item to all the other individual items. This is done by assigning each track its own fixed position in an array, and this track element is represented as a vector of the length of the number of playlists that exist. The values are binary, a 0 if the track doesn't exist in the playlist, and a 1 if it is inside that playlist.

|   | Playlist 1  |  Playlist 2 | Playlist 3  |
|---|-------------|-------------|-------------|
|  Track 1 |  0 |  1 | 1  |
|  Track 2 |  1 |  1 | 1  |
|  Track 3 |  0 |  1 | 0  |

*Item-based binary vector visualized*

The similarity of the tracks can then be computed using cosine similarity (the dot product of two track's vectors square rooted and divided etc. etc.).

These similarity scores are then simply added together to produce a list of similar tracks.

But this approach will often produce a popularity bias, only recommending the most popular songs. To counter this, the similarities can be weighed with their IDF(inverse document frequency) values. 
```
idf (t) = log10(|P| / |Pt|)
```
Basically the frequency of track `n` is counted, divided by total number of playlists, and the log value of that.
The more a track occurs, the less value it has.

This lets less popular but possibly more relevant tracks be recommended.

```py
if self.idf_weight != None:
    self.idf = pd.DataFrame()
    self.idf['idf'] = data.groupby( self.item_key ).size()
    self.idf['idf'] = np.log( data[self.session_key].nunique() / self.idf['idf'] )
    self.idf['idf'] = ( self.idf['idf'] - self.idf['idf'].min() ) / ( self.idf['idf'].max() - self.idf['idf'].min() )
    self.idf = pd.Series( index=self.idf.index, data=self.idf['idf']  )
```
*item-cf's idf weighting in action*

```py
res = res.merge( sim_list.to_frame('tmp'), how="left", left_on='track_id', right_index=True )
if self.idf_weight != None:
    res['tmp'] = res['tmp'] * self.idf[ prev ]
if self.pop_weight != None:
    res['tmp'] = res['tmp'] * self.pop[ prev ] # * (1 - self.pop[ prev ])
res['confidence'] += getattr(self, self.weighting)( res['tmp'].fillna(0), i + 2 )
```
*The similarities are added up for each song. res['confidence']=similarity*




### Session-based Nearest Neighbours (s-knn)
Session-based, or playlist based nearest neighbours is a collaborative filter very similar to the item-based approach mentioned above.
Instead of drawing similarities between tracks and where they appear in playlists, s-knn looks at playlists and which other playlists also contain the same songs.

Cosine similarity is used to compute the playlists' relations, and IDF is used again to give more weight to the less popular tracks.

This approach gains its "nearest neighbours" name by only comparing the `m` latest edited sessions/playlists to the supplied playlist. According to the writers, using m=5000 did not decrease accuracy, and rather even improved Spotify's *Clicks* metric.


|   | Track 1  |  Track 2 | Track 3  |
|---|---|---|---|
|  Playlist 1 |  0 |  1 | 1  |
|  Playlist 2 |  1 |  1 | 1  |
|  Playlist 3 |  0 |  1 | 0  |

*Playlist-based binary vector visualized*

```py
self.idf = pd.DataFrame()
self.idf['idf'] = train.groupby( self.item_key ).size()
self.idf['idf'] = np.log( train[self.session_key].nunique() / self.idf['idf'] )
self.idf = self.idf['idf'].to_dict()
```
*The idf is calculated for each item*

As in item-cf, but inversed, all the similar playlist's songs will be accumulated with the respective song's similarity score, and the highest scoring tracks will be recommended.


### IDF Extension (idf-knn)
Idf-knn works otherwise like s-knn, but instead of giving the tracks in the feature vectors a binary 1 or 0, they are calculating the tf-idf of each track and assigning that value instead.
Term frequency, in the context of a page of text, tells you how many times a word appears in the text versus the number of total words.
In this approach though, they are counting the number of songs in a playlist, add a small adjustment factor and then have it divide a 1:
```
tf(playlist, track) = 1 / (<occurence of track> + adjustment_factor)

tfidf(playlist, track) = tf(playlist, track) * idf(track)
```
This means the value will be 0, or a weighted value to be used for cosine similarity without having to add an idf value later on.

```py
if count_num_neighbours<=num_neighbours:
    for item in train_tfidfs[train_container]:
        # dont recommend tracks already in test playlist
        if item in test_tfidfs[test_container]:
            continue
        score = train_tfidfs[train_container][item] * sim
        if item in preds:
            score += preds[item]
        preds[item] = score
```
*The algo in action. Multiplies the tf-idf value with similarity and sets that to the item's score*

Again, nearest neighbours are used again for the similarity computation, and here they noted that m = 1000 produced the best results.

### Matrix Factorization (als-mf)

Normally, a matrix factorization is computed on datasets with user ratings, like how matrix factorization was popularized by Simon Funk in 2006's Netflix challenge.

The authors here however have extracted a latent type for each "user"(playlist) from the implicit choice of tracks in the playlist.

```py
ones = np.ones( len(data) )
        
row_ind = data.ItemIdx
col_ind = data.SessionIdx
        
self.mat = sparse.csr_matrix((ones, (row_ind, col_ind)))

if self.algo == 'als':
    self.model = implicit.als.AlternatingLeastSquares( factors=self.factors, iterations=self.epochs, regularization=self.reg )

self.model.fit(self.mat)
```    
*The team decided to use "als" instead of the "bpr" model, according to the code on github*

Idf is once again used to de-emphasize the most popular tracks.

With matrix factorization, the track similarities are computed as the dot product of the track's latent factors and the playlist.

### String Matching (string-match)

String matching is used on the playlist's names simply to find playlists with the most similar name to the input. Tracks from the most similar name will be recommended.
To get more hits, the names are normalized using stemming. Stemming means that a word gets cut to its "root" form, from which other words are constructed. For example "cats" will be stemmed to "cat", and "computers" get stemmed to "comput".

### Title Factorization (title-mf)

Title-mf seems to use matrix factorization in order to find correlations between a playlist's name and the tracks it contains.

The matrix is cold-started similarly to als-mf, with the playlist names counting as "users" and guessing a latent type based on the implicit choice of tracks the "user" contains.

Instead of matching to just one (the most similar) playlist, title-mf uses the k-most similar names, counts the occurrence of each track in these playlists, and then multiplies them by the playlist's similarity.

## Hybridization

The value of their approach comes from the combined usage of all these techniques.

```py
### Load in all the pre-computed vectors/matrices
diskknn = Solution( FOLDER_TEST + 'results_diskknn.csv' )
sknn = Solution( FOLDER_TEST + 'results_sknn.csv' )
iknn = Solution( FOLDER_TEST + 'results_iknn.csv' )
implicit = Solution( FOLDER_TEST + 'results_implicit.csv' )

smatch = Solution( FOLDER_TEST + 'results_smatch.csv' )
imatch = Solution( FOLDER_TEST + 'results_imatch.csv' )

### Combines the string matching methods. The 2nd argument are the weights for each method. 
titlerec = Weighted( [smatch,imatch], [0.5,0.5] ) #best one for title only
### This uses all track/playlist based approaches
hybrid = Weighted( [diskknn,iknn,sknn,implicit], [0.4,0.3,0.2,0.1] ) #best one for lists with 5+ seed tracks
firstcat = Weighted( [hybrid,titlerec], [0.7,0.3] ) #best one for lists with only one seed track

### And all those combined to the main recommender
algs['recommender'] = Switch( [titlerec,firstcat,hybrid], [1,5,101] ) #main
```

The trained vectors got saved to csv files, which now can be loaded into the big `Switch` class that houses the algorithms and hierarchy how they are to be used depending on the input. We have added our own comments marked with `###` into the code block above to explain what is happening.

```py
for k, v in algs.items():
    tpredict = time.time()
    res = v.predict( name, actions, playlist_id=pid, artists=artist_ids, num_hidden=num_hidden )
    pt = time.time() - tpredict
```
*The main algo is run on test data, which predictions get saved to a csv later on*

### Hybrid rules
![](https://people.arcada.fi/~penttinj/images/Screenshot_20211116_014318.png)

*Rule diagram*

The final solution they entered MPD challenge with used two approaches:
- Weighted - Two or more of the methods were combined and weighted to produce recommendations. The flow is illustrated by the green squares in the diagram.
- Switching - The model uses different combinations of weighted methods depending on the number of seed tracks and playlist name. The best results were obtained with:
    - Name only: Weighed combinations of string-match and title-mf
    - More than two tracks: Weighed combination of track-based methods.
    - Name and one track: Weighed combination of both the name-only and track-based hybrids.

## Results
![](https://people.arcada.fi/~penttinj/images/results.png)

*Final results for the MPD challenge*
- Metrics:
    - Precision N(RP): higher=better
    - NDCG: higher=better
    - Clicks@500: lower=better

Their switch model scored only 5-7% lower than the challenge winners, impressive considering the "simple" and well-known methods that have been described in this article. Each technique have their own strengths & weaknesses, and the team has managed to leverage the strength of the methods to cover for each other's weaknesses.

For example, item-cf is accurate when it comes to predicting the correct tracks, it scores low on the Clicks metric because the predictions are not ending up in the top of the list.
However, s-knn scores the correct tracks higher up in its list even though it itself has weaker overall recommendations.

## Discussion

This paper was a positive insight since it shows value in learning these traditional techniques. You don't always have to solve problems with more data, more layers and more gpu's!

It was interesting to find out our suspicion that item-cf biases towards more popular tracks among the playlists, and we were speculating as well to use IDF to scale their emphasis down. Although applying IDF to our own solution would most likely make the computation incredibly slow, since we aren't using any external libraries.
Which is another insight, their project did not seem to use any advanced libraries for item-cf/k-nn either! All we discovered was the use of Pandas dataframes. They seem to be way more efficient & fast than what the eye might judge!

We must say that while the flow of their project on GitHub was very understandable, their code was difficult to follow because of almost non-existent code comments, and cryptic variable names; A mistake we will try to avoid in our studies.

# Part III
## Sexism on Spotify

### What is our view on "Sexism on Spotify"?

According to the article the song in the most popular tracks gets recommended more often in playlists. If the recommender system is recommending mostly tracks by male artists it may influence users, which in turn shifts the collaborative weights to recommend even more male artists.

It is then fully possible that the recommender could be stuck in loop, a loop that creates an echo chamber of male artists and users that prefer males, that future emphasizes the feedback loop and even the Spotify platform itself.

Some genres may be skewed based on how male dominated they are. Rock and rap are extremely male dominated genres so it is hard to see how these two genres would not have recommendations skewed towards men. Encouraging a more even gender balance may be needed in order to balance the more male dominated music genres.

### How do we evaluate the "Smirnoff Equalizer on Spotify"?

The Smirnoff Equalizer tool is a collaboration between Spotify and Smirnoff. The tool analyses a user’s listening habits, providing a breakdown of the percentage of male vs. female artists they listen to. The equalizer will then provide a tailored, gender-balanced playlist, with an option to adjust the number of women in said playlist.

To us, the Smirnoff Equalizer seems to be more of a marketing gimmick and band-aid for publicity than an actual attempt to bring more gender balance to the Spotify playlist recommendations.

The tool was on an external site that is no longer active, meaning we can’t officially try it and evaluate it ourselves; this brings more weight on our earlier argument that it is a marketing gimmick.

For it to have been an actual attempt to change the imbalance, the tool should have been a more permanent solution, even integrated into the Spotify-app and not been branded by a partner, and if a partner was to be even considered there is an argument to make that Smirnoff is a poor one, considering how alcohol induced behavior usually affects equality. We’d rather not discuss the Smirnoff topic future.

As for how many knew about the Smirnoff Equalizer existence, we can only make an educated guess but for a majority in our team, it was a new thing, so perhaps Spotify’s users also didn’t know of its existence.

Trying to have forced equality through the equalizer tool just a band-aid and won’t fix the underlying problem with the recommendations system being skewed heavily towards men.

![vectors](/media/smirnoff.png)

### Conclusion regarding "Sexism on Spotify?"

Trying to have forced equality through the Smirnoff equalizer tool is a low effort band-aid fix and at worse a way to brush the problems aside rather than make fundamental changes in our opinion. The Smirnoff equalizer won’t fix the underlying problem with the recommendations being skewed heavily towards men. In a perfect world, the goal should be to have recommendations that aren’t biased against any gender, a neutrality if you will.

## Strategic Recommendation Bias

The paper outlines a base case where the recommender system’s goal is to maximize the consumer utility by recommending the optimal content mix. In the most basic of cases the consumer can’t search for content but has to rely on recommendations.

There are two types of content provided type A with a low royalty cost and type B with a high royalty cost. There are three types of consumers one(1) with a preference for A, consumer two(2) with a preference for a mix of A and B and a third(3) consumer that prefers type B.

### Strategic recommendation minimizing costs

In this case the platform has designed its recommendation system and adjusted its subscription price to maximize profitability by minimizing the costs.

Consumer 1 is only recommended content A. Consumer 2 is recommended a personalized mix of A and B with a bias for A and consumer 3 is provided with very biased recommendation that skews against their preferences so that they leave the platform.

Strategic recommendation can reduce royalty rates to a level lower than that obtained when the recommender system only aims at maximizing consumers’ utility.

![vectors](/media/ab-123.png)

### Use recommender system to strategically leverage content providers

By steering or threatening to steer consumers to cheaper content the platform can offer contracts with the content providers for reduced rates in exchange for increased recommendations. This can also pin content providers against each other, by having one content provider offer lower royalty rates than their competitors in exchange for more recommendations in the system.

The platform can reduce the content providers power and decrease their royalty payments by using this type of strategy.

### Vertical integration
Vertical integration is a way for the platform to reduce royalty payments. This can be done by the platform producing their own content or by merging with a content provider. This will naturally lower the royalties the platform pays.
Spotify has been bypassing record labels by licensing music directly from independent artists, in order to pay lower royalties.

The demand `for platform independent content providers will be lowered #???`. The best response for the content providers is keeping their royalties high while facing low demand.

Vertical integration will mostly benefit the platforms and harm the content providers.

### Can we imagine this becoming a real problem which will be unnoticed by users?

In the case of strategic recommendation to minimize costs. Users with a preference for content A will most likely not notice any biases in the recommendations as the system already fits with their optimal content mix, but consumers with other preferences may well notice the biased recommendations, however as the saying goes, what you don’t know won’t hurt you, this is aptly true for recommendations as it might be hard for the unaware to realize their recommendations are being altered if a platform has a shift in strategy especially if they market their change as improvement/update.

Vertical integration however is more likely to be noticed by more users than the previous case, `minimizing cost strategy #???`, especially if the contents origin is visible.

### What could be the possible risk and harm for users and artists?

By recommending users content that minimize the royalty payments to content providers, the platform will decrease the consumer utility. By minimizing costs, the users with other preferences than type A (low royalties) are affected negatively, in the form of less personalized recommendations. Users with content preference for type B (high royalties) are affected the most by biased recommendations, that in worst case may cause them to leave the platform. In turn this will then also affect content providers of content type B as their content will be recommend less and to fewer users. 

Consumer type three, who are intended to be pushed of the platform, pushes the royalties even lower for these content providers.

A biased recommender system can be used to play providers against each other with the use of special agreements to reduce the royalties in exchange for increased recommendations. This will reduce the content providers royalties more than when trying to maximize consumer utility.

By vertical integration the platforms gain more power by producing its own content or merging with a content provider. This will harm the independent content providers in the form of decreased demand. It can however lead to renegotiation with content providers to decrease royalty payments. This might cause a decrease in available content on the platform, which will negatively affect the users of type two and three.

### What is our suggestion to address this problem?

The best outcome would be to encourage platforms to recommend content based on personalized recommendations. This maximizes user utility, but the platform will have increased costs in form of royalty payments to content providers.
More transparency in different platforms content recommender systems would help negate the first case, by making it clearer for the users how the content is recommended. Perhaps it should be a consumer right to know why and how a recommendation was made to stifle the current flood of manipulation.

Content providers could also try to band together to negotiate the royalty rates in order to prevent the platforms form playing the providers against each other, a true and tested solution like unions is something we’d suggest.
To prevent too much vertical integration one of our ideas as team is to recommend newer and stronger anti-trust laws in order to prevent platforms from becoming too large and powerful. This is already an issue with almost the whole movie industry being under the same *umbrella* already.

