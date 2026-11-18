# Analytics Report Insight

## Project Overview
**Analytics Report Insight** is an end-to-end data analytics and predictive modeling project designed to transform raw diagnostic records into clear, actionable business and operational intelligence. Built for clinical operations stakeholders, this project analyzes **569 breast tumor cases** to identify the primary physical predictors of malignancy, establish an interpretable two-stage triage rule, and evaluate a lightweight classification model that achieves **93.0% overall accuracy** and **92.9% malignant recall**.

### Core Objectives
1. **Identify Key Diagnostic Signals:** Determine which routine tumor measurements most strongly distinguish malignant from benign cases.
2. **Evaluate Predictive Reliability:** Assess how accurately an interpretable classification model can support diagnostic screening.
3. **Define Actionable Triage Rules:** Translate statistical findings into plain-language operational guidelines to prioritize borderline cases and reduce time-to-diagnosis.

---

## Dataset Overview
* **Dataset File:** `breast_cancer_clean.csv` (pre-cleaned dataset with zero missing values, duplicate records, or label inconsistencies)
* **Dimensions:** 569 patient records across 8 core tumor measurement features and 1 confirmed diagnosis target column
* **Target Variable (`diagnosis`):**
  * **Benign:** 357 cases (62.7%)
  * **Malignant:** 212 cases (37.3%)
* **Features Analyzed:**
  * **Size Measurements:** `perimeter_mean`, `radius_mean`, and `area_mean` — the strongest diagnostic predictors, showing strong correlations with diagnosis ($r = -0.74$, $r = -0.73$, and $r = -0.71$, respectively).
  * **Shape Irregularity Measurements:** `concavity_mean` and `compactness_mean` — secondary diagnostic signals used to differentiate borderline, average-size cases.
  * **Cellular Morphology Measurements:** `texture_mean`, `smoothness_mean`, and `symmetry_mean` — supporting metrics included in statistical comparisons and predictive modeling.
  * **Engineered Feature (`radius_category`):** Categorical tumor size tiers (`Small`, `Medium`, `Large`, and `Very Large`) created for SQL-based risk segmentation.

---

## Key Analytical Insights & Model Results
* **Tumor Size as the Primary Signal:** Malignant tumors have a 44% larger average radius and more than double the mean area of benign tumors.
* **Monotonic Risk Progression:** SQL segmentation across tumor size tiers reveals that malignancy rates rise steadily from **3.5% (Small)** to **20.0% (Medium)**, **85.2% (Large)**, and **100.0% (Very Large)**.
* **Two-Signal Separation:** Combining `radius_mean` with `concavity_mean` separates benign and malignant clusters far more cleanly in the narrow overlap zone (radius 13–15) than either metric alone.
* **Model Performance:** A Logistic Regression classifier evaluated on a stratified 80/20 test split (114 cases) correctly classified **106 of 114 cases (93.0% accuracy)** and identified **39 of 42 malignant cases (92.9% recall)**, with false negatives concentrated exclusively in the medium-size borderline range.
* **Recommended Triage Workflow:** Apply tumor size as a fast first-pass screening rule, and route medium-size borderline cases to the predictive model or human clinical review while prioritizing malignant recall over raw accuracy.

---

## Project Steps & Methodology

### 1. Business Framing & Data Ingestion
* Defined stakeholder interpretability requirements for non-technical clinical and operations teams.
* Loaded the cleaned 569-row dataset (`breast_cancer_clean.csv`) and verified schema integrity and class balance.

### 2. Statistical & Correlation Analysis
* Computed Pearson correlation coefficients between all 8 core measurements and binary diagnosis outcomes.
* Compared feature means across benign and malignant groups to quantify size and shape disparities.

### 3. SQL-Based Risk Segmentation
* Binned `radius_mean` into four interpretable size tiers (`Small`, `Medium`, `Large`, `Very Large`).
* Executed in-memory SQLite aggregation queries to compute exact malignancy rates across each size category.

### 4. Exploratory Data Visualization
* Created five stakeholder-focused visualizations using Matplotlib and Seaborn:
  * Case distribution by diagnosis (`benign` vs. `malignant`)
  * Distribution of mean radius by diagnosis highlighting the borderline overlap zone
  * Scatter plot of radius versus concavity demonstrating combined signal separation
  * Malignancy rate progression across tumor size categories
  * Confusion matrix heatmap for the Logistic Regression classifier

### 5. Predictive Modeling & Evaluation
* Partitioned the dataset using an 80/20 stratified train-test split (`random_state=42`) and standardized features with `StandardScaler`.
* Trained a `LogisticRegression` model and evaluated Accuracy, Precision, Recall, F1-score, and false-negative distribution.

### 6. Documentation & Reporting
* Compiled reproducible Python and SQL workflows in `Analytics_Report_Insights_Notebook.ipynb`.
* Summarized executive insights, visual findings, and operational recommendations in `Analytics_Report_Insights_Documentation.docx`.

---

## Tech Stack
* **Programming & Querying:** Python 3.10, SQL (SQLite)
* **Data Analysis & Manipulation:** Pandas, NumPy
* **Machine Learning:** Scikit-learn (`LogisticRegression`, `StandardScaler`, `train_test_split`)
* **Data Visualization:** Matplotlib, Seaborn
* **Environment & Documentation:** Jupyter Notebook, Microsoft Word

---

## Repository Structure
```text
Analytics-Report-Insights/
├── Analytics_Report_Insights_Notebook.ipynb       # Reproducible Python & SQL analysis, charts, and model evaluation
├── Analytics_Report_Insights_Documentation.docx   # Stakeholder-facing executive report and triage recommendations
└── README.md                                      # Project overview, methodology, and setup guide
```

---

## Getting Started
1. **Clone the repository:**
   ```bash
   git clone https://github.com/SAMEDWORK/Analytics-Report-Insights.git
   cd Analytics-Report-Insights
   ```
2. **Explore the Notebook:**
   Place `breast_cancer_clean.csv` in the project root directory and run `Analytics_Report_Insights_Notebook.ipynb` in Jupyter Notebook, VS Code, or Google Colab to reproduce all statistics, SQL queries, charts, and model metrics.
3. **Read the Executive Report:**
   Open `Analytics_Report_Insights_Documentation.docx` for the plain-language summary, visual walkthrough, and recommended clinical triage framework.
