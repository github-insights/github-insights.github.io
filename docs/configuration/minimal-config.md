# Minimal Config

To get started with the service there is a minimal amount of configuration needed
to get the service running. This configuration contains the following:

| environement variable name        | datatype | description |
|-----------------------------------|-|-------------|
| APP_GITHUB_ORG                    | string | the name of the organization |
| APP_GITHUB_APPLICATION_ID         | number | The id of the Github App installed for authorizing the Api calls made by Github Metics (In the case of `AuthApp Github Metrics` is: 882159)|
| APP_GITHUB_APPLICATION_INSTALL_ID | number | The id of the installation of the above mentioned Github App |
| APP_GITHUB_APPLICATION_PEM        | string with newlines | A private key generated through the [installation of the Github App](../getting-started.md#authentication). This environement variable is just the whole `pem` string provided by Github |


By default only the `Repository Count` and `Workflow Runs` features are active.
If you would like to collect any other metrics turn on the relevant features.

<table>
    <thead>
        <tr style="background-color: lightgray;">
            <th></th>
            <th>datatype</th>
            <th>default</th>
            <th>description</th>
        </tr>
    </thead>
    <tbody>    
        <tr>
            <td rowspan=2>Workflow Runs</td>
            <td colspan=3><code>EXPORTER_WORKFLOW_RUNS_FEATURE</code></td>
        </tr>
        <tr>
            <td>boolean</td>
            <td><code>true</code></td>
            <td>Collects data on all workflow runs created since yesterday 00:00, this includes workflow-runs of all statuses.</td>
        </tr>
        <tr>
            <td rowspan=2>Active Workflow Runs</td>
            <td colspan=3><code>EXPORTER_ACTIVE_WORKFLOW_RUNS_FEATURE</code></td>
        </tr>
        <tr>
            <td>boolean</td>
            <td><code>false</code></td>
            <td>Collects data only the the currently active workflow-runs, meaning any workflow-run that has not failed or completed.</td>
        </tr>
        <tr>
            <td rowspan=2>Jobs</td>
            <td colspan=3><code>EXPORTER_JOBS_FEATURE</code></td>
        </tr>
        <tr>
            <td>boolean</td>
            <td><code>false</code></td>
            <td>Collects data on all jobs of all the workflow-runs created since yesterday 00:00.</td>
        </tr>
        <tr>
            <td rowspan=2>Repository Count</td>
            <td colspan=3><code>EXPORTER_REPOSITORIES_FEATURE</code></td>
        </tr>
        <tr>
            <td>boolean</td>
            <td><code>true</code></td>
            <td>Collects data on the number of repositories in the organization.</td>
        </tr>
        <tr>
            <td rowspan=2>Workflow Run Build Times</td>
            <td colspan=3><code>EXPORTER_WORKFLOW_RUN_BUILD_TIMES_FEATURE</code></td>
        </tr>
        <tr>
            <td>boolean</td>
            <td><code>false</code></td>
            <td>Aggregates workflow-run build-times to both a total count as well as a average for all workflow-runs created since yesterday 00:00.</td>
        </tr>
        <tr>
            <td rowspan=2>Self Hosted Runners</td>
            <td colspan=3><code>EXPORTER_SELF_HOSTED_RUNNERS_FEATURE</code></td>
        </tr>
        <tr>
            <td>boolean</td>
            <td><code>false</code></td>
            <td>Collects data on all self-hosted runners present on the organization as well as any repository that the token has access to.</td>
        </tr>
        <tr>
            <td rowspan=2>Pull Requests</td>
            <td colspan=3><code>EXPORTER_PULL_REQUESTS_FEATURE</code></td>
        </tr>
        <tr>
            <td>boolean</td>
            <td><code>false</code></td>
            <td>Aggregates status counts of pull requests over the last few days.</td>
        </tr>
    </tbody>
</table>
