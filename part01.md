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
