# SVD-based recommendation system: music artists dataset
In my previous project I gathered movie information via webscraping andcreated a simple metadata-based system (it was solely based on the genre, description, title, stars, and directors of the movie). The "system" was very simple and did not take into account any user information whatsoever, which made me curious about recommendation systems in general. 

As I was researching, I came across model-based recommendation systems which tend to be very accurate, and I decided to explore one of them. This project allowed me to study how singular value decomposition (SVD) can be used in recommendation systems to make predictions about users. Using Last FM dataset, I created a recommender system that uses SVD to predict what music artists a user might like and return a list with top recommendations for that user.

In this file, I will talk about the following:

1. Types of recommendation systems;
2. What is SVD?;
3. How is SVD matrix decomposition used in recommendation systems;
4. Explanation of my coding implementation;

## Types of recommendation systems
1. Content-based recommendation systems:
   This is the type of system I created in my previous project. These are the systems that utilise the information about an item such as its description, its script, or other relevant information to find similar items that then would be recommended to a user. The comparing of two items can be done via natural language processing so that the similarity between items can be quantified. However, as I mentioned in my previous project, this approach fails to incorporate any information about user preferences, behaviour or history, meaning it is not personalized.
2. Collaborative filtering:
   a. Memory-based:
     - Item-item filtering:
       Such system finds users that are similar to the given user based on the ratings they have given, then it proposes the items the similar users liked. 
     - User-user filtering:
       This system takes in an item and finds other items that users who liked the input item also liked, then it returns those similar items.
   While memory-based systems do not require training and are based on a measure such as 
   b. Model-based:
