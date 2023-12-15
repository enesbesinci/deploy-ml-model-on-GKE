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

You can access the ML model, codes and Web Application from this link: https://github.com/erkansirin78/flask-iris-classification

This repo contains everything about the model.

### A Brief Summary of the Model

This is a simple Iris Flower Classification Model Deployment project as Flask App. [1]

The model is very simple, we enter our 4 input variables and the model returns us which type of plant it is. We also have a simple front-end to make this a web application.

Here are images from the application:

![image](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/12d1eb44-03ef-4abe-8d7c-d62d05fd9da6)

![image](https://github.com/enesbesinci/deploy-ml-model-on-GKE/assets/110482608/70b89136-a676-4c12-b222-b3d4135b07d7)
















  

