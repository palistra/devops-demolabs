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

  Some of which we will not confguire:
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
## Task 3: Create  Microservices Toolchain

1. Click **Create** to create the toolchain.

![CreateToolchainButton](screenshots/CreateToolchainButton.png)

2. The Microservices toolchain is created.
![ToolchainCreated](screenshots/ToolchainCreated.png)
