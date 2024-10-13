# F-Wilks' Lambda

## Description

This Python code (with pseudocode provided in PseudocodeFWilks.md) addresses the process used in the F-Wilks' Lambda method, as described in the paper:
- [F-Wilks' Lambda: A Hybrid Multivariate Descriptor to Enhance Feature Selection in Machine Learning Algorithms over a Priori Evaluation](link).

The code is designed in Python to perform multivariate analyses (MANOVA) and to utilize machine learning methods. The paper also employs techniques such as Design of Experiments (DOE) and multiobjective optimization (MO). The extraction of variables was based on commonly used files extracted from the [UCI Machine Learning repository](https://archive.ics.uci.edu/), and the results found can be verified in the provided spreadsheets.

The code is designed to execute all functions using Python, allowing users to work with their own datasets; however, the dataset used in the paper is also provided. This dataset was created from a Design of Experiments (DOE) using responses generated through Computational Fluid Dynamics (CFD). The extraction of latent variables was performed with Principal Components Factor Analysis (PCFA), and the multiobjective optimization was done with Normal Boundary Intersection (NBI), while the metrics for evaluating the responses used in the paper were Shannon Entropy (S) and Generalized Distance (GD).

The main goal is to allow users to execute the F-Wilks' Lambda process in Python across all stages of the method. The provided code does not focus on creating graphs but rather on applying the method.

## How to Use

1. Download or clone this repository to your local machine.
2. Convert the pseudocode into the programming language of your choice.
3. Use your own dataset or the datasets provided by the author.
4. Run the Python code blocks and compare the results obtained using ML with the strategy proposed in the paper.
5. The code includes detailed comments and spreadsheets to help you understand its functionality.

## Excel Spreadsheets

The available Excel files are listed below.

1. **Original Datasets**  
   [Original_Datasets.xlsx](Original_Datasets.xlsx)  
   *Classification datasets extracted from the UCI Machine Learning repository.*

2. **Factor Datasets**  
   [Factor_Datasets.xlsx](Factor_Datasets.xlsx)  
   *Datasets with varimax factor scores (obtained from the original data) and the classes.*

3. **Results Cases**  
   [Results_Cases.xlsx](Results_Cases.xlsx)  
   *Design, MANOVA Statistics, ML Evaluation Metrics, correlations between MANOVA and ML, verification of results.*

## Contact

If you have any questions or suggestions, feel free to reach out via:

- **Email**: [matheusc_pereira@hotmail.com](mailto:matheusc_pereira@hotmail.com)
- **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/matheuscostapereira/)
- **Lattes**: [Lattes Profile](https://lattes.cnpq.br/7025666927284220)
