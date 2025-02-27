# SODor
This is the official implementation of the paper "Long-Term EEG Partitioning for Seizure Onset Detection", AAAI 2025 (Oral).

<img width="937" alt="image" src="https://github.com/user-attachments/assets/2e849b68-6753-470f-89f0-f9991e9cfeee" />


This is a two-stage model based on a graph model and time series clustering.
We first train a second-level classification model, generate logit-wise feature vectors, and locally store these vectors.
Then, we concatenate logit features and implement sequence-level clustering for onset detection.




## Onset Detection 

The Onset detection is a clustering solver (based on TICC [1]) for efficiently segmenting and clustering a multivariate time series. It takes as input a T-by-n data matrix, a regularization parameter `lambda` and smoothness parameter `beta,` the window size `w`, and the number of clusters `k`.  Clustering breaks the T timestamps into segments where each segment belongs to one of the `k` clusters. The total number of segments is affected by the smoothness parameter `beta`.
It does so by running an EM algorithm where the solver alternately assigns points to clusters using a dynamic programming algorithm and updates the cluster parameters by solving a Toeplitz Inverse Covariance Estimation problem. 
The descriptions of parameters are as follows:

* `window_size`: the size of the sliding window
* `number_of_clusters`: the number of underlying clusters 'k'
* `lambda_parameter`: sparsity of the Markov Random Field (MRF) for each of the clusters. The sparsity of the inverse covariance matrix of each cluster.
* `beta`: The switching penalty.
* `maxIters`: the maximum iterations of the algorithm before convergence. Default value is 100.
* `threshold`: convergence threshold
* `write_out_file`: Boolean. Flag indicating if the computed inverse covariances for each of the clusters should be saved.
* `prefix_string`: Location of the folder to which you want to save the outputs.


The `TICC.fit(input_file)`-function runs the clustering algorithm on a specific dataset to learn the model parameters.

* `input_file`: Location of the data matrix of size T-by-n.

An array of cluster assignments for each time point is returned in the form of a dictionary with keys being the `cluster_id` (from `0` to `k-1`) and the values being the cluster MRFs.


## Example Usage

You can quickly run this Jupyter file `Sodor_clustering_main.ipynb` and check the visualization.

The `data` folder contains the CHB-MIT example. 

For parameter optimization, you can use BIC in `src/TICC_helper.py`, and optimize it via in `src/admm_solver.py`.
For further algorithm details, you can refer to [1].


## References

[1] D. Hallac, S. Vare, S. Boyd, and J. Leskovec, Toeplitz Inverse Covariance-Based Clustering of
Multivariate Time Series Data, Proceedings of the 23rd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining. 215--223


## Citation
If you find our work useful in your research, please consider citing:
```
@misc{SODorAAAI2025,
      title={Long-Term EEG Partitioning for Seizure Onset Detection}, 
      author={Zheng Chen and Yasuko Matsubara and Yasushi Sakurai and Jimeng Sun},
      year={2024},
      eprint={2412.15598},
      archivePrefix={arXiv},
      primaryClass={cs.LG},
      url={https://arxiv.org/abs/2412.15598}, 
}

```
