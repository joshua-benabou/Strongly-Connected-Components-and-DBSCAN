## README: Strongly Connected Components and DBSCAN

In this Python project, completed in Jupyer Notebook, we implement 2 algorithms for finding strongly connected components in directed graphs (Kosaraju and Tarjan),
as well DBSCAN, a clustering method for geometrical datasets. We test the SCC-finders On Erdos-Renyi graphs. We also compare the two methods on the US flight network.

NOTE: We have included some jpegs in this zip file which are not included in the report.

## Modules required:

Beyond the standard ones: the Networkx module (display), sklearn, and pandas (for loading airport data)

## Data files:

We have tested on the following real data, which we include in the Zip file.

p2p-Gnutella08.txt
p2p-Gnutella24.txt
p2p-Gnutella31.txt
iris.txt
Wiki-Vote.txt
covid data.txt
routes.dat
airports.dat

If you would like to run the SCC on other real datasets, enter the name of your txt file at the top of code cell 4, 
and set 'skip' to the number of lines to ignore from the top while reading data. Finally, make sure there is no whitespace after the last non-empty line of your txt file.

## Graph
This class defines a digraph object, which has the following fields: V (number of vertices), E (number of edges), graph (adjency list, as a dictionary), c (number of SCC's), label (SCC labels), networkx_G (Networkx graph for display)
A Graph object on nodes is created by calling Graph(n). The nodes are enumerated 0 to n-1. We use
 
    addEdge(self,i,j)

to an an edge i->j. We compute SCC label array 'label' and the number of SCC's 'c' by calling

    findSCCs_korasaju(self)
or
    findSCCs_tarjan(self)

These methods give the same SCC's, with in general different labels. Graph also contains auxillary functions for the SCC-finders. To study SCC size distribution, call 

    freq_plot(self,*args)

which prints the list of cluster sizes and their frequency. Set args=False to not display results as a bar graph. Finally

    display(self,*args)

displays a Graph by constructing a Networkx graph. The default node positions are determined according to the spring model, and 
nodes are colored according to their SCC label. We may also use args to specify various display properties: pos (dictionary of positions of nodes in the plane), node size, edge width, turn off arrows, and matplotlib axes and title

## Erdos-Renyi

In code cell 3 we define:

    erdos_renyi(n,p)

which generates an Erdos-Renyi digraph on n vertices by constructing, for each pair (i,j) distinct, an edge i->j with probability p.

## class point

To implement DBSCAN, we have defined a point class representing a point in R^d. The fields are int d (dimmension), self.coords (array of d doubles, defined in constructor). 
The functions

	dist(self,q)
	taxicab_dist(self,q)

define respectively the euclidean and taxicab distance between the point object and another point q. Given an array of points P='points', we define the following distance functions from PxP to R with:

	euclidean_dist(points,i,j)
	taxicab_dist(points,i,j)
	haversine_dist(points,i,j)

Haversine_dist calcutes the great-circle distance between two points points[i] and points[j].  This function will be used for studying the US flight network, and point coordinates must be specificed as (latitude,longitude).

## DBSCAN

Given a set of points 'points', a distance function between them 'distFunc', a search radius 'eps', and a minimum number of points to form a dense region 'M', call

	dbscan(points, distFunc, eps, minPts, *args)

to run DBSCAN, which returns the pair (nb of cluster, cluster labels). A cluster label of -1 represents outliers. Set args=False to not print out results. 

##Generating synthetic datasets

We have relied for this part mostly on the the synthetic datasets half-moons, make-blobs, and make-circles in the sklearn.dataset module. 
We have also coded our own functions:

	generate_uniform_square(n, L)

generates n points at random and uniformly in an LxL square.

	generate_blobs(n,centers, radius)

generates n points at random and uniformly in a set consisting of the union of some number of circles with specified centers and radii.

##Heuristics for choosing epsilon

We have implemented two heuristics for choosing epsilon (for M fixed). The first one is to select epsilon which maximises the silhouette score of the corresponding DBSCAN clustering.
We calculate this as the average of the silhouette coefficient s(p) over all non-outliers points p. Call:

	silhouette_score(points,distFunc,label,nb_c)

where nb_c,label is the output of DBSCAN. The other heuristic is to display a k nearest-neighbor (knn) plot for k=minPts and choose epsilon at the elbow of this plot.
To calculate k nearest-neighbor distances (with linear scan), call:

	kn_distances(points,distFunc,k):



