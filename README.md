# 🗺️ High Resolution Land Cover Mapping

> 🏫 University of Trento | 🛰️ Remote Sensing Lab | 📅 A.Y. 2024/2025 | 🎓 Master Thesis

<p align="center">
  <video src="https://github.com/user-attachments/assets/4dbe472a-9cca-4cff-8756-371fbb9e9bbc" controls width="720">
  </video>
</p>

## Motivation

How big is the Amazon rainforest? Providing a reasonable estimate has been a challenging task for decades. Early attempts relied on ground campaigns, which were demanding and of limited accuracy. It was only with the advent of satellites that the margin of error reduced drastically. Given a satellite acquisition of the Amazon basin, one simply needs to count all pixels associated with the rainforest. Multiplying the number of pixels by their spatial resolution we get our estimate: around 6 million square kilometers.

This simple example gives a first hint of the potential of land cover mapping, which consists of segmenting a remotely sensed image according to the biophysical cover type of the surface. However, the power of this approach extends far beyond area estimation. With satellite archives now spanning over thirty years, we can analyze ecosystem evolution over time and detect changes. For instance, we can identify which areas have been deforested and estimate when that happened, monitor urban sprawl, track wetland loss, observe the expansion of agricultural fields and plantations, assess the impact of mining activities and study water body dynamics. These data represent an invaluable source of information to keep track of land cover changes. However, how to extract this information is still an active research area.

In the last decade, significant progress in the field has been achieved thanks to three converging factors: the development of new models able to effectively capture spatial and temporal dependencies in satellite data; the scale of computational capabilities that enabled processing of petabytes of imagery; and the availability of open satellite data granted by new policies adopted by NASA and ESA. The primary bottleneck in the development of reliable systems is now represented by the scarcity of highly accurate training data, whose generation is expensive and resource-intensive due to several inherent challenges.
First, the spatial extent of target regions makes it infeasible to manually label entire areas or even substantial portions. A second reason is the variability of class features within the region of interest, requiring a proper sampling of the training datasets. Moreover, properly labeling the data requires domain expertise. While crowdsourcing could be suitable for very high resolution classification tasks, more skills are necessary to interpret multispectral or hyperspectral imagery, which is generally characterized by lower spatial resolution but richer spectral information. Finally, determining which samples should be prioritized is complex: points that are easy to classify may be less informative compared to boundary pixels or those pixels containing class mixtures. All these factors make the production of representative training datasets challenging, labor-intensive and costly.

Many approaches have been explored to deal with this constraint. One of the most promising is weak supervision, which consists of leveraging less reliable labels from multiple heterogeneous sources to create the large-scale datasets required for training deep learning models. By integrating information from diverse sources, this approach can potentially yield more robust systems capable of identifying features and patterns more effectively. Many specific aspects need to be considered while developing weak supervision strategies. Data from different sources must first be harmonized to a common spatial reference system and resolution. Geolocation errors, projection distortions and resampling artifacts can cause spatial misalignments, leading to systematic misclassifications. Moreover, labels assigned to the same pixel may be in conflict across various sources due to differences in classification schemes or labeling errors. Temporal mismatches add further complexity: maps are often produced at different times, so discrepancies may reflect real land-cover changes rather than classification errors. Finally, label uncertainty can be due to specific samples, to their classes or to model biases caused by a domain shift with respect to the training region.

## Proposed Approach

In this work, we propose a multi-task weakly supervised framework for land cover mapping that exploits a set of heterogeneous digital products as independent tasks. The architecture consists of a shared backbone and multiple classification heads, each yielding predictions over a distinct task. We adapt the consensus mechanism proposed in [1], originally developed to learn the hierarchical structure inherent in land cover classes, and show that the same principle can be applied to learn cross-task correlations between classes across different classification tasks. Building on this, we develop an integrated framework that simultaneously enforces hierarchical and cross-task consistency.
The combination of hard parameter sharing with the proposed consensus mechanism allows the model to learn a robust data representation, mitigating the adverse effects of label noise present in the weak sources. A key strength of the approach is that semantic consistency is enforced in a fully self-supervised manner, and the learned relationships are encoded in interpretable matrices. To validate the method, we consider four freely available digital products as independent tasks within two large areas in Amazonia: three land cover maps and a canopy height map. Despite their differing resolutions, classification schemes and temporal inconsistencies, the framework successfully integrates the complementary supervision signals provided by each source, yielding more robust data representations. Finally, we investigate the use of learned cross-task relationships for domain adaptation, showing that when inter-task correlations are sufficiently stable across regions, they can be leveraged to recover supervision for a missing label source in the target area.

Our main contributions can be summarized as follows:
1. We demonstrate that more accurate land cover maps can be derived by integrating multiple outdated digital products.
2. We propose a multi-task weak supervision framework that exploits class relationships across different label sources through a consensus mechanism.
3. We introduce a loss function that aligns classification and regression tasks, with learned weights that reflect physically meaningful inter-class relationships.
4. We show that learned relationships can be exploited for domain adaptation.

## Study Area

<p align="center">
  <a href="http://3.120.171.141/21kuq/">
    <img src="images/tile_21KUQ.png" width="180">
  </a>
  <img src="images/sud_america.png" width="400">
  <a href="http://3.120.171.141/22kgv/">
    <img src="images/tile_22KGV.png" width="180">
  </a>
</p>

<p align="center"><i>Clicca su una tile per aprire la visualizzazione interattiva →</i></p>

## Organizzazione repository

```
├── scripts/                        # Folder containing experiment setups
│   └── exp_<id>/                   # Each experiment folder (e.g., exp_0, exp_1, ...)
│       ├── config_files/           # Config files for the experiment
│       │   └── exp_<id>.yaml
│       └── exp_<id>.py             # Main script for the experiment
│
├── src/                            # Source code modules
│   ├── datasets/                   # Data loading and preprocessing
│   ├── models/                     # Model architectures
│   ├── losses/                     # Custom loss functions
│   └── utils/                      # Utility functions
│
├── .gitignore
├── Dockerfile
├── entrypoint.sh
├── launch_docker.sh
├── launch_queue.sh
├── LICENSE
├── pyproject.toml
└── README.md

```

## Main Reference Papers
- [1] 
