
While most of us know that the weather becomes hotter as we approach towards equator, however if we are asked to prove it we need to get some evidence from the available data. 

Lets create a Python script to visualize the weather of 500+ cities across the world of varying distance from the equator.Our objective is to build following scatter plots to determine any relationship with respect to distance from equator. 

•	Temperature (F) vs. Latitude

•	Humidity (%) vs. Latitude

•	Cloudiness (%) vs. Latitude

•	Wind Speed (mph) vs. Latitude

Our approach:

•	Randomly select at least 500 unique (non-repeat) cities based on latitude and longitude. this is quite a simple task by generating a random list of latitude and longitudes (and zipping together for brevity).

•	Find a city nearest to each lat/long co-ordinates in our randomly generated data. This task can be accomplished by following 2 options: (a) using Citipy library available at PyPy, which basically uses a database of 46K cities to generate a KDTree object (b) create our own code to carry out the task of citipy library. For reasons explained further below, it was decided to use our own code.

•	Perform a weather check on each of the cities using a series of successive API calls. Below code access openweathermap.org API's (If you wish to run this code, please generate an API key and paste in 'api_keys.py' file). Results will be saved in a pandas dataframe.

•	Plot desired scatter plots using matplotlib 

We will include a print log of each city as it's being processed with the city number and city name. We will save both a CSV of all data retrieved and png images for each scatter plot. We will look for three observable trends based on the data.

Why not to use citipy library.. and what other approaches we can take?

(1) citipy implementation uses a maxmind database of approx 46K cities to create a KDtree object for searching city nearest to a lat/long co-ordinate. openweathermap.org provides weather data for 200K+ cities and hence using a 46K cities database would not give enough chance to our code to operate with a larger database to extract more random samples. Also citipy does not take advantage of better precision using cartesian 3D co-ordinates to calculate distances on the earth's surface.

(2) a larger database of maxmind available at kaggle, covers approx 3.1 million cities, but would not be much useful to us as the number of cities covered by openweathermap.org are only 200K+ . So while we can expect to find a city much nearer to our random lat/long, there are less than 10% chances of our API call to openweathermap.org getting successful result. openweathermap API guidelines indicate 60 API calls per minute for free accounts and if we take this into consideration, failed API calls will substantially add to our execution time. Hence In order to limit our API calls, we would like a high success rate.

(3) Best approach is to have a cities database which approximately matches the openweathermap.org database, so as to get more successful API call results. While looking at the openweathermap.org API documentation, i found a json file containing list of city names/country and lat/long for which openweathermap.org provides weather data. This perfectly serves our purpose. Our code imports this json file, converts to a dataframe and creates a KDtree object to search for nearest neighbour. We also converted lat/long co-ordinates to a cartesian equivalent (3D coordinates with 0,0,0 at center of earth) to find true nearest neighbour. Also the code tracks API calls and wait for a minute after every 60 API calls to adhere to API guidance.

(4) Admittedly using our approach is a overkill for this particular problem wherein accuracy and API call success rate may not be much relevant, however i still decided to implement a more precise/optimized approach.

