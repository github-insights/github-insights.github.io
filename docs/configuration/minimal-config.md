# Minimal Config

To get started with the service there is a minimal amount of configuration needed
to get the service running. This configuration contains the following:

| environement variable name        | datatype | description |
|-----------------------------------|-|-------------|
| APP_GITHUB_ORG                    | string | the name of the organization |
| APP_GITHUB_APPLICATION_ID         | number | The id of the Github App installed for authorizing the Api calls made by Github Metics (In the case of `AuthApp Github Metrics` is: 882159)|
| APP_GITHUB_APPLICATION_INSTALL_ID | number | The id of the installation of the above mentioned Github App |
| APP_GITHUB_APPLICATION_PEM        | string with newlines | A private key generated through the installation of the Github App. This environement variable is just the whole `pem` string provided by Github |

But to actually get any metrics to be fetched they have to be turned on. For this
turn on at least one of these otherwise no metrics will be collected.

| environment variable name                 | datatype|description                                                                                                                         |
|-------------------------------------------|-|------------------------------------------------------------------------------------------------------------------------------------|
| EXPORTER_ACTIVE_WORKFLOW_RUNS_FEATURE     | boolean |Collects data only the the currently active workflow-runs, meaning any workflow-run that has not failed or completed.               |
| EXPORTER_WORKFLOW_RUNS_FEATURE            | boolean |Collects data on all workflow runs created since yesterday 00:00, this includes workflow-runs of all statuses.                      |
| EXPORTER_JOBS_FEATURE                     | boolean |Collects data on all jobs of all the workflow-runs created since yesterday 00:00.                                                   |
| EXPORTER_REPOSITORIES_FEATURE             | boolean |Collects data on the number of repositories in the organization.                                                                    |
| EXPORTER_WORKFLOW_RUN_BUILD_TIMES_FEATURE | boolean |Aggregates workflow-run build-times to both a total count as well as a average for all workflow-runs created since yesterday 00:00. |
| EXPORTER_SELF_HOSTED_RUNNERS_FEATURE      | boolean |Collects data on all self-hosted runners present on the organization as well as any repository that the token has access to.        |
| EXPORTER_PULL_REQUESTS_FEATURE            | boolean |Aggregates status counts of pull requests over the last few days.                                                                   |
