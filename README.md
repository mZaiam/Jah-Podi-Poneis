# Jah'Podi Pôneis - Predicting Bang Gap

## Introduction

The "band gap" is a property representing the energy difference between the highest energy state in the valence band and the lowest energy state in the conduction band in insulating or semiconductor materials. This energy value is related to the material's conductivity: the smaller the value of the band gap, the higher the conductivity.

The objective of this work is to use machine learning models and strategies to predict the band gap of materials in two distinct ways:

1. Using only the molar proportion of materials as features.
2. Using the molar proportion multiplied by the Pauling scale electronegativity values of the atoms composing the materials as features.

This work utilizes the `expt_gap` dataset from the `matminer` library, containing experimental band gap values of various semiconductor materials, which is the variable we aim to predict. Four models were trained for each case: linear regression and random forest with standard normalization, and linear regression and random forest with maximum absolute normalization and dimensionality reduction with PCA.

## General Project Description

Initially, we aimed to relate the chemical formula of the material to its band gap value, assuming that atoms and their proportion would influence this value. However, we had the idea to add another piece of information to this prediction: the electronegativity value of each atom. This intuition comes from the fact that more electronegative atoms attract their electrons more, thus complicating their promotion from the valence band to the conduction band. Therefore, we hypothesized that the electronegativity values of atoms also influence the prediction of the band gap value of a material. To test this hypothesis, we created two feature sets: one containing only the molar proportion of the material, and another containing the molar proportion multiplied by the electronegativity value of each atom. We trained the same models with both feature sets to compare the results. There are various electronegativity scales, but in our case, we used the Pauling electronegativity definition.

Starting the data preprocessing, we parsed the chemical formulas of each material to extract the molar proportion of each material from the strings in the `expt_gap` dataset. We used the `Composition` function from the `pymatgen` library for this purpose and later added the electronegativity values using the `mendeleev` library.

The models selected for prediction were: linear regression and random forest. Linear regression was chosen due to the hypothesis that there might be a linear relationship between the features and the band gap value. Random forest was chosen because it is an extremely robust model, being formed by multiple decision trees. Perhaps the band gap value behaves like separated regions in the feature space, a characteristic that could be captured by the random forest model. In both models, we tested dimensionality reduction with PCA, using 90% of the initial features' variance.

All models used features after maximum absolute normalization. Random forests underwent hyperparameter optimization through random search in a predefined search space due to the computational cost of training these models. To have a baseline model for comparison, we trained a baseline model for each case.

In summary, the `main.ipynb` notebook is structured as follows:

1. **Imports:** Initially, the necessary libraries and functions for the project development are imported, including `matplotlib`, `numpy`, `pymatgen`, `mendeleev`, `matminer`, and `sklearn`.

2. **Dataset Pre-Processing:** The dataset is transformed for the application of machine learning algorithms.

3. **Case Division:** The notebook works with two feature sets to train the machine learning models. "Case 1" uses only the molar proportion of the materials as features, while "Case 2" uses the molar proportion multiplied by the electronegativity value.

4. **Results Discussion:** At the end of the notebook, there is a discussion of the results, including an analysis of the performance of each model in both cases and a reassessment of the initial hypotheses.

## Credits
- The project uses the `expt_gap` dataset available at [Next-Gen Materials Project](https://hackingmaterials.lbl.gov/matminer/dataset_summary.html#expt-gap).

## Acknowledgments

Thanks to Professor Daniel Roberto Cassar for the Machine Learning course.

## Observations

This was a group project, and this is my edited version. I only translated the notebook to english and changed some minor things. If you're interested in seeing the original project, just check the link int the top of this fork.
