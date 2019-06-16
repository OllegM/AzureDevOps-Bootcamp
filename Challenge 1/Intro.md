# Sentiment on social media (Twitter) goes negative due to issue with site

## Description

A team member has deployed a tiny change to the website. However, the change hides the [Checkout] button. Everything is fully functional, but the purchases in web store are decreasing in rapid rate. There are no errors, last deployment was green and telemetry is indicating the usual load. What's going on? The system needs to recover quickly to stop the bleeding of purchases and avoid the same situation in the future.

## Learning objectives

### In this challenge you will learn all about

* Using social media (such as twitter) as a valuable feedback tool when doing website / service updates
* Rolling back change by re-deploying previous working version
* Provision necessary Azure resources for sentiment analysis - azure function, cognitive services, application insights
* Configuring and deploying azure function code
* Add Twitter Sentiment Analysis on your teams dashboard to be proactive
* Add Twitter Sentiment Analysis to your release pipeline to be proactive
* In case of problems, how to provision your own twitter application for integration