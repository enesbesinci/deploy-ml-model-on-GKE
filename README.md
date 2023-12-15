# Deploy a Machine Learning Model on Google Kubernetes Engine

Hello everyone, in this project I will show you how to deploy a machine learning model on Google Kubernetes Engine (also knows as GKE).

![image](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/af238664-490f-4b3e-a6bc-af8d700d1be0)

## Introduction

Machine Learning (ML) has become an integral part of almost every industry today. After developing a powerful ML model, the next step is to make it accessible and scalable. This is where Docker, a containerization platform, and Kubernetes, a container orchestration platform, come in. By leveraging Docker and Google Kubernetes Engine, we can seamlessly deploy and manage a variety of containerized applications, including our machine learning models.

### Key Terms of this Project
Before we dive into the deployment process, let's get familiar with some key terms:

*1. Docker*

Docker is a containerization platform that allows us to package our applications and their dependencies into a standardized unit, known as a container. This ensures consistency across different environments and facilitates seamless deployment.

*2. Kubernetes*

Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It simplifies the process of running applications in production and efficiently manages containerized workloads.

*3. Google Cloud Platform (GCP)*

Google Cloud Platform provides a suite of cloud computing services, and GKE is a managed Kubernetes service offered by GCP. GKE abstracts the complexities of managing Kubernetes clusters, allowing us to focus on deploying and running our applications.

Throughout this project, we'll explore the seamless integration of Docker, Kubernetes, and GCP to deploy and manage a machine learning model, unlocking the potential for scalable and efficient deployment.

## STEP 1: Create a Machine Learning Model and Web Application

The main objective of this project is to show how to deploy a machine learning model on GKE, so we won't build an ML model or web application, instead we will use a ready-to-use ML model and web application.

You can access the machine learning model's and web application's code from this link: https://github.com/erkansirin78/flask-iris-classification

This repo contains everything about the model.

### A Brief Summary of the Model

This is a simple Iris Flower Classification Model Deployment project as Flask App. [1]

The model is very simple, we enter our 4 input variables and the model returns us which type of plant it is. We also have a simple front-end to make this a web application.

Here are images from the application:

![image](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/12d1eb44-03ef-4abe-8d7c-d62d05fd9da6)

![image](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/70b89136-a676-4c12-b222-b3d4135b07d7)

Now that you understand the Model and the Web Application Front End, we can get to the point.

## STEP 2: Containerization

As you know, if we want to deploy our applications in Kubernetes, we must first containerize our applications. In order to do this, we should create a Dockerfile.

![image](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/406c7700-91ed-4e1b-ada6-fe8efe4e934a)

Let's explain the Dockerfile.

### *FROM python:3.6-slim:*

This line sets the base image. This Docker container will use the official Python image labeled "slim", which contains Python version 3.6. The "slim" label indicates that the base image is a smaller, minimal version.

### *COPY requirements.txt requirements.txt:*

This line copies the requirements.txt file from your local machine to the Docker container. This file contains the dependencies for the Python application.

### *RUN pip install --upgrade pip:*

This line causes pip to be upgraded. It will upgrade the pip version on the image you are using to the latest version.

### *RUN pip install -r requirements.txt:*

This line installs the dependencies listed in requirements.txt. It installs the required Python packages from the file containing the packages required by the application.

### *COPY . /opt/:*

This line copies all files (.) of the local project to the /opt/ directory of the Docker container.

### *WORKDIR /opt:*

This line sets the working directory to /opt/. Subsequent commands will be executed in this directory.

### *EXPOSE 8080:*

This line specifies the port on which the application in the Docker container will listen. This opens port 8080.

### *ENTRYPOINT FLASK_APP=app.py flask run --host=0.0.0.0.0 --port=8080:*

This line specifies the command to run when the Docker container is started. The FLASK_APP environment variable is set to app.py and then the flask run command is run. --host=0.0.0.0.0 ensures that the Flask app is accessible from all IP addresses and --port=8080 specifies that the app listens on port 8080.

Yes, now we have a Dockerfile file. Now that we have created our Dockerfile, we can move to Kubernetes.

## STEP 3: Create a Project on Google Cloud Platform

Now, we will continue with GCP. When you enter the GCP, you will be greeted by a screen like this. You must create a new project to continue (GCP offers a $300 trial for new members, valid for 90 days)

![image](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/655c0cfe-9a18-480e-a80a-cfb389dd05a3)

Then you must activate the Cloud Shell at the top right of the screen.

![image](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/a1e093a1-9488-4c68-9383-cf1200b1dfdf)

At the bottom, the console where we will perform our operations is opened. Now let's assign our project id to the variable named "PROJECT_ID".

![export_proejct_id](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/a522f3ab-e343-43ad-b19c-211b39b71e5e)

If you don't know your project id, you can see it by clicking Dashboard from the drop-down menu on the left side.

![image](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/5f40ded0-afb1-4f54-ba47-8e37068fcbd1)

![id_bulma](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/4856054d-4166-4743-b416-a548435a6d47)

Then import everything about your application into this console, either from your local computer or from a GitHub repo (recommended).

![Screenshot 2023-12-15 204910](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/941e3f2f-009d-4fe3-9bbb-0a7f3f8c5d1f)

![Screenshot 2023-12-15 161948](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/26f5d1b7-32f5-40f9-90a4-79798478bfe6)

I will pull the project from the GitHub repo with "git clone" command.

![git_clone](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/72b081f1-2dcd-42fd-8ba7-ba1bdacf1581)

We pulled it off successfully. Let's look at the contents of the repo.

![git_clone_içeriği](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/f2b61b12-ef11-495e-8e91-92ff7ee0988c)

## STEP 4: Enable the Api's and Create a Artifact Registry

What is Artifact Registry? Artifact Registry is a Google Cloud Platform service where developers can store and manage Docker images, Maven packages, npm packages and other artifacts. We will store our Docker images in this registry.

for more information about Artifact Registry, look at the documentation: https://cloud.google.com/artifact-registry/docs/docker/store-docker-container-images

Now we know what is Artifact Registry, now we need to enable the Artifact Registry API and then create an Artifact Registry.

There are 2 ways to enable Apis. The first way we can enable it with gcloud, the second way we can enable it with the interface.

![artifact_api_enable](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/d98f822a-4b4e-4b65-afe0-0d5f5c0de239)

Let's check the Api if it's enable.

![artifact_api_kontrol_et](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/aec3f791-20b3-4780-9729-f4cdf3c389e4)

Yes, it's enabled.

Now we can create a Artifact Registry.

As with APIs, we can create an Artifact Registry with the interface or with gcloud. In this project, I will create it in a more complex way, i.e. through the console with gcloud, you can create it with the interface if you want.

When creating an Artifact Registry Google asks us for 3 mandatory information, 1) the name of the Artifact Registry, in this case my-repo, 2) the format, in this case Docker, and 3) the location, in this case us-west1.

![create_artifact_dockert_regisrry](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/87060abb-73ad-433c-8640-2e20283202ba)

To see details about our Artifact Registry

![artifact_reg_listele](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/3912a3e6-5cf2-4e8d-a419-4cd9d6f317fa)

As you can see below, we have created a Artifact Registry.

![my_repo_oluşturuldu](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/0751d195-8500-4678-9668-a2fcb3219747)

Now I will go into the project directory I pulled from GitHub and create a Docker Image with Docker commands. Best practice is to create an image directly with "docker build -t" and tag the tag as the repo address (e.g. "us-west1-docker.pkg.dev/${PROJECT_ID}/REPO_ADI/IMAGE NAME:TAG).

![docker_imaj_inşa_et](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/f8e33e48-0af2-4e5e-ad31-b044e3bb02a0)

Let's see the our all images.

![imajlari_listele](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/00bb6993-ec9f-46ac-85ae-b664f928adf9)

Then we can push our image to the Artifact Registry.

![imajı_pushla](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/68e65b2f-98af-4d0d-86e3-14c367af3af8)

Let's check if the image exists or not

![imaj_push_kontrol](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/5c154110-7fc3-4845-982e-747a26d9e3b9)

## STEP 5: Create a Kubernetes Cluster and Deploy the Model into it

First, let's enable the Api for Kubernetes.

![k8s_-api_açma](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/27dacb8c-af40-417b-a306-6da634215fa3)

Then we will create a Cluster. We can create an Autopilot cluster using Google Cloud CLI or Google Cloud console. I will continue with Gcloud.

But first, what is Autopilot?

![image](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/4f3efc27-9707-42a1-921d-30756639a832)

Let's create a cluster. When creating an Autopilot cluster, there are 2 mandatory information we need to enter. 1-) Cluster name, in this example "my-cluster" and 2-) Location, in this example us-west1

![cluster_oluştur_auot](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/6c049ccb-fb8b-49ab-9282-5eebaa716c5f)

Let's see the cluster.

![cluste_gör_ekran](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/f9e34912-a551-4ac2-90dd-940430489bb8)

Let's see it in more detail.

![cluster_detay](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/26fafb2f-de52-4bd1-b25b-13a6b367ceb4)

Yes, we have create a cluster, now we can connect to the cluster, to connect to cluster, get a credential.

![cluster_credential](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/74fb0cdb-401f-4e68-b89d-b86341e515a1)

This command configures kubectl to use the cluster you created.

Now that we can run kubectl commands on Gcloud, let's see our nodes.

![kubectl _Getnodes](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/1c759df0-2b7c-41a7-93f2-ed2b91fe3eda)

For now, we don't have any node because we created an autopilot cluster and we haven't deployed any application so we haven't been allocated a node.





















































  

