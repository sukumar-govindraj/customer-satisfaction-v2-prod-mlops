h1. Predicting Customer Satisfaction Before Purchase

{toc}

h2. Overview
For a given customer's historical data, predict the review score for the next order or purchase.
Data: Brazilian E-Commerce Public Dataset by Olist (100k orders, 2016–2018) with features from order status, pricing, freight to customer location and product attributes.

h2. Purpose
This repository demonstrates how ZenML empowers your business to build and deploy ML pipelines by:

* Providing a framework and template to base your own work on
* Integrating with MLflow for experiment tracking and deployment
* Simplifying the build and deployment of production-ready ML pipelines

h2. Prerequisites & Setup

h3. Python Requirements
Clone and install dependencies:
{panel\:title=Install Dependencies}
{code\:bash}
git clone [https://github.com/zenml-io/zenml-projects.git](https://github.com/zenml-io/zenml-projects.git)
cd zenml-projects/customer-satisfaction
pip install -r requirements.txt
{code}
{panel}

h3. ZenML Dashboard (v0.20.0+)
To explore the React-based dashboard, install server dependencies and launch:
{code\:bash}
pip install zenml\["server"]
zenml up
{code}

h3. MLflow Integration
If running `run_deployment.py`, install MLflow integration and configure stack:
{code\:bash}
zenml integration install mlflow -y
zenml experiment-tracker register mlflow\_tracker --flavor=mlflow
zenml model-deployer register mlflow --flavor=mlflow
zenml stack register mlflow\_stack -a default -o default -d mlflow -e mlflow\_tracker --set
{code}

h2. Resources & References

* Blog: \[Predicting how a customer will feel before ordering|[https://blog.zenml.io/customer\_satisfaction/](https://blog.zenml.io/customer_satisfaction/)]
* Video: \[YouTube Explanation|[https://youtu.be/L3\_pFTlF9EQ](https://youtu.be/L3_pFTlF9EQ)]

h2. Solution Architecture
An end-to-end pipeline for continuous prediction & deployment, plus a Streamlit app for business consumption.

{panel\:title=Mermaid Diagram|borderStyle=solid|borderColor=#ccc}

```mermaid
flowchart LR
  subgraph Training Pipeline
    A[ingest_data] --> B[clean_data]
    B --> C[train_model]
    C --> D[evaluation]
  end
  subgraph Deployment Pipeline
    A --> E[deployment_trigger]
    E --> F[model_deployer]
  end
  subgraph Streamlit App
    F --> G[prediction_service_loader]
    G --> H[.predict()]
  end
```

{panel}

h2. Pipeline Details

h3. Training Pipeline Steps
|| Step || Description ||
\| ingest\_data | Ingest raw CSV data into a DataFrame |
\| clean\_data | Clean dataset and drop unwanted columns |
\| train\_model | Train ML model with MLflow autologging |
\| evaluation | Evaluate model, log metrics/artifacts to MLflow |

h3. Deployment Pipeline Steps
|| Step || Description ||
\| deployment\_trigger | Check MSE threshold to decide deployment |
\| model\_deployer | Deploy model as MLflow service if criteria met |


h2. Usage
Run pipelines:
{code\:bash}

# Training

python run\_pipeline.py

# Continuous deployment

python run\_deployment.py

# Stop service

git run\_deployment.py --stop-service
{code}

h2. FAQ
{panel}
|| Issue || Solution ||
\| No Step found for the name mlflow\_deployer | Delete artifact store and rerun:
{code\:bash}
zenml artifact-store describe
rm -rf PATH
{code} |
\| No Environment component named mlflow registered | Install integration:
{code\:bash}
zenml integration install mlflow -y
{code} |
{panel}

h2. License
This project is licensed under Apache 2.0. See \[LICENSE|./LICENSE].
