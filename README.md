# Unemployment Rate Forecasting with SVM and Neural Networks

Comparative analysis of Support Vector Machines and Artificial Neural Networks for predicting unemployment rates using socioeconomic indicators.

## Overview

This project evaluates two machine learning approaches for forecasting unemployment rates in Australia using nearly 40 years of quarterly data (1981-2020). The analysis compares Support Vector Machine (SVM) and Artificial Neural Network (ANN) performance while exploring the impact of different model architectures and hyperparameters.

## Problem Statement

Unemployment is a critical socioeconomic indicator affecting poverty levels, public welfare systems, and population health outcomes. This project assesses whether seven socioeconomic predictors can reliably estimate unemployment rates using advanced machine learning techniques.

## Dataset

- **Time Period:** June 1981 - September 2020 (158 quarterly observations)
- **Target Variable:** Unemployment Rate (%)
  - Range: 4.1% - 11.133%
  - Mean: 6.852%
- **Predictor Variables (7 socioeconomic indicators):**
  - GDP (% change)
  - Government Final Expenditure (% change)
  - All Sector Final Expenditure (% change)
  - Term of Trade Index (%)
  - All Group CPI
  - Job Vacancies (x1000) - *strongest predictor*
  - Estimated Resident Population (x1000)

**Data Preprocessing:**
- Missing value imputation using polynomial interpolation (population) and Stine interpolation (job vacancies)
- Feature scaling for neural network compatibility
- Train-test split: Data through 2017 (training), March 2018-September 2020 (testing)

## Methodology

### Support Vector Machine (SVM)
- **Kernel:** Radial Basis Function (RBF)
- **Hyperparameter Tuning:** 
  - Grid search over cost (C) and sigma parameters
  - 100-fold cross-validation with 10 repeats
  - Optimal parameters selected via repeated CV
- **Rationale:** SVM chosen for handling complex non-linear relationships with numeric predictors in small-to-medium datasets

### Artificial Neural Network (ANN)
- **Architecture:** 2 hidden layers (7 neurons, 1 neuron)
- **Activation:** Default (logistic)
- **Training:** Maximum 10 million steps
- **Experiments:** Tested varying numbers of layers (1-10) and neurons (1-10) to assess overfitting/underfitting

## Key Results

### Model Performance

| Model | Dataset | RMSE | R² | MAE |
|-------|---------|------|-----|-----|
| **SVM** | Training | 0.504 | 0.923 | 0.314 |
| **SVM** | Testing | 0.382 | **0.941** | 0.331 |
| **ANN** | Training | 0.237 | **0.983** | 0.187 |
| **ANN** | Testing | 0.455 | 0.961 | 0.422 |

### Key Findings

- **SVM Performance:** 
  - R² of 0.92 on training data indicates excellent fit
  - Better performance on test data (R² = 0.94) likely due to small test set
  - MAE of 0.31-0.33 represents ~4.5-4.8% error relative to mean unemployment rate

- **ANN Performance:**
  - Exceptional training accuracy (R² = 0.98) with minimal error
  - Strong generalization to test data (R² = 0.96)
  - Risk of overfitting with increased neurons/layers
  - Optimal architecture: 2 layers with 7 and 1 neurons respectively

- **Architecture Experiments:**
  - Increasing layers with single neuron showed underfitting
  - Increasing neurons (>4) showed overfitting on test data
  - Training time increased dramatically with architecture complexity

### Predictor Importance

Job Vacancies showed strongest correlation with unemployment rate, followed by Estimated Resident Population and All Group CPI. Government Final Expenditure showed minimal correlation.

## Technologies Used

- **Language:** R (4.x)
- **Key Packages:**
  - `e1071` - SVM implementation
  - `neuralnet` - Neural network modeling
  - `caret` - Model training, tuning, and evaluation
  - `imputeTS` - Time series imputation
  - `tidyverse` - Data manipulation and visualization

## Files

- `unemployment_analysis.Rmd` - Full R Markdown analysis with code
- `unemployment_report.pdf` - Comprehensive report with results and discussion

## Running the Analysis
```R
# Install required packages
install.packages(c("tidyverse", "e1071", "neuralnet", "caret", "imputeTS"))

# Open and knit the RMarkdown file
rmarkdown::render("unemployment_analysis.Rmd")
```

**Note:** Some model training steps are computationally intensive and may take several hours. Pre-trained models are saved as RDS files to avoid re-running.

## Model Comparison

| Aspect | SVM | ANN |
|--------|-----|-----|
| **Training Time** | 0.01 seconds | 9.41 seconds |
| **Tuning Time** | ~4 minutes | Not performed |
| **Cross-Validation** | ✓ (100-fold, 10 repeats) | ✗ |
| **Interpretability** | Low (radial kernel) | Very Low (black box) |
| **Best R²** | 0.941 (test) | 0.983 (train) |
| **Generalization** | Excellent | Good (some overfitting risk) |

## Limitations & Future Work

### Current Limitations
- Small test dataset (11 observations)
- No cross-validation performed for ANN
- Limited hyperparameter exploration for ANN
- Architecture selection was semi-random rather than systematic

### Suggested Improvements

**Methodology:**
- Implement cross-validation for ANN model
- Systematic hyperparameter optimization for both models
- Explore ensemble methods
- Test additional architectures (LSTM, GRU for time series)

**Data:**
- Gather more observations to increase test set size
- Investigate additional predictors (e.g., employment by sector, wage growth)
- Validate models on data from other countries
- Consider seasonal decomposition

## Computational Details

- **Environment:** R 4.x
- **Training Machine:** Intel i7-9750H, 32GB RAM
- **Parallelization:** Not implemented (small dataset)
- **Hyperparameter Search Space:** 
  - SVM: 6 cost values × 6 sigma values = 36 combinations
  - Total tuning time: ~255 seconds

## References

Full references available in PDF report.

## License

MIT License
