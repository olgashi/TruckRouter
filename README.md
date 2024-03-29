# Truck Router

<p align="center">
  <img width="130" alt="truckrouter logo" src="https://user-images.githubusercontent.com/41551585/184756065-9dbc34ef-f79c-4d30-ae54-62206188b5bb.png">
</p>


### CLI application that calculates minimum distance travelled by University Trucks using greedy algorithm.

Given an input of requirements (as a spreadsheet) routes trucks to their destinations in a way that minimizes the total number of miles travelled.

## Overview
Basic greedy algorithm (located in core_algorithm.py) was used for determining the distance between two addresses. A path between any two addresses is stored in a graph data structure.
Since we know that we have at least one path for any two addresses, it is reasonable to say the average time ‘look up’ for a single path is O(1). Lookup for all packages is O(n), where n is a total number of packages. The resulting total distance travelled for all trucks/deliveries is 70.10 miles.

### Data Structures - explanation
*Graph data structure* (graph) is used to maintain and access information about shipping destinations along with distances between them. Each vertex in the graph represents a single location. 

Graph has a list of all edges along with edges connected to them (list of key value pairs, where key is a given edge and value is a list of all vertices connected to it).

Additionally, there is a list on the graph data structure itself; it contains information about distances between all edges, and is represented by key value pairs where key is a set of two locations and value is the distance between them. In this solution there is only one connecting edge for any two vertices (two if we count both directions).

*Hash Table data structure* is used to represent package data. When populating table with package objects, the solution hashes the package id of a specific package and maps it to a Package object containing all provided details for that object. ‘Maps’, means that it associates a hashed key value of the package with the package object itself. Since each package has a unique id, there are no collisions. 

Each Package object is an instance of a Package class and it contains all required information for that specific package.

Hash Table data structure data can be manipulated and accessed via insert, search and remove methods, each having O(1) run time complexity.

### Core Algorithm Overview
● Manually separate packages into 3 piles (taking into account any restrictions such as special notes and package delivery deadlines), one is designated for the first truck and two are designated for the second truck (two separate trips). Third truck is not used.

● The first truck starts making deliveries and leaves the HUB at 8:00 am, the second truck goes out at 9:05 am (as stated in the requirements). Once the second truck is finished, it comes back to the HUB to get the remaining packages and then delivers those as well.

● Repeat for all three piles until all packages are delivered:
For each package on a given truck at each trip:
- Look up an address pair which represents a key, in that key the first element is the current location and second is a shipping address of the package. Look up is performed in a list of key-value pairs where key is a pair and the value is the distance between the two locations.
Look up has run time complexity of O(1).
- If the pair exists, then add corresponding distance to the running total; update package status and actual delivery time.

### Loading and parsing package and location data
Location data and package data is loaded from csv files. There are two separate methods that load data into a corresponding data structure, read_packages_csv_file and read_locations_csv_file (both located in utils/input_data_utils.py).

In case the solution has to be scaled (either number of packages or number of locations), no additional modifications to the code are needed, it will support any number of packages or locations. The only requirement is that csv files, that contain location and package data, are kept in the same format.

### Maintainability
Overall, code was written with maintainability in mind. The program is organized into separate modules for clarity and easy updates.

**Data structure to store the package data**
Each package and its corresponding details are stored in a Hash Table data structure. We know that each package is unique and there are no collisions, so a single lookup (search) is done with an average-case runtime of O(1).

Populating a Hash Table with packages takes O(n), where n is the number of packages. Main advantage of keeping package data in a Hash Table is the speed of the lookup (O(1))

**Interface for the insert and look-up functions to view the status of any package at any time**
Interface is provided and ran via main.py. Once main.py is run, the delivery simulation is performed and
the user is able to lookup the status of the package at any time during the day delivery takes place (using data generated by the simulation).

Additionally, there is a functionality that lets the user see the total distance all trucks traveled and information for a specific package, including actual delivery time and status.

Once the program is started, the user sees several options for commands they can use: id, time, distance, clear, exit.

### Justification of the choice of the core algorithm
Core algorithm of the solution has two main advantages: Ease of implementation and Speed

Once the package data is added to a hash table and location data is set up as a graph data structure, the algorithm for determining the distance between two points is a simple key-value lookup that takes constant time. For all packages its run time complexity is O(n).

### Justification of data structure choice
Graph data structure is used to store data about shipping destinations along with distances between them. Each location (node) is represented by a vertex and each vertex is connected to every other vertex by one edge. There is a list on the graph data structure, that contains information about distances between all edges, and is represented by key value pairs where key is a set of two locations and value is the distance between them. The solution has one connecting edge for any two vertices (two if we count both directions). Using graph data structure allows for easy look up of any connecting edge and a way to know which vertices any given vertex is connected to (especially imported if the solution will need to be updated in the future to accommodate a different core algorithm).

Hash Table data structure is used to represent package data. It hashes the package id of a specific package and associates a hashed key value of the package with the package object itself. Since each package has a unique id, there are no collisions. It allows for easy search, insert or delete, each having O(1) run time complexity.

No changes to the code needed if more than 40 packages are to be delivered. The only requirement is that at least 1 path for any two locations exists.

Two other data structures that can meet the same criteria and requirements:
Packages can be stored in a queue, which can be represented as a Linked-list data structure. The main drawback is that lookup of a package will take O(n) time in the worst case scenario (n is number of packages), whereas with a hash table it takes O(1).

In order for this solution to be optimal, we need to determine the optimal destination as we are building the queue, so that the order of packages in a queue represents the path from the HUB to the last package’s shipping address.

Packages can also be stored in a basic list or array. The main disadvantage is that lookup takes O(n) in the worst case scenario.
