# Effective Nearest-Neighbour Music Recommendations
##  General Idea

- [Code](https://github.com/rn5l/rsc18)

This team's approach, hereafter refered to as their teamname KAENEN, used a hybrid system of conceptually simple algorithms to produce accurate predictions. KAENEN placed 7th in the main competition and 3rd in the creative sidetrack, out of a 100 teams.
Their hybrid method uses and combines variations of nearest-neighbor, matrix factorization and string matching algorithms to achieve accuracies just 5-7% lower than that of the winners of the competition.
Their approach shows that you can achieve good accuracy without having to use more complex methods such as deep learning and without specialized hardware like GPU's (which happen to be in a terrible shortage right now!).

What they generally have done is separately trained recommenders such as item-based collaborative filter(similar to what our team achieved), playlist based K nearest neighbour, title matching+matrix factorization, and then combined the results of those recommenders. The combination used depends on what the input type is; for example the playlist name + one seed track results in a combination of title string matching and the combined weighted result of all the item-based recommenders, while an input with 2>= seed tracks will use a weighted combination of the item & playlist based recommenders. The combinations will be explained later on in this article.

For their creative track, they used the same hybrid method, but additionally retrieved song metadata from the Spotify API to aid in finding track similarities.

### Dictionary
- Session-based = Only input is a playlist name or a set of songs. Called "session" because there is no longer term information about the user to base the recommendations off of.

## Data Prep
Maybe not even needed? It's good filler tho

## The Different Methods
In their final solution, KAENEN implemented 6 techniques to calculate the recommendations:
- Item-based Collaborative Filtering (item-cf)
- Session-based Nearest Neighbors (s-knn)
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

The similarity of the tracks can then be computed using cosine similarity (the dot product of two track's vectors square rooted and divided and stuff).

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




### Session-based Nearest Neighbors (s-knn)
Session-based, or playlist based nearest neighbors is a collaborative filter very similar to the item-based approach mentioned above.
Instead of drawing similarities between tracks and where they appear in playlists, s-knn looks at playlists and which other playlists also contain the same songs.

Cosine similarity is used to compute the playlists' relations, and IDF is used again to give more weight to the less popular tracks.

This approach gains its "nearest neighbors" name by only comparing the `m` latest edited sessions/playlists to the supplied playlist. According to the writers, using m=5000 did not decrease accuracy, and rather even improved Spotify's *Clicks* metric.


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

Again, nearest neighbors are used again for the similarity computation, and here they noted that m = 1000 produced the best results.

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
To get more hits, the names are normalized using stemming. Stemming means that a word gets cut to its "root" form from which other words are constructed. For example "cats" will be stemmed to "cat", and "computers" get stemmed to "comput".

### Title Factorization (title-mf)

Title-mf seems to use matrix factorization in order to find correlations between a playlist's name and the tracks it contains.

The matrix is cold-started similarly to als-mf, with the playlist names counting as "users" and guessing a latent type based on the implicit choice of tracks the "user" contains.

Instead of matching to just one (the most similar) playlist, title-mf uses the k-most similar names, counts the occurence of each track in these playlists, and then multiplies them by the playlist's similarity.

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
- Switching - The model uses different combinations of weighted methods depending on the amount of seed tracks and playlist name. The best results were obtained with:
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

For example item-cf is accurate when it comes to predicting the correct tracks, it scores low on the Clicks metric because the predictions are not ending up in the top of the list.
However s-knn scores the correct tracks higher up in its list even though it itself has weaker overall recommendations.

## Discussion

This paper was a positive insight since it shows value in learning these traditional techniques. You don't always have to solve problems with more data, more layers and more gpu's!

It was interesting to find out our suspicion that item-cf biases towards more popular tracks among the playlists, and we were speculating as well to use IDF to scale their emphasis down. Although applying IDF to our own solution would most likely make the computation incredibly slow, since we aren't using any external libraries.
Which is another insight, their project did not seem to use any advanced libraries for item-cf/k-nn either! All we discovered was the use of Pandas dataframes. They seem to be way more efficient & fast than what the eye might judge!

We must say that while the flow of their project on GitHub was very understandable, their code was difficult to follow because of almost non-existant code comments, and cryptic variable names...