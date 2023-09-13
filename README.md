# CryptoClustering

For this challenge, I used both python and unsupervised learning to predict if cryptocurrencies are affected by 24-hour or 7-day price changes.

The file used in this challenge can be found in the resources folder (crypto_market_data.csv).

## Preparation of the data

The data was prepared using StandardScaler() module from scikit-learn and was applied to the csv file to normalize the data. The new data frame (df_market_data) used the coinid as the index.  I then plotted a line graph of each of the price change data sets.

#### Snapshot of the df_market_data

![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/01_df_market_data.png)


</br>

#### Line graph indicating the price change percentage over different time intervals for each coinid

![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/02%20line%20graph.png)

</br>

I double checked to ensure that all the data was captured in a float64.

![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/03_float_64.png)</br>
</br>

Using 'StandardScaler' module from scitkit-learn i normalized the data in the csv file.

![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/04_normalized_array.png)</br>
</br>

A data frame was constructed from the normalized array and the coinid was set as the index.


![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/05_transformed_df.png)</br>
</br>


## Find the Best Value for k Using the Original Data

I created a list to input the possible k values called inertia.  Using the KMeans function I fit the scaled data to the model and appended the cluster count to the list.  I made a data frame from this list name df_elbow_1.

![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/06_elbow_1.png)

The data from df_elbow_1 was used to make a line graph.  From this graph, it was determined the best value for k was 4.  The biggest changes were observed in the elbow curve prior to 4.  After k=4 the slope of the line appeared significantly reduced.  

#### Elbow Curve of the Original Scaled Data
![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/07_elbow_original.png)</br>
</br>

## Cluster Cryptocurrencies with K-means Using the Original Data

Using the K means model with the best value for K=4, I transformed all the normalized data in the df_market_data_transformed data frame using n_clusters = 4 and random_state = 1. The predicted cluster value was added to the transformed data frame.  

A scatter plot comparing the 24 hour percentage price change vs the 7 day percentage price change was created with the normalized data.  Each of the predicted clusters was indicated on the graph using a different colour.  From the resulting data, it was observed that there was no visible distinction in the cluster groups.  

#### Scatter plot of price change percent for the original normalized data for 24 hours vs 7 days.
![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/08_scatter_plot_original.png) 
</br>


## Optimize Clusters with Principal Component Analysis

To further analyze the original normalized data and hopefully obtain better cluster separation, principal component analysis (PCA) was carried out on the original normalized data.  The following Array was obtained (first five rows):

![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/09_pca_array.png)
</br>

The explained variance was calculated for the first three principal components.  It was determined that 88% of the total information is found in the first three components with the maximum information is stored in the first component (37%) followed by 32% in the second component and 19% in the third.  

![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/10-array-components.png)
</br>

A data frame was created from the PCA data and the coinid was set as the index.

![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/11_PCA_DF.png)
</br>

## Find the Best Value for k Using the PCA Data

I created a list to input the k values from the PCA data and called it inertia.  Using the KMeans function I fit the PCA data to the model and appended the cluster count to the list.  An elbow curve was created using the cluster counts (k).  From the graph, it was determined that the best value for k was 4 for the PCA data.  Both the original data and the PCA data used 4 as the best value for k.  

![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/12_PCA_Inertia.png)
</br>

#### Elbow Curve for the PCA data
![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/13_elbow_PCA.png)
</br>

## Cluster Cryptocurrencies with K-means Using the PCA Data

Using the K Means model with the best value for K=4, I transformed the PCA data using n_clusters = 4 and random state = 1.  The predicted cluster value was added to the PCA data frame. 

A scatter plot was created of PCA 1 vs PCA 2.  From the scatter plot it was observed that there were two groups that were well centred around zero (cluster 1 and cluster 0).  Cluster 3 and Cluster 4 appear to only consist of 1 point each, but they appear to be outliers from the other two groups.  

#### Scatter plot of PCA1 vs PCA2
![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/14_PCA_scatter.png)
</br>

## Visualize and Compare the Results

Both the elbow curve of the original data and that of the PCA data were compared.  In the original data the curve seems more rounded but there is definitely a joint or elbow at K=4.  The PCA data has been more defined and looks like a straight line from K=1 to K=4.  There is a very obvious joint indicated that K=4 gives the best fit. 

#### Composite diagram of both the elbow curve from the original data as well as the PCA data.
![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/15_Elbow_compare.png)
</br>

When looking at the two scatter plots together, it could be observed that with the original data, the four clusters were not as well defined.  We could see that the price change at 24hrs vs 7 days indicated that cluster 2 was not really defined from cluster 1 whereas cluster 0 was well defined from the other three clusters.  Even the difference between cluster 1 and 3 was not clear.  

Once PCA analysis was carried out, there were 4 distinct clusters when comparing PCA1 and PCA2.  Cluster 3 and Cluster 2 also appeared as though they were outliers from the other two clusters.  Clusters 0 and 1 were two very distinct clusters from each other.  

#### Composite diagram of both the scatter plots from the original data (price change percent at 24hr vs 7 days) and the PCA data (PCA1 vs PCA2)
![](https://github.com/TraceyGeneau/CryptoClustering/blob/main/images/16_scatter_compare.png)
</br>


If we were to use fewer features to cluster this data, we would see the following impact:


1. You could end up with less accurate cluster assignments.  This would be due to the reduced information available to help the algorithm distinguish between points in a cluster.  
  
  
2. If you use less features, your model becomes simplified.  If you are really trying to understand how each cluster differs, this would make the task very difficult.  By increasing the features, you will see more distinct cluster groups.  Of course, there will come a point that too many features are not beneficial in your analysis too.  This is why it is important to use the elbow value when determining clusters.




