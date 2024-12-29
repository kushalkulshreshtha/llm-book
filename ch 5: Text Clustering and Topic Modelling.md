## Text Clustering

Overview
The purpose of this project is to explore text clustering using Large Language Models (LLMs). By applying clustering techniques, we can group similar abstracts or documents together, which aids in gaining better insights from the dataset.

Example Dataset
We use a dataset containing abstracts from various research papers. Clustering these abstracts helps us identify common themes or topics across the dataset.

### Process

Step 1: Generate Embeddings
Use an LLM to create embeddings for the text data. These embeddings capture the semantic meaning of the text.

Step 2: Dimensionality Reduction

The embeddings generated in Step 1 are typically high-dimensional. To make them more manageable, apply a dimensionality reduction technique.
Choice of Technique: While PCA is a popular choice, we use UMAP (Uniform Manifold Approximation and Projection). UMAP projects data points from a higher-dimensional space to a lower-dimensional space, prioritizing the preservation of local structure while attempting to retain as much global structure as possible.

Step 3: Clustering
The reduced-dimensional embeddings are passed to a clustering algorithm to form meaningful groups.

Clustering Algorithm: HDBSCAN
For this project, we use Hierarchical Density-Based Spatial Clustering of Applications with Noise (HDBSCAN).

How HDBSCAN Works:
Initialization:
Assumes each data point starts as its own cluster.

Building the Hierarchy:
A. Combines data points into clusters using a bottom-up approach </br>
B. Points are merged based on the maximum reachability distance. </br>
C. Maximum Reachability Distance: Defined as the maximum of the core distance and the distance between two points. </br>
D. Core Distance: The distance between a given point and its Nth nearest neighbor, where N is specified as MinPts (minimum points). </br>
E. Cluster Pruning: Clusters are pruned based on their stability. </br>
F. Stability: Determined by varying thresholds of λ (lambda), which is the inverse of the core distance </br>
G. Clusters that consistently appear across varying thresholds are considered stable, reaffirming their existence </br>
