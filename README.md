# SVD-based recommendation system: music artists dataset
In my previous project I gathered movie information via webscraping andcreated a simple metadata-based system (it was solely based on the genre, description, title, stars, and directors of the movie). The "system" was very simple and did not take into account any user information whatsoever, which made me curious about recommendation systems in general. 

As I was researching, I came across model-based recommendation systems which tend to be very accurate, and I decided to explore one of them. This project allowed me to study how singular value decomposition (SVD) can be used in recommendation systems to make predictions about users. Using Last FM dataset, I created a recommender system that uses SVD to predict what music artists a user might like and return a list with top recommendations for that user.

In this file, I will talk about the following:

1. Types of recommendation systems;
   a. What is SVD?;
2. How is SVD matrix decomposition used in recommendation systems;
3. Explanation of my coding implementation;

## Types of recommendation systems
1. Content-based recommendation systems:
   This is the type of system I created in my previous project. These are the systems that utilise the information about an item such as its description, its script, or other relevant information to find similar items that then would be recommended to a user. The comparing of two items can be done via natural language processing so that the similarity between items can be quantified. However, as I mentioned in my previous project, this approach fails to incorporate any information about user preferences, behaviour or history, meaning it is not personalized.
2. Collaborative filtering:
   a. Memory-based:
     - Item-item filtering:
       Such system finds users that are similar to the given user based on the ratings they have given, then it proposes the items the similar users liked. 
     - User-user filtering:
       This system takes in an item and finds other items that users who liked the input item also liked, then it returns those similar items.
   While memory-based systems do not require training and are based on arithmetic operations (cosine similarity, etc), their performance drops as there are more users and items. As the new items and users are added, the number of arithmetic operations needed to assess the user-user or item-item similarity rises as well, leading to a signifocant increase in run time.
   b. Model-based:
      - Deep learning
      - Matrix factorization:
        In matrix factorization approach we are essentially breaking down our "user_id, item_id, rating" matrix into three lower dimensional matrices and mapping the users and items onto the space of n factors. These factores can be genres (how much a particular movie represents a certain genre/how much the user likes the certain genre), mood (what kind of mood a particular song has/how much the user likes the songs in this mood), etc. Breaking down the "user_id, item_id, rating" matrix into two, we are representing each user and item as a datapoint in the 3-dimensional space of these n factors (genre, mood, etc).

### What is SVD?
SVD - Singular Value Decomposition of a matrix into three matrices ("itemsXnfactors", "nfactorsXusers", "eigenvalues" matrix). In the paragraph above I talked about the first two matrices, which map items and users as datapoints onto the three-dimensional space of the n factors. With regards to the "eigenvalues" matrix, this matrix represents the "strength/importance" of each factor in determining the item's rating. 

## How is SVD matrix decomposition used in recommendation systems?
Once we have decomposed the original matrix into three matrices, we can multiply them by each other to get the original matrix. For example, if we want to know the approximate rating user 5 gave to artist 3, we need to get the dot porduct of the corresponding row and column in the two lower dimensional matrices. SVD is able to estimate this rating pretty well since it minimises SSE (Sum of Square Error). However, SVD is not defined for the missing values we want to estimate. Therefore, we can train the model on the known ratings so that we will be able to predict the unknown ones. Hence, our objective is to minimise the difference between the given rating and the one the model estimates. This difference can be quantified using RSME (Root Mean Square Error), thus, our objective would be to minimise RMSE:
<img width="503" alt="Screenshot 2023-06-28 at 8 06 50 PM" src="https://github.com/miraslavats/SVD-based-rec-system-music-artists/assets/112869592/3bdcdfc2-111e-46f1-be5b-5e1a3be9212c">

Once we minimise RMSE on the known ratings, we will be able to predict the unknown ratings as well!
In conclusion, by training the model on the known ratings (Trying different SVDs to calculate the error), we are trying to minimise RMSE.This way we will be able to predict the unknown ratings somewhat accurately. 

## Explanation of my coding implementation
In my code, I am using surprise library to train, test, and evaluate the SVD model. In the SVD() surprise fnction I am specifying two parameters: n_factors and n_epochs. n_factors determines the number of latent factors (genres, mood, etc) the model has. The greater the number, the more nuanced the model is since it is able to capture slight differences in music artists' genres or the users' preferences better. However, with a bigger number icreases the risk of overfitting (that is the model fits the training data too well and is able to make less accurate predictions on the unseen data). The other parameter I specified was n_epochs. This parameter determines how many "trials" the algorithm takes to determine the optimal SVD. Both these parameters have an impact on the run time and performance of the model. This is why I tried several different combinations to determine the parameters that both have a good performance (minimise RMSE) and do not run endlessly. These are the results I got:
<img width="776" alt="Screenshot 2023-06-27 at 7 40 31 PM" src="https://github.com/miraslavats/SVD-based-rec-system-music-artists/assets/112869592/722dbac2-ca9f-464f-97b3-c0c4db97eadc">
<img width="776" alt="Screenshot 2023-06-27 at 7 40 52 PM" src="https://github.com/miraslavats/SVD-based-rec-system-music-artists/assets/112869592/fc80ae92-fb77-48d2-afcf-c3dc89be24f0">

After training and evaluating the model, I used it to make predictions for a specific user and find the n most highly predicted artists to recommend:

<img width="746" alt="Screenshot 2023-06-27 at 7 44 08 PM" src="https://github.com/miraslavats/SVD-based-rec-system-music-artists/assets/112869592/9e8ca16c-5557-4d00-8d65-09aba06a8bb8">

The full code and more explanation can be found in the .ipynb file.
I used [Last FM](https://www.last.fm) [360K users dataset](http://ocelma.net/MusicRecommendationDataset/lastfm-360K.html).
## References
1. Steve Huang; January 24th, 2018; "Introduction to Recommender System. Part 1 (Collaborative Filtering, Singular Value Decomposition)"; https://hackernoon.com/introduction-to-recommender-system-part-1-collaborative-filtering-singular-value-decomposition-44c9659c5e75;
2. Microsoft Corporation; 2021; "Surprise Singular Value Decomposition (SVD)"; https://github.com/microsoft/recommenders/blob/main/examples/02_model_collaborative_filtering/surprise_svd_deep_dive.ipynb
3. Artificial Intelligence - All in One; "Mining Massive Datasets - Stanford University"; April 13, 2016; https://www.youtube.com/watch?v=HY3Csl52PfE
4. Nick Becker; November 10, 2016; "Matrix Factorization for Movie Recommendations in Python"; https://beckernick.github.io/matrix-factorization-recommender/
