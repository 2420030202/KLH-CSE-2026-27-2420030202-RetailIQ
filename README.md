# RetailIQ

![RetailIQ logo](assets/retailiq_logo.svg)

**An interactive retail analytics and machine-learning application**

[Source repository](https://github.com/2420030202/KLH-CSE-2026-27-2420030202-RetailIQ)

## Project overview

RetailIQ is a Python application built with Streamlit for exploring tabular data, including retail datasets. Users upload a CSV or Excel file, review dataset quality and descriptive analysis, and run suitable predictive or unsupervised workflows. Results and recommendations depend on the uploaded data and the task it can support.

The application combines data profiling, cleaning, visualization, model evaluation, and model explanations in one interface. It is a local, interactive decision-support project; this repository does not establish a deployed service or guarantee business outcomes.

## Problem statement

Raw retail data can contain missing values, duplicates, inconsistent column names, mixed data types, and patterns that are difficult to identify by inspecting tables alone. Analysts need an accessible way to understand a dataset before selecting a useful analysis, while predictive results should be evaluated and interpreted in the context of their input data.

RetailIQ addresses this workflow by profiling and preparing uploaded tabular data, presenting analytical views, and exposing modeling and explanation tools when the data is suitable.

## Project objectives

- Make it easier to inspect unfamiliar tabular datasets.
- Surface missing values, duplicates, column types, and other data-quality signals.
- Provide logged, conservative cleaning operations.
- Support exploratory, statistical, and visual analysis.
- Offer classification, regression, clustering, forecasting, and anomaly-detection workflows where the data supports them.
- Compare model results and provide feature or prediction explanations where available.
- Present data-grounded recommendations as decision support, not as guaranteed business advice.

## Implemented features

- **Dataset upload:** CSV, XLSX, and XLS files.
- **Dataset profiling:** dimensions, column types and roles, missing values, duplicates, unique values, and date-like fields.
- **Data preparation:** standardizes column names, removes fully empty columns and exact duplicate rows, trims text, and converts detected date-like fields. The application displays cleaning information.
- **Exploration:** dataset summaries, distributions, relationships, category breakdowns, statistical analysis, and Plotly visualizations.
- **Supervised modeling:** classification and regression baselines, automatic target/task suggestions, task-aware metrics, and feature-importance information.
- **Additional analysis:** hybrid modeling, K-Means clustering, Isolation Forest anomaly detection, and lag-based forecasting.
- **Explainability:** SHAP and LIME workflows for supported models, plus permutation-based feature importance in predictive workflows.
- **Decision support:** rule-based and analysis-linked recommendations, customer-risk analysis for suitable labeled outcomes, and deterministic data-story/insight text.
- **Export and evaluation:** filtered data can be downloaded as CSV; research-evaluation runs write CSV artifacts under `results/`.

The availability and validity of a result depend on the dataset, target, class distribution, feature types, and model compatibility. See [Dataset requirements and limitations](#dataset-requirements-and-limitations).

## Application navigation

The current application defines 26 navigation destinations. They are grouped below by purpose for readability; the sidebar uses its own group labels.

### Dataset workspace

- Executive Overview
- Overview
- Dataset Understanding
- Data
- Data Quality
- Data Cleaning

### Exploration and communication

- EDA & Statistics
- Feature Engineering
- Visual Analytics
- Breakdown
- Relationships
- Data Story

### Predictive modeling and explanations

- Predictive Analytics
- Classification
- Regression
- Model Comparison
- Hybrid Model
- Explainable AI
- Customer Risk Analysis

### Unsupervised and time-series analysis

- Clustering
- Forecasting
- Anomaly Detection

### Recommendations, governance, and research

- Recommendations
- Methodology
- Responsible AI
- Research Evaluation

## Technology stack

Runtime packages are declared in [`requirements.txt`](requirements.txt). The development/test dependency file, [`requirements-dev.txt`](requirements-dev.txt), includes those packages and pytest.

| Technology | Use in this repository |
|---|---|
| Python | Application and analytics code |
| Streamlit | Interactive user interface and session state |
| Pandas, NumPy | Tabular data processing and numerical operations |
| Plotly | Interactive charts |
| scikit-learn | Preprocessing, classification, regression, clustering, anomaly detection, forecasting models, and evaluation |
| SciPy | Statistical tests and analysis |
| SHAP, LIME | Model explanation workflows where supported |
| OpenPyXL, xlrd | Excel workbook input |
| pytest | Automated tests (development dependency) |
| GitHub Actions | Continuous integration on pushes and pull requests |
| Docker | Optional container build defined by the repository Dockerfile |

The application uses Streamlit and custom CSS; it does not use React. SQL and Power BI are not implemented application integrations.

## System architecture

- **UI and orchestration — [`app.py`](app.py):** configures the Streamlit interface, navigation, upload flow, session state, and page rendering.
- **Profiling and cleaning — [`src/analyzer.py`](src/analyzer.py), [`src/cleaner.py`](src/cleaner.py):** inspect dataset structure and apply cleaning operations.
- **Recommendations and visualizations — [`src/recommender.py`](src/recommender.py), [`src/visualizer.py`](src/visualizer.py):** produce data-shape-based suggestions and reusable Plotly charts.
- **Predictive analytics — [`src/ml_analyzer.py`](src/ml_analyzer.py), [`src/dav_support.py`](src/dav_support.py), [`src/advanced_ml.py`](src/advanced_ml.py):** provide task detection, preprocessing, model fitting/evaluation, hybrid models, explainability, forecasts, anomaly analysis, and related insights.
- **Research evaluation — [`src/research_suite.py`](src/research_suite.py), [`scripts/run_research_suite.py`](scripts/run_research_suite.py):** prepare and evaluate benchmark datasets and write CSV results.

See the [detailed architecture diagram](docs/architecture.md) for a visual overview of these components and their data flows.

There is no database layer. The active upload and analysis state are held in Streamlit session state; they are not stored as a persistent application database. The application can export filtered data as CSV, and the research suite writes its evaluation artifacts to disk under `results/`.

## Data flow

1. A user uploads a CSV, XLSX, or XLS file through the Streamlit UI.
2. RetailIQ reads the file into a Pandas DataFrame and creates an upload fingerprint for session handling.
3. The raw DataFrame is profiled, a cleaning plan is applied, and the cleaned DataFrame is profiled.
4. Raw/cleaned data and analysis state are held in the Streamlit session while the user navigates and filters the dataset.
5. A selected analysis uses the relevant columns and task-specific preprocessing. Modeling, explanations, recommendations, and charts are produced only where supported.
6. Users can download the filtered view as CSV. Research-suite evaluations save their own CSV datasets/results under the configured repository folders.

## Installation

Use Python 3.11 to match the GitHub Actions test configuration. The Dockerfile currently uses Python 3.12; other Python versions are not declared as tested in the repository.

Clone the repository:

```bash
git clone https://github.com/2420030202/KLH-CSE-2026-27-2420030202-RetailIQ.git
cd KLH-CSE-2026-27-2420030202-RetailIQ
```

### Windows — PowerShell

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If PowerShell does not recognize `py`, use the Python 3.11 interpreter command available on your system to create the environment. In Command Prompt, activate with:

```bat
.venv\Scripts\activate
```

### macOS and Linux

```bash
python3.11 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Run these commands from the repository root. `RUN_RETAILIQ.bat` is an additional Windows launcher.

## Run RetailIQ

With the virtual environment activated, from the repository root:

```bash
python -m streamlit run app.py
```

Streamlit prints a local address in the terminal. Open that address in a browser and upload a dataset to begin.

### Optional: Docker

From the repository root:

```bash
docker build -t retailiq .
docker run --rm -p 8501:8501 retailiq
```

The Dockerfile exposes the Streamlit application on port 8501.

## Tests

Install the development dependencies and run the same compilation and pytest commands used by CI. Run them from the repository root:

```bash
python -m pip install -r requirements-dev.txt
python -m compileall -q app.py src scripts tests
python -m pytest -ra --tb=short
```

`pytest.ini` points pytest at `tests/` and adds the repository root to the import path.

## GitHub Actions CI

The workflow at [`.github/workflows/ci.yml`](.github/workflows/ci.yml) runs on pushes and pull requests. Its matrix covers `ubuntu-latest` and `windows-latest` with Python 3.11. Each job installs `requirements-dev.txt`, compiles the Python sources, and runs pytest from the repository root. The workflow is CI configuration, not evidence that the project is deployed.

## Machine-learning models and explainability

The implemented model families include:

- **Classification:** Logistic Regression, Decision Tree, Random Forest, and Gradient Boosting.
- **Regression:** Linear Regression, Decision Tree Regressor, Random Forest Regressor, and Gradient Boosting Regressor.
- **Hybrid modeling:** a stacking ensemble; classification can use soft voting when the training data cannot support the stacking cross-validation split.
- **Clustering:** K-Means.
- **Anomaly detection:** Isolation Forest.
- **Forecasting:** lag-feature Ridge and Gradient Boosting regression models, with chronological holdout evaluation and recursive forecasts.

Supervised evaluation reports task-appropriate metrics, including Accuracy, weighted Precision/Recall/F1 for classification and MAE, RMSE, and R² for regression. Some supported classification workflows also calculate ROC-AUC and PR-AUC. The exact metrics vary by workflow. SHAP and LIME are used where the model and input data are compatible. Permutation importance measures the change in validation performance after shuffling input features; it is not a causal explanation.

These are analytical baselines and decision-support methods. Their presence does not establish that any model is optimal or suitable for production use.

## Dataset requirements and limitations

- The upload interface accepts CSV, XLSX, and XLS. Excel support uses OpenPyXL and xlrd.
- The application is schema-adaptive, but analyses need relevant columns. Supervised learning requires a usable target and predictors; automatic target selection is based on conservative name/task heuristics and may need user confirmation.
- The supervised workflow recommends at least 30 usable labeled rows. Classification needs multiple classes, and class distribution can limit splitting, cross-validation, and model fitting. Regression requires at least five distinct target values.
- Clustering requires at least 20 rows and two varying numeric features in the implemented evaluation helper.
- Forecasting needs a detectable date column, a numeric measure, and enough distinct time periods for lag features; the code checks for at least 15 distinct dates and 12 complete lag observations after aggregation.
- SHAP/LIME explanations are conditional on supported models, data shapes, and installed packages. A model may run when a particular explanation is unavailable.
- No SQL database, remote data connector, authentication service, or persistent upload store is implemented. Working upload state is session-scoped.
- Model outputs and recommendations depend on data quality, representativeness, target definitions, and the evaluation design. They should be reviewed by a person with relevant domain context.
- Do not upload confidential, sensitive, or unauthorized data.

## Documentation and research resources

The following existing documentation is retained and linked for deeper context:

- [Application overview and legacy setup notes](docs/README_APPLICATION.md) — contains useful background, but includes older version/feature statements that may not describe the current code.
- [Frontend notes](docs/README_FRONTEND.md)
- [Final setup notes](docs/FINAL_SETUP.md)
- [Data and analytics methodology](docs/methodology.md)
- [Machine-learning methodology](docs/ml_methodology.md)
- [Advanced ML methodology](docs/advanced_ml_methodology.md)
- [Research evaluation methodology](docs/research_evaluation.md)
- [Research dataset protocol](docs/research_dataset_protocol.md)
- [DAV alignment self-assessment](docs/DAV_COMPLIANCE_REPORT.md) — explicitly not an external certification.
- [Backend and DAV audit report](docs/RETAILIQ_BACKEND_AND_DAV_AUDIT_REPORT.md) — a scoped audit with historical validation results and limitations.
- [Dataset directory notes](data/README.md) and [results directory notes](results/README.md)
- [Project roadmap image](docs/Batch_13_DAV_ProjectRoadmap_AnalyticsWorkflow.png) — planning context; items shown in the roadmap are not necessarily implemented.

The repository also contains academic reports and presentation documents under `docs/`. Their presence alone does not establish that every proposed capability is implemented.

## Project structure

```text
RetailIQ/
├── .github/workflows/ci.yml       # GitHub Actions CI
├── app.py                         # Streamlit entry point and UI orchestration
├── assets/retailiq_logo.svg       # Existing project logo
├── data/                          # Sample/demo datasets and dataset notes
├── docs/                          # Application, methodology, research, and audit docs
├── research_datasets/
│   ├── advanced/                  # Synthetic forecast/anomaly evaluation data
│   ├── domain/                    # Retail-domain datasets
│   ├── public/                    # Public benchmark dataset files
│   ├── synthetic/                 # Synthetic retail datasets
│   └── dataset_manifest.csv
├── results/                       # Research/evaluation CSVs and execution video
├── scripts/run_research_suite.py  # Research-suite command-line entry point
├── src/                           # Profiling, cleaning, ML, and research modules
├── tests/                         # Automated tests
├── Dockerfile
├── pytest.ini
├── requirements.txt
├── requirements-dev.txt
└── RUN_RETAILIQ.bat
```

## Current limitations and possible future improvements

Current limitations include dataset-dependent modeling and explanations, no persistent application database or remote data source, and historical documentation that still contains outdated feature/version statements. The checked-in DAV documents are self-assessments; formal compliance cannot be established without the applicable official rubric.

Possible future work, not represented as implemented features, includes reconciling legacy documentation with the current source, documenting dataset provenance and licenses alongside each dataset, expanding automated UI/integration coverage, and evaluating additional persistence or data-source options if project requirements call for them.

## Project and contribution information

Repository: [2420030202/KLH-CSE-2026-27-2420030202-RetailIQ](https://github.com/2420030202/KLH-CSE-2026-27-2420030202-RetailIQ)

This is a team academic project for the 2026–2027 academic year. The existing project record lists:

| University ID | Team member |
|---|---|
| 2420030246 | V. Maneesh Kumar Reddy |
| 2420030202 | K. Kumar Satya Sai |
| 2420030234 | Y. Guna Veera Sri Amith |
| 2420030496 | P. Akhil Kumar Reddy |

**Project supervisor:** Dr. K Swapnika

This README does not assign individual ownership of modules or claim that any one team member implemented the entire project. Contributions should be described using verified work history and clearly scoped changes.
