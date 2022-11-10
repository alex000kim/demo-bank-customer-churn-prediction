# E2E ML project using open-source MLOps tools

## Problem Description and Dataset
This dataset contains 10,000 records, each of which corresponds to a different bank's user. The target is `Exited`, a binary variable that describes whether the user decided to leave the bank. There are row and customer identifiers, four columns describing personal information about the user (surname, location, gender and age), and some other columns containing information related to the loan (such as credit score, the current balance in the user's account and whether they are an active member among others).

Dataset source: https://www.kaggle.com/datasets/filippoo/deep-learning-az-ann

## Use Case
The objective is to train an ML model that returns the probability of a customer churning. This is a binary classification task, therefore F1-score is a good metric to evaluate the performance of this dataset as it weights recall and precision equally, and a good retrieval algorithm will maximize both precision and recall simultaneously.


## Setup
Python 3.8+ is required to run code from this repo.
```bash
$ python -m venv .venv
$ source .venv/bin/activate
$ pip install -U pip
$ pip install -r requirements.txt
```

### Rerun pipeline
```bash
$ dvc pull
$ dvc repro
```

### Run and test web API locally
Run:
```bash
$ uvicorn src.app.main:app --host 0.0.0.0 --port 8080
```

Test:
```bash
curl -X 'POST' 'http://0.0.0.0:8080/predict' -H 'accept: application/json' -H 'Content-Type: application/json' -d '[
{
        "CreditScore": 619,
        "Age": 42,
        "Tenure": 2,
        "Balance": 0,
        "NumOfProducts": 1,
        "HasCrCard": 1,
        "IsActiveMember": 1,
        "EstimatedSalary": 101348.88
},
{
        "CreditScore": 699,
        "Age": 39,
        "Tenure": 21,
        "Balance": 0,
        "NumOfProducts": 2,
        "HasCrCard": 0,
        "IsActiveMember": 0,
        "EstimatedSalary": 93826.63
}
]'
```

### Run and test web API locally with Docker
Build Docker image:
```bash
docker build . -t churn-api-image
```
Run web API:
```bash
docker run -p 8080:8080 churn-api-image
```
Test:
```bash
curl -X 'POST' 'http://0.0.0.0:8080/predict' -H 'accept: application/json' -H 'Content-Type: application/json' -d '[
{
        "CreditScore": 619,
        "Age": 42,
        "Tenure": 2,
        "Balance": 0,
        "NumOfProducts": 1,
        "HasCrCard": 1,
        "IsActiveMember": 1,
        "EstimatedSalary": 101348.88
},
{
        "CreditScore": 699,
        "Age": 39,
        "Tenure": 21,
        "Balance": 0,
        "NumOfProducts": 2,
        "HasCrCard": 0,
        "IsActiveMember": 0,
        "EstimatedSalary": 93826.63
}
]'
```