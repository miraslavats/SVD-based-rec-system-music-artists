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
Once we have decomposed the original matrix into three matrices, we can multiply them by each other to get the original matrix. For example, if we want to know the approximate rating user 5 gave to artist 3, we need to get the dot porduct of the corresponding row and column in the two lower dimensional matrices. SVD is able to estimate this rating pretty well since it minimises SSE (Sum of Square Error). However, SVD is not defined for the missing values we want to estimate. Therefore, we can train the model on the known ratings so that we will be able to predict the unknown ones. Hence, our objective is to minimise the difference between the given rating and the one the model estimates. This difference can be quantified using RSME (Root Mean Square Error), thus, our objective would be to minimise RMSE. Once we minimise RMSE on the known ratings, we will be able to predict the unknown ratings as well!
In conclusion, by training the model on the known ratings (Trying different SVDs to calculate the error), we are trying to minimise RMSE.This way we will be able to predict the unknown ratings somewhat accurately. 

## Explanation of my coding implementation
In my code, I am using surprise library to calculate and train 

