---
id: k-means_prototypes
aliases: []
tags: []
---

# k-means_prototypes

## 2025/04/16
- Can use ==K-means clustering== algorithm to find out the cluster(a kind of a codebook) of the residual prototypes.
---
> [!tip]
> The term residual prototypes here means the prototypes of the different classes of the residual.
---
- The code below is the example code of K-means clustering from Claude.
---
```python K-means clustering
from sklearn.cluster import KMeans
import numpy as np

X = np.random.random((20, 512))  # 20 random vectors, dim 512

# make a model with 5 clusters
kmeans = KMeans(n_clusters=5, random_state=42)

# train he model
kmeans.fit(X)

# check the label of the cluster
labels = kmeans.labels_
print("cluster label:", labels)

# check the center of he cluster
centroids = kmeans.cluster_centers_
print("shape of the center:", centroids.shape)  # it'll be (5, 512)
```
---
- Or I can also think about my own clustering algorithm.
