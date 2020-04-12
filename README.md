# Team 3 Client Project: Misinformation on Social Media During Disasters
### Group Members: Viviana Roseth, Maninder Virk, and Beth Hamilton

## Problem Statement: 

While social media increasingly becomes the first source of information about disasters, this information is not always correct. Posts can be misleading or mistaken. On rarer occasion, users intentionally spread misinformation. How can emergency management agencies identify incorrect information before, during, and after disasters?

## Executive Summary 

To start our project, we decided to pick one recent disaster as our focus. We chose a domestic disaster so that our tweets would be primarily in English. We focused on the 2019 wildfires in California. While the 2019 wildfire season was not as active as 2018, according to the Insurance Information Institute, from January 1st until November 22nd, there were more than 46,000 wildfires. Significant fires include the Kincade Fire in Sonoma County, which started on October 23rd and burned an area more than twice the size of San Francisco.[1]  

Subsequently, we decided to look at the extensive research that has already been done on this issue. According to a report by the Department of Homeland Security, misinformation can take many different forms: incorrect, insufficient, opportunistic, outdated.[2] A 2018 MIT study highlighted that false information is more likely to be spread than accurate information. The study showed that false information is more novel, and is 70% more likely to be retweeted by bots or humans than accurate information.[3] A 2016 Knight Foundation study showed that there are four different types of misinformation: Trump Support, Hard Conservative, Conservative, and Other right-leaning.[4] We found these categories to be relevent to our wildfire data since FEMA funding became a political issue during the 2019 season. 

The research about disaster information on social media has mostly focused on Twitter. According to a 2016 report by the Middle East Institute, Twitter is a popular platform for disaster related information, and in the aftermath of Hurricane Sandy in 2012, more than 20 million tweets were released. Many of the tweets contained valuable and actionable information, but the informative tweets were often overwhelmed by scam messages and misinformation.[5] 

From there, we used the Python library "GetOldTweets3" to collect tweets with search terms related to the California wildfires. We collected over 28,000 tweets from October and November 2019, during the worst part of this year's wildfire season in California. 

Since Hurricane Sandy in 2012, the proliferation of bots has only become even more intense. Therefore, we decided to focus on bots as one aspect of our project. According to a 2018 study by computer science researchers Srijan Kumar (Stanford) and Neil Shah (Carnegie Mellon), bots are dangerous because of the scale on which they can operate. “Bots operate on a large scale for two purposes: first, to spread the same content, e.g., by retweeting, to a large audience, and second, by following each other to increase the social status of the accounts and apparent trustworthiness of information.”[3] It is not only bots that can spread misinformation, and of course humans are responsible for spreading misinformation and conspiracy theories as well. However, bots are especially dangerous because they can spread this information so quickly and efficiently. 

We decided to use a Python package called to Botometer to help with our analysis. The Botometer is a tool developed by researchers at the Indiana University Network Science Institute (IUNI) and the Center for Complex Networks and Systems Research (CNetS). Scores are displayed as percentages. These percentages are the probability that a twitter account is human or bot; the closer to 0 a score is the higher the likelihood it is a human and the closer to 1 a score is the higher the likelihood it is a bot. According to the Botometer’s website, the “probability calculation uses Bayes’ theorem to take into account an estimate of the overall prevalence of bots, so as to balance false positives with false negatives”.[4] For more information, See Maninder's blog post about the Botometer here: https://medium.com/@m.virk1/botometer-eac76a270516. 

Our first modeling approach was to use Botometer information to train some NLP classification models. We classified any user with a rating over 50% chance of automation as a likely bot, and used that as our predicted variable. We used the usernames, text of the tweets, hashtags and mentions as our predictor variables. We fit classification models using Naive Bayes, Decision Tree, Bagging Classifer, Random Forest, Extra Trees, and AdaBoost with TFIDF Vectorizer to give a higher weight to more unique or important words in the tweets. Because likely bots are only 6% of our dataset, we used bootstrapping to balance our classes. We got over 99% accuracy with the Extra Trees model in predicting the likely bot ratings based on our botometer data. 

Our second modeling approach was to use unsupervised models for K-means and DBSCAN clustering using TFIDF Vectorizer, different n-gram combinations, and stop words including wildfire-related vocabulary common to all tweets.The clustering helped us to see relationships between words and clusters possibly associated with misinformation. 

## Data Dictionary 

|Feature|Type|Description|
|---|---|---|
|bot_rating|float|A rating given by the Botometer, between 0 and 1, representing the percent chance of automation|
|date|object|Date and time stamp for tweet sent in Coordinated Universal Time| 
|hashtags|object|A word or phrase preceded by a hash sign (#), used to identify messages on a specific topic.|
|id|integer|ID Number assigned to tweet|
|likely_bot|boolean|A rating given for classification modeling, 0(false) for less than .5 chance of being a bot, 1(true) for more than .5 chance of being a bot.|
|mentions|object|Twitter users mentioned by author of tweet|
|text|object|Text of tweet, excluding hashtags and mentions|
|times_favorited|integer|Number of times a tweet has been marked as a favorite by another Twitter user|
|times_retweeted|integer|Number of times a tweet has been shared by another Twitter user|
|tweet_to|object|Twitter account to which a tweet is directed|
|username|object|Name/handle of Twitter user authoring tweet|
|words|object|A column combining text columns for tweet_to, mentions, hashtags, and text|

## Areas for Further Research

- One area for further research is Social Network Analysis (SNA), a method to see relationships between Twitter uses and bots tweeting out misinformation. I found a blog post about this methodology here on Towards Data Science: https://towardsdatascience.com/generating-twitter-ego-networks-detecting-ego-communities-93897883d255. This would be worth investigating if there was more time available, especially since research shows that bots follow each other to proliferate misinformation. 

- Other clustering methods

## Conclusions

- The Botometer was a useful tool, but it moves extemely slowly. Because Twitter has a limit of 180 requests per 15 minute window for this kind of information, it can take many hours to process a batch of thousands of tweets. However, the classification models can be trained on the botometer information and identify possible bots to a reasonably high degree of accuracy. 

- Although machine learning methods including supervised and unsupervised learning can be useful at identifying misinformation and bots in tweets, this is a problem that needs some human analysis as well. As stated in the FAQs on the Botometer site, bot detection is hard and there are some kinds of bot-like behaviors that are spotted more easily by humans. Machine learning is a tool to use, but it should be used side-by-side with human analysis. 

- Not all bots are bad, and some humans are worse. When looking at many of the tweets tweeted by bots, much of the information put out seemed to be fairly inocuous. On the other hand, when we did an experiment and pulled tweets related to wildfire related conspiracies (e.g. the fire was started by lasers, the Illuminati, or the 'deep state'), we found that the percentage of likely bots in those tweets was only about 3% compared to 6% for our overall dataset. 

## References 

1. https://www.iii.org/fact-statistic/facts-statistics-wildfires

2. https://www.dhs.gov/sites/default/files/publications/SMWG_Countering-False-Info-Social-Media-Disasters-Emergencies_Mar2018-508.pdf

3. https://www.mei.edu/publications/information-filtering-social-media-during-disasters

4. https://knightfoundation.org/features/misinfo/

5. https://arxiv.org/pdf/1804.08559.pdf

5. https://www.dhs.gov/sites/default/files/publications/SMWG_Countering-False-Info-Social-Media-Disasters-Emergencies_Mar2018-508.pdf

6. https://botometer.iuni.iu.edu/#!/faq#what-is-cap
