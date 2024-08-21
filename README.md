# Deploy Your Flask App to Kubernetes Using EKS

## Overview
This is the project starter repo for the course Server Deployment, Containerization, and Testing.

In this project we will containerize and deploy a Flask API to a Kubernetes cluster using Docker, AWS EKS, CodePipeline, and CodeBuild.

# Initial setup
## Create Stack Using aws command line

### Create an EKS cluster using CloudFormation Stack
To understand the problem in depth, we will create kubernetes cluster principals through CloudFormation

    ```sh
    aws cloudformation deploy --template-file  .\cloudformation\deployt-eks.yaml --stack-name flask-apps --tags project=flaskapp --capabilities CAPABILITY_NAMED_IAM
    ```
    ```sh
    ├── cloudformation/
    │ ├── deployt-eks.yaml
    │ └── server-parameters.json
    ```

### Create role 
    ```sh
    aws iam create-role --role-name RoleFlaskDeploy --assume-role-policy-document file://trust.json --output text --query 'Role.Arn'
    ```
## Put policy for role
    ```sh
    aws iam put-role-policy --role-name UdacityFlaskDeployCBKubectlRole --policy-name eks-describe --policy-document file://iam-role-policy.json
    ``` 

## Create stack for codepipeline
    ```sh
    aws cloudformation create-stack --stack-name codepipeline-stack   --template-body file://ci-cd-codepipeline.cfn.yml  --capabilities CAPABILITY_NAMED_IAM  --parameters ParameterKey=GitHubToken,ParameterValue=""
    ``` 

##

### Prerequisites
- Python 3.7
- pip

### Build and test the container locally

1. Clone the repository:
    ```sh
    git clone https://github.com/phan-van-thuy/flask-apps
    cd flask-apps
    ```

2. Create and activate a virtual environment:
    ```sh
    python -m venv venv

    # On Windows
    venv\Scripts\activate

    # On macOS/Linux
    source venv/bin/activate
    ```

3. Install the required dependencies:
    ```sh
    pip install -r requirements.txt
    ```
4. Run Flask App on local computer
    ```sh
    python app.py
    ```
## Project Structure
sources/
│
├── QuoteEngine/
│ ├── init.py
│ ├── IngestorInterface.py
│ ├── TextIngestor.py
│ ├── DocxIngestor.py
│ ├── PDFIngestor.py
│ ├── CSVIngestor.py
│ └── QuoteModel.py
│
├── MemeEngine/
│ ├── init.py
│ └── MemeEngine.py
│
├── _data/
│ ├── DogQuotes/
│ │ ├── DogQuotesCSV.csv
│ │ ├── DogQuotesDOCX.docx
│ │ ├── DogQuotesPDF.pdf
│ │ └── DogQuotesTXT.txt
│ ├── photos/
│ │ └── vietnam/
│ │ ├── xander_1.jpg
│ │ ├── xander_2.jpg
│ │ ├── xander_3.jpg
│ │ └── xander_4.jpg
│ └── SimpleLines/
│ ├── SimpleLines.csv
│ ├── SimpleLines.docx
│ ├── SimpleLines.pdf
│ └── SimpleLines.txt
│
├── templates/
│ ├── base.html
│ ├── meme_form.html
│ └── meme.html
│
├── app.py
├── meme.py
├── requirements.txt
└── Dockerfile
