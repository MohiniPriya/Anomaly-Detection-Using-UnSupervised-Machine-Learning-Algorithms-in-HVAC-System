Anomaly Detection Strategy for AHUs using Unsupervised Techniques
Project Purpose
1.	Building a data-driven, unsupervised learning strategy to identify anomalous behavior in the operation of an Air Handling Unit (AHU).
2.	To identify how AHU can be controlled efficiently
3.	Applying exploratory data analysis to identify non-optimal working conditions of the AHU.
4.	Identifying clusters to categorize data and detecting anomaly thresholds for each feature in each cluster.
Data Description The raw data contained 25 csv files with approximately one year of data sampled at varied time intervals in 2 different time zones (EDT & EST) These csv files included AHU controlled temperature data, outside air temperature, compressor data, loader 1 and 2 data, duct static data, etc. Included external dataset with outside air relative humidity information.
EDA Timestamp data converted from EDT to EST. The timestamps were bucketed on an hourly basis. Used the last known value as the current value for any missing timestamp. Performed interpolation using forward fill method to handle irregular time intervals and then merged all the files in one centralized file. After merging, there were 8731 instances, which after removing the missing rows had final value of 8628.
Skewness &Transformation: All the numerical attributes had skewness less than -2, except Economizer Position. Applied (Log + 1) transformation to reduce its skewness. Many of the economizer values are 0. Thus, for a quick fix, I added 1 to each data point. This works well since the log of 1 is 0. Furthermore, the same spread is retained since all points are increased by 1.
Feature Engineering: To get more insights about the AHU operations, I extracted following binary features based on Timestamp column.
1.	Operational/Non- Operational Hours: The operational hours in the building are between 6 AM and 6 PM.
2.	Week Day/Weekend
Applied unsupervised machine learning algorithms (K-Means Clustering and Isolation Forest) on time series data collected from an Air Handling Unit of a building to detect anomalous behavior of the system.
Used Elbow Plot and Silhouette Plot to identify the optimal number of clusters for K-Means. Scaling of Numerical variables was applied in this case as K Means Clustering uses Euclidean Distance to form the groups before calculating the distance and feeding it to the algorithm.
Anomaly Detection Using Isolation Forest Isolation Forest explicitly identifies anomalies instead of profiling normal data points. Isolation Forest, like any tree ensemble method based on decision trees. A score of one indicates that the point is anomalous and 0 indicates normal. Contamination factor of 0.1 used in the Isolation Forest model.
Plotted the Discharge Temperature over time during the holiday week of December. Anomalies were high on Dec 25th, Dec 26th, 31st Jan and 1st Jan
Anomaly Inspection on a Holiday & Non-Holiday Plotted Discharge Temperature on a holiday (4th July) showed more anomalies during operational and non-operational hours as compared to a non- holiday working day on 5th July.
Anomaly Threshold Detection on a per-feature per-cluster basis Ran the k-means centroids through the isolation forest model and verified that the centroids are non-anomalous. For each feature independently, the attribute values were incremented and decremented to identify the anomaly threshold. Attribute value in each centroid was increased and decreased up to 200 times.
Approach for Anomaly Detection Matched the Isolation Forest anomaly labels with K Means cluster labels. The smallest K means cluster has the highest outlier fraction compared to all other clusters. Hence, to determine anomaly threshold using K Means model, cluster 5 has been used considering it the most anomalous cluster. In addition, Isolation Forest model has been used separately using K-Means centroid value to detect anomaly threshold.
Plotted Common Anomalies in K Means & Isolation Forest Combination of K means & used Isolation Forest Algorithms for clustering and anomaly detection. 
Summary: Combination of K means & Isolation Forest Algorithms used in clustering and anomaly detection.
Threshold values identified for a few of the attributes using contamination factor 0.2. 
The AHU performance found to be sub-optimal for non-operating hours.
