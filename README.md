# Broad MBTA API 

## How to run?

Open a Jupyter Notebook Session and run each code block. Outputs and inputs will be displayed right under the relevant code blocks. The only package required is request which Jupyter Notebooks (that are installed through the anaconda package) tend to already have. 

## Why Jupyter Notebook approach? 

I took this approach as notebooks are easy to share and I can segment out different code blocks. Which lets me test my filtering or pathfinding methods without repeatedly calling the MBTA APIs. Since I'm never changing the data gathered from the API, I can reuse the data in subsequent methods without recalling the APIs to refresh the data. This notebook is very easy to share and to run. 

### Question 1

Write a program that retrieves data representing all, what we'll call "subway" routes: "Light Rail" (type 0) and
“Heavy Rail” (type 1). The program should list their “long names” on the console.
Partial example of long name output: Red Line, Blue Line, Orange Line...
There are two ways to filter results for subway-only routes. Think about the two options below and choose:
  1. Download all results from https://api-v3.mbta.com/routes then filter locally
  2. Rely on the server API (i.e., https://api-v3.mbta.com/routes?filter[type]=0,1) to filter before results
are received. Please document your decision and your reasons for it.

I went with a server-side filtering approach as I did only needed specific data from the API. This saves time for me as I can trust the server to return the filter data correctly without having to write a function to filter the entire dataset myself which also reduces the size of the dataset. 

#### Relevant Functions:
>- def getRailResponseJSON() -> Sends get request to MBTA Routes API, converts the data into JSON, returns the JSON.
>- def getLongName() -> Takes JSON data and returns list of all Long Names.
>- #Question 1 Answer Code block -> calls above functions and prints output.

### Question 2

Extend your program so it displays the following additional information.
1. The name of the subway route with the most stops as well as a count of its stops.
2. The name of the subway route with the fewest stops as well as a count of its stops.
3. A list of the stops that connect two or more subway routes along with the relevant route names for each of those stops.

I downloaded the relevant data from the MBTA Stop API based on the information Question 1 provided. The third part of this question enables me to start setting up relevant data structures that I can use for Question 3.

#### Relevant Functions:
>- def getRouteStops() -> Takes JSON Rail data, extracts a list of ids to query the MBTA Stops API. Converts that data into JSON and adds the stop names to a master set of stop names and a dictionary of routes to their respective stops.
>- def getMaxStops() -> returns the dictionary key that has the most stops.
>- def getMinStops() -> returns the dictionary key that has the least stops.
>- def getStopRoutesAndIntersections() -> Finds every stop that connects multiple routes, creates a dictionary of routes to intersecting stops.
>- Question 2 Answer Code block -> Calls above functions and prints output.

### Question 3

Question 3
Extend your program again such that the user can provide any two stops on the subway routes you listed for
question 1.

1. Davis to Kendall/MIT -> Red Line
2. Ashmont to Arlington -> Red Line, Green Line B

#### Relevant Functions:
>- def findRoute() -> function to find the first route between two stops. First it checks if the 2nd stop is on the same route as the first stop. If not, the function checks the list of routes that the first route is connected to. Function then updates the first stop to the next available intersection stop. Function recurses through list of available intersecting stops on every route till it finds a connection to the end point. traversedRoutes list and traversedIntersectionStops list are used to prevent backtracking. Returns traversedRoutes which represents a valid path from the very first point to the last point.
>- def findShortestRoute() -> function to gather a list of valid paths between two stops. Logic is similar to the findRoute() function. traversedRoutes and traversedIntersectionStops are strings in this function to prevent unexpected updates to either variable through the recursion. Once all valid paths are found, the function checks how many route delimiter characters are in each character in the list of strings. This way if any route has a longer name, it will still be processed correctly.
>- def inputStops() -> function that asks user for two stop inputs. Validates that stop names are correct. The input is case-sensitive. 'quit' is the only way to escape out of the function.
>- Question 3 Answer Code block -> calls above functions and outputs the results.