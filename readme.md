# Automated Visa Application Prediction

An end-to-end machine learning project that predicts whether a US visa application will be approved or denied. The system follows a modular architecture covering data ingestion, validation, transformation, model training, evaluation, and deployment  with a web interface for real time predictions.

## Architecture
<img src ="assets/Visa App Pred.png  width="900" <alt="VisaArchitecture">


## Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Usage](#usage)
- [ML Pipeline Workflow](#ml-pipeline-workflow)
- [Deployment (AWS CI/CD with GitHub Actions)](#deployment-aws-cicd-with-github-actions)
- [License](#license)

## Overview

The US visa approval process involves evaluating numerous applicant attributes such as education level, job experience, wage offer, continent of origin, and more. This project trains a classification model on historical visa application data and serves predictions through a Flask-based web application.

Key highlights:

- Modular, production-ready codebase with clearly separated concerns (constants, configuration entities, artifact entities, components, and pipelines).
- Data stored and retrieved from MongoDB Atlas for scalable, cloud-based data management.
- Model artifacts stored in AWS S3 for versioning and easy retrieval.
- Dockerized application with CI/CD via GitHub Actions deploying to AWS EC2 through ECR.
- Interactive web UI built with HTML/CSS and Flask for submitting visa applications and receiving instant predictions.

## Tech Stack

| Category | Technologies |
|---|---|
| Language | Python 3.8 |
| ML/Data | scikit-learn, pandas, NumPy, imbalanced-learn |
| Database | MongoDB Atlas |
| Web Framework | Flask |
| Cloud | AWS S3, EC2, ECR, IAM |
| Containerization | Docker |
| CI/CD | GitHub Actions |
| Notebook | Jupyter Notebook (EDA & experimentation) |

## Project Structure

```
├── .github/workflows/    # GitHub Actions CI/CD pipeline
├── config/               # Configuration files (schema, model params)
├── flowcharts/           # Architecture and pipeline diagrams
├── notebook/             # Jupyter notebooks for EDA and prototyping
├── static/css/           # Frontend stylesheets
├── templates/            # HTML templates for the Flask web app
├── us_visa/              # Core ML package
│   ├── constants/        # Project-wide constants
│   ├── entity/           # Config and artifact entity definitions
│   ├── components/       # Data ingestion, validation, transformation, model trainer, evaluator, pusher
│   ├── pipeline/         # Training and prediction pipelines
│   └── utils/            # Utility and helper functions
├── app.py                # Flask application entry point
├── demo.py               # Quick demo / pipeline runner
├── Dockerfile            # Container image definition
├── requirements.txt      # Python dependencies
├── setup.py              # Package setup for pip install
└── template.py           # Project scaffolding script
```

## Getting Started

### Prerequisites

- [Anaconda](https://www.anaconda.com/) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
- Python 3.8
- MongoDB Atlas account (for data storage)
- AWS account (for model storage and deployment)

### Installation

1. **Clone the repository**

```bash
git clone https://github.com/Nihal7778/Automated-Visa-Application-Prediction-.git
cd Automated-Visa-Application-Prediction-
```

2. **Create and activate a conda environment**

```bash
conda create -n visa python=3.8 -y
conda activate visa
```

3. **Install dependencies**

```bash
pip install -r requirements.txt
```

## Environment Variables

The following environment variables must be set before running the application:

```bash
# MongoDB connection string
export MONGODB_URL="mongodb+srv://<username>:<password>@<cluster>.mongodb.net/<dbname>"

# AWS credentials (for S3 model storage)
export AWS_ACCESS_KEY_ID=<your-access-key>
export AWS_SECRET_ACCESS_KEY=<your-secret-key>
```

## Usage

### Run the web application

```bash
python app.py
```

Then open your browser and navigate to `http://localhost:8080` (or the configured port) to access the prediction form.

### Run the training pipeline

```bash
python demo.py
```

This triggers the full ML pipeline: data ingestion → validation → transformation → model training → evaluation → model pusher.

## ML Pipeline Workflow

The pipeline follows a six-stage workflow:

1. **Constants** — Centralized project constants (file paths, column names, thresholds).
2. **Config Entity** — Configuration classes that define paths, parameters, and settings for each pipeline stage.
3. **Artifact Entity** — Data classes representing the outputs (artifacts) produced by each component.
4. **Components** — Core logic for data ingestion, data validation, data transformation, model training, model evaluation, and model pushing.
5. **Pipeline** — Orchestrates components into a training pipeline and a prediction pipeline.
6. **App / Demo** — Entry points that run the pipeline or serve the Flask web application.

## Deployment (AWS CI/CD with GitHub Actions)

The project supports automated deployment to AWS using GitHub Actions:

1. **Build** the Docker image from the source code.
2. **Push** the image to AWS Elastic Container Registry (ECR).
3. **Launch** an EC2 instance (Ubuntu) and pull the image from ECR.
4. **Run** the Docker container on EC2 to serve the application.

### Required AWS IAM Policies

- `AmazonEC2ContainerRegistryFullAccess`
- `AmazonEC2FullAccess`

### EC2 Instance Setup

```bash
# Update packages
sudo apt-get update -y
sudo apt-get upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

### GitHub Actions Secrets

Configure the following secrets in your GitHub repository settings:

| Secret | Description |
|---|---|
| `AWS_ACCESS_KEY_ID` | IAM user access key |
| `AWS_SECRET_ACCESS_KEY` | IAM user secret key |
| `AWS_REGION` | AWS region (e.g., `us-east-1`) |
| `AWS_ECR_LOGIN_URI` | ECR login URI |
| `ECR_REPOSITORY_NAME` | ECR repository name |
| `MONGODB_URL` | MongoDB Atlas connection string |

