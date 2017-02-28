# Creating Open Toolchains for IBM Bluemix

## Objective
This series of labs shows how to set up a productive Continuous Delivery toolchain with a sample that consists of three microservices. After you finish this part of the series, you will be familiar with a toolchain that demonstrates practices from the IBM® Bluemix® Garage Method. ***Note:*** Toolchains are currently available in the US South region only and the instructions in this lab are written for the US South region.

To create this toolchain, we will use a sample to create an online store that consists of three microservices: a Catalog API, an Orders API, and a UI that calls both of the APIs. The toolchain is preconfigured for continuous delivery, source control, blue-green deployment, functional testing, issue tracking, online editing, and alert notification.  We will explore the various integrations.

### Online Store sample

The online store consists of three microservices:

1. Catalog API: A back-end RESTful API that tracks all of the items in the store.
2. Orders API: A back-end RESTful API that tracks all store orders.
3. UI: A simple UI that displays all of the items in the store catalog, and that can create orders. This PHP UI calls both of the REST APIs.

The Catalog and Orders API are backed by a Cloudant store. As part of deploying this application a no cost Cloudant service instance will be created.

### Pipelines, stages and deployment environments - oh my!

In the real world, many enterprises have a process for developing, testing and deploy code to production.  The lab scenario we will go through shows how Bluemix Continuous Delivery toolchains can be used to to automate that process.

- The code for the online store is already in three different GitHub repositories, one per microservice.  As part of creating the Continuous Delivery toolchain, you will clone the repositories to your own GitHub account.
- Three delivery pipelines will also be created, again one per microservice.  Each pipeline can be run in parallel.
- Each delivery pipeline will consists of a number of stages (Build, Dev, Test and Prod).
- Each stage will consist of one or more jobs that perform a task such a build the code, deploy the code, or testing the codes.
- As part of the respoective delivery pipeline, each microservice is deployed to three environments: development, test, and production. You end up with nine deployed apps.
- While we could edit locally (using Eclipse for example) and push the code up to Bluemix, we will instead use the Eclipse Orion Web IDE, which you can use to edit your code and deploy it with the pipeline from a web browser.
- We want our application to scale as needed so we will be deploying one of the microservices into Bluemix containers

<div class="page-break"></div>
Conceptually, the process looks like:

  ![PipelinesAndStages](Screenshots/PipelinesAndStages.jpg)

### Teaming

Software development is a team activity.  The lab scenario also shows how Bluemix Continuous Delivery tool integrations can be used to alerts teams when activities occur (such as builds or deployments) as well as when events happen (such as a build failing or an application outage).

- Slack is configured to alert the team when activities occur
- [PagerDuty | IBM Alert Notification] is configured to alert the team when events happen
- Bluemix Availbility Monitoring is configured to monitor the application in production and alert the team when outages occur

It sounds like a lot ... and it is!  Thankfully, it is all handled by a Bluemix Continuous Delivery toolchain.  And even better, we will use an existing template to give a great starting point.

<div class="page-break"></div>

## Prerequisites
Prior to running these labs, you must have a Bluemix account, a GitHub account and access to a lab laptop.  Follow the steps in Lab 0 to create one or both of those accounts.

## Labs
- [Lab 0: Create Bluemix and GitHub accounts](Lab-0-Pre-reqs.md)
- [Lab 1: Create Toolchain for Sample Application](Lab-1-Create-Toolchain.md)
- [This is text](#Lab-1:-Create-Toolchain-for-Sample-Application)
- [Lab 2: Build and deploy to dev space](Lab-2-Build-and-Deploy-to-dev-space.md)
- [Lab 3: Customize Toolchain to add Slack Integration](Lab-3-Customize-Toolchain-Slack.md)
- [Lab 4: Customize Toolchain to allow full deployment](Lab-4-Customize-Toolchain-Full-Deployment.md)
- [Lab 5: Customize Toolchain to add Bluemix Availability Monitoring](Lab-5-Customize-Toolchain-BAM.md)
- [Lab 6: Add Bluemix IBM Alert Notification](Lab-6-Add-Bluemix-IAN.md)
- [Lab 7: Modify Pipeline for Catalog to deploy Catalog to Containers](Lab-8-Modify-Pipeline-for-Catalog-Containers.md)
- [Lab 8: Add auto-scaling support to Catalog](Lab-8-Add-auto-scaling-support-to-Catalog.md)

[comment]: # (Lab 8: Answer the guided questions for the BlueCompute tool chain example)
[comment]: # (Lab 6: Deliver a UI Change)

[comment]: # (Lab 1: Click-create and customize Toolchain for Order and UI Pipelines. Run Toolchain to deploy to Dev and Prod space. Test UI with browser, see output for Order.)

[comment]: # (Lab 2: Set-up Pipeline for Catalog - more manual effort. Run Toolchain to deploy to Dev and Prod space. See output for Catalog.)

[comment]: # (Lab 3: Customize Pipeline for Catalog to incorporate real test scripts for Dev space - not sure what tool to put into the Toolchain, Sauce Labs is nice but not free ... mochajs ? Something else?. First test is fine. Break app and show how test finds bug. How to notify team?)
[comment]: # (Lab 4: Customize Toolchain to add Slack Integration and show how team is auto-notified when test fails. Fix bug, run test again, deploy to production.)
[comment]: # (Lab 5: Customize Toolchain to add Bluemix Availability Monitoring for Production. Break something to show what happens - be nice to have some sort of scalability problem?)
[comment]: # (Lab 6: Add Bluemix IBM Alert Notification to notify people when production breaks.)
[comment]: # (Lab 7: Modify Pipeline for Catalog to deploy Catalog to Containers in Prod space to handle scability issue)
[comment]: # (Lab 8: Add auto-scaling support to Catalog)


# Lab 1: Create Toolchain for Sample Application

## Objective
This lab takes you throught the process of creating the Continuous Delivery toolchain for the sample online application.

**Tasks**:
- Task 1: Display Microservices Toolchain panel
- Task 2: Understand Microservices Toolchain panel
- Task 3: Create  Microservices Toolchain

## Task 1: Display Microservices Toolchain panel

1. We need to get to the DevOps Services. Click on the hamburger menu, then **Menu**.

  ![BluemixMenuBar](screenshots/BluemixMenuBar.jpg)
2. Click on **Services** then **DevOps**.
3. Click on **Toolchains**.
4. Click on **Create a Toolchain**.

## Task 2: Understand Microservices Toolchain panel

1. There are a number of ways to create a Toolchain.  One way is from a template, another way is to start from an existing application.  Enterprise can create their own templates or use one of the pre-existing one. For this exercise, we will start with an existing template, the Microservices toolchain.  Click **Microservices toolchain**.
2. For an explanation of this toolchain, read the paragraphs on the left.  This toolchain integrates various tools, some of which we will configure:
  - GitHub
  - Delivery pipeline

  Some of which we will not configure:
  - PagerDuty
  - Sauce labs
  - Slack

  And some of which do not require configuration.

  - DevOps Insights
  - Eclipse Orion Web IDE

3. The **Organization** field is the name of the Bluemix Organization in which this toolchain will be created.
4. The **Toochain Name** names is a generated name for this Toochain.  Rename it to something memorable or leave it at the default generated name (which is what the screenshots will do).
5. Click on **GitHub**. GitHub is where the source of the application is stored, one GitHub repo per application (so three GitHub repos). We will set up our Toolchain to create a clone of each repo for use in this lab.  
6. If you haven't authorized Bluemix to access GitHub, you need to:

    1. Click **Authorize** to go to the GitHub website.
    2. Enter your GitHub username and password.
    3. Click **Sign in**.
    2. Click **Authorize application**.

7. Once authorized, you see the three Source Repositories (one for each of Catalog, Orders and UI) where the code is stored and three corresponding Target Repositories, where the Source Repositories will be cloned. The Target Repository name is generated and just like Toolchain Name, you can leave the default generated name or make it something more memorable.
  ![GitHubConfiguration](screenshots/GitHubConfiguration.png)
8. Click on **Delivery Pipeline**. We will be creating three delivery pipelines, one for each microservices. This is where the application name for each microservice will be specified, as well as the Bluemix Region, Organization and Space where the microservices will be deployed.
9. The application names for the three microservices must be unique in the Bluemix environment so it is best to leave them as generated.
  ![PipelineConfiguration](screenshots/PipelineConfiguration.png)
10. Pipelines can only be created in the US South region so to keep things simple we will deploy to only the US South region and in the Organization we are logged into.
11. We have three spaces for our environment corresponding to the lifecycle we are using.

    1. Development (**dev**) where code development takes place
    2. Testing or Quality Assurance (**qa**) where testing takes plave
    3. Production (**prod**) where the application is available to end users (in our lab scenario, we do not restrict access to the dev or qa applications but in real life you would).

  ![DeployConfiguration](screenshots/DeployConfiguration.png)

<div class="page-break"></div>

## Task 3: Create  Microservices Toolchain

1. Click **Create** to create the toolchain.

  ![CreateToolchainButton](screenshots/CreateToolchainButton.png)
2. The Microservices toolchain is created.

  ![ToolchainCreated](screenshots/ToolchainCreated.png)
