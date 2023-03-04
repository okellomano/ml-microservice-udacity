[![CircleCI](https://dl.circleci.com/status-badge/img/gh/okellomano/ml-microservice-udacity/tree/master.svg?style=svg)](https://dl.circleci.com/status-badge/redirect/gh/okellomano/ml-microservice-udacity/tree/master)


## Project Overview
This project involves deploying a machine learning model as a microservice API using Docker and Kubernetes.

We are given a pre-trained, `sklearn` model that has been trained to predict housing prices in Boston according to several features, such as average rooms in a home and data about highway access, teacher-to-pupil ratios, and so on. You can read more about the data, which was initially taken from Kaggle, on [the data source site](https://www.kaggle.com/c/boston-housing). This project tests your ability to operationalize a Python flask app—in a provided file, `app.py`—that serves out predictions (inference) about housing prices through API calls. This project could be extended to any pre-trained machine learning model, such as those for image recognition and data labeling.

The following steps were taken to operationalize the API:

* Containerized the model using Docker and created a Dockerfile to define the API image.
* Uploaded the API image to a container registry, such as Docker Hub.
* Deployed the API to a Kubernetes cluster using a deployment YAML file.
* Configured the Kubernetes service to expose the API to external clients.
* Implemented logging to diagnose issues.
* Defined an automated testing environment using CircleCI
 


### Project Tasks

Your project goal is to operationalize this working, machine learning microservice using [kubernetes](https://kubernetes.io/), which is an open-source system for automating the management of containerized applications. In this project you will:
* Test your project code using linting
* Complete a Dockerfile to containerize this application
* Deploy your containerized application using Docker and make a prediction
* Improve the log statements in the source code for this application
* Configure Kubernetes and create a Kubernetes cluster
* Deploy a container using Kubernetes and make a prediction
* Upload a complete Github repo with CircleCI to indicate that your code has been tested

### A short explanation of the files in the repository

* .circleci: For the CircleCI build server
* model_data : this folder contains the pretrained sklearn model and housing csv files
* output_txt_files: folder contains sample output logs from running ./run_docker.sh and ./run_kubernetes.sh
* app.py : contains the flask app
* Dockerfile: contains instructions to containerize the application
* Makefile : contains instructions for environment setup and lint tests
* requirements.txt: list of required dependencies
* run_docker.sh: bash script to build Docker image and run the application in a Docker container
* upload_docker.sh: bash script to upload the built Docker image to Dockerhub
* run_kubernetes.sh: bash script to run the application in a Kubernetes cluster
* make_prediction.sh: bash script to make predictions against the Docker container and k8s cluster
* README.md: this README file
---

## Setup the Environment

* Create a virtualenv with Python 3.7 and activate it. Refer to this link for help on specifying the Python version in the virtualenv. 
```bash
python3 -m pip install --user virtualenv
# You should have Python 3.7 available in your host. 
# Check the Python path using `which python3`
# Use a command similar to this one:
python3 -m virtualenv --python=<path-to-Python3.7> .devops
source .devops/bin/activate
```
* Run `make install` to install the necessary dependencies
* Run `make lint` (pylint app.py files and hadolint Dockerfile) to detect errors in the code.
* Alternatively, instead of running `make install` and `make lint` independently, you can simply run `make all` after activating the environment. This will install the necessary dependencies and detect errors in the code.

### Running `app.py`

1. Standalone:  `python app.py`
2. Run in Docker:  `./run_docker.sh`
3. Run in Kubernetes:  `./run_kubernetes.sh`

### Kubernetes Steps

1. Setup and Configure Docker locally
    * First, install Docker. Refer `[here](https://docs.docker.com/engine/install/ubuntu/)` for different installattion methods.
    * Run `./run_docker.sh`
    * Run `docker ps` or `systemctl status docker.service` to check if the Docker service is running. If not, run `sudo systemctl start docker` to start it.
    * Run `./make_prediction.sh` to make prediction and copy/paste the logging output at terminal to `output_txt_files/docker_out.txt`
    
2. Setup and Configure Kubernetes locally
    * Install a hypervisor: First, you need to install a hypervisor like VirtualBox or VMware. You can install VirtualBox by running the following command: `sudo apt-get install virtualbox`
    * Next, you need to install kubectl, which is the command line tool used to manage Kubernetes clusters. You can install kubectl by running the following command:
        `sudo apt-get update && sudo apt-get install -y apt-transport-https`
        `curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -`
        `echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list`
        `sudo apt-get update`
        `sudo apt-get install -y kubectl`
    * Finally, you can install Minikube by running the following command: `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb && sudo dpkg -i minikube_latest_amd64.deb`
    * Once Minikube is installed, you can start it by running the following command: `minikube start`
    * You can verify that everything is working correctly by running the following command: `kubectl version`
    * Run `kubectl get pods` to see which pods are running
    * Run `./run_kubernetes.sh`
    * Run `./make_prediction.sh` to make prediction and copy/paste the logging info at terminal to `output_txt_files/kubernetes_out.txt`
    
3. Create Flask app in Container
    * Run ``./run_docker.sh` to build and start the Flask app container.
    * Run `./upload_docker.sh` to upload the container to docker hub.
