# F-Wilks' Lambda

## Description

This Python code (with pseudocode provided in [PseudocodeFWilks.md](PseudocodeFWilks.md)) addresses the process used in the **F-Wilks' Lambda** method, as described in the paper:
- [F-Wilks' Lambda: A Hybrid Multivariate Descriptor to Enhance Feature Selection in Machine Learning Algorithms over a Priori Evaluation](link).

The code is designed in Python to perform multivariate analyses (MANOVA) and apply machine learning methods, with a focus on Feature Selection. Additionally, the paper employs techniques such as Design of Experiments (DOE) and multiobjective optimization (MO). Variable extraction was based on commonly used datasets from the [UCI Machine Learning repository](https://archive.ics.uci.edu/), and the results found can be verified in the provided spreadsheets.

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
   *Classification datasets extracted from the [UCI Machine Learning repository](https://archive.ics.uci.edu/).*

2. **Factor Datasets**  
   [Factor_Datasets.xlsx](Factor_Datasets.xlsx)  
   *Datasets with varimax factor scores (obtained from the original data) and the classes.*

3. **Results Cases**  
   [Results_Cases.xlsx](Results_Cases.xlsx)  
   *Design, MANOVA Statistics, ML Evaluation Metrics, correlations between MANOVA and ML, verification of results.*

4. **Results Case - Wine**  
   [Result_Case_Wine_Complete.xlsx](Result_Case_Wine_Complete.xlsx)  
   *Comprehensive analysis of the Wine Case, incorporating cross-validation, assessing data balancing, and evaluating feature importance.*

## Contact

If you have any questions or suggestions, feel free to reach out via:

- **Email**: [matheusc_pereira@hotmail.com](mailto:matheusc_pereira@hotmail.com)
- **LinkedIn**: [LinkedIn Profile](https://www.linkedin.com/in/matheuscostapereira/)
- **Lattes**: [Lattes Profile](https://lattes.cnpq.br/7025666927284220)
