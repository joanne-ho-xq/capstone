![London](https://ik.imgkit.net/3vlqs5axxjf/BTNE/uploadedImages/1_Sections/Traveler_Management/London%20Westminster.jpeg) 
# Capstone Project: Travel Recommender System

### Contents:
- [Problem Statement](#Problem-Statement)
- [Background](#Background) 
- [Datasets](#Datasets) 
- [Data Dictionary](#Data-Dictionary)
- [Brief Summary of Analysis](#Brief-Summary-of-Analysis)
- [Packages](#Packages)
- [Conclusions](#Conclusions)
- [Limitations & Further Works](#Limitations-&-Further-Works)

### Problem Statement
With the rise in the demand for international travel, especially due to the pent-up demand from the ease of Covid travel restrictions, Singapore travellers are spoilt for choice and to decide on which places of interest to visit during the limited vacation time is no easy feat. 

Furthermore, most [tour packages](https://www.chanbrothers.com/package-tours) and [travel websites](https://www.tripadvisor.com.sg/Attractions-g186338-Activities-oa0-London_England.html) promote the typical sights and attractions instead of places where locals rate.

As such, we propose a travel recommender system to discover the hidden gems, in particular, of London.

---

### Background

According to the [top travel trends among Singapore travellers in 2023](https://www.humanresourcesonline.net/top-travel-trends-among-singapore-travellers-in-2023), London, United Kingdom is ranked one of the top 10 popular travel destinations. As such, our project will primarily focus on the hidden gems of London.

Based on a [preliminary study on Social Media Data Analytics for Tourism](https://ceur-ws.org/Vol-1748/paper-12.pdf), it supports using social media as a useful data source for touristic decision making since it can provide real-time insights of tourists' visiting patterns. Hence, we will be using an Instagram dataset to build our recommender system.

To discover the hidden gems of London, we will refer to a popular travel website, Trip Advisor, to find out the Top 50 tourist spots which will then be excluded from our Instagram dataset. According to [London Tourism Statistics](https://gowithguide.com/blog/london-tourism-statistics-2023-all-you-need-to-know-5213), the average length of stay in London for tourists is about 4.6 days. Assuming a tourist covers 5 spots for 5 days, 25 spots will be covered. As such, with the top 50 tourist spots excluded from our recommender system (an amount double of the 25 spots), it should be safe to say that our system will uncover the hidden gems for our users.

---

### Datasets
The datasets used in this recommender system project are acquired from Kaggle. It is a [2019 Instagram dataset](https://www.kaggle.com/datasets/shmalex/instagram-dataset?select=instagram_posts.csv) containing 42 million posts, 1.2 million locations and 4.5 million profiles. 

The datasets are saved under the [`instagram-dataset`](./instagram-dataset/) folder.

There are three datasets under this Kaggle site:
* [`instagram_locations.csv`](./instagram-dataset/instagram_locations.csv): this data contains about 1.2 million locations from all over the world
* [`instagram_posts.csv`](./instagram-dataset/instagram_posts.csv): this data contains posts with location information, date, number of likes and comments, captions, etc
* [`instagram_profiles.csv`](./instagram-dataset/instagram_profiles.csv): this data contains profiles of many different people and businesses

For this project, we will only make use of the first two datasets.

---

### Data Dictionary

**instagram_locations:**
|Feature|Type|Description|
|:---|:---|:---| 
|sid|integer|sequence ID|
|id|integer|Instagrams ID for that could be used on the website ex: ID=230466055 the url is https://www.instagram.com/explore/locations/230466055|
|name|string|name of location|
|street|string|street address|
|zip|string|zip code|
|city|string|name of city|
|region|string|name of region|
|cd|string|country code|
|phone|string|phone number in the format as on Instagram|
|aj_exact_city_match|boolean|Instagram's internal key|
|aj_exact_country_match|boolean|Instagram's internal key|
|blurb|string|description of the place|
|dir_city_id|integer|Instagrams internal city ID|
|dir_city_name|string|name of city|
|dir_city_slug|string|city tag|
|dir_country_id|string|country ID|
|dir_country_name|string|name of country|
|lat|float|latitude|
|lng|float|longitude|
|primary_alias_on_fb|string|name on Facebook|
|slug|string|tag|
|website|string|URL to website, may contain more than 1 URL|
|cts|string|timestamp when the location was visited|

**instagram_posts:**
|Feature|Type|Description|
|:---|:---|:---| 
|sid|integer|sequence ID|
|sid_profile|integer|sequence ID of the profile|
|post_id|string|Instagram ID of post|
|profile_id|float|Instagram ID of profile|
|location_id|float|Instagram ID of location|
|cts|string|timestamp when post was created|
|post_type|integer|1 - photo, 2 - video, 3 - multi|
|description|string|caption of post|
|numbr_likes|float|number of likes at the moment it was visited|
|number_comments|float|number of comments at the moment it was visited|

---

### Brief Summary of Analysis
Under the [`codes`](./codes/) folder for this project, the code notebooks are named in numerical order.
- 00_data_acquisition_instagram: this contains the codes to acquire the datasets from Kaggle
- 00_data_acquisition_tripadvisor: this contains the codes used to scrape the top 50 places of interest from Trip Advisor
- 01_dataset_preparation: cleaning of Instagram datasets
- 02_hidden_gems: removal of top 50 places of interest from Instagram datasets and further cleaning to get to the final list of hidden gems
- 03_sentiment_analysis: feature engineering of user ratings needed for collaborative-filtering recommender system
- 04_zero_shot: attempt to classify locations based on the types of attractions to build a hybrid recommender system
- 05_surprise_SVD: baseline model using scikit surprise library to build a recommender system using the SVD matrix factorisation algorithm and GridSearch for best metrics
- 06_surprise_NMF: alternative model using scikit surprise library to build a recommender system using the NMF matrix factorisation algorithm and GridSearch for best metrics
- 07_mf_nn: alternative model to build a recommender system based on matrix factorisation using neural network 

All the output from the above notebooks are saved under the [`output`](./codes/output/) folder.

The codes for the final selected model have been run using MLFlow to track the metrics. These codes are contained in the [`mlflow`](./mlflow/) folder.

The final selected model was deployed using Streamlit and the codes are contained in the [`streamlit`](./streamlit/) folder.

---

### Packages
The following packages were downloaded to build the recommender system:
- [opendatasets](https://pypi.org/project/opendatasets/): to download the datasets from Kaggle
- [vaderSentiment](https://pypi.org/project/vaderSentiment/): to do sentiment analysis on text data
- [scikit-surprise](https://pypi.org/project/scikit-surprise/): to build and analyse recommender systems that deal with explicit rating data

---

### Conclusions
- For this project, we aim to build a travel recommender to uncover the hidden gems of London.
- The categories of the attractions are as follows:
1. boat tours and water sports
1. pubs and nightlife
1. sights and landmarks
1. spas and wellness
1. fun and games
1. museums
1. classes and workshops
1. nature and parks
1. markets
1. neighbourhoods
- We have built a collaborative-filtering recommender system using a matrix factorisation algorithm called SVD. 
- Out of the top 100 recommendations and a threshold of 2.5 out of 3 for a positive rating, the model is able to predict with a precision@k value of 0.8031 and recall@k value of 0.7748 which are acceptable.
- The model has been deployed on [Streamlit](https://london-recsys.streamlit.app/). On that interface, users can input their Instagram ID and if it is within the dataset used in this project, a personalised recommendation is given. If not, one of the top 20 most popular locations is recommended.
- The recommendations are shuffled so users can keep getting different recommendations whenever they hit the 'submit' button.

---

### Limitations & Further Works
- Due to the constraints of this project, the locations are not fully cleaned - there are still locations which are not part of the intended ten categories of attractions present in the recommender system data.
- For users whose Instagram ID is not part of the recommender system data, their recommendations will be generic instead of personalised.
- As the zero shot package was not able to classify the locations according to the intended categories accurately, a hybrid recommender system could not be fulfilled. As such, further works can be done to look at how the locations can be classified efficiently and accurately to create a hybrid recommender system, so that new users can get more personalised recommendations as well.