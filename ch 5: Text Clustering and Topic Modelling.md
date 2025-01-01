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

**How HDBSCAN Works:**
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

**Topic Modelling**
Process of extracting themes from in the collection of textual data is called as topic modelling <br>
Classical technqiues such as Latent Dirichlet allocation (baed on probability of words in the corpus can be used) </br>

**BERTopic Modelling Technique**
1. Framework leverages group of documents having semantically similar text to extract various types of topic representation </br>
2. Two step framework - firt step is to obtain the clusters and the second step is to leverage thses clusters to extract topic </br>
3. Advantage is topic modelling is now independent of choice of Clusteting algorithm and hence topic modelling can be idependently fine tune </br>
4. How does it work ? </br>
5. Once the clusters are formed, countevctoriser is initialized to understand the frequency of the words in the document.</br>
6. However the major caveat here is the representation becomes document level and not the cluster level and also stop words will be shown repeatedly </br>
7. In order to overcome the same the BERTopic Modelling Technique uses class based c-TF-IDF function </br>
8. The term frequecy i.e. TF calculated at document level is now multiplied by IDF value given by log(A/Cf + 1) where A is the average frequency of all words across all clusters
   and Cf is the total frequency of each word. </br>
9. This way the words that appear in only specific clusters will have lower value of denominator i.e. Cf and words appearing very commonly will have higher denominator and lower      value of IDF.
10. As also mentioned eariler each step in the BERTopic Modelling is modular and can be replaced depending on the choice. E.g. the nlper/gte-small sentence tokenizer can be replaced with various other availabe, same is true for clustering algorithm used </br>

How to calculate c-TF-IDF:

![image](https://github.com/user-attachments/assets/fdd859f5-3810-4ae6-b126-8a18b1622f07)

Read more at: [link](https://maartengr.github.io/BERTopic/getting_started/ctfidf/ctfidf.html)

**Limitations**
1. One of the limitations of the BERTopic Modelling involves that the end output i.e. topics are charecterized and represented by a bag of words </br>

**Improving Outcome**
1. The above limitation can be overcome by replacing BOW with embeddings using a representation models </br>
2. This process adds additional blocks to the LEGO of the BERTopic Modelling framework which essentially helps in fine tuning the existing topics

**Different Types of Representation Blocks**
1. **KeyBERTInspired**: KeyBERT extracts keywords from texts by comparing word and document embeddings through cosine similarity
2. Cosine Similarity is calculated between document's embedding and topic the doc. corresponds to
3. Average document per topic is then calculated and compared with the embeddings of the candidate keywords to re-rank the keywords

**Maximal marginal relevance**
1. Advantage: Avoids redudant word to be part of topic keywords
2. The process is performed by embedding the set of candidate keywords and calcualating the next keyword to add.\
3. Thus In MMR from a intial keywords size of 30 we come to smaller size keyword of 10 which although is more diverse

**Leveraging Generative Models of Extracting Labels**
1. Based on the topics obtained Generative models can be further leveraged to come up with short label based on the keywords


