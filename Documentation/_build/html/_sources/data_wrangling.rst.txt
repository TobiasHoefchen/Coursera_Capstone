Approach to solve the problem
=============================

As I pinpointed in the Introduction, i want to solve this problem by following these steps:

1. Collect the data
2. Get an insight on the data and process them
3. Use k-Means clustering to cluster the data
4. Visualizing the results
   

------------

1. Collecting the data
----------------------
The most important part of every data science project is to get good data.
To fullfill this requirement, I chose the following data sources:

- `List of neighborhoods in Toronto <https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M>`_
- :download:`Geospatial data <../Geospatial_Coordinates.csv>` (Latitude, Longitude) of every postal code in Toronto
- Venue data of every neighborhood
- Venue categories

In order to get this data I used the Python Package *BeautifulSoup* to scrape
the data from the webpages. The venue data is provided by the `Foursquare <https://foursquare.com/>`_
API. I decided to use the API by using the Python Package *requests* and search for categories of venues. 
Therefore, I used this `list of categories <https://developer.foursquare.com/docs/build-with-foursquare/categories/>`_ to 
create the API-requests. 


2. Get an insight on the data and process them
----------------------------------------------
I gained an insight on the data by visualizing them on a geospatial map (:download:`Boroughs of Toronto <boroughs.html>`) using *folium*. To do so,
I had to integrate the geospatial data from the .csv file.


After collecting the data I processed them by combining the boroughs from wikipedia, the geospatial data from the .csv file
and the venue data that I got from the API by making categorial requests. After some data wrangling I reveiced the following dataframe: 

.. figure:: ./_plots/processed_data_head.png
    :scale: 60%

    Processed dataframe (only 4/103 rows displayed)

Each number represents the quantity of venues of one category within a radius of 500 m of the borough center (provided by the coordinates).



3. Use k-Means clustering to cluster the data
---------------------------------------------
After processing the data, I applied k-Means clustering from *sklearn*. The algorithm simply needs the processed, a random state and the number of clusters.
I set the random state to 0 (basically any integer would have done the job) and used the `Elbow Technique <https://en.wikipedia.org/wiki/Elbow_method_(clustering)>`_
to determine the number of required clusters. The figure below displays the minkowski distortion. It was calculated by this formula:

.. code-block:: python

    distortions = []
    K = range(1, 15)
    for k in K:
        # run k-means clustering
        kmeanModel = KMeans(n_clusters=k, random_state=0).fit(X)
        
        # calculate distance to each cluster_center and sum over distances to closest cluster center
        distortions.append(sum(np.min(cdist(X, kmeanModel.cluster_centers_, 'minkowski'), axis=1)) / X.shape[0])


.. figure:: ./_plots/elbow_technique.png
    :scale: 100%

    Plotting minkowski distortion over number of clusters

I chose 4 as the number of clusters because any larger number of clusters only decreases the distortion by little.

Now I was able to execute the k-Means algorithm on the data. 


4. Visualizing the results
--------------------------
To visualize the results I decided to renew the geospatial visualization while drawing each cluster in a different color. 
The results can be seen :download:`here <boroughs_clustered.html>`. 
In addition I created a piechart for each cluster to display the major categorial components of each cluster. 
I chose one color per category and sticked to it to make a comparism of the different clusters easier. 

.. figure:: ./_plots/piechart_cluster0.png

    Piechart Cluster 0


.. figure:: ./_plots/piechart_cluster1.png

    Piechart Cluster 1


.. figure:: ./_plots/piechart_cluster2.png

    Piechart Cluster 2


.. figure:: ./_plots/piechart_cluster3.png

    Piechart Cluster 3

The following table sums up the results:

.. list-table:: Cluster overview
    :widths: 15, 25, 50
    :header-rows: 1

    * - Cluster
      - Suitable for: 
      - Comment: 
    * - Cluster 0
      - Smaller households
      - Close to city center, Multiple Outdoor venues
    * - Cluster 1
      - Larger households
      - People who want to live close to work, good Transport
    * - Cluster 2
      - Younger persons
      - In the city center, good Nightlife
    * - Cluster 3
      - Larger households
      - People who want to live close to work, good Outdoor venues


5. Conclusion
-------------
If you already live in Toronto you now have the option to take a look on the map of clustered data
and look for your current neighborhood. After that you can decide wether you want to move to 
a similar neighborhood or you want to avoid this kind of neighborhood.
If you want to move to Toronto you now can use these results to pick neighborhoods that 
fit your venue needs the most.