# MANOVA

# Load the spreadsheets
df <- LoadExcel("winefa.xlsx")
design_df <- LoadExcel("design1.xlsx")

# Encode the column "species (y)" to numeric values
label_encoder <- CreateEncoder()
df['y'] <- label_encoder.Transform(df['y'])

# Separate the target variable (y)
y <- df['y']

# List to store results
results <- CreateList()

# Iterate over each row in the design spreadsheet
For each row in design_df:
    # Select columns that have the value 1 in the current row of design_df
    selected_columns <- SelectColumns(row, value=1)

    # Create the subset of data with the selected columns
    X_subset <- df[selected_columns]
    X <- ConvertToValues(X_subset)
    groups <- ConvertToValues(y)

    # Number of groups and variables
    num_groups <- CountUnique(groups)
    num_vars <- GetDimension(X)[1]

    # Calculate the Error (E) and Model (H) Covariance Matrices
    group_means <- CalculateMeansByGroup(X, groups)
    grand_mean <- CalculateMean(X)

    E <- InitializeZeroMatrix(num_vars, num_vars)
    H <- InitializeZeroMatrix(num_vars, num_vars)

    For each group in Unique(groups):
        Xi <- GetSubset(X, groups == group)
        mean_g <- CalculateMean(Xi)
        E <- AddOuterProduct(E, CalculateOuterProduct(Xi, mean_g))
        H <- AddOuterProduct(H, CalculateOuterProduct(mean_g - grand_mean, mean_g - grand_mean) * Count(Xi))

    # Add a small constant to avoid singularity issues
    E <- AddSmallConstant(E, 1e-10)

    # Calculate eigenvalues of E^-1 * H
    E_inv <- Invert(E)
    eigenvalues <- CalculateEigenvalues(E_inv * H)

    # Calculate Wilks' Lambda
    wilks_lambda <- Determinant(E) / Determinant(E + H)
    
    # Calculate Lawley-Hotelling Trace
    lawley_hotelling_trace <- Sum(eigenvalues)
    
    # Calculate Pillai's Trace
    H_plus_E_inv <- Invert(H + E)
    pillai_trace <- Sum(Diagonal(H * H_plus_E_inv))
    
    # Calculate F-Statistics
    p <- num_vars
    q <- num_groups - 1
    v <- GetDimension(X)[0] - p - q - 1
    s <- Min(p, q)
    m <- 0.5 * (Abs(p - q) - 1)
    n <- 0.5 * (v - p - 1)
    U <- Sum(eigenvalues)
    
    # Calculate F
    F <- 2 * (s * n + 1) * (U / (s^2 * (2 * m + s + 1)))

    # Ensure only real values are displayed
    lawley_hotelling_trace <- RealPart(lawley_hotelling_trace)
    pillai_trace <- RealPart(pillai_trace)
    F <- RealPart(F)
    
    AddToList(results, (CurrentIndex + 1, wilks_lambda, lawley_hotelling_trace, pillai_trace, F, Join(selected_columns))

# Create a DataFrame to store the results
results_df <- CreateDataFrame(results, columns=['Iteration', 'Wilks', 'Lawley-Hotelling', 'Pillai\'s', 'F', 'Variables'])

# Save the results DataFrame to the Excel spreadsheet
SaveExcel(results_df, 'analisevariada_MANOVA.xlsx', sheet_name='Manova')

# StratifiedK-Fold

# Load the original spreadsheet and the design spreadsheet
df <- LoadExcel("winefa.xlsx")
design_df <- LoadExcel("design1.xlsx")

# Create a copy of the original DataFrame
df_copy <- Copy(df)

# Encode the column "class (y)" to numeric values
label_encoder <- CreateEncoder()
df_copy['y'] <- label_encoder.Transform(df_copy['y'])

# Separate the independent variables (X) and the target variable (y)
X <- RemoveColumn(df_copy, 'y')
y <- df_copy['y']

# List to store the mean of results from each iteration
all_means <- CreateList()

# Iterate over each row in the design spreadsheet
For each row in design_df:
    # Select columns that have the value 1 in the current row of design_df
    selected_columns <- SelectColumns(row, value=1)
    
    # Create the subset of data with the selected columns
    X_subset <- X[selected_columns]

    # List of models to evaluate
    models <- {
        'Logistic Regression': CreateLogisticRegression(),
        'Linear Discriminant Analysis': CreateLinearDiscriminantAnalysis(),
        'K-Nearest Neighbors': CreateKNN(),
        'Support Vector Machine': CreateSVM(),
        'Decision Tree': CreateDecisionTree(),
        'Random Forest': CreateRandomForest(),
        'Gradient Boosting': CreateGradientBoosting(),
        'XGBoost': CreateXGBoost()
    }
    
    # Dictionary to store results for this iteration
    results <- {model_name: {metric: [] for metric in ['accuracy', 'precision', 'recall', 'f1-score', 'logloss']} for model_name in models}
    
    # Stratified K-Fold with 5 folds
    skf <- CreateStratifiedKFold(n_splits=5, shuffle=True, random_state=i)
    
    For each (train_index, test_index) in skf.split(X_subset, y):
        X_train_skf, X_test_skf <- X_subset.iloc[train_index], X_subset.iloc[test_index]
        y_train_skf, y_test_skf <- y.iloc[train_index], y.iloc[test_index]

        For each (model_name, model) in models:
            # Train the model
            model.Fit(X_train_skf, y_train_skf)
            
            # Make predictions
            y_pred_skf <- model.Predict(X_test_skf)
            y_prob_skf <- model.PredictProba(X_test_skf) if hasattr(model, "PredictProba") else None
            
            # Calculate metrics
            accuracy_skf <- CalculateAccuracy(y_test_skf, y_pred_skf)
            class_report_dict_skf <- GenerateClassificationReport(y_test_skf, y_pred_skf, output_dict=True)
            
            # Adjust the number of classes for log_loss
            logloss_value <- CalculateLogLoss(y_test_skf, y_prob_skf, labels=Unique(y)) if y_prob_skf is not None else None
            
            # Store the stratified results
            results[model_name]['accuracy'].append(accuracy_skf)
            results[model_name]['precision'].append(class_report_dict_skf['weighted avg']['precision'])
            results[model_name]['recall'].append(class_report_dict_skf['weighted avg']['recall'])
            results[model_name]['f1-score'].append(class_report_dict_skf['weighted avg']['f1-score'])
            results[model_name]['logloss'].append(logloss_value)
    
    # Calculate the mean results of Stratified K-Fold for each model
    iteration_means = {}
    For each (model_name, metrics) in results:
        model_abbreviation = {
            'Logistic Regression': 'LR',
            'Linear Discriminant Analysis': 'LDA',
            'K-Nearest Neighbors': 'KNN',
            'Support Vector Machine': 'SVM',
            'Decision Tree': 'DT',
            'Random Forest': 'RF',
            'Gradient Boosting': 'GB',
            'XGBoost': 'XGB'
        }[model_name]
        
        iteration_means[f"{model_abbreviation}(A)"] = CalculateMean(metrics['accuracy'])
        iteration_means[f"{model_abbreviation}(P)"] = CalculateMean(metrics['precision'])
        iteration_means[f"{model_abbreviation}(R)"] = CalculateMean(metrics['recall'])
        iteration_means[f"{model_abbreviation}(F1)"] = CalculateMean(metrics['f1-score'])
        iteration_means[f"{model_abbreviation}(LogLoss)"] = CalculateMean(metrics['logloss'])
    
    # Add the mean results of this iteration to the overall list
    AddToList(all_means, iteration_means)

# Create a final DataFrame with results from all iterations
final_df <- CreateDataFrame(all_means)

# Reorder the DataFrame columns to the desired order
final_df <- ReorderColumns(final_df,
    ['LR(A)', 'LR(P)', 'LR(R)', 'LR(F1)', 'LR(LogLoss)',
     'LDA(A)', 'LDA(P)', 'LDA(R)', 'LDA(F1)', 'LDA(LogLoss)',
     'KNN(A)', 'KNN(P)', 'KNN(R)', 'KNN(F1)', 'KNN(LogLoss)',
     'SVM(A)', 'SVM(P)', 'SVM(R)', 'SVM(F1)', 'SVM(LogLoss)',
     'DT(A)', 'DT(P)', 'DT(R)', 'DT(F1)', 'DT(LogLoss)',
     'RF(A)', 'RF(P)', 'RF(R)', 'RF(F1)', 'RF(LogLoss)',
     'GB(A)', 'GB(P)', 'GB(R)', 'GB(F1)', 'GB(LogLoss)',
     'XGB(A)', 'XGB(P)', 'XGB(R)', 'XGB(F1)', 'XGB(LogLoss)']
)

# Save the final DataFrame to the Excel spreadsheet
SaveExcel(final_df, 'analisevariada_stratifiedkfold.xlsx', sheet_name='Stratified KFold')
