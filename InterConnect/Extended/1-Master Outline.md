# Bluemix DevOps Toolchain Lab

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
- Slack, which you can use to get real-time notifications about the status of builds and deployments.

## Prerequisites
Prior to running these labs, you must have a Bluemix account, a GitHub account and access to a lab laptop.  Follow the steps in Lab 0 to create one or both of those accounts.

## Labs
- [Lab 0: Getting setup](lab-0-getting-setup.md)
- [Lab 1: Read Template Paper](lab-1-read-template-paper.md)
- [Lab 2: Create Order Toolchain Using Pre-Built Template](lab-2-create-order-toolchain-using-pre-built-template.md)
- [Lab 3: Create Catalog Toolchain by Hand](lab-3-create-catalog-toolchain-by-hand.md)
- [Lab 4: Create UI Toolchain from deployed application](lab-4-create-ui-toolchain-from-deployed-application.md)

- [Lab 4: Build Template and Create UI Toolchain](Lab-3-UI-Pipeline.md)
- [Lab 5: Add Slack Integration](Lab-5-Slack.md)
- [Lab 6: Add Availability and Monitoring (and Notification?)]
- [Lab 7: Add Bluemix Availability Monitoring]
- [Add Container for Catalog]
- [Add Auto-Scaling]
- [Add Active Deploy / Blue/Green]

- [Lab 8: Answer the guided questions for the BlueCompute tool chain example](Lab-8-Answer-guided-questions.md)

[//]: # (- [Lab 6: Deliver a UI Change](#lab-7-Deliver-a-UI-Change) )
