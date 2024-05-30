# Metrics
|                  | Metric             | Labels | Description                                                                                                       |
|------------------|--------------------|--------|-------------------------------------------------------------------------------------------------------------------|
| **Repositories** | repositories_count | -    |  All Repositories that the Github Installation token has access to and exposes a total current count metric. |
|                  |                    |        |                                                                                                               |
| **Workflow Runs** | workflow_runs     | status {DONE, IN_PROGRESS, PENDING, FAILED} | For all Repositories retrieves all workflow runs from the start of yesterday until time of export  and display a count of them for every status.|
| | active_workflow_runs  | status {DONE, IN_PROGRESS, PENDING, FAILED} | For all Repositories all active workflow runs and display a count of them for every status. |
|| workflow_runs_total_build_times | status {DONE, IN_PROGRESS, PENDING, FAILED} | For all Repositories the total build time of the workflow runs from the start of yesterday until the time of export. |
|| workflow_runs_average_build_times | status {DONE, IN_PROGRESS, PENDING, FAILED} | For all Repositories the average build time of the workflow runs from the start of yesterday until the time of export   |
||||
| **Jobs** | workflow_run_jobs | status {DONE, PENDING, IN_PROGRESS}  conclusion {SUCCESS, FAILURE, ACTION_REQUIRED, NEUTRAL} | For all Workflow runs of the last day retrieve all jobs and a count of every status/conclusion. |
||||
| **Pull Requests** | pull_requests_count_of_last_{X}_days X{1,2,7,28,365} | state {OPEN, CLOSED, MERGED} | For pull requests of the last <X> days a count of each state | 
| | pull_request_throughput_of_last_{X}_days X{7,14,28,365} | | Merged Pull Requests per day over the period X |
||||
| **Self Hosted Runners** | self_hosted_runners | os {MAC_OS, LINUX, WINDOWS} status {OFFLINE, IDLE, BUSY}|  for all selfhosted runners of the org/repos a count of their Operating System and Status |

