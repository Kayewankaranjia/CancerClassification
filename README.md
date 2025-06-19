# CancerClassifier

## 🧪 Solution Overview

This solution includes three primary components—**Data Ingestion**, **Model Training**, and **Prediction**—all orchestrated via Databricks Jobs and tracked using MLflow.

---

### 📥 Data Ingestion

- Reads input data from a CSV file.
- Generates a data profiling report using [`ydata-profiling`](https://github.com/ydataai/ydata-profiling) for exploratory data analysis.
- Performs feature engineering steps:
  - **Log transformation** to reduce skewness.
  - **Outlier clipping** to handle extreme values.
  - **Derived features** for additional signal.
- Splits the dataset by holding out the **last 15 days** of data for future prediction use.

---

### 🤖 Model Training

- Uses the cleaned dataset from the ingestion step.
- Trains a **LightGBM** model.
- Evaluates model performance through:
  - ROC Curve
  - KDE Curve
  - Confusion Matrix
- Stores the trained model, evaluation metrics, and plots in an **MLflow experiment** as artifacts.

---

### 🔮 Prediction

- Uses the previously held-out data for inference.
- Loads a trained model (currently using a **hardcoded version**; a model promotion strategy is recommended for production).
- Outputs predictions for the held-out period.

---

### ⚙️ Configuration and Orchestration

- `**databricks.yml**`:
  - Defines environment configurations for **development (`dev`)** and **production (`prod`)**.
- `**resources/CancerClassifier.job.yml**`:
  - Defines two orchestrated jobs/pipelines:
    - **Train Job**: Executes both ingestion and training stages.
    - **Prediction Job**: Executes the prediction step independently.

---

## 📊 Data Source

- [Breast Cancer Coimbra Dataset on Kaggle](https://www.kaggle.com/datasets/yasserhessein/breast-cancer-coimbra-data-set)

---

## 🚀 Getting Started with Databricks Deployment

1. **Install Databricks CLI**  
   [Instructions here](https://docs.databricks.com/dev-tools/cli/databricks-cli.html)

2. **Authenticate with your Databricks workspace**:
   ```bash
   databricks configure

3. **Deploy to Development**:

   ```bash
   databricks bundle deploy --target dev
   ```

   > `"dev"` is the default target, so `--target` is optional.

4. **Deploy to Production**:

   ```bash
   databricks bundle deploy --target prod
   ```

5. **Run a job/pipeline manually**:

   ```bash
   databricks bundle run
   ```

6. **Optional Developer Tooling**
   Install the Databricks VS Code Extension:
   [Databricks VS Code Extension](https://docs.databricks.com/dev-tools/vscode-ext.html)

7. **Databricks Asset Bundles Documentation**
   Learn more about bundles and CI/CD configuration:
   [Databricks Asset Bundles](https://docs.databricks.com/dev-tools/bundles/index.html)

---

## ✅ TODO

* [ ] Add a CI/CD pipeline using **Terraform** or **Azure DevOps** to automate deployment, testing, and promotion workflows.