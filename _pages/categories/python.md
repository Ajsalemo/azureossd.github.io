---
permalink: "/python/"
layout: single
toc: true
title: "Python"
sidebar: 
    nav: "links"
---

App Service on Linux supports `Python` as a runtime stack built-in image.

>**Python Update Policy** - App Service upgrades the underlying Python runtime of your application as part of the regular platform updates. As a result of this regular update process, your application will be automatically updated to the latest patch version of Python available for that platform. For more information about current supported Python versions check [Support Timeline](https://github.com/Azure/app-service-linux-docs/blob/master/Runtime_Support/python_support.md#support-timeline).

> Find additional Python articles on [Technet/TechCommunity - Apps on Azure Blog](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/bg-p/AppsonAzureBlog/label-name/Python).


You can find a compilation of resources by categories:

# Linux 

## Availability and Post Deployment issues
- [Troubleshooting 'failed to find attribute' errors on Python Linux App Services](https://azureossd.github.io/2023/01/30/Troubleshooting-'failed-to-find-attribute'-errors-on-Python-Linux-App-Services/index.html)
- [Python on Linux App Service and ModuleNotFoundError](https://azureossd.github.io/2022/11/24/Python-on-Linux-App-Service-and-ModuleNotFound-Errors/index.html)
- [Python Deployment - Could not find output.tar.gz](https://azureossd.github.io/2023/03/28/Python-Depolyment-Could-Not-Find-output.tar.gz/index.html)
- [Python Startup - Could not open shared object file: no such file or directory](https://azureossd.github.io/2023/04/17/Python-Deployment-could-not-open-shared-object-file/index.html)
- [Python Deployments: ‘File not found’ and filesystem directory differences with app content](https://azureossd.github.io/2023/06/12/Python-Deployments-File-not-found-and-filesystem-directory-differences-with-app-content/index.html)
- [Python on App Service Linux and why to avoid installing packages on startup](https://azureossd.github.io/2023/06/09/Python-on-App-Service-Linux-and-why-to-avoid-installing-packages-on-startup/index.html)
- [Gunicorn - ‘[CRITICAL] Worker Timeout’ on App Service Linux](https://azureossd.github.io/2024/07/01/Gunicorn-and-Critical-Worker-Timeouts-on-App-Service-Linux/index.html)


## Configuration
- [Configuring Gunicorn worker classes and other general settings](https://azureossd.github.io/2023/01/27/Configuring-Gunicorn-worker-classes-and-other-general-settings/index.html)
- [Gunicorn Update HTTP Headers on Azure App Service](https://azureossd.github.io/2022/08/03/Gunicorn-update-HTTP-headers-On-Azure-App-Service/index.html)

## Deployment
- [Troubleshooting Python deployments on App Service Linux](https://azureossd.github.io/2023/04/17/troubleshooting-python-deployments-on-appservice-linux/index.html)
- [Deploying Python Applications using Github Action](https://azureossd.github.io//2023/08/09/Deploying-Python-Applications-using-Github-Actions/index.html)
- [Deploying your own virtual environment with Python and App Service Linux](https://azureossd.github.io/2024/07/25/Deploying-your-own-virtual-environment-with-Python-and-App-Service-Linux/index.html)

- **Frameworks**
  - [Django Deployment on App Service Linux](https://azureossd.github.io/2022/02/20/Django-Deployment-on-App-Service-Linux/index.html)
  - [Flask Deployment on App Service Linux](https://azureossd.github.io/2022/02/17/Flask-Deployment-on-App-Service-Linux/index.html)
  - [Deploying a Python FastAPI app to App Service Linux](https://azureossd.github.io/2024/04/23/Deploying-a-Python-FastAPI-app-to-App-Service-Linux/index.html)
  - [Deploying a Python Streamlit app to App Service Linux](https://azureossd.github.io/2024/04/18/Deploying-a-Python-Streamlit-app-to-App-Service-Linux/index.html)
  - [Deploying a Quart app to Python App Service Linux](https://azureossd.github.io/2024/07/17/Deploying-a-Quart-app-to-Python-App-Service-Linux/index.html)
  - [Deploying a Relfex application to Azure Container Apps](https://azureossd.github.io/2024/08/24/Deploying-Reflex-Application-to-Azure-Container-Apps/index.html)

## Performance
 - [Using CProfile to troubleshoot high CPU on Linux Python App Services](https://azureossd.github.io/2023/05/15/Python-Preformance-High-CPU-CProfile/index.html)
 - [Python - Slow execution / high CPU - wSGI/aSGI multi-worker strategy](https://azureossd.github.io/2025/01/28/Python-slow-execution-high-CPU-wsGI-aSGI-multi-worker-strategy/index.html)
 - [Profiling Python applications with high CPU on App Service Linux](https://azureossd.github.io/2025/01/30/2025-Python-using-profilers-for-applications-with-high-CPU-on-App-Service-Linux/index.html)
