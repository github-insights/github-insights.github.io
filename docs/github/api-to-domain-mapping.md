# Api to Domain Mapping

## Overview

Since there are a lot of transformations happening from the github api to our
internal `domain` model, here is a quick overview of it. The intermediary layer
in the `github-adapter` serves to map the response json directly to java classes
which then know how to transform themselves into actual `domain` classes. Aside
from the whole classes, we are also mapping things like possible statuses. We do
this because it reduces the cardinality of the resulting prometheus metric. Look
below for more details.

## Workflow Run

<p align="center">
    <img
        width="75%"
        src="../../images/mappings/workflow-runs-light.png#only-light"
    />
    <img 
        width="75%"
        src="../../images/mappings/workflow-runs-dark.png#only-dark"
    />
</p>


Both the conclusion as well as the status are grouped into the `WorkflowRunStatus`
enum. This leads to some dataloss which is acceptable for the better decoupleing.
First the `conclusion` is queried for what the final status should be but if it
does not give a clear response then the `status` determines the final `WorkflowRunStatus`.

### Conclusion

<table>
    <thead>
        <tr style="background-color: lightgray;">
            <th>github status strings</th>
            <th>WorkflowRunStatus enum</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"failure"</div>
                <div>"cancelled"</div>
                <div>"startup_failure"</div>
            </td>
            <td>FAILED</td>
        </tr>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"success"</div>
            </td>
            <td>DONE</td>
        </tr>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"neutral"</div>
                <div>"in_progress"</div>
                <div>null</div>
            </td>
            <td>passed on to status</td>
        </tr>
    </tbody>
</table>


### Status

<table>
    <thead>
        <tr style="background-color: lightgray;">
            <th>github status strings</th>
            <th>WorkflowRunStatus enum</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"completed"</div>
                <div>"success"</div>
            </td>
            <td>DONE</td>
        </tr>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"action_required"</div>
                <div>"cancelled"</div>
                <div>"failure"</div>
                <div>"neutral"</div>
                <div>"skipped"</div>
                <div>"stale"</div>
                <div>"timed_out"</div>
            </td>
            <td>FAILED</td>
        </tr>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"in_progress"</div>
            </td>
            <td>IN_PROGRESS</td>
        </tr>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"queued"</div>
                <div>"requested"</div>
                <div>"waiting"</div>
                <div>"pending"</div>
            </td>
            <td>PENDING</td>
        </tr>
    </tbody>
</table>

### Build Times

Although a separate query, there is no separate object on the domain. Insead
the adapter sets the build time parameter on the domain after having done the query
to the timing endpoint.

<p align="center">
    <img
        width="100%"
        src="../../images/mappings/workflow-run-build-times-light.png#only-light"
    />
    <img 
        width="100%"
        src="../../images/mappings/workflow-run-build-times-dark.png#only-dark"
    />
</p>

## Jobs

<p align="center">
    <img
        width="100%"
        src="../../images/mappings/jobs-light.png#only-light"
    />
    <img 
        width="100%"
        src="../../images/mappings/jobs-dark.png#only-dark"
    />
</p>

Unlike Workflow Runs, Jobs has the `conclusion` and `status` split up into two 
different enums. This should be changed in the future into one status to reduce
complexity and coupling.

### Conclusion

<table>
    <thead>
        <tr style="background-color: lightgray;">
            <th>github conclusion strings</th>
            <th>JobConclusion enum</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"success"</div>
            </td>
            <td>SUCCESS</td>
        </tr>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"neutral"</div>
                <div>"skipped"</div>
                <div>"null"</div>
            </td>
            <td>NEUTRAL</td>
        </tr>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"action_required"</div>
            </td>
            <td>ACTION_REQUIRED</td>
        </tr>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"failure"</div>
                <div>"cancelled"</div>
                <div>"timed_out"</div>
                <div>"action_required"</div>
            </td>
            <td>FAILURE</td>
        </tr>
    </tbody>
</table>

### Status

<table>
    <thead>
        <tr style="background-color: lightgray;">
            <th>github status strings</th>
            <th>JobStatus enum</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"requested"</div>
                <div>"queued"</div>
                <div>"pending"</div>
            </td>
            <td>PENDING</td>
        </tr>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"in_progress"</div>
            </td>
            <td>IN_PROGRESS</td>
        </tr>
        <tr>
            <td style="display: flex; gap: 10px; flex-wrap: wrap;">
                <div>"completed"</div>
            </td>
            <td>DONE</td>
        </tr>
    </tbody>
</table>

## Pull Request

The Pull Requests endpoint is currently the only endpoint that returns an array
on the root level which makes it a little different from the other response
objects.

<p align="center">
    <img
        width="75%"
        src="../../images/mappings/pull-requests-light.png#only-light"
    />
    <img 
        width="75%"
        src="../../images/mappings/pull-requests-dark.png#only-dark"
    />
</p>


## Repository

<p align="center">
    <img
        width="75%"
        src="../../images/mappings/repository-light.png#only-light"
    />
    <img 
        width="75%"
        src="../../images/mappings/repository-dark.png#only-dark"
    />
</p>

## Self Hosted Runner

As displayed by the grafic below, self hosted runners can be present on both the
organization as well as each repository. The response json is still the same in
both cases simplifying the mapping quite a lot.

<p align="center">
    <img
        width="100%"
        src="../../images/mappings/self-hosted-runners-light.png#only-light"
    />
    <img 
        width="100%"
        src="../../images/mappings/self-hosted-runners-dark.png#only-dark"
    />
</p>

