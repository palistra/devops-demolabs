# Lab 2: Set-up Toolchain for Catalog

## Objective
This lab adds the Catalog application to the Toolchain.  And now that you have experience working with Toolchains and Pipelines, the next lab does not go into the same level of detail.  You may want to refer to the prior lab if you need additional details as you will be creating the same set of artifacts for the Catalog application.

**Tasks**:
- [Task 1: Go to devops-toolchain](#task-1-go-to-devops-toolchain)
- [Task 2: Create Toolchain for Catalog](#task-1-create-toolchain-for-catalog)
- [Task 3: Add and Configure GitHub Integration for Catalog](#task-2-add-and-configure-github-integration-for-catalog)
- [Task 4: Add Catalog Delivery Pipeline](#task-3-add-catalog-delivery-pipeline)
- [Task 5: Configure Catalog Delivery Pipeline](#task-4-configure-catalog-delivery-pipeline)

## Task 1: Go to devops-toolchain
<ol>
<li>If you are not on <b>devops-toolchain-lab</b> Toolchain:
<p>
<img src="screenshots/DevOpsToolchainLab.jpg" alt="DevOpsToolchainLab">
<p>perform the following steps:
<ol>
<li>Click on the Bluemix Menu Bar
<p>
<img src="screenshots/BluemixMenuBar.jpg" alt="BluemixMenuBar">
<li>Click <b>Services</b>
<li>Select DevOps to display all the Toolchains.
<li>Click on the <b>devops-toolchain-lab</b> tile.
</ol>

## Task 2: Add and Configure GitHub Integration for Catalog
The code for the Catalog microservice already exists in a GitHub repository (https://github.com/open-toolchain/Microservices_CatalogAPI).  We will clone this repository and link to the clone.
<ol>
<li>Click on <b>Add a Tool</b> on the right side of the screen to add a Tool Integration.
<li>Click on <b>GitHub</b> to add integration with GitHub to the Toolchain.
<li>Select 'Clone' as the Repository type.
<li>Enter "<i>githubuserid</i>/catalog-api-toolchain-lab.git" for the New Repository Name.
<br>where <i>githubuserid</i> is your GitHub userid.
<li>Enter "https://github.com/open-toolchain/Microservices_CatalogAPI" for the Source repository URL.
<li>Click <b>Create Integration</b>.
</ol>

## Task 3: Add Catalog Delivery Pipeline
Now that you have a Git repository clone of the code, we will add a Delivery Pipeline to deploy it and test it.

  1. Click on **Add a Tool** on the right side of the screen to add a Tool Integration.
  2. Click on **Delivery Pipeline** to create a new Delivery Pipeline (we will add tool integrations to this).
  3. Under 'Pipeline name:', enter "catalog-api-toolchain-lab" and select the 'Show apps in the VIEW APP menu' checkbox.
  4. Click **Create Integration**.
  5. The catalog-toolchain-lab delivery pipeline is displayed.

## Task 4: Configure Catalog Delivery Pipeline

<ol>
<li>Now to configure the catalog-api-toolchain-lab delivery pipeline. Four stages will be added: Build, Dev, Test and Prod.
<ul>
<li>The <b>Build</b> stage has one job, performing the initial build of the code from the GitHub Repository.
<li>The <b>Dev</b> stage has two jobs, taking the output from the Build stage and deploying on Bluemix into the <i>dev</i> space, then performing automated functional tests.
<li>The <b>Test</b> stage has two jobs, taking the output from the Dev stage and deploying on Bluemix into the <i>qa</i> space, then performing automated tests.
<li>The <b>Prod</b> stage has one job, taking the output from the Test stage and deploying on Bluemix into the <i>prod</i> space.  This stage will also check to see there is an earlier instance of this application running and if it is, keep it around in case the deploy of the new version of the app has problems.  If the new version deploys successfully, the old version is deleted.  If not, the new version is deleted and the old version continues to run.
</ul>
<p>Click on the <b>Delivery Pipeline</b> tile for the catalog-api-toolchain-lab delivery pipeline.
<li>Add the <b>Build</b> stage and jobs.
<ol>
    <li>Click on <b>ADD STAGE</b>.
    <li>On the <b>INPUT</b> tab, enter "Build" for Stage Name. Note that:
    <ul>
    <li>'Input Type' is set to a SCM Repository, in this case, Git.
    <li>Make sure'Git Repository' points to the "catalog" repo from the dropdown list.    
    <li>'Git URL' is set to the URL of the Git Repository we just cloned.
    <li>'Branch' is set to "Master".
    <li>'Stage Trigger' is set to "Run jobs whenever a change is pushed to Git", resulting in the Build stage running continuously when Git is updated.
    </ul>
    <li>Click the <b>Jobs</b> tab.
    <li>Click <b>ADD JOB</b>.
    <li>Click the <b>+</b> and select <b>Build</b> for the JOB TYPE.
    <li>On the Job configuration panel, note that:
    <ul>
    <li>'Builder Type' is set to "Simple" (other options are available on the pull-down).
    <li>'Run Conditions' is set to "Stop running this stage if this job fails" to prevent any other jobs in this stage from running and to make the stage failed is this Job fails.
    </ul>
    <li>Click <b>Save</b> to save the <b>Build</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> stage.  This stage has not been run. Click on the <b>Run Stage</b> icon to run the build.
    <p>The JOBS section shows the Build was successful.
    <p>The <b>Build</b> stage has been successfully added and executed.
</ol>
<li>Add the <b>Dev</b> stage and jobs (remember, two jobs, taking the output from the Build stage and deploying on Bluemix into the <i>dev</i> space, then performing automated functional tests).
<ol>
    <li>Click on <b>ADD STAGE</b>.
    <li>On the <b>INPUT</b> tab, enter "Dev" for Stage Name. Note that:
    <ul>
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
    <li>'Application Name' is "catalog-api-toolchain-lab".
    <li>Type the following into the "Deploy Script" section.
<pre><code>
#!/bin/bash
#get user name
a=$(cf services | grep @)
b=${a%@&#42;}
c=($b)
len=${#c[@]}
user_name=${c[len-1]}
#add Cloudant service
cf create-service cloudantNoSQLDB Lite myMicroservicesCloudant
# Push app
export CF_APP_NAME="$user_name-dev-$CF_APP"
cf push "${CF_APP_NAME}"
echo "Pushed App Name: ${CF_APP_NAME}."
export APP_URL=http://$(cf app $CF_APP_NAME | grep urls: | awk '{print $2}')
# View logs
#cf logs "${CF_APP_NAME}" --recent
</code></pre>
<p>This will create and deploy the cloudantNoSQLDB service, update the APP_URL environment variable for use by the functional test job and deploy the Catalog application.    
    <li>'Run Conditions' is set to "Stop running this stage if this job fails" to prevent any other jobs in this stage from running and to make the stage failed is this Job fails.
    </ul>
    <li>The bash script just entered into the Deploy Script references both the <i>CF_APP_NAME</i> and <i>APP_URL</i> environment variable ($CF_APP is provided by default).  These need to be added to the environment variables as Text.  Click the <b>ENVIRONMENT PROPERTIES</b> tab.
    <li>Click <b>ADD PROPERTY</b> and select <b>Text Property</b>.
    <li>Enter "CF_APP_NAME" as the 'Name'.  Do not enter anything for the 'Value'.
    <li>Click <b>ADD PROPERTY</b> and select <b>Text Property</b>.
    <li>Enter "APP_URL" as the 'Name'.  Do not enter anything for the 'Value'.
    <li>Click the <b>Jobs</b> tab.
    <li>Click <b>ADD JOB</b>.
    <li>Click the <b>+</b> and select <b>Test</b> for the JOB TYPE.
    <li>On the Job configuration panel, note that:
    <ul compact>
    <li>Job name <b>Functional Tests</b>.
    <li>'Tester Type' is <b>Simple</b>.
    Enter the following code to the <b>Test Command</b>.
<pre><code>
#!/bin/bash
export CATALOG_API_TEST_SERVER=$APP_URL
GRUNTFILE="tests/Gruntfile.js"
if [ -f $GRUNTFILE ]; then
  npm install -g npm@3.7.2 ### work around default npm 2.1.1 instability
  npm install
  grunt dev-fvt --no-color -f --gruntfile $GRUNTFILE --base .
 else
   echo "$GRUNTFILE not found."
fi    
</code></pre>
This bash shell will execute Grunt JavaScript tasks to run functional test scripts on the catalog service prior to deploying the service to the Test stage (and qa space).
    </ul>
    <li>Click <b>Save</b> to save the <b>Dev</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b> and <b>Dev</b> stages.  The <b>Dev</b> stage has not been run. Click on the <b>Run Stage</b> icon to run the <b>Dev</b> stage to deploy Catalog application and run the functional tests.
    <p>The JOBS section shows the Stage was successful. Click on "View logs and history" to the Job log.
    <li>LAST EXECUTION RESULT displays the url to the successfully deployed application (<i>user_name</i>-dev-catalog-api-toolchain-lab.mybluemix.net) as well as a link to the runtime log.
    <br>Click on "<i>user_name</i>-dev-catalog-api-toolchain-lab.mybluemix.net" to access the running application.
    <p>The <b>Dev</b> stage has been successfully added and executed.
</ol>

<li>Add the <b>Test</b> stage (remember, two jobs, one to deploy to the <i>qa</i> space and another to perform an automated test).  We will clone the <b>Dev</b> stage and make some modifications.
<ol>
    <li>Ensure the catalog-api-toolchain-lab <b>Delivery Pipeline</b> is displayed.
    <li>On the <b>Dev</b> stage, click the <b>Stage Configuration</b> and select "Clone Stage".
    <li>Rename the cloned stage to <b>Test</b> (from <b>Dev [copy]</b>).
    <li>On the <b>Jobs</b> tab, for the <b>Deploy</b> job, change the space to <b>qa</b> (from <b>dev</b>) (or Create a new space called <b>qa</b> if not on the dropdown) and change the deploy script to change CF_APP_NAME to "$user_name-test-$CF_APP" from "$user_name-dev-$CF_APP".
    <li>On the <b>Jobs</b> tab, enter the following code to the <b>Test Command</b>.
<pre><code>
#!/bin/bash
# invoke tests here
echo "Testing of App Name ${CF_APP_NAME} was successful"      
</code></pre>
This 'test' script just echos the app name to the console log.  In a real environment, we would execute automated test tools and scripts to validate the deployed service still worked.
<br>
    <li>Click <b>Save</b> to save the <b>Test</b> stage.
    <li>The <b>Delivery Pipeline</b> displays the <b>Build</b>, <b>Dev</b> and <b>Test</b> stages.  The <b>Test</b> stage has not been run.
    Click on the <b>Run Stage</b> icon to run the <b>Test</b> stage and deploy the order API to the <i>test</i> space.
    <li>As before for the <b>Dev</b> stage, the JOBS section shows the Deploy and Test Jobs were successful. Click <b>Test</b> to display the log for the <b>Test</b> job. Notice the "Testing of App Name" message was echoed.    
    <li>Click on "test-catalog-toolchain-lab.mybluemix.net" to access the running application.
    </ol><p>The <b>Test</b> stage has been successfully added and executed.
<li>Add the <b>Prod</b> stage (remember, one job, to deploy to the <i>prod</i> space).  This stage will also check to see there is an earlier instance of this application running and if it is, keep it around in case the deploy of the new version of the app has problems.  If the new version deploys successfully, the old version is deleted.  If not, the new version is deleted and the old version continues to run.
<br>We will clone the <b>Dev</b> stage and make some modifications.
<ol>
<li>Ensure the catalog-api-toolchain-lab <b>Delivery Pipeline</b> is displayed.
    <li>On the <b>Dev</b> stage, click the <b>Stage Configuration</b> and select "Clone Stage".
    <li>Rename the cloned stage to <b>Prod</b> (from <b>Dev [copy]</b>).
    <li>On the <b>Jobs</b> tab, change the Job name to 'Blue/Green Deploy', change the space from <b>dev</b> to <b>prod</b> (or Create a new space called <b>prod</b> if not on the dropdown) and change the deploy script to the following:
<pre><code>
#!/bin/bash
#get user name
a=$(cf services | grep @)
b=${a%@&#42;}
c=($b)
len=${#c[@]}
user_name=${c[len-1]}
export CF_APP_NAME="$user_name-prod-$CF_APP"
#add Cloudant service
cf create-service cloudantNoSQLDB Lite myMicroservicesCloudant
if ! cf app $CF_APP_NAME; then  
  cf push $CF_APP_NAME
else
  OLD_CF_APP=${CF_APP_NAME}-OLD-$(date +"%s")
  rollback() {
    set +e  
    if cf app $OLD_CF_APP; then
      cf logs $CF_APP_NAME --recent
      cf delete $CF_APP_NAME -f
      cf rename $OLD_CF_APP $CF_APP_NAME
    fi
    exit 1
  }
  set -e
  trap rollback ERR
  cf rename $CF_APP_NAME $OLD_CF_APP
  cf push $CF_APP_NAME
  cf delete $OLD_CF_APP -f
fi
</code></pre>
    <li>Click <b>Save</b> to save the <b>Prod</b> stage.
    <li>Click on <b>Run Stage</b> to run the <b>Prod</b> stage and deploy the order API to the <i>prod</i> space.
    <li>The JOBS section shows the Deploy was successful. Inspect the Job log.
    <br>Click on the blue arrow to display the Delivery Pipeline. Click on "<i>user_name</i>-catalog-api-toolchain-lab.mybluemix.net" to access the running application.
    <p>The <b>Prod</b> stage has been successfully added and executed.  The Catalog application has been deployed to production.
</ol>
