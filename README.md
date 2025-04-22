# bioactivity_acetylcholinesterase

This folder contains all the relevant notebooks covering the workflow of a bioinformatics drug discovery project with the target acetylcholinesterase. There are primarily 5 major steps and each have dedicated notebooks.

 -  Bioactivity Data Collection
 -  Exploratory Data Analysis
 -  Descriptor Analysis
 -  Model Building
 -  Model Comparison
 
<br>

## Data Sourcing and Processing

The focus of this step is to leverage the ChEMBL Database which is a curated database of bioactive molecules with drug-like properties. I search through the database for molecules that are bioactive for our protein. Our focus from the data is on the canonical_SMILES column (Simplified Molecular Input Line Entry System) that represents the structure of the molecule in a particular way (nomenclature) and standard_value. The standard_value data is given in the IC50 unit, meaning the amount it takes to reduce the protein activity by 50 % and is given in nM. Compounds with less than 1000 nm will be considered active and more than 10000 nM will be considered inactive. I am also converting to pIC50 = -log10(IC50). This make the data more readable and easier to model in machine learning. 1 mM is 3, 1 nM is 9.

<br>

## Exploratory data analysis

The focus of this notebook is to  analyze how "drug like" the compounds are using the Lipiniski descriptors developed by Chistopher Lipinski at Pfizer. His Rule-of-Five looks for molecules with 4 descriptors:
- molecular weight < 500 Dalton, because heavier molecules often struggle to cross cell membranes and get absorbed.
- Octanol-water partition coefficient (LogP) <5. Lower LogP is more water-soluble, higher LogP is more fat-soluble. <5 is a good balance of hydrophilicity and lipophilicity
- Hydrogen bond donors <5. 
- Hydrogen bond accepters <10
Too many may mean the molecule may form strong interactions with water, making it hard to cross membranes.
Here I analyze the 4 descriptors with the Mann Whitney U Test between the bioactive and non-bioactive compounds to visualize significant difference.

<br>

## Descriptor Analysis

I am cleaning the chemical structure of the SMILES notation, such as removing salts, and generating Pubchem fingerprints, which binary vectors (i.e., strings of 0s and 1s) that represent the presence or absence of specific substructures or chemical features in a molecule. This will be our features for machine learning.

<br>

## Model Building

The Pubchem fingerprints that 881 features. The goal for the machine learning task is the see which functional groups or fingerprints are potent for designing a drug. The pIC50 is the label, the fingerprints are the features. I do feature selection and drop low variability features. A random forest model is used as the initial model. I also use Tree SHAP to interpret the importance of the features. We improve the R2 by random hyperparameter tuning.
<br>

## Model Comparison
Using LazyRegressor, I am testing different regression models as classifier (clf). From the results of the initial test, it seems random forest is a decent choice giving low error with a relatively low amount of inference time. To improve the model, I would maybe make adjustments to feature engineering and do Bayesian hyperparameter tuning. 


Thank you to the Data Professor Chanin Nantesanemat!