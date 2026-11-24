Customer Segmentation with Clustering
Segmenting 68,300 e-commerce customers with k-means and hierarchical clustering, choosing the number of clusters with four independent methods, and visualising the result with PCA and t-SNE.

University of Cambridge, Institute of Continuing Education — Data Science, Machine Learning and AI programme, April 2025.

Problem
"Who are our customers?" Group customers by purchasing behaviour so that marketing, retention and pricing can be targeted, using a real-world e-commerce dataset (SAS, 2024).

Data
Raw records	951,669 order lines, 20 columns
Quality issues	Currency symbols and brackets in numeric fields; 21 duplicate rows; 12% missing state/province
Modelling table	68,300 customers × 5 features
Method
Cleaning — stripped currency symbols and brackets, converted types, reported missing values and duplicates.

Feature engineering — one row per customer with Frequency, Recency, Customer Lifetime Value (CLV), Average Unit Cost and Age. Date of birth was converted to age in years to minimise personal data.

Outlier analysis — IQR boxplots, z-scores (|z| > 3), and variance inflation factors (Frequency and CLV ≈ 22). Capping at the 99th percentile was tested; outliers were retained because they represent genuine high-value customers.

Choosing k — standardised features, a Ward-linkage clustermap, then four criteria:

Method	Suggested k
Elbow (within-cluster sum of squares)	4
Silhouette score	7
Davies–Bouldin score	6
Hierarchical dendrogram (Ward)	6
k = 6 was selected: k = 4 merges distinct high-value groups, and the extra cluster at k = 7 splits an already fragmented group. A cross-tab of k-means against agglomerative clusters checks agreement between methods.

Dimensionality reduction — PCA for an interpretable 2-D view; t-SNE at perplexities 30, 50, 70 and 90, with perplexity 50 giving the clearest separation (following van der Maaten & Hinton, 2008, and Wattenberg et al., 2016).

Outputs — per-cluster profiles and boxplots, and a customer-to-cluster table exported for stakeholders.

Findings
Six segments, including a small, very high-value group of frequent buyers; a group buying high-cost items infrequently (average unit cost ≈ $181); and low-engagement groups. Two segments stand out as profit drivers:

frequent buyers of lower-cost items, and
less frequent buyers of high-cost items.
Recency and Age contributed little to separation and may be redundant.

Limitations and next steps
Aggregation error in Frequency and CLV. Order counts and revenue totals were computed per row and then summed again per customer, which squares Frequency (e.g. 35 orders → 1,225) and multiplies CLV by the number of orders. This explains the high VIFs and the extreme CLV spread flagged in the notebook. Fix: count unique Order IDs and sum Total Revenue once per customer, then re-run clustering. Relative rankings are largely preserved, but segment sizes and boundaries are likely to change.
Recency reference date. Recency is measured from the date the notebook was run; it should be measured from the last date in the dataset.
Reproducibility. Some intermediate cells fit k-means to a PCA projection from an earlier step; the final k = 6 model is fitted on the scaled features with a fixed random seed.
Next steps. Log-transform skewed features, compare Gaussian mixture models or DBSCAN, and validate segments against campaign response data.
Repository contents
Path	Contents
notebooks/	Analysis notebook
reports/	Written report
figures/	Exported notebook figures and report pages
Tools
Python · pandas · NumPy · scikit-learn (KMeans, AgglomerativeClustering, PCA, t-SNE, silhouette and Davies–Bouldin scores) · SciPy (Ward linkage, dendrograms) · statsmodels (VIF) · seaborn · matplotlib

Reference
SAS (2024) CUSTOMERS_CLEAN [data set]. Last revised 15 December 2021.
