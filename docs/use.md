# Use
There are a few ways to use this service. but for all of them you will need to authorize the service to your Github organization. to do so there is a [Github application](https://github.com/apps/authapp-github-metrics). Add this to your organization and select which repositories it has access to or simply all repositories in the organization. The service will make a request to see which repositories it has access to and then only the metrics relative to those repositories are exported.

![image](https://github.com/github-insights/github-metrics/assets/89661092/79e56315-b95b-41d3-a3c6-2a4ca410a224)

The required environment variables can be found at [.env-template](../blob/main/.env-template)
| ENV Variable | Value |
| ------------ | ----- |
| APP_GITHUB_ORG | the organization you want to export metrics for |
| APP_GITHUB_APPLICATION_ID | 882159|
| APP_GITHUB_APPLICATION_INSTALL_ID | the installation id of the app in your organization (can be found in the URL when you configure the app) ![image](https://github.com/github-insights/github-metrics/assets/89661092/adb27fd7-4e70-4008-a5de-650dddcc0f54) |
| APP_GITHUB_APPLICATION_PEM | the RSA private key supplied by the GH app |

this will then run out of the box with minimal/default features. to configure more features have a look further down the page.

The service itself only exposes the metrics to an end point /actuator/prometheus. How you scrape these is up to you. (see below for our deployment example using Kubernetes, Grafana alloy, and Grafana cloud) The intent is for this to simply mix into your existing metric logging solution.

# Configuration - Timothy

# Grafana - Heinrich
Grafana is a great tool for visualizing time series data so is a perfect solution to display the data generated by this service. We have created some great example dashboards that you can copy from as you wish.

TODO: link public dashboard???? or merely the JSON export

# Creating your own Image - Timothy

# Deployment Example - Timothy
