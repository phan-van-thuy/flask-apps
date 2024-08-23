# Meme Generator Project

## Overview
This project generates memes with quotes using a web application and command line interface. It supports ingesting quotes from various file formats including TXT, DOCX, PDF, and CSV, and generates images with captions using the Pillow library.

## Setup Instructions
# Create Stack Using AWS
## Create role 
aws iam create-role --role-name RoleFlaskDeploy --assume-role-policy-document file://trust.json --output text --query 'Role.Arn'
## Put policy for role
aws iam put-role-policy --role-name UdacityFlaskDeployCBKubectlRole --policy-name eks-describe --policy-document file://iam-role-policy.json
## Create Cluster Kubernetes using CloudFormation stack
aws cloudformation deploy --template-file  ./cloudformation/deployt-eks.yaml --stack-name flask-apps --tags project=flaskapp --capabilities CAPABILITY_NAMED_IAM
## Create stack for code pipeline
aws cloudformation create-stack --stack-name codepipeline-stack   --template-body file://ci-cd-codepipeline.cfn.yml  --capabilities CAPABILITY_NAMED_IAM  --parameters ParameterKey=GitHubToken,ParameterValue=""

### Prerequisites
- Python 3.6+
- pip

### Installation

1. Clone the repository:
    ```sh
    git clone https://github.com/phan-van-thuy/advanced-python-project-02.git
    cd advanced-python-project-02
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

### Running the Web Application

1. Start the Flask server:
    ```sh
    python main.py
    ```

2. Open a web browser and navigate to `http://127.0.0.1:8080`.

### Running the Web Application using docker
ADD .env_file for testing local run app flask
create file .env_file
add: 
    JWT_SECRET='myjwtsecret'
    LOG_LEVEL=DEBUG

1. Build image from Dockerfile
    ```sh
    docker build . -t flask:v1 
    ```
2. Run flask app on docker
    ```sh
    docker run --name flaskapp --env-file=.env_file -p 80:8080 flask:v1 
    ```

## Project Structure
### The Docker image should run and endpoints respond.

Administrator@DESKTOP-2361KFI MINGW64 ~/Desktop/AWS-Lerning/Flask-App-EKS/sources (master)
$ curl --request GET 'http://localhost:80/'
"Healthy"

Administrator@DESKTOP-2361KFI MINGW64 ~/Desktop/AWS-Lerning/Flask-App-EKS/sources (master)
$ curl --request GET 'http://localhost:8080/'
curl: (7) Failed to connect to localhost port 8080 after 2254 ms: Couldn't connect to server

Administrator@DESKTOP-2361KFI MINGW64 ~/Desktop/AWS-Lerning/Flask-App-EKS/sources (master)
$ export TOKEN=`curl --data '{"email":"abc@xyz.com","password":"WindowsPwd"}' --header "Content-Type: application/json" -X POST localhost:80/auth  | jq -r '.token'`
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   217  100   170  100    47   5667   1566 --:--:-- --:--:-- --:--:--  7482

Administrator@DESKTOP-2361KFI MINGW64 ~/Desktop/AWS-Lerning/Flask-App-EKS/sources (master)
$ echo $TOKEN
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE3MjU1OTI1OTQsIm5iZiI6MTcyNDM4Mjk5NCwiZW1haWwiOiJhYmNAeHl6LmNvbSJ9.9jxktWyjDaHrybLScawtmlxUmFelkIFwY-3gypdd2uA

Administrator@DESKTOP-2361KFI MINGW64 ~/Desktop/AWS-Lerning/Flask-App-EKS/sources (master)
$ curl --request GET 'http://localhost:80/contents' -H "Authorization: Bearer ${TOKEN}" | jq .
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    58  100    58    0     0   1437      0 --:--:-- --:--:-- --:--:--  1450
{
  "email": "abc@xyz.com",
  "exp": 1725592594,
  "nbf": 1724382994
}

Administrator@DESKTOP-2361KFI MINGW64 ~/Desktop/AWS-Lerning/Flask-App-EKS/sources (master)
$ docker ps
CONTAINER ID   IMAGE      COMMAND                   CREATED          STATUS          PORTS                  NAMES
707179a5736e   flask:v1   "gunicorn -b :8080 m…"   17 minutes ago   Up 17 minutes   0.0.0.0:80->8080/tcp   sharp_cannon

Administrator@DESKTOP-2361KFI MINGW64 ~/Desktop/AWS-Lerning/Flask-App-EKS/sources (master)
$ 

## The API successfully runs from EKS.

### URL OF SVC EXTERNAL-IP FLASK APP
    aa68fcfd9313c4de2aff8d349c69a3ae-172488418.us-east-1.elb.amazonaws.com

### The AWS LOADBALANCERR run and endpoints respond.