# F-Wilks' Lambda

## Description

This Python code implements the complete process for the **Varimax Rotated Factor Normal Boundary Intersection (VRF-NBI)** method, as described in the paper:
- [Nonlinear Multiobjective Optimization of Efficiency Conditions using a CFD-DOE Hybrid Approach: A Practical Application in Centrifugal Fans for Industrial Ovens](link).

The code is designed to execute all functions using Python, allowing users to work with their own dataset; however, the dataset used in the paper is also provided. This dataset was created from a Design of Experiments (DOE) using responses generated through Computational Fluid Dynamics (CFD). The extraction of latent variables was performed with Principal Components Factor Analysis (PCFA), and the multiobjective optimization was done with Normal Boundary Intersection (NBI), while the metrics for evaluating the responses used in the paper were Shannon Entropy (S) and Generalized Distance (GD). 

The main goal is to allow users to execute the VRF-NBI process in Python, across all stages of the method. The provided code does not focus on creating graphs but rather on applying the method.


## How to Use

1. Download or clone this repository to your local machine.
2. Open Jupyter Notebook.
3. Make sure the libraries are installed on your machine.
4. Use your own dataset or the one provided by the author.
5. Run the Python code blocks and obtain the results.
6. The code includes detailed comments and auxiliary figures to help you understand its functionality.


## Excel Spreadsheets

The available Excel files are listed below.

1. **Original Datasets**  
   ![Original_Datasets.xlsx](Original_Datasets.xlsx)  
   *Classification datasets extracted from UCI Machine Learning (https://archive.ics.uci.edu/).*

2. **Factor Datasets**  
   ![Factor_Datasets.xlsx](Factor_Datasets.xlsx)  
   *Datasets with varimax factor scores (obtained from the original data) and the classes.*

3. **Results Cases**  
   ![Results_Cases.xlsx](Results_Cases.xlsx)  
   *Design, MANOVA Statistics, ML Evaluation Metrics, correlations between MANOVA and ML, verification of results.*

## Contact

If you have any questions or suggestions, feel free to reach out via:

- **Email**: [matheusc_pereira@hotmail.com](mailto:matheusc_pereira@hotmail.com)
- **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/matheuscostapereira/)
- **Lattes**: [Lattes Profile](https://lattes.cnpq.br/7025666927284220)
