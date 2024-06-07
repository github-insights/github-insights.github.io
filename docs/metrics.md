# Metrics

All metrics also have a `organization` tag on them with the set organization as
the value. Although not that important it is nice in the case that the service
is being used on two organizations at the same time but pushing the data to a 
single frontend.

<table>
    <thead>
        <tr style="background-color: lightgray;">
            <th></th>
            <th>metric name</th>
            <th>labels</th>
            <th>description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Repositories</td>
            <td><nobr><code>repositories_count</code></nobr></td>
            <td>-</td>
            <td>
                All Repositories that the Github Installation token has access
                to and exposes a total current count metric.
            </td>
        </tr>
        <tr>
            <td rowspan=4>Workflow Runs</td>
            <td><nobr><code>workflow_runs</code></nobr></td>
            <td rowspan=4><code>status</code> {DONE, IN_PROGRESS, PENDING, FAILED}</td>
            <td>
                For all Repositories retrieves all workflow runs from the start
                of yesterday until time of export and display a count of them
                for every status.
            </td>
        </tr>
        <tr>
            <td><nobr><code>active_workflow_runs</code></nobr></td>
            <td>
                For all Repositories all active workflow runs and display a 
                count of them for every status.
            </td>
        </tr>
        <tr>
            <td><nobr><code>workflow_runs_total_build_times</code></nobr></td>
            <td>
                For all Repositories the total build time of the workflow runs
                from the start of yesterday until the time of export.
            </td>
        </tr>
        <tr>
            <td><nobr><code>workflow_runs_average_build_times</code></nobr></td>
            <td>
                For all Repositories the average build time of the workflow runs
                from the start of yesterday until the time of export
            </td>
        </tr>
        <tr>
            <td>Jobs</td>
            <td><nobr><code>workflow_run_jobs</code></nobr></td>
            <td>
                <code>status</code> {DONE, PENDING, IN_PROGRESS}<br><br>
                <code>conclusion</code> {SUCCESS, FAILURE, ACTION_REQUIRED, NEUTRAL}
            </td>
            <td>
                For all Workflow runs of the last day retrieve all jobs and a
                count of every status/conclusion.
            </td>
        </tr>
        <tr>
            <td rowspan=2>Pull Requests</td>
            <td>
                <nobr><code>pull_requests_count_of_last_{X}_days</code></nobr>
                {1,2,7,28,365}
            </td>
            <td rowspan=2><code>state</code> {OPEN, CLOSED, MERGED}</td>
            <td>
                For pull requests of the last days a count of each state
            </td>
        </tr>
        <tr>
            <td>
                <nobr><code>pull_request_throughput_of_last_{X}_days</code></nobr>
                {7,14,28,365}
            </td>
            <td>
                Merged Pull Requests per day over the period X
            </td>
        </tr>
        <tr>
            <td>Self Hosted Runners</td>
            <td><nobr><code>self_hosted_runners</code></nobr></td>
            <td><code>os</code> {MAC_OS, LINUX, WINDOWS}<br><br><code>status</code> {OFFLINE, IDLE, BUSY}</td>
            <td>
                For all selfhosted runners of the org/repos a count of their
                Operating System and Status.
            </td>
        </tr>
    </tbody>
</table>

