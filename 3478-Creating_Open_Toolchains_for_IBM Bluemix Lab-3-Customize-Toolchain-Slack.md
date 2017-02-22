# Lab 3: Customize Toolchain to add Slack Integration

## Objective
This lab will integrate Slack into the Continuous Delivery Toolchain. [Slack](http://slack.com/) is a cloud-based team collaboration tool.  We will integrate Slack into our Toolchain so team members get notified when development events, such as builds, occur.

**Tasks**:
- Task 1: Integrate Slack
- Task 2: Examine Slack notifications

## Task 1: Integrate Slack

1. If we needed to add Slack to a Toolchain, we would click **Add a Tool** on the Toolchain display and select **Slack** from the available integrations.  We don't have to do this as the Microservices template already included Slack in the Toolchain but we did not configure it.  We also have a Slack user ID already created (_bluemix_interconnect_).

2. You should be displaying the Toolchain.
  ![ToolchainCreated2](screenshots/ToolchainCreated2.png)
  If not, click on the hamburger menu, then **Menu**.  Then click on **Services** then **DevOps**.  Then click on **Toolchains**.  And finally click on the toolchain you created.

3. Click on **Configure** to configure the connection between Bluemix and Slack.
  ![ClickConfigureSlack](screenshots/ClickConfigureSlack.png)

3. Enter the following all as one string as the Slack webhook.
  **https://hooks.slack.com/services/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**


This displays the **GitHub Issues** page.  Issues are used to track todos, bugs, feature requests, and more.  Each GitHub repository (_repo_ for short) can include issues.  The Microservices template we used only included issues for the UI repo.
  ![GitHubIssuePage](screenshots/GitHubIssuePage.png)
  Return to the Microservices toolchain by either clicking on the **Go back one page** arrow on the browser or, if you clicked the right-mouse button to open a new tab, close the GitHub Issues page. (Note that the remainder of these lab instructions will not go into this level of detail on opening and closing pages and tabs - pick the method that is best for you.)

4. **Code** is where [GitHub](https://github.com/) code repos, [Sauce Labs](https://saucelabs.com/) and Eclipse Orion Web IDE are integrated. Clicking on one of the three repos will display the respective (cloned) repo
  ![GitHubPage](screenshots/GitHubPage.png)
  while clicking on the **Eclipse Orion Web IDE** will display the Web editor.  We do not have a Sauce Labs account, so we really don't need the Sauce Labs integration.  We will leave it alone for now.
 5. **Deliver** is where the code gets built, tested and deployed through the integrations of build pipelines, one per microservice. We explore build pipelines later.
 6. **Manage** is where the intergrations to management tools, such as [Pager Duty](https://www.pagerduty.com/), get added.
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

10. Details of the each job are displayed when the job is selected.
  ![UIDevStage3](screenshots/UIDevStage3.png)
  The lines in the Deploy Script:<br>
  `export CF_APP_NAME="dev-$CF_APP"`<br>
  `cf push "${CF_APP_NAME}"`<br>
set the name of the application to deploy as the application name prefaced with _dev_ and the issues the Cloud Foundry command to deploy it.

11. Return to the delivery pipeline. Click on the application link to display the application.
  ![UIPipelineClickApp](screenshots/UIPipelineClickApp.png)

12. The application is displayed. We deployed all the microservices to the _dev_ space and prefaced each deployed app with _dev_.
  ![UIPipelineDevApp](screenshots/UIPipelineDevApp.png)
