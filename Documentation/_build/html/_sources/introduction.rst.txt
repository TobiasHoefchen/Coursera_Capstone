Introduction and business problem description
=============================================

Introduction
------------

If you move to another city, you always have to decide to which part of the
city you want to move. As you can see in the figure, Toronto has a lot of boroughs while 
each borough is different. 
In this project I chose Toronto because Toronto's wikipedia has a good data quality. Moreover, there was a .csv file with geodata given in one of the 
Coursera courses.

.. figure:: ./_plots/toronto_boroughs.png
    :scale: 40%

    `Source <https://en.wikipedia.org/wiki/List_of_neighbourhoods_in_Toronto>`_



----


Business problem
----------------

This project has two use cases:

 1. You already live in Toronto, but you have to or want to move to another part of the city.
    In this case you might want to move to a part that is similar to your current neighborhood.
 2. You want to move to Toronto but you aren't sure to neighborhood you want to move.
   
To answer these problems I will use data provided in the internet and data science techniques 
to gain knowledge from the data because it will be too large to analyze in a conversative way.


----


Approach to solve the business problem
--------------------------------------

Divide main problem into the following smaller problems:

    1. Collect the data
    2. Get an insight on the data and process them
    3. Use k-Means clustering to cluster the data
    4. Visualizing the results