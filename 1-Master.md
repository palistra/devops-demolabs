#Bluemix DevOps Toolchain Lab

## Objective
This series of labs shows how to set up a productive toolchain with a sample that consists of three microservices. After you finish this part of the series, you will be familiar with a toolchain that demonstrates practices from the IBM® Bluemix® Garage Method. Toolchains are available in the US South region only.

To create this toolchain, we will use a application that consists of three microservices:

- Catalog API: A back-end RESTful API that tracks all of the items in the store. This Node.js app is built with Express and uses an IBM Cloudant® database to persist the catalog of items.
- Orders API: A back-end RESTful API that tracks all store orders. This Node.js app uses the IBM SQL Database for Bluemix to store the orders in a SQL database.
- UI: A simple UI that displays all of the items in the store catalog, and that can create orders. This PHP UI calls both of the REST APIs.

Each microservice is deployed to three environments: development, test, and production. You end up with nine deployed apps.

You will be creating a toolchain and adding the following integrations:
- Three GitHub repositories (repos), with GitHub Issues enabled. Each microservice has one GitHub repo.
- Three delivery pipelines, one for each GitHub repo. The pipelines are independent and can run in parallel.
- The Eclipse Orion Web IDE, which you can use to edit your code and deploy it with the pipeline from a web browser.
- PagerDuty, which notifies your operations team about critical incidents.
- Slack, which you can use to get real-time notifications about the status of builds and deployments.

## Prerequisites
Prior to running these labs, you must have a Bluemix account, a GitHub account and access to a lab laptop.  Follow the steps in Lab 0 to create one or both of those accounts.

## Labs
- [Lab 0: Create Bluemix and GitHub accounts](Lab-0-Pre-reqs.md)
- [Lab 1: Set-up Toolchain for Order](Lab-1-Order-Pipeline.md)
- [Lab 2: Set-up Toolchain for Catalog](Lab-2-Catalog-Pipeline.md)
- [Lab 3: Set-up Toolchain for UI](Lab-3-UI-Pipeline.md)
- [Lab 4: Add PagerDuty Integration](Lab-4-PagerDuty.md)
- [Lab 5: Add Slack Integration](Lab-5-Slack.md)
- [Lab 6: Read Template Paper](Lab-6-Read-Template-Paper.md)
- [Lab 7: Create a tool chain from an example template](Lab-7-Create-Toolchain-From-Template.md)
- [Lab 8: Answer the guided questions for the BlueCompute tool chain example](Lab-8-Answer-guided-questions.md)

[//]: # (- [Lab 6: Deliver a UI Change](#lab-7-Deliver-a-UI-Change) )
