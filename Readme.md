# Blinkit Clone: Full-Stack Grocery Delivery Platform

A production-grade grocery delivery platform built with the MERN stack, containerized using Docker, and orchestrated on AWS EKS using a GitOps workflow.

## 🚀 Architecture Overview
[Image of an end-to-end CI/CD and deployment architecture diagram]

This project demonstrates a professional DevOps lifecycle, separating application logic from infrastructure configuration to meet industry standards.

---

## 🛠 Tech Stack
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-EF7B4D?style=for-the-badge&logo=argocd&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-635BFF?style=for-the-badge&logo=stripe&logoColor=white)

---

## ⚙️ DevOps Workflow
* **Separation of Concerns:** Two-repo strategy (Application vs. Infrastructure).
* **CI Pipeline (Jenkins):** Automates build, test, and container image generation pushed to AWS ECR.
* **CD Pipeline (ArgoCD):** Implements GitOps; continuously monitors the infrastructure repo to synchronize the desired state with the AWS EKS cluster.

## 📺 Project Demo
Watch the full architectural walkthrough here:
[**YouTube: Full-Stack Architecture on AWS EKS**](https://youtu.be/8oiIAg-Q1js)

## 👤 Connect With Me
* **LinkedIn:** [Hardik Yadav](https://www.linkedin.com/in/hardik-yadav-54458630a/)

---
*Built with passion for automation and scalable systems.*