# Creating Open Toolchains for IBM Bluemix

## Objective
This series of labs shows how to set up a productive Continuous Delivery toolchain with a sample that consists of three microservices. After you finish this part of the series, you will be familiar with a toolchain that demonstrates practices from the IBM® Bluemix® Garage Method. ***Note:*** Toolchains are currently available in the US South region only and the instructions in this lab are written for the US South region.

To create this toolchain, we will use a sample to create an online store that consists of three microservices: a Catalog API, an Orders API, and a UI that calls both of the APIs. The toolchain is pre-configured for continuous delivery, source control, blue-green deployment, functional testing, issue tracking, online editing, and alert notification.  We will explore the various integrations.

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
- As part of the respective delivery pipeline, each microservice is deployed to three environments: development, test, and production. You end up with nine deployed applications.
- While we could edit locally (using Eclipse for example) and push the code up to Bluemix, we will instead use the Eclipse Orion Web IDE, which you can use to edit your code and deploy it with the pipeline from a web browser.
- We want our application to scale as needed so we will be deploying one of the microservices into Bluemix containers

<div class="page-break"></div>
Conceptually, the process looks like:

  ![PipelinesAndStages](Screenshots/PipelinesAndStages.jpg)

### Teaming

Software development is a team activity.  The lab scenario also shows how Bluemix Continuous Delivery tool integrations can be used to alerts teams when activities occur (such as builds or deployments) as well as when events happen (such as a build failing or an application outage).

- Slack is configured to alert the team when activities occur
- [PagerDuty | IBM Alert Notification] is configured to alert the team when events happen
- Bluemix Availability Monitoring is configured to monitor the application in production and alert the team when outages occur

It sounds like a lot ... and it is!  Thankfully, it is all handled by a Bluemix Continuous Delivery toolchain.  And even better, we will use an existing template to give a great starting point.

<div class="page-break"></div>

## Prerequisites
Prior to running these labs, you must have a Bluemix account, a GitHub account and access to a lab laptop.  Follow the steps in Lab 0 to create one or both of those accounts.

## Labs
- [Lab 0: Create Bluemix and GitHub accounts](Lab-0-Pre-reqs.md)
- [Lab 1: Create Toolchain for Sample Application](Lab-1-Create-Toolchain.md)
- [This is text](#lab-1-create-toolchain-for-sample-application)
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

[comment]: # (Lab 3: Customize Pipeline for Catalog to incorporate real test scripts for Dev space - not sure what tool to put into the Toolchain, Sauce Labs is nice but not free ... mochajs ? Something else?. First test is fine. Break application and show how test finds bug. How to notify team?)
[comment]: # (Lab 4: Customize Toolchain to add Slack Integration and show how team is auto-notified when test fails. Fix bug, run test again, deploy to production.)
[comment]: # (Lab 5: Customize Toolchain to add Bluemix Availability Monitoring for Production. Break something to show what happens - be nice to have some sort of scalability problem?)
[comment]: # (Lab 6: Add Bluemix IBM Alert Notification to notify people when production breaks.)
[comment]: # (Lab 7: Modify Pipeline for Catalog to deploy Catalog to Containers in Prod space to handle scalability issue)
[comment]: # (Lab 8: Add auto-scaling support to Catalog)

<div class="page-break"></div>

# Lab 0 Create Bluemix and GitHub accounts

## Objective
This lab sets up the prerequisite accounts for the remaining labs.

## Prerequisites
Prior to running these labs, you must have a Bluemix account, a GitHub account and access to a lab laptop. Follow the steps in this lab to create one or both of those accounts.  Note you will need access to your eMail account to confirm the account setup activity.

**Tasks**:
- [Task 1: Create Bluemix trial account](#task-1-create-bluemix-account)
- [Task 2: Create GitHub account](#task-2-create-github-account)

## Task 1: Create Bluemix trial account

1. If you already have an active Bluemix account, you can skip this task.
2. Open a web browser and enter the following URL: https://console.ng.bluemix.net/
3. Click on the **Sign Up** button.
4. Follow the directions to fill out the form. Note you will need access to an eMail account to confirm the account setup activity. Make note of the password you specify.
5. Click **Create Account**. This will cause Bluemix to send an email to the eMail account you specified.
6. Login into the eMail account you specified. Open the eMail with the subject: _Action Required: Confirm your Bluemix account_.
7. Click on the **Confirm Account** button.
8. You now have an active Bluemix trial account.

## Task 2: Create GitHub account

1. If you already have a GitHub account, skip this task.
2. In a web browser, enter the following URL: https://github.com/
3. Follow the directions to fill out the form. Note you will need access to an eMail account to confirm the account setup activity. Make note of the password you specify.
4. Click on the **Sign up for GitHub** button. This will cause GitHub to send an email to the eMail account you specified.
5. Login into the eMail account you specified. Open the eMail from GitHub with the subject: _Please verify your email address_.
6. Click on the **Verify email address** link.
8. You now have an active GitHub account.

<div class="page-break"></div>
# Lab 1: Create Toolchain for Sample Application

# Lab 1 Create Toolchain for Sample Application

## Objective
This lab takes you through the process of creating the Continuous Delivery toolchain for the sample online application.

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
4. The **Toolchain Name** names is a generated name for this Toolchain.  Rename it to something memorable or leave it at the default generated name (which is what the screenshots will do).
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
    2. Testing or Quality Assurance (**qa**) where testing takes place
    3. Production (**prod**) where the application is available to end users (in our lab scenario, we do not restrict access to the dev or qa applications but in real life you would).

  ![DeployConfiguration](screenshots/DeployConfiguration.png)

<div class="page-break"></div>

## Task 3: Create  Microservices Toolchain

1. Click **Create** to create the toolchain.

  ![CreateToolchainButton](screenshots/CreateToolchainButton.png)
2. The Microservices toolchain is created.

  ![ToolchainCreated](screenshots/ToolchainCreated.png)

<div class="page-break"></div>

# Lab 2: Build and deploy to dev space

## Objective
This lab will build the all three microservices and deploy them to the Development space.

**Tasks**:
- Task 1: Explore the Microservices toolchain
- Task 2: Examine the build pipelines

## Task 1: Explore the Microservices toolchain

1. IBM Bluemix created the Continuous Delivery Toolchain based on the Microservices template.  At the top you see the pieces of the [Bluemix Garage Method](https://www.ibm.com/devops/method/) and where each tool integration fits.
  ![ThinkCodeBar](screenshots/ThinkCodeBar.png)

2. **Think** is where the [GitHub](https://github.com/) Issues database is listed.  Click on the **ui-toolchain-demo** link (or right-mouse button click and select **Open Link in New Tab**, then select the new tab to save time later on).  
  ![GitHubIssue](screenshots/GitHubIssue.png)

3. This displays the **GitHub Issues** page.  Issues are used to track todos, bugs, feature requests, and more.  Each GitHub repository (_repo_ for short) can include issues.  The Microservices template we used only included issues for the UI repo.
  ![GitHubIssuePage](screenshots/GitHubIssuePage.png)
  Return to the Microservices toolchain by either clicking on the **Go back one page** arrow on the browser or, if you clicked the right-mouse button to open a new tab, close the GitHub Issues tab. (Note that the remainder of these lab instructions will not go into this level of detail on opening and closing pages and tabs - pick the method that is best for you.)

4. **Code** is where [GitHub](https://github.com/) code repos, [Sauce Labs](https://saucelabs.com/) and Eclipse Orion Web IDE are integrated. Clicking on one of the three repos will display the respective (cloned) repo
  ![GitHubPage](screenshots/GitHubPage.png)
  while clicking on the **Eclipse Orion Web IDE** will display the Web editor.  We do not have a Sauce Labs account, so we really don't need the Sauce Labs integration.  We will leave it alone for now.
 5. **Deliver** is where the code gets built, tested and deployed through the integrations of build pipelines, one per microservice. We explore build pipelines later.
 6. **Manage** is where the integrations to management tools, such as [Pager Duty](https://www.pagerduty.com/), get added.
 7. **Learn** is where the integrations to tools helping to drive application insight, such as [DevOps Insights](https://console.ng.bluemix.net/catalog/services/devops-insights/), get added.
 8. **Culture** is where the integrations to teams collaborate more effectively are integrated, such as [Slack](http://slack.com/).

## Task 2: Examine the build pipelines

1. Clicking on one of the GitHub repo links or the **Configured** link will display the respective GitHub repo.
  ![GitHubRepoLink](screenshots/GitHubRepoLink.png)

2. You can either click on the three vertical dots on the upper-right of the _ui-toolchain-demo_ Delivery Pipeline tile to display **Action** and then click on **Configure**
  ![UIPipelineConfigureLink](screenshots/UIPipelineConfigureLink.png)
  **or** click on the circle in the center of the _ui-toolchain-demo_ Delivery Pipeline tile
  ![UIPipelineConfigureLink2](screenshots/UIPipelineConfigureLink2.png)
  to display the UI delivery pipeline.
  ![UIPipeline](screenshots/UIPipeline.png)

3. While we were busy exploring the toolchain, the various pipelines (remember, we have 3) started the build process.  The UI delivery pipeline displays the status of each stage in the UI pipeline.  The **Build** and **Dev** stage passed, while the **Test** stage failed.  The **Prod** stage was not even attempted.  If you look at the other 2 pipelines (catalog and orders) you would see similar results.

4. In the Build stage, click **View logs and history**
  ![ClickUIBuildLog](screenshots/ClickUIBuildLog.png)
  to display the commands and results of the Build stage, in this case simply cloning the repo.
  ![UIBuildLog](screenshots/UIBuildLog.png)
  Click the arrow to the left of Pipeline
  ![UIClickPipelineArrowToReturn](screenshots/ClickPipelineArrowToReturn.png)
  to return to the delivery pipeline.

5. In the **Dev** stage, click **View logs and history** to display the commands and results of the Dev stage. This stage deployed the UI microservice to the dev space.
  ![UIDevBuildResults](screenshots/UIDevBuildResults.png)

6. If you scroll through the log, you can see all the details of that job.

7. Click **Configure** in the upper right hand corner of the display
  ![UIDevClickConfigure](screenshots/UIDevClickConfigure.png)
  to display the Stage Configuration screen.
  ![UIDevStage1](screenshots/UIDevStage1.png)

8. The Stage Configuration displays details about the stage.  The _INPUT_ tab displays the input to the stage, the _JOBS_ tabs displays the discrete pieces of the stage, and the _ENVIRONMENT PROPERTIES_ tab displays variables used by the jobs in the stage.
  ![UIDevStage4](screenshots/UIDevStage4.png)

9. The UI Dev stage has just one job, the _Deploy_ job. The job name can be changed by simply typing over the name. The job can be removed by clicking the **Remove** button and a new job can be added by clicking the **Add Job** button.
  ![UIDevStage2](screenshots/UIDevStage2.png)

<div class="page-break"></div>

10. Details of each job are displayed when the job is selected.
  ![UIDevStage3](screenshots/UIDevStage3.png)
  The lines in the Deploy Script:<br>
  `export CF_APP_NAME="dev-$CF_APP"`<br>
  `cf push "${CF_APP_NAME}"`<br>
set the name of the application to deploy as the application name prefaced with _dev_ and the issues the Cloud Foundry command to deploy it.

11. Return to the delivery pipeline. Click on the application link to display the application.
  ![UIPipelineClickApp](screenshots/UIPipelineClickApp.png)

12. The application is displayed. We deployed all the microservices to the _dev_ space and prefaced each deployed app with _dev_.
  ![UIPipelineDevApp](screenshots/UIPipelineDevApp.png)

13. Close the application tab to return to the delivery pipeline, then return to the Pipeline and finally return to the Toolchain.
  ![ToolchainCreated](screenshots/ToolchainCreated.png)

<div class="page-break"></div>

# Lab 3: Customize Toolchain to add Slack Integration

## Objective
This lab will integrate Slack into the Continuous Delivery Toolchain. [Slack](http://slack.com/) is a cloud-based team collaboration tool.  We will integrate Slack into our Toolchain so team members get notified when development events, such as builds, occur.

**Tasks**:
- Task 1: Integrate Slack
- Task 2: Work with Slack
- Task 3: Modify Toolchain for Sauce Labs test job
- Task 4: Modify Toolchain for Functional Tests job
- Task 5: Examine PROD Stage

## Task 1: Integrate Slack

1. If we needed to add Slack to a Toolchain, we would click **Add a Tool** on the Toolchain display and select **Slack** from the available integrations.  We don't have to do this as the Microservices template already included Slack in the Toolchain but we did not configure it.  We also have a Slack user ID already created (_bluemix_interconnect_).

2. You should be displaying the Toolchain.
  ![ToolchainCreated2](screenshots/ToolchainCreated2.png)
  If not, click on the hamburger menu, then **Menu**.  Then click on **Services** then **DevOps**.  Then click on **Toolchains**.  And finally click on the toolchain you created.

3. Click on **Configure** to configure the connection between Bluemix and Slack.
  ![ClickConfigureSlack](screenshots/ClickConfigureSlack.png)

3. Enter the following information:
   1. Slack webhook (all one string):
   **https://hooks.slack.com/services/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**

   2. Slack channel:
   **interconnect_devops**

   3. Slack team URL Host name:
   **bluemixdevopslab**

   ![CreateSlack](screenshots/CreateSlack.png)

4. Click **Save Integration** to save the information.

5. Slack should now be Configured
   ![ConfiguredSlack](screenshots/ConfiguredSlack.png)

## Task 2: Work with Slack

1. In the browser, open a new tab and go to the following URL to go to the (already created) Slack team.
   https://bluemixdevopslab.slack.com

2. Enter the following information:
   1. Email address: **BluemixInterConnectDevOps@gmail.com**
   2. Password: **devops4me**
   ![SignIntoSlack](screenshots/SignIntoSlack.png)

   and click **Sign In**.

3. Click on the **interconnect_devops** channel to show the messages for that channel.
  ![SlackChannel](screenshots/SlackChannel.png)

  This channel will show all the messages the Toolchain sends to it.

4. Leaving the Slack browser tab open, switch over to the Toolchain browser tab.
5. Click on the UI Delivery Pipeline tile to display the UI Delivery Pipeline.
  ![ClickUIDeliveryPipeline](screenshots/ClickUIDeliveryPipeline.png)
6. Click on the arrow in the DEV stage to run the jobs in the DEV stage.
  ![UIDevRunBuild](screenshots/UIDevRunBuild.png)
7. The DEV stage runs the jobs and you get a visual indication of the progress of the stage.
  ![UIDevBuildRunningBMX](screenshots/UIDevBuildRunningBMX.png)
8. Switch to the Slack browser tab.  Here you also get a visual indication of the progress of the DEV stage process.  This is useful for notifying team members when events occur without them being logged into Bluemix the DEV stage passed but the TEST stage failed.  The _Deploy_ job was successful, but the _Sauce Labs Test_ job failed and the _Functional Tests_ job was not attempted.
  ![UIDevFailedBuildSlack](screenshots/UIDevFailedBuildSlack.png)
9. Switch back to the Bluemix browser tab. The Toolchain indicates the same results. Something is wrong with the Toolchain.
  ![UIDevFailedBuildBMX](screenshots/UIDevFailedBuildBMX.png)

## Task 3: Modify Toolchain for Sauce Labs test job

1. Remember we ran the DEV stage.  Why did the TEST stage start?  In the TEST stage tile, click on the gear and then click on **Configure Stage**.
  ![UITestStageClickToConfigure](screenshots/UITestStageClickToConfigure.png)
1. The TEST stage has three jobs, _Deploy_, _Sauce Labs Test_, and _Functional Tests_.
  ![UITestStageJobs](screenshots/UITestStageJobs.png)
2. The _Deploy_ job is highlighted (the blue circle around the icon), so details are the _Deploy_ job are displayed.
3. Click **INPUT** to display the input settings for the TEST stage.
  ![UITestStageClickInput](screenshots/UITestStageClickInput.png)
4. The Stage Trigger for the TEST stage indicates that this stage will run when the prior stage is complete.  So in this case, when the DEV stage is complete, the Toolchain started the TEST stage.
  ![UITestStageTrigger](screenshots/UITestStageTrigger.png)
5. Click on **JOBS** to display the jobs for the TEST stage.
6. Click on the **Sauce Labs Test** icon to display the details for the _Sauce Labs Test_ job.
7. At the bottom of the details, under _Run Conditions_, the option to stop the stage if this job (the _Sauce Labs Test_ job) fails. That explains why this stage stopped and why the _Functional Tests_ job did not run.
  ![UITestStageSauceJobStop](screenshots/UITestStageSauceJobStop.png)
8. Why did the _Sauce Labs Test_ job fail?  As you may recall, we never configured that tool integration after we created the Toolchain.  Sauce Labs requires a valid userid and password and we do not have those.  If we did, we would configure the Sauce Labs integration with those details.  So we have two choices, either remove the _Sauce Labs Test_ job from the TEST stage or, assuming we may one day get a Sauce Labs account, allow the _Sauce Labs Test_ job to fail but continue the stage.  We will choose this option.
9. Deselect the option to stop the stage if this job fails and click **SAVE**.
  ![UITestStageSauceJobGo](screenshots/UITestStageSauceJobGo.png)
10. Click on the arrow in the TEST stage to run the jobs in the TEST stage.
  ![UITestStageRun](screenshots/UITestStageRun.png)
11. Both Bluemix and Slack show the TEST stage executing, even though the _Sauce Labs Test_ job still fails.
  ![UITestStageRunSlack](screenshots/UITestStageRunSlack.png)
12. After a few moments (or minutes), the _Functional Tests_ job also fails.  This is the first time we executed it.  There is still a problem.
  ![UITestStageFVTestFails](screenshots/UITestStageFVTestFails.png)

## Task 4: Modify Toolchain for Functional Tests job

1. Click **View logs and history**.
  ![UITestStageFVTestClickLog](screenshots/UITestStageFVTestClickLog.png)
2. The stage log history displays  the number of times the stage has been executed and the results, showing the individual jobs.
  ![UITestStageLogHistoryFailures](screenshots/UITestStageLogHistoryFailures.png)

  The _Deploy_ job passed and the _Sauce Labs Test_ job and the _Functional Tests_ job both failed.

3. Click **Functional Tests** to display the log for that job.
  ![UITestStageLogHistoryClickFVTest](screenshots/UITestStageLogHistoryClickFVTest.png)
4. Scroll to the bottom of the log file. The message indicates that the missing Sauce Labs credentials are again making the job fail.
  ![UITestStageLogHistoryMissingSauceLabs](screenshots/UITestStageLogHistoryMissingSauceLabs.png)
5. We could modify the _Functional Tests_ job options to allow the TEST stage to continue despite the failure of the _Functional Tests_ job.  Instead, we will remove the job from the stage.
6. Scroll back to the top of the browser tab. Click **CONFIGURE** to configure the TEST stage.
  ![UITestStageLogHistoryClickToConfigure](screenshots/UITestStageLogHistoryClickToConfigure.png)
7. Click the **Functional Tests** job to select it.
  ![UITestStageSelectFVT](screenshots/UITestStageSelectFVT.png)
8. Click **REMOVE** to delete the _Functional Tests_ job from the TEST stage.  Click **OK** to confirm.
  ![UITestStageRemoveFVT](screenshots/UITestStageRemoveFVT.png)
9. Scroll to the bottom of the page and click **SAVE** to save the stage changes.
10. The Delivery Pipeline is displayed.  Run the _TEST_ stage by clicking on the arrow in the TEST stage.
11. Look at both the Bluemix console and the Slack channel. The _Deploy_ job runs (and passes), the _Sauce Labs Test_ job fails, and the _TEST_ stage completes.
  ![UITestStagePassed](screenshots/UITestStagePassed.png)
12. Click on the application link to display the application running in the _TEST_ space.
    ![UIPipelineTestClickApp](screenshots/UIPipelineTestClickApp.png)

## Task 5: Examine PROD Stage

1. Since the _TEST_ stage completed, the next stage, _PROD_, executed.  The _PROD_ stage successfully executed.
  ![UIProdStagePassedBMX](screenshots/UIProdStagePassedBMX.png)
2. Display the Slack browser tab. This also shows the successful execution of the _PROD_ stage.
  ![UIProdStageSuccessSlack](screenshots/UIProdStageSuccessSlack.png)
3. Click on the **PASSED** link at the end of the line _Job '**Blue/Green Deploy**' in Stage '**PROD**' #1 has passed_.  
  ![UIProdStageSlackClickForBlueGreenJob](screenshots/UIProdStageSlackClickForBlueGreenJob.png)
  This displays the job log for the _Blue/Green Deploy_ job in the _PROD_ stage.  We could have gotten to this display from the Bluemix console, but clicking on the link in Slack accesses the log directly from Slack without the need to go through Bluemix.
4. The log shows the first step of the job failing.
  ![UIProdStageSlackBlueGreenJobFailedFind](screenshots/UIProdStageSlackBlueGreenJobFailedFind.png)
5. A look at the script used in this job (by clicking on **CONFIGURE** in the upper right hand corner) shows the following:
 ```
#!/bin/bash
     if ! cf app $CF_APP; then
       cf push $CF_APP
     else
       OLD_CF_APP=${CF_APP}-OLD-$(date +"%s")
       rollback() {
         set +e
         if cf app $OLD_CF_APP; then
           cf logs $CF_APP --recent
           cf delete $CF_APP -f
           cf rename $OLD_CF_APP $CF_APP
         fi
         exit 1
       }
       set -e
       trap rollback ERR
       cf rename $CF_APP $OLD_CF_APP
       cf push $CF_APP
       cf delete $OLD_CF_APP -f
     fi
```
  This script does a _Blue-green_ deployment. A blue-green deployment is a release technique reducing downtime and risk by running two identical production environments called Blue and Green. At any time, only one of the environments is live, with the live environment serving all production traffic.

  The script first issues the Cloud Foundry (cf) app command for the UI application to be deployed.  If the command returns without finding the application, then the script pushes (deploys) the UI application.
  ```
  #!/bin/bash
      if ! cf app $CF_APP; then
        cf push $CF_APP
  ```
  This is what happens the first time this job and script are run, as the application has never been deployed.
6. The end of the log shows the successful deployment.  All that failed was finding a running UI application.
  ![UIProdStageSlackBlueGreenJobFailedFindDeploy](screenshots/UIProdStageSlackBlueGreenJobFailedFindDeploy.png)
6. Return to the delivery pipeline (by clicking on the left arrow next to _Pipeline_).
  ![UIProdStageClickPipeline](screenshots/UIProdStageClickPipeline.png)
7. Run the _PROD_ stage again by clicking on the arrow next to _PROD_.
8. Wait for the _PROD_ stage to complete, remembering you can watch either the Bluemix console or the Slack channel.
9. Once the _PROD_ stage completes, look at the job log for the deployment.  It shows the first step of the job finding a running UI application.
  ![UIProdStageSlackBlueGreenJobGoodFind](screenshots/UIProdStageSlackBlueGreenJobGoodFind.png)
10. Scrolling down the log further, we see the old UI application being renamed and the new one being deployed.
  ![UIProdStageSlackBlueGreenJobGoodFindRename](screenshots/UIProdStageSlackBlueGreenJobGoodFindRename.png)
11. Finally, once the new UI application is successfully deployed, the old one is deleted.
  ![UIProdStageSlackBlueGreenJobGoodFindDelete](screenshots/UIProdStageSlackBlueGreenJobGoodFindDelete.png)
12. Return to the Pipeline by clicking on the arrow to the left of _Pipeline_ in the upper left corner.
13. Return to the Toolchain by clicking on the arrow to the left of _Toolchain_ in the upper left corner.
