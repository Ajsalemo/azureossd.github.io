---
permalink: "/nodejs/"
layout: single
toc: true
title: "Nodejs"
sidebar: 
    nav: "links"
---

App Service on Linux supports `Node.js` as a runtime stack built-in image.

>**Node.js Update Policy** - App Service upgrades the underlying Node.js runtime and SDK of your application as part of the regular platform updates. As a result of this update process, your application will be automatically updated to the latest patch version available in the platform for the configured runtime of your app. For more information about current supported node.js versions check [Support Timeline](https://github.com/Azure/app-service-linux-docs/blob/master/Runtime_Support/node_support.md#support-timeline).

> Find additional Node.js articles on [Technet/TechCommunity - Apps on Azure Blog](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/bg-p/AppsonAzureBlog/label-name/Node.js).



You can find a compilation of resources by categories:

# Linux

## General
- [App Service recommendations for Node.js version upgrades and end of life](https://azureossd.github.io/2023/03/29/App-service-recommendations-for-nodejs-version-upgrades-and-end-of-life/index.html)

## Availability and Post Deployment issues
- [Missing or undefined environment variables](https://azureossd.github.io/2022/11/14/Missing-or-undefined-environment-variables-with-Node-on-App-Service-Linux/index.html)
- [Module not found](https://azureossd.github.io/2022/10/25/Module-not-found-with-Node-on-App-Service-Linux/index.html)
- [NPM executables not being found](https://azureossd.github.io/2022/10/24/NPM-Executables-not-being-found-at-startup-on-App-Service-Linux/index.html)
- [getaddrinfo ENOTFOUND](https://azureossd.github.io/2022/09/30/Node-applications-on-App-Service-Linux-and-getaddrinfo-ENOTFOUND/index.html)
- [ENOSPC: System limit for number of file watchers reached](https://azureossd.github.io/2022/09/28/ENOSPC-System-limit-for-number-of-file-watchers-reached/index.html)
- [Node Deployments and 'node_modules.tar.gz: cannot open: no such file or directory'](https://azureossd.github.io/2023/05/15/Node-Deployments-and-node_modules-tar-gz-cannot-open-no-such-file-or-directory/index.html)
- [Node Deployments and 'npm ERR! Missing Script'](https://azureossd.github.io/2023/05/16/Node-Deployments-and-npm-ERR!-Missing-script/index.html)
- [Node.js 12 applications failing by Optional chaining](https://azureossd.github.io/2022/09/06/Nodejs-12-failing-by-Optional-Chaining/index.html)
- [Troubleshooting a ‘Module was compiled against a different Nodejs version’ errors](https://azureossd.github.io/2023/05/31/Troubleshooting-a-Module-was-compiled-against-a-different-Node.js-version-errors/index.html)
- [NPM executables not being found on App Service Linux](https://azureossd.github.io/2022/10/24/NPM-Executables-not-being-found-at-startup-on-App-Service-Linux/index.html)

## Deployment
- [Troubleshooting Node.js deployments on App Service Linux](https://azureossd.github.io/2023/02/09/troubleshooting-nodejs-deployments-on-appservice-linux/index.html)
- [Yarn install timeouts and private packages](https://azureossd.github.io/2023/03/24/yarn-install-timeouts-and-private-packages/index.html)
- [NodeJs GitHub Action Deployment on App Service Linux Using Publish Profile](https://azureossd.github.io/2025/04/16/NodeJs-GitHub-Action-Deployment-on-App-Service-Linux-Using-Publish-Profile/index.html)
- **Frameworks**
  - [Next.js](https://azureossd.github.io/2022/10/18/NextJS-deployment-on-App-Service-Linux/index.html)
  - [Vue](https://azureossd.github.io/2022/02/11/Vue-Deployment-on-App-Service-Linux/index.html)
  - [Nest](https://azureossd.github.io/2022/02/11/Nest-Deployment-on-App-Service-Linux/index.html)
  - [React](https://azureossd.github.io/2022/02/07/React-Deployment-on-App-Service-Linux/index.html)
  - [Angular](https://azureossd.github.io/2022/01/29/Angular-Deployment-on-App-Service-Linux/index.html)
  - [Nuxtjs](https://azureossd.github.io/2022/01/28/Nuxtjs-Deployment-with-Azure-DevOps-Pipelines/index.html)
  - [Puppeteer](https://azureossd.github.io/2023/05/05/Running-Puppeteer-on-Azure-App-Service-Linux/index.html)

## Configuration and Best Practices
- [Using PM2 on App Service Linux](https://azureossd.github.io/2022/02/22/Using-PM2-on-App-Service-Linux/index.html)
- [Node.js with Keep-Alives and Connection Reuse](https://azureossd.github.io/2022/03/10/NodeJS-with-Keep-Alives-and-Connection-Reuse/index.html)
- [Node.js and Redirects or Rewrite scenarios](https://azureossd.github.io/2022/01/16/NodeJS-and-Redirects-or-Rewrites-on-App-Service-Linux/index.html)
- [Running Production Build Node.js Apps](https://azureossd.github.io/2020/04/30/run-production-build-on-app-service-linux/index.html)
  - **SSL and certificates**
    - [Loading and accessing certificates in Node.js](https://azureossd.github.io/2021/02/03/Loading-certificates-using-nodejs/index.html)
    - [Configuring SSL Certificates with Node.js](https://azureossd.github.io/2020/02/11/configuring-ssl-certificates-with-nodejs/index.html)
- [Custom Startup Script for Node.js](https://azureossd.github.io/2020/01/23/custom-startup-for-nodejs-python/index.html)
- [Using modern Yarn for deployment with Node.js](https://azureossd.github.io/2022/08/10/Using-modern-Yarn-for-deployment-with-Node.js-on-Azure-App-Service/index.html)
- [Best practices for not using Development Servers on Nodejs applications](https://azureossd.github.io/2022/10/26/Best-practices-for-not-using-Development-Servers-on-Nodejs-applications-and-App-Service-Linux/index.html)
- [Why to avoid installing packages in startup scripts](https://azureossd.github.io/2022/10/14/Nodejs-on-App-Service-Linux-and-why-to-avoiding-installing-packages-in-startup-scripts/index.html)
- [Rebuild Your Node.js Project the Right Way](https://azureossd.github.io/2024/10/14/rebuild-your-nodejs-project-the-right-way/index.html)

## Performance
- [Troubleshooting Node.js High Memory scenarios](https://azureossd.github.io/2021/12/10/Troubleshooting-NodeJS-High-Memory-scenarios-in-App-Service-Linux/index.html)
- [Troubleshooting Node.js High CPU scenarios](https://azureossd.github.io/2021/12/09/Troubleshooting-NodeJS-High-CPU-scenarios-in-App-Service-Linux/index.html)


# Windows
## Configuration and Best Practices
- [Avoiding hardcoding Node versions](https://azureossd.github.io/2022/06/24/Avoiding-hardcoding-Node-versions-on-App-Service-Windows/index.html)

## Performance
- [Troubleshooting Common iisnode Issues](https://azureossd.github.io/2022/10/17/troubleshooting-common-iisnode-issues/index.html)
- [Troubleshooting Node.js High CPU and Memory scenarios in App Service Windows](https://azureossd.github.io/2021/12/14/Troubleshooting-NodeJS-High-CPU-and-Memory-scenarios-in-App-Service-Windows/index.html)

## Deployment
- [NestJS deployment on App Service Windows](https://azureossd.github.io/2022/11/29/NestJS-deployment-on-App-Service-Windows/index.html)
- [NextJS deployment on App Service Windows](https://azureossd.github.io/2024/02/06/NextJS-deployment-on-App-Service-Windows/index.html)
- [Deploying Angular SSR (Universal) to App Service Windows](https://azureossd.github.io/2024/07/30/Deploying-Angular-SSR-to-App-Service-Windows/index.html)
- [React deployment on App Service Windows](https://azureossd.github.io/2022/11/06/React-deployment-on-App-Service-Windows/index.html)
- [Deploying Angular SSR (Universal) to App Service Windows](https://azureossd.github.io/2024/07/30/Deploying-Angular-SSR-to-App-Service-Windows/index.html)