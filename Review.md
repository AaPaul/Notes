Reviews
==========

### Cluster
Cluster is a set of data assigned to groups/clusters based on:
1. similar to one another within the same group
2. dissimilar to the objects in other groups

**Use distance-based approaches to determine the similarity or dissimilarity**
- high intra-class similarity: cohesive 
- low inter-class similarity: distinctive

#### K-Means
Basically, I didn't understand the detailed fundamental of this method. So I need to review it and re-learn it.
what is kmeans, the meaning of k value
how to use it
##### fundamental
K-means: Each cluster is represented by the center of the cluster.
Minimize the sum of squared distances
$$E = \sum\nolimits_{i=1}^k \sum\nolimits_{p \in C_i}(p-c_i)^2 $$
<center>(where $c_i$ is the centroid or medoid of cluster $C_i$)</center>

1. We first choose the k points as the **initial** central points of the cluster. Therefore, we have k clusters now.

2. Calculate the distance between the central points and other points. Assign each object to the cluster to which it is closest based on mean value (threshold).

3. **New Centroids**
    - Update the centroid (i.e., by mean value) for each cluster
    - Assign each object to the cluster with the nearest centroid

4. Stop when the assignment does not change, or repeat Step 2.

##### how to use it
1. Firstly, we use elbow method to choose the relatively best k value. 
 - Elbow method
  1. run k-means clustering on the dataset for a range of values of k (e.g., 1 to 10)
  2. for each value of k, calculate the within-cluster SSE
  3. Plot the within-cluster SSE.
  4. The value of k should be the elbow point

2. call the method KMeans from the sklearn

##### Weaknesses
- Need to specify K
- Sensitive to noisy data and outliers
- Usually choose a local optimum instead of the global one

### CNN structures
Two key points of object classification
- feature extraction
- classification

**Figure out the structure of AlexNet, VGG16 and ResNet**

The Meaning of Dropout and why it works


