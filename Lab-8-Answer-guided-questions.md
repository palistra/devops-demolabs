# Lab 8: Answer the guided questions

## Objective
This lab show how to integrate your toolchain with Slack.  Slack provides real-time messaging for team communications. You can integrate Slack with your Bluemix DevOps Services project so that notifications about build results from your Build & Deploy pipeline are posted on a Slack channel.

**Tasks**:
- [Answer the following guided questions for the BlueCompute tool chain example](Bluecompute-guided.md)

### Guided Questions for walkthrough of BlueCompute DevOps Toolchain

1. Locate the template toolchain.yml file in the .bluemix folder of the GitHub repository. Locate the bff-inventory GitHub integration service in the tool chain located in the organization defined for this training.  Identify where in the bff-inventory yaml and the tool chain service where use of Git Issues is configured.

2. When creating a new tool chain from a template and linking GitHub repository, what choices do you have for the linking process?

3. The entire toolchain template could be documented in a single yaml file (e.g. toolchain.yml). However, you can also separate each delivery pipeline configuration into different yaml files (e.g. xx.pipeline.yml). Blue compute example uses separate pipeline.yml for each microservice.
    - For the Netflix eureka microservice delivery pipeline, what is the name of the file that contains the pipeline configuration?
    - For the same microservice, how is the pipeline.yml referenced in the main toolchain.yml?

4. What build automation solution is used to build the docker image for the micro-socialreview microservice? How is the defined in yaml and where do you see it in the delivery pipeline (hint: its noted in the first part of the build script)

5. Why are there two jobs in the micro-socialreview deploy stage?

6. What is the Builder Type for the bff-inventory microservice? Why is this type used? Where is it annotated in the YAML?

7. ".json" files can be used to reconfigure a UI in the tool chain. For example, you can use json to define environment variables for a deploy or build job. The variables are then referenced in the pipeline or toolchain YAML files. Open the toolchain.yml located in the .bluemix folder and navigate to the  “pipeline-netflix-zuul” line. Locate the settings for configuration/parameters/env/DOMAIN. "{{deploy.parameters.route-domain}}". Explain what this variable represents. (Hint: navigate to the deploy.json file and locate the route-domain property.

8. Locate the result of the configuration in #7 in the tool chain. Where did you find it?

9. What setting would you make in order to run the build each time a change it detected in the associated source control repository?

10. What setting would you make to automatically run the deploy stage based on a the outcomes of a preceding stage?
