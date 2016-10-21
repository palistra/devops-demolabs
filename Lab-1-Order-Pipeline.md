# Objective
This series of labs shows how to set up a productive toolchain with a sample that consists of three microservices. After you finish this part of the series, you will be familiar with a toolchain that demonstrates practices from the IBM® Bluemix® Garage Method. Toolchains are available in the US South region only.

## Prerequisites
Prior to running these labs, you must have a Bluemix account, a GitHub account and access to a lab laptop.  Follow the steps in Lab 0 to create one or both of those accounts.

## Labs
- [Lab 0: Create Bluemix and GitHub accounts](#lab-0-Create-Bluemix-and-GitHub-accounts)
- [Lab 1: Set-up Toolchain for Order](#lab-1-Set-up-Toolchain-for-Order)
- [Lab 2: Set-up Toolchain for Catalog](#lab-2-Set-up-Toolchain-for-Catalog)
- [Lab 3: Set-up Toolchain for UI](#lab-3-Set-up-Toolchain-for-UI)
- [Lab 4: Add PagerDuty Integration](#lab-4-Add-PagerDuty-Integration)
- [Lab 5: Add Slack Integration](#lab-5-Add-Slack-Integration)
- [Lab 6: Deliver a UI Change](#lab-7-Deliver-a-UI-Change)

# Lab 0: Create Bluemix and GitHub accounts
**Tasks**:
- [Task 1: Create Bluemix account](#task-1-Create-Bluemix-account)
- [Task 2: Create GitHub account](#task-2-Create-GitHub-account)

## Task 1: Create Bluemix account
1. If you already have a Bluemix account, skip this task.
2. In a web browser, enter the following URL: https://console.ng.bluemix.net/
3. Click on the SIGN UP button.
4. Follow the directions to fill out the form. Note you will need access to your IBM eMail account in order to register.

## Task 2: Create GitHub account
1. If you already have a GitHub account, skip this task.
2. In a web browser, enter the following URL: https://github.com/
3. Click on the SIGN UP button.
4. Follow the directions to fill out the form. Note you will need access to your IBM eMail account in order to register.

# Lab 1: Set-up Toolchain for Order
**Tasks**:
- [Task 1: Log into IBM Bluemix](#task-1-log-into-ibm-Bluemix)
- [Task 2: Create Toolchain for Order](#task-2-create-toolchain-for-order)
- [Task 3: Add and Configure GitHub Integration for Order](#task-3-add-and-configure-github-integration-for-order)
- [Task 4: Add Order Delivery Pipeline](#task-4-add-order-delivery-pipeline)
- [Task 5: Configure Order Delivery Pipeline](#task-4-configure-order-delivery-pipeline)

## Task 1: Log into IBM Bluemix
1. If you are not already logged into IBM Bluemix, log into IBM Bluemix.
![Bluemix](screenshots/bluemix-login.jpg)

## Task 2: Create Toolchain for Order
  1. Click on **DevOps**.

  ![DevOps](screenshots/DevOpsImage.jpg)
  2. Click on **Toolchains**.

  ![Toolchain](screenshots/ToolChain.jpg)
  3. Click on the **+** plus icon on the right side of the screen.

  ![AddToolchain](screenshots/AddToolchain.jpg)
  4. Click on **Build your own toolchain**.

  ![BuildYourOwnToolchain](screenshots/BuildYourOwnToolchain.jpg)
  5. Under 'Toolchain Settings', enter the name "devops-toolchain-lab" and click **Create**.

  ![BuildYourOwnToolchain](screenshots/DevOpsToolchainName.jpg)

  Your Toolchain is created and you are redirected to the Toolchain panel.

## Task 3: Add and Configure GitHub Integration for Order
The code for the Order microservice already exists in a GitHub repository (https://github.com/open-toolchain/Microservices_OrdersAPI).  We will clone this repository and link to the clone.

  1. Click on the **+** plus icon on the right side of the screen to add a Tool Integration.
  2. Click on **GitHub** to add integration with GitHub to the Toolchain.

  ![ClickOnGitHub](screenshots/ClickOnGitHub.jpg)

<ul>
<li>Select 'Clone' as the Repository type.
<li>Enter "https://github.com/githubuserid/orders-api-toolchain-lab.git" for the New Repository Name.
<li>Enter "https://github.com/open-toolchain/Microservices_OrdersAPI" for the Source repository URL.
<li>Ensure the 'Enable GitHub Issues' checkbox is selected.
</ul>

![CreateNewGitHub](screenshots/CreateNewGitHub.jpg)

<ul>     
<li>Click <b>Create Integration</b>.
</ul>

  The devops-toolchain-lab tool integrations is displayed.

  ![CreateNewGitHubResult](screenshots/CreateNewGitHubResult.jpg)

## Task 4: Add Order Delivery Pipeline
Now that you have a Git repository clone of the code, we will add a Delivery Pipeline to deploy it and test it.

  1. Click on the **+** plus icon on the right side of the screen to add a Tool Integration.
  2. Click on **Delivery Pipeline** to create a new Delivery Pipeline (we will add tool integrations to this).
  3. Under 'Pipeline name:', enter "orders-toolchain-lab" and select the 'Show apps in the VIEW APP menu' checkbox.

  ![CreateDeliveryPipeline](screenshots/CreateDeliveryPipeline.jpg)
  4. Click **Create Integration**.
  5. The devops-toolchain-lab delivery pipeline is displayed.

    ![CreateDeliveryPipelineResult](screenshots/CreateDeliveryPipelineResult.jpg)

## Task 5: Configure Order Delivery Pipeline

<ol compact>
<li>Now to configure the orders-toolchain-lab delivery pipeline. Four stages will be added: Build, Dev, Test and Prod.
<ul compact>
<li>The <b>Build</b> stage has one job, performing the initial build of the code from the GitHub Repository.
<li>The <b>Dev</b> stage has one job, taking the output from the Build stage and deploying on Bluemix into the <i>dev</i> space.
<li>The <b>Test</b> stage has two jobs, taking the output from the Dev  stage and deploying on Bluemix into the <i>qa</i> space, then performing automated tests.
<li>The <b>Prod</b> stage has one job, taking the output from the Test stage and deploying on Bluemix into the <i>prod</i> space.  This stage will also check to see there is an earlier instance of this application running and if it is, keep it around in case the deploy of the new version of the app has problems.  If the new version deploys successfully, the old version is deleted.  If not, the new version is deleted and the old version continues to run.
</ul>
<p>Click on the <b>Delivery Pipeline</b> tile.
<br>
<img src="screenshots/ClickConfigureDeliveryToolchain.jpg" alt="ClickConfigureDeliveryToolchain">
<li>Add the <b>Build</b> stage and jobs.
<ol compact>
    <li>Click on <b>ADD STAGE</b>.
    <li>On the <b>INPUT</b> tab, enter "Build" for Stage Name. Note that:
    <ul compact>
    <li>'Input Type' is set to a SCM Repository, in this case, Git.
    <li>'Git Repository' is set to the name of the Git Repository we just cloned.
    <li>'Git URL' is set to the URL of the Git Repository we just cloned.
    <li>'Branch' is set to "Master".
    <li>'Stage Trigger' is set to "Run jobs whenever a change is pushed to Git", resulting in the Build stage running continuously when Git is updated.
    <br>
    <img src="screenshots/CreateOrderDeliveryPipelineBuildStage.jpg" alt="CreateOrderDeliveryPipelineBuildStage">
    </ul>
    <li>Click the <b>Jobs</b> tab.
    <li>Click <b>ADD JOB</b>.
    <li>Click the <b>+</b> and select <b>Build</b> for the JOB TYPE.
    <br>
    <img src="screenshots/AddBuildJobToOrderDeliveryPipelineBuildStage.jpg" alt="AddBuildJobToOrderDeliveryPipelineBuildStage">
    <li>On the Job configuration panel, note that:
    <ul compact>
    <li>'Builder Type' is set to "Simple" (other options are available on the pull-down).
    <li>'Run Conditions' is set to "Stop running this stage if this job fails" to prevent any other jobs in this stage from running and to make the stage failed is this Job fails.
    <br>
    <img src="screenshots/BuildJobToOrderDeliveryPipelineBuildStageConfiguration.jpg" alt="BuildJobToOrderDeliveryPipelineBuildStageConfiguration">
    </ul>
    <li>Click <b>Save</b> to save the <b>Build</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> stage.  This stage has not been run. Click on the <b>Run Stage</b> icon to run the build.
    <br>
    <img src="screenshots/RunBuildButton.jpg" alt="RunBuildButton">
    <p>The JOBS section shows the Build was successful.
    <br>
    <img src="screenshots/BuildSuccessful.jpg" alt="BuildSuccessful.jpg">
    <p>The <b>Build</b> stage has been successfully added and executed.
</ol>
<li>Add the <b>Dev</b> stage and jobs (remember, just one job, deploying to the <i>dev</i> space).
<ol compact>
    <li>Click on <b>ADD STAGE</b>.
    <li>On the <b>INPUT</b> tab, enter "Dev" for Stage Name. Note that:
    <ul compact>
    <li>'Input Type' is set to Build Artifacts (from the <b>Build</b> stage).
    <li>'Stage' and 'Job' are both 'Build'.
    <li>'Stage Trigger' is set to "Run jobs when the previous stage is completed", resulting in the Dev stage running when the <b>Build</b> stage successfully completes.
    </ul>
    <li>Click the <b>Jobs</b> tab.
    <li>Click <b>ADD JOB</b>.
    <li>Click the <b>+</b> and select <b>Deploy</b> for the JOB TYPE.
    <li>On the Job configuration panel, note that:
    <ul compact>
    <li>'Deployer Type' is set to "Cloud Foundry" (other options are available on the pull-down).
    <li>'Target' is set to "US South - https://api.ng/bluemix.net" as this is where the code will be deployed.
    <li>'Space' is set to "dev" (or Create a new space called <b>dev</b> if not on the dropdown).
    <li>Type the following into the "Deploy Script" section. This will create and deploy the cloudantNoSQLDB service:
    <code>cf create-service cloudantNoSQLDB Shared myMicroservicesCloudant</code>
    then deploy the application:
    <code>cf push "${CF_APP_NAME}"</code>.
    <pre>    
        #!/bin/bash
        cf create-service cloudantNoSQLDB Shared myMicroservicesCloudant
        # Push app
        export CF_APP_NAME="dev-$CF_APP"
        cf push "${CF_APP_NAME}"
        echo "Pushed App Name: ${CF_APP_NAME}."
        # View logs
        #cf logs "${CF_APP_NAME}" --recent
    </pre>    
    <li>'Run Conditions' is set to "Stop running this stage if this job fails" to prevent any other jobs in this stage from running and to make the stage failed is this Job fails.
    <br>
    <img src="screenshots/DevStageDevJobOrderDeliveryPipeline.jpg" alt="DevStageDevJobOrderDeliveryPipeline.jpg">
    </ul>
    <li>The bash script just entered into the Deploy Script references the <i>CF_APP_NAME</i> environment variable ($CF_APP is provided by default).  CF_APP_NAME needs to be added to the environment variables.
    <li>Click the <b>ENVIRONMENT PROPERTIES</b> tab.
    <li>Click <b>ADD PROPERTY</b> and select <b>Text Property</b>.
    <li>Enter "CF_APP_NAME" as the 'Name'.  Do not enter anything for the 'Value'.
    <li>Click <b>Save</b> to save the <b>Dev</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> and <b>Dev</b> stages.  The <b>Dev</b> stage has not been run. Click on the <b>Run Stage</b> icon to run the <b>Dev</b> stage and deploy the order API.
    <br>
    <img src="screenshots/RunningDevStageOrderDeliveryPipeline.jpg" alt="RunningDevStageOrderDeliveryPipeline">
    <p>The JOBS section shows the Deploy was successful. Click on "View logs and history" to the Job log.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineExecutionLog.jpg" alt="DevStageOrderDeliveryPipelineExecutionLog">
    <li>LAST EXECUTION RESULT displays the url to the successfully deployed application (dev-orders-toolchain-lab.mybluemix.net) as well as a link to the runtime log.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineExecutionResult.jpg" alt="DevStageOrderDeliveryPipelineExecutionResult">
    <br>Click on "dev-orders-toolchain-lab.mybluemix.net" to access the running application.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineRunning.jpg" alt="DevStageOrderDeliveryPipelineRunning">
    <p>The <b>Dev</b> stage has been successfully added and executed.
</ol>

<li>Add the <b>Test</b> stage (remember, two jobs, one to deploy to the <i>test</i> space and another to perform an automated test).  We will clone the <b>Dev</b> stage and make some modifications.
<ol compact>
    <li>Ensure the <b>Delivery Pipeline</b> is displayed.
    <li>On the <b>Dev</b> stage, click the <b>Stage Configuration</b> and select "Clone Stage".
    <br>
    <img src="screenshots/CloneDevStageOrderDeliveryPipeline.jpg" alt="CloneDevStageOrderDeliveryPipeline">
    <li>Rename the cloned stage from <b>Dev [copy]</b> to <b>Test</b>.
    <li>On the <b>Jobs</b> tab, change the space from <b>dev</b> to <b>qa</b> (or Create a new space called <b>qa</b> if not on the dropdown) and change the deploy script to change CF_APP_NAME to "test-$CF_APP" from "dev-$CF_APP".
    <br>
    <img src="screenshots/TestStageDeployStep.jpg" alt="TestStageDeployStep">
    <li>Add a new Job of type Test called <b>Test</b>.  There are a number of different Testers available. For this exercise, we will select the default Simple Tester. Enter the following code to the <b>Test Command</b>.
    <pre>    
      #!/bin/bash
      # invoke tests here
      echo "Testing of App Name ${CF_APP_NAME} was successful"      
    </pre>
    <li>Click the <b>ENVIRONMENT PROPERTIES</b> tab. Note the environment variable CF_APP_NAME is already present.
    <li>Click <b>Save</b> to save the <b>Test</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> and <b>Dev</b> stages.  The <b>Dev</b> stage has not been run.
    Click on the <b>Run Stage</b> icon to run the <b>Dev</b> stage and deploy the order API to the <i>test</i> space.
    <li>As before for the <b>Dev</b> stage, the JOBS section shows the Deploy and Test Jobs were successful. Click <b>Test</b> to display the log for the <b>Test</b> job.
    <br>
    <img src="screenshots/TestStageOrderDeliveryPipelineClickTestLog.jpg" alt="TestStageOrderDeliveryPipelineClickTestLog">
    <li>The <b>Test</b> job was successful.
    <br>
    <img src="screenshots/TestStageOrderDeliveryPipelineSuccessfulTestLog.jpg" alt="TestStageOrderDeliveryPipelineSuccessfulTestLog">
    <br>Click on "test-orders-toolchain-lab.mybluemix.net" to access the running application.
    <p>The <b>Test</b> stage has been successfully added and executed.
</ol>
<li>Add the <b>Prod</b> stage (remember, one job, to deploy to the <i>prod</i> space).   This stage will also check to see there is an earlier instance of this application running and if it is, keep it around in case the deploy of the new version of the app has problems.  If the new version deploys successfully, the old version is deleted.  If not, the new version is deleted and the old version continues to run.
<br>We will clone the <b>Dev</b> stage and make some modifications..
<ol compact>

<li>Ensure the <b>Delivery Pipeline</b> is displayed.
    <li>On the <b>Dev</b> stage, click the <b>Stage Configuration</b> and select "Clone Stage".
    <li>Rename the cloned stage from <b>Dev [copy]</b> to <b>Prod</b>.
    <li>On the <b>Jobs</b> tab, change the space from <b>dev</b> to <b>prod</b> (or Create a new space called <b>prod</b> if not on the dropdown) and change the deploy script to the following:
    <pre>
      #!/bin/bash
      cf create-service cloudantNoSQLDB Shared myMicroservicesCloudant
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
    </pre>     
    <br>
    <img src="screenshots/ProdStageDeployStep.jpg" alt="ProdStageDeployStep">
    <li>Click the <b>ENVIRONMENT PROPERTIES</b> tab. Note that the environment variable CF_APP_NAME is already present.
    <li>Click <b>Save</b> to save the <b>Prod</b> stage.
    <li>Click on <b>Run Stage</b> to run the <b>Prod</b> stage and deploy the order API to the <i>prod</i> space.
    <li>The JOBS section shows the Deploy was successful. Inspect the Job log.
    <br>
    <img src="screenshots/ProdStageOrderDeliveryPipelineExecutionResult1.jpg" alt="ProdStageOrderDeliveryPipelineExecutionResult1">
    <li>Redeploy the stage.  
    <li>The JOBS section shows the Deploy was successful. Inspect the Job log.  Note the application was renamed, replace and the old version deleted.
    <br>
    <img src="screenshots/ProdStageOrderDeliveryPipelineExecutionResult3.jpg" alt="ProdStageOrderDeliveryPipelineExecutionResult3">
    <br>
    <img src="screenshots/ProdStageOrderDeliveryPipelineExecutionResult2.jpg" alt="ProdStageOrderDeliveryPipelineExecutionResult2">
    <br>Click on the blue arrow to display the Delivery Pipeline. Click on "orders-toolchain-lab.mybluemix.net" to access the running application.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineRunning.jpg" alt="DevStageOrderDeliveryPipelineRunning">
    <p>The <b>Prod</b> stage has been successfully added and executed.  The Orders application has been deployed to production.
</ol>




# Lab 2: Set-up Toolchain for Catalog
Now that you have experience working with Toolchains and Pipelines, the next lab does not go into the same level of detail.  You may want to refer to the prior lab if you need additional details.  
You will be creating the same set of artifacts for the Catalog application.
**Tasks**:
- [Task 1: Create Toolchain for Catalog](#task-1-create-toolchain-for-catalog)
- [Task 2: Add and Configure GitHub Integration for Catalog](#task-2-add-and-configure-github-integration-for-catalog)
- [Task 3: Add Catalog Delivery Pipeline](#task-3-add-catalog-delivery-pipeline)
- [Task 4: Configure Catalog Delivery Pipeline](#task-4-configure-catalog-delivery-pipeline)

## Task 1: Create Toolchain for Catalog
  1. Click on **DevOps**.
  2. Click on **Toolchains**.
  3. Click on the **+** plus icon on the right side of the screen.
  4. Click on **Build your own toolchain**.
  5. Under 'Toolchain Settings', enter the name "catalog-toolchain-lab" and click **Create**.

  Your Toolchain is created and you are redirected to the Toolchain panel.

## Task 2: Add and Configure GitHub Integration for Catalog
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

## Task 3: Add Catalog Delivery Pipeline
Now that you have a Git repository clone of the code, we will add a Delivery Pipeline to deploy it and test it.

  1. Click on the **+** plus icon on the right side of the screen to add a Tool Integration.
  2. Click on **Delivery Pipeline** to create a new Delivery Pipeline (we will add tool integrations to this).
  3. Under 'Pipeline name:', enter "catalog-toolchain-lab" and select the 'Show apps in the VIEW APP menu' checkbox.
  4. Click **Create Integration**.
  5. The catalog-toolchain-lab delivery pipeline is displayed.

## Task 4: Configure Catalog Delivery Pipeline

<ol compact>
<li>Now to configure the catalog-toolchain-lab delivery pipeline. Four stages will be added: Build, Dev, Test and Prod.
<ul compact>
<li>The <b>Build</b> stage has one job, performing the initial build of the code from the GitHub Repository.
<li>The <b>Dev</b> stage has one job, taking the output from the Build stage and deploying on Bluemix into the <i>dev</i> space.
<li>The <b>Test</b> stage has two jobs, taking the output from the Dev stage and deploying on Bluemix into the <i>qa</i> space, then performing automated tests.
<li>The <b>Prod</b> stage has one job, taking the output from the Test stage and deploying on Bluemix into the <i>prod</i> space.  This stage will also check to see there is an earlier instance of this application running and if it is, keep it around in case the deploy of the new version of the app has problems.  If the new version deploys successfully, the old version is deleted.  If not, the new version is deleted and the old version continues to run.
</ul>
<p>Click on the <b>Delivery Pipeline</b> tile for the catalog-toolchain-lab delivery pipeline.
<li>Add the <b>Build</b> stage and jobs.
<ol compact>
    <li>Click on <b>ADD STAGE</b>.
    <li>On the <b>INPUT</b> tab, enter "Build" for Stage Name. Note that:
    <ul compact>
    <li>'Input Type' is set to a SCM Repository, in this case, Git.
    <li>'Git Repository' is set to the name of the Git Repository we just cloned.
    _________________________________________
    <li>'Git URL' is set to the URL of the Git Repository we just cloned.
    <li>'Branch' is set to "Master".
    <li>'Stage Trigger' is set to "Run jobs whenever a change is pushed to Git", resulting in the Build stage running continuously when Git is updated.
    </ul>
    <li>Click the <b>Jobs</b> tab.
    <li>Click <b>ADD JOB</b>.
    <li>Click the <b>+</b> and select <b>Build</b> for the JOB TYPE.
    <br>
    <img src="screenshots/AddBuildJobToOrderDeliveryPipelineBuildStage.jpg" alt="AddBuildJobToOrderDeliveryPipelineBuildStage">
    <li>On the Job configuration panel, note that:
    <ul compact>
    <li>'Builder Type' is set to "Simple" (other options are available on the pull-down).
    <li>'Run Conditions' is set to "Stop running this stage if this job fails" to prevent any other jobs in this stage from running and to make the stage failed is this Job fails.
    <br>
    <img src="screenshots/BuildJobToOrderDeliveryPipelineBuildStageConfiguration.jpg" alt="BuildJobToOrderDeliveryPipelineBuildStageConfiguration">
    </ul>
    <li>Click <b>Save</b> to save the <b>Build</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> stage.  This stage has not been run. Click on the <b>Run Stage</b> icon to run the build.
    <br>
    <img src="screenshots/RunBuildButton.jpg" alt="RunBuildButton">
    <p>The JOBS section shows the Build was successful.
    <br>
    <img src="screenshots/BuildSuccessful.jpg" alt="BuildSuccessful.jpg">
    <p>The <b>Build</b> stage has been successfully added and executed.
</ol>
<li>Add the <b>Dev</b> stage and jobs (remember, just one job, deploying to the <i>dev</i> space).
<ol compact>
    <li>Click on <b>ADD STAGE</b>.
    <li>On the <b>INPUT</b> tab, enter "Dev" for Stage Name. Note that:
    <ul compact>
    <li>'Input Type' is set to Build Artifacts (from the <b>Build</b> stage).
    <li>'Stage' and 'Job' are both 'Build'.
    <li>'Stage Trigger' is set to "Run jobs when the previous stage is completed", resulting in the Dev stage running when the <b>Build</b> stage successfully completes.
    </ul>
    <li>Click the <b>Jobs</b> tab.
    <li>Click <b>ADD JOB</b>.
    <li>Click the <b>+</b> and select <b>Deploy</b> for the JOB TYPE.
    <li>On the Job configuration panel, note that:
    <ul compact>
    <li>'Deployer Type' is set to "Cloud Foundry" (other options are available on the pull-down).
    <li>'Target' is set to "US South - https://api.ng/bluemix.net" as this is where the code will be deployed.
    <li>'Space' is set to "dev" (or Create a new space called <b>dev</b> if not on the dropdown).
    <li>Type the following into the "Deploy Script" section. This will create and deploy the cloudantNoSQLDB service:
    <code>cf create-service cloudantNoSQLDB Shared myMicroservicesCloudant</code>
    then deploy the application:
    <code>cf push "${CF_APP_NAME}"</code>.
    <pre>    
        #!/bin/bash
        cf create-service cloudantNoSQLDB Shared myMicroservicesCloudant
        # Push app
        export CF_APP_NAME="dev-$CF_APP"
        cf push "${CF_APP_NAME}"
        echo "Pushed App Name: ${CF_APP_NAME}."
        # View logs
        #cf logs "${CF_APP_NAME}" --recent
    </pre>    
    <li>'Run Conditions' is set to "Stop running this stage if this job fails" to prevent any other jobs in this stage from running and to make the stage failed is this Job fails.
    <br>
    <img src="screenshots/DevStageDevJobOrderDeliveryPipeline.jpg" alt="DevStageDevJobOrderDeliveryPipeline.jpg">
    </ul>
    <li>The bash script just entered into the Deploy Script references the <i>CF_APP_NAME</i> environment variable ($CF_APP is provided by default).  CF_APP_NAME needs to be added to the environment variables.
    <li>Click the <b>ENVIRONMENT PROPERTIES</b> tab.
    <li>Click <b>ADD PROPERTY</b> and select <b>Text Property</b>.
    <li>Enter "CF_APP_NAME" as the 'Name'.  Do not enter anything for the 'Value'.
    <li>Click <b>Save</b> to save the <b>Dev</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> and <b>Dev</b> stages.  The <b>Dev</b> stage has not been run. Click on the <b>Run Stage</b> icon to run the <b>Dev</b> stage and deploy the order API.
    <br>
    <img src="screenshots/RunningDevStageOrderDeliveryPipeline.jpg" alt="RunningDevStageOrderDeliveryPipeline">
    <p>The JOBS section shows the Deploy was successful. Click on "View logs and history" to the Job log.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineExecutionLog.jpg" alt="DevStageOrderDeliveryPipelineExecutionLog">
    <li>LAST EXECUTION RESULT displays the url to the successfully deployed application (dev-catalog-toolchain-lab.mybluemix.net) as well as a link to the runtime log.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineExecutionResult.jpg" alt="DevStageOrderDeliveryPipelineExecutionResult">
    <br>Click on "dev-catalog-toolchain-lab.mybluemix.net" to access the running application.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineRunning.jpg" alt="DevStageOrderDeliveryPipelineRunning">
    <p>The <b>Dev</b> stage has been successfully added and executed.
</ol>

<li>Add the <b>Test</b> stage (remember, two jobs, one to deploy to the <i>test</i> space and another to perform an automated test).  We will clone the <b>Dev</b> stage and make some modifications.
<ol compact>
    <li>Ensure the <b>Delivery Pipeline</b> is displayed.
    <li>On the <b>Dev</b> stage, click the <b>Stage Configuration</b> and select "Clone Stage".
    <br>
    <img src="screenshots/CloneDevStageOrderDeliveryPipeline.jpg" alt="CloneDevStageOrderDeliveryPipeline">
    <li>Rename the cloned stage from <b>Dev [copy]</b> to <b>Test</b>.
    <li>On the <b>Jobs</b> tab, change the space from <b>dev</b> to <b>qa</b> (or Create a new space called <b>qa</b> if not on the dropdown) and change the deploy script to change CF_APP_NAME to "test-$CF_APP" from "dev-$CF_APP".
    <br>
    <img src="screenshots/TestStageDeployStep.jpg" alt="TestStageDeployStep">
    <li>Add a new Job of type Test called <b>Test</b>.  There are a number of different Testers available. For this exercise, we will select the default Simple Tester. Enter the following code to the <b>Test Command</b>.
    <pre>    
      #!/bin/bash
      # invoke tests here
      echo "Testing of App Name ${CF_APP_NAME} was successful"      
    </pre>
    <li>Click the <b>ENVIRONMENT PROPERTIES</b> tab. Note the environment variable CF_APP_NAME is already present.
    <li>Click <b>Save</b> to save the <b>Test</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> and <b>Dev</b> stages.  The <b>Dev</b> stage has not been run.
    Click on the <b>Run Stage</b> icon to run the <b>Dev</b> stage and deploy the order API to the <i>test</i> space.
    <li>As before for the <b>Dev</b> stage, the JOBS section shows the Deploy and Test Jobs were successful. Click <b>Test</b> to display the log for the <b>Test</b> job.
    <br>
    <img src="screenshots/TestStageOrderDeliveryPipelineClickTestLog.jpg" alt="TestStageOrderDeliveryPipelineClickTestLog">
    <li>The <b>Test</b> job was successful.
    <br>
    <img src="screenshots/TestStageOrderDeliveryPipelineSuccessfulTestLog.jpg" alt="TestStageOrderDeliveryPipelineSuccessfulTestLog">
    <br>Click on "test-orders-toolchain-lab.mybluemix.net" to access the running application.
    <p>The <b>Test</b> stage has been successfully added and executed.
</ol>
<li>Add the <b>Prod</b> stage (remember, one job, to deploy to the <i>prod</i> space).   This stage will also check to see there is an earlier instance of this application running and if it is, keep it around in case the deploy of the new version of the app has problems.  If the new version deploys successfully, the old version is deleted.  If not, the new version is deleted and the old version continues to run.
<br>We will clone the <b>Dev</b> stage and make some modifications..
<ol compact>

<li>Ensure the <b>Delivery Pipeline</b> is displayed.
    <li>On the <b>Dev</b> stage, click the <b>Stage Configuration</b> and select "Clone Stage".
    <li>Rename the cloned stage from <b>Dev [copy]</b> to <b>Prod</b>.
    <li>On the <b>Jobs</b> tab, change the space from <b>dev</b> to <b>prod</b> (or Create a new space called <b>prod</b> if not on the dropdown) and change the deploy script to the following:
    <pre>
      #!/bin/bash
      cf create-service cloudantNoSQLDB Shared myMicroservicesCloudant
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
    </pre>     
    <br>
    <img src="screenshots/ProdStageDeployStep.jpg" alt="ProdStageDeployStep">
    <li>Click the <b>ENVIRONMENT PROPERTIES</b> tab. Note that the environment variable CF_APP_NAME is already present.
    <li>Click <b>Save</b> to save the <b>Prod</b> stage.
    <li>Click on <b>Run Stage</b> to run the <b>Prod</b> stage and deploy the order API to the <i>prod</i> space.
    <li>The JOBS section shows the Deploy was successful. Inspect the Job log.
    <br>
    <img src="screenshots/ProdStageOrderDeliveryPipelineExecutionResult1.jpg" alt="ProdStageOrderDeliveryPipelineExecutionResult1">
    <li>Redeploy the stage.  
    <li>The JOBS section shows the Deploy was successful. Inspect the Job log.  Note the application was renamed, replace and the old version deleted.
    <br>
    <img src="screenshots/ProdStageOrderDeliveryPipelineExecutionResult3.jpg" alt="ProdStageOrderDeliveryPipelineExecutionResult3">
    <br>
    <img src="screenshots/ProdStageOrderDeliveryPipelineExecutionResult2.jpg" alt="ProdStageOrderDeliveryPipelineExecutionResult2">
    <br>Click on the blue arrow to display the Delivery Pipeline. Click on "orders-toolchain-lab.mybluemix.net" to access the running application.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineRunning.jpg" alt="DevStageOrderDeliveryPipelineRunning">
    <p>The <b>Prod</b> stage has been successfully added and executed.  The Orders application has been deployed to production.
</ol>







******************************************************************************************************************
# Lab 3: Set-up Toolchain for UI
You will be creating the pipeline for the UI application.
**Tasks**:
- [Task 1: Log into IBM Bluemix](#task-1-log-into-ibm-Bluemix)
- [Task 2: Create Toolchain for Catalog](#task-2-create-toolchain-for-catalog)
- [Task 3: Add and Configure GitHub Integration for Catalog](#task-3-add-and-configure-github-integration-for-catalog)
- [Task 4: Add Catalog Delivery Pipeline](#task-4-add-catalog-delivery-pipeline)
- [Task 5: Configure Catalog Delivery Pipeline](#task-4-configure-catalog-delivery-pipeline)

## Task 2: Create Toolchain for Catalog
  1. Click on **DevOps**.
  2. Click on **Toolchains**.
  3. Click on the **+** plus icon on the right side of the screen.
  4. Click on **Build your own toolchain**.
  5. Under 'Toolchain Settings', enter the name "catalog-toolchain-lab" and click **Create**.

  Your Toolchain is created and you are redirected to the Toolchain panel.

## Task 4: Add Catalog Delivery Pipeline
Now that you have a Git repository clone of the code, we will add a Delivery Pipeline to deploy it and test it.

  1. Click on the **+** plus icon on the right side of the screen to add a Tool Integration.
  2. Click on **Delivery Pipeline** to create a new Delivery Pipeline (we will add tool integrations to this).
  3. Under 'Pipeline name:', enter "orders-toolchain-lab" and select the 'Show apps in the VIEW APP menu' checkbox.

  ![CreateDeliveryPipeline](screenshots/CreateDeliveryPipeline.jpg)
  4. Click **Create Integration**.
  5. The orders-toolchain-lab delivery pipeline is displayed.

    ![CreateDeliveryPipelineResult](screenshots/CreateDeliveryPipelineResult.jpg)

## Task 5: Configure Catalog Delivery Pipeline

<ol compact>
<li>Now to configure the orders-toolchain-lab delivery pipeline. Four stages will be added: Build, Dev, Test and Prod.
<ul compact>
<li>The <b>Build</b> stage has one job, performing the initial build of the code from the GitHub Repository.
<li>The <b>Dev</b> stage has one job, taking the output from the Build stage and deploying on Bluemix into the <i>dev</i> space.
<li>The <b>Test</b> stage has two jobs, taking the output from the Dev  stage and deploying on Bluemix into the <i>qa</i> space, then performing automated tests.
<li>The <b>Prod</b> stage has one job, taking the output from the Test stage and deploying on Bluemix into the <i>prod</i> space.  This stage will also check to see there is an earlier instance of this application running and if it is, keep it around in case the deploy of the new version of the app has problems.  If the new version deploys successfully, the old version is deleted.  If not, the new version is deleted and the old version continues to run.
</ul>
<p>Click on the <b>Delivery Pipeline</b> tile.
<br>
<img src="screenshots/ClickConfigureDeliveryToolchain.jpg" alt="ClickConfigureDeliveryToolchain">
<li>Add the <b>Build</b> stage and jobs.
<ol compact>
    <li>Click on <b>ADD STAGE</b>.
    <li>On the <b>INPUT</b> tab, enter "Build" for Stage Name. Note that:
    <ul compact>
    <li>'Input Type' is set to a SCM Repository, in this case, Git.
    <li>'Git Repository' is set to the name of the Git Repository we just cloned.
    <li>'Git URL' is set to the URL of the Git Repository we just cloned.
    <li>'Branch' is set to "Master".
    <li>'Stage Trigger' is set to "Run jobs whenever a change is pushed to Git", resulting in the Build stage running continuously when Git is updated.
    <br>
    <img src="screenshots/CreateOrderDeliveryPipelineBuildStage.jpg" alt="CreateOrderDeliveryPipelineBuildStage">
    </ul>
    <li>Click the <b>Jobs</b> tab.
    <li>Click <b>ADD JOB</b>.
    <li>Click the <b>+</b> and select <b>Build</b> for the JOB TYPE.
    <br>
    <img src="screenshots/AddBuildJobToOrderDeliveryPipelineBuildStage.jpg" alt="AddBuildJobToOrderDeliveryPipelineBuildStage">
    <li>On the Job configuration panel, note that:
    <ul compact>
    <li>'Builder Type' is set to "Simple" (other options are available on the pull-down).
    <li>'Run Conditions' is set to "Stop running this stage if this job fails" to prevent any other jobs in this stage from running and to make the stage failed is this Job fails.
    <br>
    <img src="screenshots/BuildJobToOrderDeliveryPipelineBuildStageConfiguration.jpg" alt="BuildJobToOrderDeliveryPipelineBuildStageConfiguration">
    </ul>
    <li>Click <b>Save</b> to save the <b>Build</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> stage.  This stage has not been run. Click on the <b>Run Stage</b> icon to run the build.
    <br>
    <img src="screenshots/RunBuildButton.jpg" alt="RunBuildButton">
    <p>The JOBS section shows the Build was successful.
    <br>
    <img src="screenshots/BuildSuccessful.jpg" alt="BuildSuccessful.jpg">
    <p>The <b>Build</b> stage has been successfully added and executed.
</ol>
<li>Add the <b>Dev</b> stage and jobs (remember, just one job, deploying to the <i>dev</i> space).
<ol compact>
    <li>Click on <b>ADD STAGE</b>.
    <li>On the <b>INPUT</b> tab, enter "Dev" for Stage Name. Note that:
    <ul compact>
    <li>'Input Type' is set to Build Artifacts (from the <b>Build</b> stage).
    <li>'Stage' and 'Job' are both 'Build'.
    <li>'Stage Trigger' is set to "Run jobs when the previous stage is completed", resulting in the Dev stage running when the <b>Build</b> stage successfully completes.
    </ul>
    <li>Click the <b>Jobs</b> tab.
    <li>Click <b>ADD JOB</b>.
    <li>Click the <b>+</b> and select <b>Deploy</b> for the JOB TYPE.
    <li>On the Job configuration panel, note that:
    <ul compact>
    <li>'Deployer Type' is set to "Cloud Foundry" (other options are available on the pull-down).
    <li>'Target' is set to "US South - https://api.ng/bluemix.net" as this is where the code will be deployed.
    <li>'Space' is set to "dev" (or Create a new space called <b>dev</b> if not on the dropdown).
    <li>Type the following into the "Deploy Script" section. This will create and deploy the cloudantNoSQLDB service:
    <code>cf create-service cloudantNoSQLDB Shared myMicroservicesCloudant</code>
    then deploy the application:
    <code>cf push "${CF_APP_NAME}"</code>.
    <pre>    
        #!/bin/bash
        cf create-service cloudantNoSQLDB Shared myMicroservicesCloudant
        # Push app
        export CF_APP_NAME="dev-$CF_APP"
        cf push "${CF_APP_NAME}"
        echo "Pushed App Name: ${CF_APP_NAME}."
        # View logs
        #cf logs "${CF_APP_NAME}" --recent
    </pre>    
    <li>'Run Conditions' is set to "Stop running this stage if this job fails" to prevent any other jobs in this stage from running and to make the stage failed is this Job fails.
    <br>
    <img src="screenshots/DevStageDevJobOrderDeliveryPipeline.jpg" alt="DevStageDevJobOrderDeliveryPipeline.jpg">
    </ul>
    <li>The bash script just entered into the Deploy Script references the <i>CF_APP_NAME</i> environment variable ($CF_APP is provided by default).  CF_APP_NAME needs to be added to the environment variables.
    <li>Click the <b>ENVIRONMENT PROPERTIES</b> tab.
    <li>Click <b>ADD PROPERTY</b> and select <b>Text Property</b>.
    <li>Enter "CF_APP_NAME" as the 'Name'.  Do not enter anything for the 'Value'.
    <li>Click <b>Save</b> to save the <b>Dev</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> and <b>Dev</b> stages.  The <b>Dev</b> stage has not been run. Click on the <b>Run Stage</b> icon to run the <b>Dev</b> stage and deploy the order API.
    <br>
    <img src="screenshots/RunningDevStageOrderDeliveryPipeline.jpg" alt="RunningDevStageOrderDeliveryPipeline">
    <p>The JOBS section shows the Deploy was successful. Click on "View logs and history" to the Job log.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineExecutionLog.jpg" alt="DevStageOrderDeliveryPipelineExecutionLog">
    <li>LAST EXECUTION RESULT displays the url to the successfully deployed application (dev-catalog-toolchain-lab.mybluemix.net) as well as a link to the runtime log.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineExecutionResult.jpg" alt="DevStageOrderDeliveryPipelineExecutionResult">
    <br>Click on "dev-catalog-toolchain-lab.mybluemix.net" to access the running application.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineRunning.jpg" alt="DevStageOrderDeliveryPipelineRunning">
    <p>The <b>Dev</b> stage has been successfully added and executed.
</ol>

<li>Add the <b>Test</b> stage (remember, two jobs, one to deploy to the <i>test</i> space and another to perform an automated test).  We will clone the <b>Dev</b> stage and make some modifications.
<ol compact>
    <li>Ensure the <b>Delivery Pipeline</b> is displayed.
    <li>On the <b>Dev</b> stage, click the <b>Stage Configuration</b> and select "Clone Stage".
    <br>
    <img src="screenshots/CloneDevStageOrderDeliveryPipeline.jpg" alt="CloneDevStageOrderDeliveryPipeline">
    <li>Rename the cloned stage from <b>Dev [copy]</b> to <b>Test</b>.
    <li>On the <b>Jobs</b> tab, change the space from <b>dev</b> to <b>qa</b> (or Create a new space called <b>qa</b> if not on the dropdown) and change the deploy script to change CF_APP_NAME to "test-$CF_APP" from "dev-$CF_APP".
    <br>
    <img src="screenshots/TestStageDeployStep.jpg" alt="TestStageDeployStep">
    <li>Add a new Job of type Test called <b>Test</b>.  There are a number of different Testers available. For this exercise, we will select the default Simple Tester. Enter the following code to the <b>Test Command</b>.
    <pre>    
      #!/bin/bash
      # invoke tests here
      echo "Testing of App Name ${CF_APP_NAME} was successful"      
    </pre>
    <li>Click the <b>ENVIRONMENT PROPERTIES</b> tab. Note the environment variable CF_APP_NAME is already present.
    <li>Click <b>Save</b> to save the <b>Test</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> and <b>Dev</b> stages.  The <b>Dev</b> stage has not been run.
    Click on the <b>Run Stage</b> icon to run the <b>Dev</b> stage and deploy the order API to the <i>test</i> space.
    <li>As before for the <b>Dev</b> stage, the JOBS section shows the Deploy and Test Jobs were successful. Click <b>Test</b> to display the log for the <b>Test</b> job.
    <br>
    <img src="screenshots/TestStageOrderDeliveryPipelineClickTestLog.jpg" alt="TestStageOrderDeliveryPipelineClickTestLog">
    <li>The <b>Test</b> job was successful.
    <br>
    <img src="screenshots/TestStageOrderDeliveryPipelineSuccessfulTestLog.jpg" alt="TestStageOrderDeliveryPipelineSuccessfulTestLog">
    <br>Click on "test-orders-toolchain-lab.mybluemix.net" to access the running application.
    <p>The <b>Test</b> stage has been successfully added and executed.
</ol>
<li>Add the <b>Prod</b> stage (remember, one job, to deploy to the <i>prod</i> space).   This stage will also check to see there is an earlier instance of this application running and if it is, keep it around in case the deploy of the new version of the app has problems.  If the new version deploys successfully, the old version is deleted.  If not, the new version is deleted and the old version continues to run.
<br>We will clone the <b>Dev</b> stage and make some modifications..
<ol compact>

<li>Ensure the <b>Delivery Pipeline</b> is displayed.
    <li>On the <b>Dev</b> stage, click the <b>Stage Configuration</b> and select "Clone Stage".
    <li>Rename the cloned stage from <b>Dev [copy]</b> to <b>Prod</b>.
    <li>On the <b>Jobs</b> tab, change the space from <b>dev</b> to <b>prod</b> (or Create a new space called <b>prod</b> if not on the dropdown) and change the deploy script to the following:
    <pre>
      #!/bin/bash
      cf create-service cloudantNoSQLDB Shared myMicroservicesCloudant
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
    </pre>     
    <br>
    <img src="screenshots/ProdStageDeployStep.jpg" alt="ProdStageDeployStep">
    <li>Click the <b>ENVIRONMENT PROPERTIES</b> tab. Note that the environment variable CF_APP_NAME is already present.
    <li>Click <b>Save</b> to save the <b>Prod</b> stage.
    <li>Click on <b>Run Stage</b> to run the <b>Prod</b> stage and deploy the order API to the <i>prod</i> space.
    <li>The JOBS section shows the Deploy was successful. Inspect the Job log.
    <br>
    <img src="screenshots/ProdStageOrderDeliveryPipelineExecutionResult1.jpg" alt="ProdStageOrderDeliveryPipelineExecutionResult1">
    <li>Redeploy the stage.  
    <li>The JOBS section shows the Deploy was successful. Inspect the Job log.  Note the application was renamed, replace and the old version deleted.
    <br>
    <img src="screenshots/ProdStageOrderDeliveryPipelineExecutionResult3.jpg" alt="ProdStageOrderDeliveryPipelineExecutionResult3">
    <br>
    <img src="screenshots/ProdStageOrderDeliveryPipelineExecutionResult2.jpg" alt="ProdStageOrderDeliveryPipelineExecutionResult2">
    <br>Click on the blue arrow to display the Delivery Pipeline. Click on "orders-toolchain-lab.mybluemix.net" to access the running application.
    <br>
    <img src="screenshots/DevStageOrderDeliveryPipelineRunning.jpg" alt="DevStageOrderDeliveryPipelineRunning">
    <p>The <b>Prod</b> stage has been successfully added and executed.  The Orders application has been deployed to production.
</ol>
*********************************************************************

# Lab 4: Add PagerDuty Integration
Now you want to send notifications to team members when something goes wrong. In this part of the lab, you add PagerDuty your toolchain and configure it so people get notified when things go wrong so problems can be fixed faster and reduce downtime.

**Tasks**:
- [Task 1: Add PagerDuty to Toolchain](#task-1-add-pagerduty-to-toolchain)
- [Task 2: Verify PagerDuty works by breaking application build](#task-2-verify-pagerduty-works-by-breaking-application-build)
- [Task 3: Fix application](#task-3-fix-application)

## Task 1: Add PagerDuty to Toolchain
1. On the toolchain's Tool Integrations page, click the add button **+**
2. Select **PagerDuty**
3. On the PagerDuty Configuration page:
   - Enter Nb8ZxY6sAWKxLp1UhA_u as the API access key
   - Enter devopsdemo as the PagerDuty service name
   - Leave the primary contact email address and phone number blank
   - OPTIONAL: If you can easily access an email account, enter that email address.  Check that account to accept the invitation to join PagerDuty.
 4. Click **Create Integration**

  ![PagerDutySetup](screenshots/PagerDutySetup.jpg)

## Task 2: Verify PagerDuty works by breaking application build
  1. On the toolchain's Tool Integrations page, click the add button **+**
  2. Select **Eclipse Orion Web IDE**.
  3. Click **Create Integration**
  4. On the toolchain's Tool Integrations page, click the **Eclipse Orion Web IDE** tile. The GitHub repos are automatically loaded in your workspace. The Web IDE workspace is on the cloud.
  5. In the file navigator, expand the catalog-api-toolchain_name repo (if needed).
  6. In the file directory, click manifest.yml to open the file.

  ![WebIDE](screenshots/WebIDE.jpg)
  7. Update the value for memory to 96g. This setting intentionally increases your memory to exceed the quota for your org. Your changes are automatically saved.
  8. Now to Push the changes.  From the Eclipse Orion Web IDE menu, click the **Git** icon.

  ![WebIDEGit](screenshots/WebIDEGit.jpg)

  9. In the Working Directory Changes section, which is in the upper-right corner of the window, make sure that the changed file is selected.

  ![WebIDEPush](screenshots/WebIDEPush.jpg)
  11. Click **Commit** to put the changes in the local master branch.
  12. Put these changes in the origin/master branch and click **Push**. Your changes are automatically built and deployed in the pipeline.
  13. Return to your toolchain's Tool Integrations page and click the pipeline tile for the catalog-api microservice to watch the stages run in response to your commit.
  14. The Deploy failed.

  ![WebIDEDeployFailed](screenshots/WebIDEDeployFailed.jpg)
  15. The PagerDuty console (which you cannot get to) shows the incident:

  ![PagerDutyConsole](screenshots/PagerDutyConsole.jpg)
  16. If you entered an email account when you setup the PagerDuty integration, that account will have an email.  The link in the email will allow you to view the incident on PagerDuty.

  ![PagerDutyeMail](screenshots/PagerDutyeMail.jpg)

## Task 3: Fix application

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
