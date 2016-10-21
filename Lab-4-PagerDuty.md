# Lab 4: Add PagerDuty Integration

# Objective
This lab show how to integrate your toolchain with PagerDuty so people get notified when things go wrong so problems can be fixed faster and reduce downtime.

## Prerequisites
Prior to running this lab, inform the instructor of an email address you would like to use for PagerDuty.  You will need access to this eMail account in order to see notifications in PagerDuty. Check that account to accept the invitation to join PagerDuty.

**Tasks**:
- [Task 1: Add PagerDuty to Toolchain](#task-1-add-pagerduty-to-toolchain)
- [Task 2: Verify PagerDuty works by breaking application build](#task-2-verify-pagerduty-works-by-breaking-application-build)
- [Task 3: Fix application](#task-3-fix-application)

## Task 1: Add PagerDuty to Toolchain
1. On the toolchain's Tool Integrations page, click the add button **+**
2. Select **PagerDuty**
3. On the PagerDuty Configuration page:
   - Enter Nb8ZxY6sAWKxLp1UhA_u as the API access key
   - Enter devopslab as the PagerDuty service name
   - Enter the email address you gave to the instructor as the primary contact email address.  Leave the phone number blank unless you want to receive text messages.
 4. Click **Create Integration**

  ![PagerDutySetup](screenshots/PagerDutySetup.jpg)
 5. You will need to access the eMail account to accept the PagerDuty invitation.

## Task 2: Add Eclipse Orion Web IDE to Toolchain
We want to modify the application and one way is to use the Web IDE.
1. On the toolchain's Tool Integrations page, click the add button **+**
2. Select **Eclipse Orion Web IDE**.
3. No configuration is needed. so click **Create Integration**.

## Task 3: Verify PagerDuty works by breaking application build
  1. On the toolchain's Tool Integrations page, click the **Eclipse Orion Web IDE** tile. The GitHub repos are automatically loaded in your workspace. The Web IDE workspace is on the cloud.
  2. In the file navigator, expand the catalog-api-toolchain_name repo (if needed).
  3. In the file directory, click manifest.yml to open the file.

  ![WebIDE](screenshots/WebIDE.jpg)
  4. Update the value for memory to 96g. This setting intentionally increases your memory to exceed the quota for your org. Your changes are automatically saved.
  5. Now to Push the changes.  From the Eclipse Orion Web IDE menu, click the **Git** icon.

  ![WebIDEGit](screenshots/WebIDEGit.jpg)

  6. In the Working Directory Changes section, which is in the upper-right corner of the window, make sure that the changed file is selected.

  ![WebIDEPush](screenshots/WebIDEPush.jpg)
  7. Click **Commit** to put the changes in the local master branch.
  8. Put these changes in the origin/master branch and click **Push**. Your changes are automatically built and deployed in the pipeline.
  9. Return to your toolchain's Tool Integrations page and click the pipeline tile for the catalog-api microservice to watch the stages run in response to your commit.
  10. The Deploy failed.

  ![WebIDEDeployFailed](screenshots/WebIDEDeployFailed.jpg)
  11. The PagerDuty console (https://ibmdevopslab.pagerduty.com/incidents) shows the incident:

  ![PagerDutyConsole](screenshots/PagerDutyConsole.jpg)
  12. If you entered an email account when you setup the PagerDuty integration, that account will have an email.  The link in the email will allow you to view the incident on PagerDuty.

  ![PagerDutyeMail](screenshots/PagerDutyeMail.jpg)

## Task 4: Fix application

Now to fix the application.
  1. On the toolchain's Tool Integrations page, click the **Eclipse Orion Web IDE** tile.
  2. In the file navigator, expand the catalog-api-toolchain_name repo (if needed).
  3. In the file directory, click manifest.yml to open the file.
  4. Update the value for memory to 96m.
  5. Now to Push the changes.  From the Eclipse Orion Web IDE menu, click the **Git** icon.
  6. In the Working Directory Changes section, which is in the upper-right corner of the window, make sure that the changed file is selected.
  7. Click **Commit** to put the changes in the local master branch.
  8. Put these changes in the origin/master branch and click **Push**. Your changes are automatically built and deployed in the pipeline.
  9. Return to your toolchain's Tool Integrations page and click the pipeline tile for the catalog-api microservice to watch the stages run in response to your commit.
  10. The deploy is successful.  And all the downstream stages run afterwards.

  ![WebIDEDeploySuccess](screenshots/WebIDEDeploySuccess.jpg)




-----------------------------------------------------------------------------------------------------------
# Lab 1: Introduction to IBM Containers and Docker

> **Difficulty**: Easy

> **Time**: 20 minutes

> **Tasks**:
>- [Prerequisites](#prerequisites)
- [Task 1: Verify your environment](#task-1-verify-your-environment)
- [Task 2: Download your public images](#task-2-download-your-public-images)
- [Task 3: Log into IBM Containers using the CLI](#task-3-log-into-ibm-containers-using-the-cli)

## Prerequisites

Prior to running this lab, you must have a Bluemix account and access to a lab laptop.
Instructions are available in [prereqs](0-prereqs.md)
to create your Bluemix account, log into the Bluemix UI, and create a unique namespace.

## Task 1: Verify your environment

Docker Engine should be installed and running in your machine. In this task we will verify that Docker is running and run our first container.

1. Open a Terminal window.  Verify that you are running a recent Docker version by running the **"docker version"** command in the terminal.  You should see something similar to the following:

        $ docker version
        Client:
         Version:      1.8.3
         API version:  1.20
         Go version:   go1.4.2
         Git commit:   f4bf5c7
         Built:        Mon Oct 12 18:01:15 UTC 2015
         OS/Arch:      darwin/amd64

        Server:
         Version:      1.8.3
         API version:  1.20
         Go version:   go1.4.2
         Git commit:   f4bf5c7
         Built:        Mon Oct 12 18:01:15 UTC 2015
         OS/Arch:      linux/amd64

2. To get started with Docker, run a simple container locally, using the `hello-world` image with the command """docker run hello-world".

        $ docker run hello-world

        Hello from Docker.
        This message shows that your installation appears to be working correctly.

        To generate this message, Docker took the following steps:

              1. The Docker client contacted the Docker daemon.
              2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
              3. The Docker daemon created a new container from that image which runs the
              executable that produces the output you are currently reading.
              4. The Docker daemon streamed that output to the Docker client, which sent it to your terminal.

        For more examples and ideas, visit [here](https://docs.docker.com/userguide/).

## Task 2: Download your public images

In this task, you will work with two public Docker images, [Let's Chat](https://github.com/sdelements/lets-chat) and [MongoDB](https://www.mongodb.org/).  The Let's Chat image is a web application that allows users to create message rooms for collaboration.  MongoDB is an open source, document-oriented database designed with both scalability and developer agility in mind.  

First, you will need to pull them down locally from the public [DockerHub](https://hub.docker.com/), which is a repository for Docker images.  By pulling (i.e., downloading) these images we will have them available on our workstation, which is required to run the images locally to learn how they function.

1. Pull the MongoDB image from DockerHub

        $ docker pull mongo
        Using default tag: latest
        latest: Pulling from library/mongo
        68e42ff590bd: Pull complete
        b4c4e8b590a7: Pull complete
        f037c6d892c5: Pull complete
        ...
        202e2c1fe066: Pull complete
        Digest: sha256:223d59692269be18696be5c4f48e3d4117c7f11e175fe760f6b575387abc1bba
        Status: Downloaded newer image for mongo:latest

2. Pull the Let's Chat image from DockerHub

        $ docker pull sdelements/lets-chat
        Using default tag: latest
        latest: Pulling from sdelements/lets-chat
        7a42f1433a16: Already exists
        3d88cbf54477: Already exists
        ...
        ca11de166bed: Already exists
        2409eb7b9e8c: Already exists
        Digest: sha256:98d1637b93a1fcc493bb00bb122602036b784e3cde25e8b3cae29abd15275206
        Status: Image is up to date for sdelements/lets-chat:latest

## Task 3: Log into IBM Containers using the CLI

In this task, we will log into the IBM Containers command line to connect to Bluemix running on the IBM Cloud.





## Task 3: Add and Configure GitHub Interation
  1. Add and Configure Order
  2. Add and Configure Catalog
  3. Add and Configure UI

# Lab 1: Set-up Pipeline for Order

**Tasks**:
- [Task 1: Log into IBM Bluemix](#task-1-log-into-ibm-Bluemix)
- [Task 2: Add and Configure Order Delivery Pipeline](#task-2-add-and-configure-order-delivery-pipeline)
Add Dev Stage and Jobs
Run Dev Stage
Add Test Stage and Jobs
Run Test Stage
Add Prod Stage and Jobs
Run Prod Stage

## Task 1: Log into IBM Bluemix
1. If you are not already logged into IBM Bluemix, log into IBM Bluemix.
![Bluemix](screenshots/bluemix-login.jpg)





## Task 4: Add and Configure Delivery Pipeline for UI
1. Add Delivery Pipeline
2. Add and Configure Dev Environment Stage
    1. Examine Input
    2. Add Deploy Job
    3. Examine Environment Properties
3. Add and Configure Test Environment Stage
    1. Examine Input
    2. Add Deploy Job
    3. Add Sauce Lab Job
        1. Tester Type
        2. Configure Service Instance
    4. Examine Environment Properties
4. Add and Configure Prod Environment Stage
    1. Examine Input
    2. Add Deploy Job
    3. Examine Environment Properties





    # Lab 3: Set-up Pipeline for UI
    You will be creating the pipeline for the UI application.
    **Tasks**:
    - [Task 1: Log into IBM Bluemix](#task-1-log-into-ibm-Bluemix)
    - [Task 2: Create Toolchain for Catalog](#task-2-create-toolchain-for-catalog)
    - [Task 3: Add and Configure GitHub Integration for Catalog](#task-3-add-and-configure-github-integration-for-catalog)
    - [Task 4: Add Catalog Delivery Pipeline](#task-4-add-catalog-delivery-pipeline)
    - [Task 5: Configure Catalog Delivery Pipeline](#task-4-configure-catalog-delivery-pipeline)

    ## Task 1: Log into IBM Bluemix
    1. If you are not already logged into IBM Bluemix, log into IBM Bluemix.
    ![Bluemix](screenshots/bluemix-login.jpg)

    ## Task 2: Create Toolchain for Catalog
      1. Click on **DevOps**.
      2. Click on **Toolchains**.
      3. Click on the **+** plus icon on the right side of the screen.
      4. Click on **Build your own toolchain**.
      5. Under 'Toolchain Settings', enter the name "catalog-toolchain-lab" and click **Create**.

      Your Toolchain is created and you are redirected to the Toolchain panel.

    ## Task 3: Add and Configure GitHub Integration for Catalog
    The code for the Catalog microservice already exists in a GitHub repository (https://github.com/open-toolchain/Microservices_CatalogAPI).  We will clone this repository and link to the clone.

      1. Click on the **+** plus icon on the right side of the screen to add a Tool Integration.
      2. Click on **GitHub** to add integration with GitHub to the Toolchain.
      3. Select 'Clone' as the Repository type.
      4. Enter "https://github.com/githubuserid/catalog-api-toolchain-lab.git" for the New Repository Name.
      5. Enter "https://github.com/open-toolchain/Microservices_CatalogAPI" for the Source repository URL.
      6. Ensure the 'Enable GitHub Issues' checkbox is selected.
      7. Click <b>Create Integration</b>.
      8. The catalog-toolchain-lab tool integrations is displayed.

      ![CreateNewGitHubResult](screenshots/CreateNewGitHubResult.jpg)
    HEREHERE  api access key Nb8ZxY6sAWKxLp1UhA_u
