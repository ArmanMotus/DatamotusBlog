---
Description: A walkability score is a number from 1 to 100, with 1 being the worst and 100 being the best. 0 to 24 means almost all...
date: "2023-10-09"
featured: true

tags:

- Walkability Score
- 15 Minutes City
- Walk Score
- Pedestrian City
- Pedestrian Paradise
- Data Analysis
- Geo Mapping
- Walkability Assessment
- Yerevan
- Armenia



title: Measuring Pedestrian-Friendliness 'Walkability Scores for Yerevan'


---


# Data Collection

A walkability score is a number from 1 to 100, with 1 being the worst and 100 being the best. 0 to 24 means almost all errands need a car, 25 to 49 means most errands need a car, 50 to 69 means you can walk to some amenities, 70 to 89 shows you can do most things by walking, and 90 to 100 indicates you don’t need a [car](https://www.paragonliving.com/blog/what-is-a-walkability-score).

To calculate the walk score for given city, at first we need  city map (nodes and edges) and necessary amenities in that city.
Using Open Streep Map with python API osmnx, we can download graph for Yerevan, which is consists of nodes and edges, where nodes are the main data type of OpenStreetMap represented with Latitude and Longitude as a geograpical coordinates and unique ID within OpenStreeMap data nodes data base, and edges are way connecting nodes together.


The plot below is Yerevan visualized with nodes - painted blue - and edges connecting nodes - painted orange. The total number of nodes and edges are **29265** and **81548** respectively.


![png](../Measuring-Pedestrian-Friendliness-Walkability-Scores-for-Yerevan/output_8_0.png)
    


Nodes and edges we have. Now we need amenities, more clearely coordinates of amenities. I used two ways to get amenities. First one is simple: I used OpenStreetMap API to get amenities with speicifed tags for Yeravan, but that is not enough, since for Yerevan OSM provide only 2000 amenities, which is not actual number. So, that is why I tried to obtain more amenities and I used Google maps API for python.

List of tags from both OpenStreetMap and Goope map is wide (total 8617 poin in the map), but since I am using [methodology]( http://pubs.cedeus.cl/omeka/files/original/b6fa690993d59007784a7a26804d42be.pdf) from [walkscore.com](https://www.walkscore.com/) website, where for general case are chosen nine tags **(grocery,resturants,shopping,coffee,banks,parks,schools,books,entertainment)**. I also chose them to be able compare my result with general cases, and in my total list of amenities, there are  **5275** amenities that correspond to this list.

![png](../Measuring-Pedestrian-Friendliness-Walkability-Scores-for-Yerevan/output_20_0.png)
    


The plot above shows more amenities in the city center and northern areas, which is obvois for Yerevan. The next step is splitting Yerevan into hexagonals, for which the final calculation will be done.

    
![png](../Measuring-Pedestrian-Friendliness-Walkability-Scores-for-Yerevan/output_33_0.png)
    

# Distance Decay Function

The distance decay function determines what percentage of a full score a category
will receive based on the distance between the address being examined, which we
refer to as the origin, and an amenity’s location. We use a polynomial distance
decay function that gives full score or near full score for amenities that are
within .25 (402 meters) miles of the origin. After this, scores decrease with distance smoothly.
At a distance of one mile, amenities receive only about 12% of the score they
would have had if they were right next to the origin. After one mile, scores
decrease less quickly with greater distance, until they reach 1.5 (2414 meter) miles, after which
they do not count towards the final score. ([Countinue from following article](http://pubs.cedeus.cl/omeka/files/original/b6fa690993d59007784a7a26804d42be.pdf))


After calculations, we have the walkability score (0-100), average walking time, and density of amenities over Yerevan, where
- walkability score is a value between 0-100 with these levels
    - 1-24 - Car-Dependent: almost all errands require a car
    - 25-49 - Car-Dependent: most errands require a car
    - 50-69 - Somewhat Walkable: some errands can be accomplished on foot
    - 70-89 - Very Walkable: most errands can be accomplished on foot
    - 90-100 - Walker’s Paradise: daily errands do not require a car ([soure](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6427661/#:~:text=A%20Walk%20Score%20between%200,Walkable%3A%20most%20errands%20can%20be))
- walk time: the time required to walk to an amenity within a hexagon 
- amenities amount: total number of amenities in a hexagon


All calculations have been done assuming the human normal speed is 4.5 km per hour. So, changing the speed will also change the entire image. 

The below pie chart shows walkability score and walk time level distribution among hexagons.


    
![png](../Measuring-Pedestrian-Friendliness-Walkability-Scores-for-Yerevan/output_56_0.png)
    


More than half of Yerevan has less than 50 walk score, but mean walkabitiy score is 41.

The two plots below show how these levels are distributed over Yerevan.

    
![png](../Measuring-Pedestrian-Friendliness-Walkability-Scores-for-Yerevan/output_58_0.png)
    



The best place for walkers in  Yerevan is the city center, and the most walkable areas are close to the center. The best result (for a single hexagon - 0.66 km square) is **99.7 with 2.36 minute walking time and 299 amenities around that area**, which center is **Tumanyan 17**. So, the best place for pedestrians is Tumanyan 17 with its surroundings. The walkscore 99.7 is the same as the score calculated by [walkscore.com](https://www.walkscore.com/score/21-tumanyan-st-yerevan-yerevan-armenia) for Yerevan (city center). The next three plots show how the values of two measures (walk score and walk time) and the number of amenities for each hexagon are distributed over Yerevan.



![png](../Measuring-Pedestrian-Friendliness-Walkability-Scores-for-Yerevan/output_60_1.png)
    
