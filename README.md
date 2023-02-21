# AICodingChallenge
This is a Python-based machine learning code to differentiate healthy vs. toxin-treated cells.

High-throughput screenings generate vast amounts of data, often unmanageable by standard method. Machine learning is a rapid and accurate approach to overcome this barrier and advance research.
Goal: classify single cells into “healthy” and “toxin-treated” categories.

Preliminary analysis:
1.	Descriptive statistics. In the training dataset, there are 83827 “POS” and 14872 “NEG” samples
2.	Find features that are different for toxin-treated (“NEG”) vs. healthy cells (“POS”).
a.	Group the samples into the two categories and calculate the fold difference for each feature
	Output: “SHAPE_Cyto_mask_Area” and “SHAPE_Cell_mask_Area” have the biggest difference between healthy and toxin-treated conditions. Moreover, most features did not show any difference between the two conditions.
b.	Perform PCA to visualize samples in two dimensions and find the driving components
Output: PC1: “SHAPE_Cell_mask_Area_sq_um”, PC2: “SHAPE_Cyto_mask_Area”
Based on this, the number of features were reduced to only ten features that contain the label “Area”.
3.	Perform k-means clustering to test whether this unsupervised machine learning algorithm can predict groupings, n_clusters = 2 (despite Elbow Method showing that the optimal number of clusters is 3).
	Output: k-means clustering performed poorly in predicting the two clusters (Fig. 1); for the training dataset, 53939 samples were allocated in Cluster 1 and 44760 samples were allocated in Cluster 2 (compare to the actual sample size per category). K-means clustering was also performed for the test dataset to test for any batch effect. For this dataset, 55776 samples were allocated in Cluster 1 and 45004 were allocated in Cluster 2 (actual sample sizes: 83897 “POS” and 16883 “NEG”). Similar distribution of samples indicated that batch effect was not observed in these datasets. 

Supervised machine learning algorithms:
1.	Multilayer perceptron (MLP) training, a widely used algorithm that has been successfully applied to distinguish healthy and apoptotic cells based on flow cytometry (Li et al., 2021). 600 MLP models were screened (two hidden layers, 20 nodes for the first hidden layer, and 30 nodes for the second layer). Models with the highest mean accuracy (after subtraction with the standard deviation) were selected to call predictions on the test dataset. The selected MLP models (10-9 and 17-21) performed with 95.34% and 95.42% accuracy on the training dataset, respectively. However, MLP 10-9 performed slightly better with 94.86% accuracy on the test dataset, compared to 94.71% for MLP 17-21 (Fig. 2).
2.	Random forest training, another popular algorithm for classification. 100 random forest models (based on the number of trees: 1-100) were screened. The selected model performed with 96.15% accuracy on the training dataset and 94.39% accuracy on the test dataset (Fig. 3), only very slightly lower than the MLP model.
3.	Gradient boosting classification, similar to random forest in which the algorithm uses decision trees. Optimization of parameters are needed for the algorithm to perform better. The selected parameter performed with 96.21% accuracy in training dataset, but only 94.61% accuracy in the test dataset (Fig. 4). This result is slightly higher than random forest, but lower than the MLP.

Finally, features importance that drive classification can be checked for the decision trees algorithms. In this case, “SHAPE_Cell_mask_Area” drives almost 50% of decision, followed by “SHAPE_Cyto_mask_Area” (27%) (Table 1). In conclusion, all three machine learning algorithms performed really well in distinguishing healthy cells population from toxin-treated cells. While random forest showed the lowest accuracy with the selected setting, the other two algorithms were considerably slower to perform (gradient boosting took the longest time), this should be put into consideration when choosing an algorithm for a high-throughput screenings.

All figures and table for this project can be found in the attached slide.

Reference:
Li, Y., Nowak, C.M., Pham, U. et al. Cell morphology-based machine learning models for human cell state classification. npj Syst Biol Appl 7, 23 (2021). https://doi.org/10.1038/s41540-021-00180-y
