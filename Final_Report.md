---


---

<h1 id="p-aligncenter-real-estate-prices--venues-data-analysis-of-london-p"><p align="center"> Real Estate Prices &amp; Venues Data Analysis of London </p></h1>
<h3 id="p-aligncenter-author-christos-kaparakisp"><p align="center"> Author: Christos Kaparakis</p></h3>
<h2 id="introduction">1. Introduction</h2>
<p align="justify">Recently, Machine Learning (ML) algorithms are widely used in the study of data instead of traditional statistics. ML algorithms bring many advantages because they can offer solutions to problems related to big amounts of data and set fewer constraints than traditional statistics. In particular, unsupervised learning algorithms are used to find patterns in data in terms of similarity between samples. Depending on the pattern within the data, different algorithms are used. </p>
<p align="justify"> London is a city with a high population and population density. As from a Real Estate investor point of view we want to invest in such places where the housing prices are low and the facilities (shops, restaurants, parks, hotels, etc.) and social venues are nearby. Keeping above things in mind it is very difficult for an individual to find such place in such big city and gather this much information. When we consider all these problems, we can create a map and information chart where the real estate index is placed on London and each district is clustered according to the venue density.</p>
<p align="justify">Foursquare is a website where people comment and rank food sites, coffee sites, malls and parks. Foursquare location data along with a clustering algorithm can suggest a neighborhood in order to help a person choose a place to live. For instance, let's think of a person that wants to buy a house in London. We can gather the average prices of houses in different neighborhoods of London, and combined with our knowledge from Foursquare we can suggest a neighborhood to focus on. The neighborhood that will be suggested, will not be a random suggestion, but instead will be a place for his pleasure. Thus, previous data from London will be used to predict a good living neighborhood for him.</p>
<h2 id="data">2. Data</h2>
<h3 id="data-collection">2.1.  Data Collection</h3>
<p align="justify">The data that will be used to adress the avobe problem are: </p>
<ul>
<li>List of areas of London was found with its boroughs and postcodes from Wikipedia. (<a href="https://en.wikipedia.org/wiki/List_of_areas_of_London">https://en.wikipedia.org/wiki/List_of_areas_of_London</a>)</li>
<li>For housing prices, we will scrap a website where latest London house prices were available with the postal codes.  (<a href="https://propertydata.co.uk/cities/london">https://propertydata.co.uk/cities/london</a>)</li>
<li>Forsquare API will be used to get the most common venues of given Borough of London.</li>
<li>For chloropleth maps we will use .geojson file of London. (<a href="https://joshuaboyd1.carto.com/tables/london_boroughs_proper/public">https://joshuaboyd1.carto.com/tables/london_boroughs_proper/public</a>)</li>
</ul>
<p align="justify">The data downloaded are the boroughs located in London. Moreover, their specific coordinates are merged. A Foursquare API request is sent in order to aqquire the surrounding venues within a radius of 500m. The data is formated using one hot encoding with the categories of each venue. Then, the venues are grouped by boroughs computing the mean of each feature.</p>
<p align="justify">The similarities will be determined based on the frequency of the categories found in the neighborhoods. These similarities found are a strong indicator for a user and can help him to decide whether to move in a particular neighborhood near the center of London or not. </p>
<h3 id="data-preprocessing">2.1.  Data Preprocessing</h3>
<p align="center">
  <img src="https://github.com/ckaparakis/Coursera_Capstone/blob/master/pictures/uncleanwiki.jpeg" title="hover text" width="350">
</p>
<p align="justify">The data scraped from Wikipedia has to be cleaned. We will drop ‘Dial Code’ and ’OS grid ref’ columns as they were of no use, remove all the hyperlinks and there are more than one Postal codes for some Locations so we are going to keep only one Postal code.</p><p>
</p><p align="justify">For the next table of average housing prices the data initially looked like this:</p><p>
</p><p align="center">
  <img src="https://github.com/ckaparakis/Coursera_Capstone/blob/master/pictures/uncleanedavgpr.png" title="hover text" width="350">
</p>
<p align="justify">First of all we will remove all null values and then get rid of unwanted columns and only kept ‘Area’ and ‘Avg price’ columns. Then ‘Avg Price’ columns contains string so we will process it to make it an integer by removing pound sign and comma.</p><p>
</p><p align="justify">After cleaning two tables we will perform inner join and merge them. Then by using geocoder library we can find the Longitudes and Latitudes of the Location and add a column of each in my dataframe.</p><p>
</p><p align="justify">We will use python folium library to visualize geographic details of London and its boroughs and create a map of London with boroughs superimposed on top. Latitude and longitude values were used to get the below visual:</p><p>
</p><p align="center">
  <img src="https://github.com/ckaparakis/Coursera_Capstone/blob/master/pictures/map.png" title="hover text" width="350">
</p>
<p align="justify">By utilizing the Foursquare API we will explore the boroughs and segment them. We set the limit as 100 venues and the radius as 500 meter for each borough from their given latitude and longitude information. Here is the first five rows of the list **london_venues** with the columns **Neighborhood**, **Neighborhood Latitude** and **Neighborhood Longitude** coming from our earlier dataset and **name**, **category**, **latitude** and **longitude** from Foursquare API.</p><p>
</p><p align="justify">By getting the number of venues for every Neighborhood, we have the following dataset:</p><p>
</p><p align="center">
  <img src="https://github.com/ckaparakis/Coursera_Capstone/blob/master/pictures/neighvencount.png" title="hover text" width="350">
</p>
<p align="justify">And finally, here is the dataset with the top 10 venues for every neighborhood:</p><p>
</p><p align="center">
  <img src="https://github.com/ckaparakis/Coursera_Capstone/blob/master/pictures/top10ven.png" title="hover text" width="350">
</p>
<h2 id="methodology">3. Methodology</h2>
<h3 id="feature-extraction">3.1. Feature Extraction</h3>
<p align="justify">For feature extraction One Hot Encoding is used in terms of categories. Therefore, each feature is a category that belongs to a venue. Each feature becomes binary, this means that 1 means this category is found in the venue and 0 means the opposite. Then, all the venues are grouped by the boroughs, computing at the same time the mean. This will give us a venue for each row and each column will contain the frequency of occurrence of that particular category.</p>
<h3 id="unsupervised-learning">3.2. Unsupervised Learning</h3>
<p align="justify">The purpose of doing unsupervised learning in this case is to find similarities between boroughs. For this reason we will implement a clustering algorithm, specifically a K-Means algorithm due to its simplicity and its approach in finding similarities and patterns. </p>
<ul>
<li><strong>K-Means:</strong></li>
</ul>
<p align="justify">Because we have common venue categories in boroughs we can easily use a K-Means algorithm in order to find similarities between the boroughs.
</p><p>This algorithm creates clusters within the data and the main objective function is to minimize the data dispersion for each cluster. Thus, each group represents a set of data with a pattern inside the muldimensional features. </p>
<p>In the following figure there is an example of how a K-Means algorithm works.</p>
<p align="center">
  <img src="https://cdn-images-1.medium.com/max/1600/1*tWaaZX75oumVwBMcKN-eHA.png" title="hover text" width="350">
</p>
<p align="justify">In order for this to work we have to input the number of the clusters.  For this reason, the elbow method is implemented. A graph comparing MSE vs number of clusters is created as shown below:
</p><p align="center">
  <img src="https://github.com/ckaparakis/Coursera_Capstone/blob/master/pictures/elbow.png" title="hover text" width="350">
</p>
<p align="justify">As it is expected, the MSE decreases over the number of clusters. The elbow method here is implemented in order to select the appropriate number of groups. In this case, it is possible to see that the elbow is found around 5. The MSE found below this number shows little changes rather than big ones. Finally, once the number of clusters is fixed, the clustering algorithm is repeated through samples and each borough is labeled according to the clusters found.</p>
<h2 id="results">4. Results</h2>
<p align="justify"> By choosing k=5 and running the algorithm we now have cluster labels for every borough. After examining each cluster we can see every one of them is different and usually tend to have different popular venues.  We can label them as:</p><p>
</p><ol>
<li>Pubs and stores</li>
<li>Hotels and Social Venues</li>
<li>Grocery Stores</li>
<li>Restaurants and Cafes</li>
<li>Parks and Outdoor activities</li>
</ol>
<p align="justify"> Also, we can visualize the average house prices for each cluster:</p><p>
</p><p align="center">
  <img src="https://github.com/ckaparakis/Coursera_Capstone/blob/master/pictures/pricebins.png" title="hover text" width="350">
</p>
<p align="justify"> We can define the above price ranges as:</p><p>
</p><ul>
<li>Low level 1</li>
<li>ow level 2</li>
<li>Average level</li>
<li>High level 1</li>
<li>High level 2</li>
</ul>
<p align="justify">For visualization purposes, the geographical data is plotted. Each color represents the cluster for which that neighborhood belongs. This image is shown below.</p>
<p align="center">
  <img src="https://github.com/ckaparakis/Coursera_Capstone/blob/master/pictures/map1.png" title="hover text" width="350">
</p>
<p align="center">
<img src="https://github.com/ckaparakis/Coursera_Capstone/blob/master/pictures/map2.png" title="hover text" width="350">
</p>
<p align="justify">It is possible to see which neighborhoods within London have very high house prices, for example Westminster and Kensington. We can clearly see that the house prices in the downtown and with Hotels and Social venues nearby are very high while in the suburbs and the neighborhoods away from the city center have low prices but the facilities are also good.</p>
<h2 id="discussion">5. Discussion</h2>
<p align="justify">It is worth noting that this work is useful only for those who live in or work to move to the city of London.</p>
<p align="justify">Moreover, it is no news that London is a big and vibrant city, with interesting places to have nearby in every one of its boroughs. That is a big plus for every future house buyer, because it gives them the opportunity to choose from a lot of different price ranges according to their budget.</p>
<h2 id="conclusion">6. Conclusion</h2>
<p align="justify">In this project a a city's neighborhoods have been clustered in order to help future buyers to target their research for a new house, in places where it is not extremely expensive but also has a lot of social venues. This involves the neighborhoods London. The data is downloaded and the venues around the neighborhoods is adquired using the Foursquare API. One Hot Encoding is used for converting the categories of the venues into a feature matrix. Then, all venues are grouped by neighborhoods and at the same time the mean is calculated. Hence, the resulting features used are the frequency of occurrence from each category in a neighborhood.</p>
<p align="justify">The K-Means clustering algorithm is used for finding similiraties between all the neighborhoods listed in the feature matrix. The elbow method is used for selecting the appropriate number of clusters. The K selected is 5, resulting in 5 different clusters of neighborhoods. Their names and represented colours on the previous map are the following:</p>
<p><strong>Clusters</strong></p>
<ul>
<li>I: Red - Neighborhoods that have around pubs and stores.</li>
<li>II: Purple - Neighborhoods that have around hotels and lots of different social venues.</li>
<li>III: Blue - Neighborhoods that have around grocery stores.</li>
<li>IV: Green - Neighborhood that have around restaurants and cafes</li>
<li>V: Orange - Neighborhoods that have around  parks and things to do outdoors.</li>
</ul>

