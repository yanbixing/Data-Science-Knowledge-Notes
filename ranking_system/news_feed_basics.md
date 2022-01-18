# News Feed

- The news feed RecSys aims to predict a "value score" to rank the items
  - The value score is calculated based on different signals like "share", "comment", "like", "tag", "video play"
    - Ref: [FB-TechBlog](https://engineering.fb.com/2021/01/26/ml-applications/news-feed-ranking/)
  - The weight of different signals is dependent on the company's objective.
    - Ref:[Quora](https://www.quora.com/How-does-Facebook-calculate-weight-for-edges-in-the-EdgeRank-formula)
    - Can manually increase the score if needed.
    - Can learn the weight of score by ML model, the label is the metric that can qualitatively characterize you objective.
  - Besides the value score from user action, other factors can to be considered includes:
    - User affinity: relation ship (similarity) between user and content.
    - Time decay: we don't want to recommend outdated news to user.
    - Ref: [Wiki-EdgeRank](https://en.wikipedia.org/wiki/EdgeRank)



### Other Knowledge

- Hashtag: '#' usually used in post/news to denote "topic"

