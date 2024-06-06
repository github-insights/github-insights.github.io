# Configuration

## Authentication

FIXME: add things

## Exporters

These configs define on which schedule the exporters will run. Although configurable
this is not so influential on the programs performacne or lazency and should be
kept on quite a quick schedule. Most of the schedule executions will only hit the
caches. To change how often the Github Api is queried use the enviroment variables
listed in the [cache eviction](#cache-eviction) section.

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
            <td colspan=3><code>EXPORTERS_SCHEDULING_WORKFLOW_RUNS</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>*/30 * * * * ?</code></td>
            <td>export schedule for workflow runs</td>
        </tr>
    
        <tr>
            <td rowspan=2>Active Workflow Runs</td>
            <td colspan=3><code>EXPORTERS_SCHEDULING_ACTIVE_WORKFLOW_RUNS</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>*/30 * * * * ?</code></td>
            <td>export schedule for active workflow runs</td>
        </tr>
    
        <tr>
            <td rowspan=2>Jobs</td>
            <td colspan=3><code>EXPORTERS_SCHEDULING_JOBS</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>*/30 * * * * ?</code></td>
            <td>export schedule for workflow jobs</td>
        </tr>
    
        <tr>
            <td rowspan=2>Workflow Run Build Times</td>
            <td colspan=3><code>EXPORTERS_SCHEDULING_WORKFLOW_RUN_BUILD_TIMES</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>45 */30 * * * ?</code></td>
            <td>export schedule for workflow build times</td>
        </tr>
    
        <tr>
            <td rowspan=2>Pull Requests</td>
            <td colspan=3><code>EXPORTERS_SCHEDULING_PULL_REQUESTS</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>30 */30 * * * ?</code></td>
            <td>export schedule for pull requests</td>
        </tr>
    
        <tr>
            <td rowspan=2>Self Hosted Runners</td>
            <td colspan=3><code>EXPORTERS_SCHEDULING_SELF_HOSTED_RUNNERS</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>*/30 * * * * ?</code></td>
            <td>export schedule for self hosted runners</td>
        </tr>
    
        <tr>
            <td rowspan=2>Repository Count</td>
            <td colspan=3><code>EXPORTERS_SCHEDULING_REPOSITORY_COUNT</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>*/30 * * * * ?</code></td>
            <td>export schedule for repository count</td>
        </tr>
    
        <tr>
            <td rowspan=2>Rate Limit State</td>
            <td colspan=3><code>EXPORTERS_SCHEDULING_API_RATELIMIT_STATE</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>*/15 * * * * ?</code></td>
            <td>export schedule for the ratelimiters state</td>
        </tr>
    </tbody>
</table>

## Cache eviction

As mentioned above, this is the imporant part of the configuration. If you are
intending to make the request interval significantly faster or slower change the
cron shedule to the corresponding shedule leading to the cache beeing evicted more
or less often and new data being fetched more or less frequently.

That said the rate limiter might stop you from making too many requests by setting
its status to a specific level. But even here you get to control how this level
influences the features. Each feture can set a level at which it will be turned
off. The logic works in the following way (as pseudocode):

```
shouldICacheEvict = if apiRateLimitStatus < configuredValue { yes } else { no }
```

There is a possibility to also set no value (`""`) which will lead to the feature
always beeing active unless the actual ratelimit is hit in which case all action
is ceased. The configured `enum string` can be one of:

```java
"CRITICAL", "WARNING", "CONCERNING", "GOOD", "OK", ""
```

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
            <td rowspan=4>Workflow Runs</td>
            <td colspan=3><code>CACHE_EVICTION_SCHEDULING_WORKFLOW_RUNS</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>30 */5 * * * ?</code></td>
            <td>schedule on which to evict workflow runs</td>
        </tr>
    
        <tr>
            <td colspan=3><code>CACHE_EVICTION_WORKFLOW_RUNS_STOP_EVICTION_STATUS</code></td>
        </tr>
        <tr>
            <td>enum string</td>
            <td><code>CONCERNING</code></td>
            <td>status on which to stop evicting workflow runs</td>
        </tr>
    
        <tr>
            <td rowspan=4>Active Workflow Runs</td>
            <td colspan=3><code>CACHE_EVICTION_SCHEDULING_ACTIVE_WORKFLOW_RUNS</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>30 * * * * ?</code></td>
            <td>schedule on which to evict active workflow runs</td>
        </tr>
    
        <tr>
            <td colspan=3><code>CACHE_EVICTION_ACTIVE_WORKFLOW_RUNS_STOP_EVICTION_STATUS</code></td>
        </tr>
        <tr>
            <td>enum string</td>
            <td></td>
            <td>status on which to stop evicting active workflow runs</td>
        </tr>
    
        <tr>
            <td rowspan=4>Jobs</td>
            <td colspan=3><code>CACHE_EVICTION_SCHEDULING_JOBS</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>0 */5 * * * ?</code></td>
            <td>schedule on which to evict jobs</td>
        </tr>
    
        <tr>
            <td colspan=3><code>CACHE_EVICTION_JOBS_STOP_EVICTION_STATUS</code></td>
        </tr>
        <tr>
            <td>enum string</td>
            <td><code>CONCERNING</code></td>
            <td>status on which to stop evicting jobs</td>
        </tr>
    
        <tr>
            <td rowspan=4>Workflow Run Build Times</td>
            <td colspan=3><code>CACHE_EVICTION_SCHEDULING_WORKFLOW_RUN_BUILD_TIMES</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>10 0 * * * ?</code></td>
            <td>schedule on which to evict workflow build times</td>
        </tr>
    
        <tr>
            <td colspan=3><code>CACHE_EVICTION_WORKFLOW_RUN_BUILD_TIMES_STOP_EVICTION_STATUS</code></td>
        </tr>
        <tr>
            <td>enum string</td>
            <td><code>CONCERNING</code></td>
            <td>status on which to stop evicting workflow build times</td>
        </tr>
    
        <tr>
            <td rowspan=4>Pull Requests</td>
            <td colspan=3><code>CACHE_EVICTION_SCHEDULING_PULL_REQUESTS</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>15 0 * * * ?</code></td>
            <td>schedule on which to evict pull requests</td>
        </tr>
    
        <tr>
            <td colspan=3><code>CACHE_EVICTION_PULL_REQUESTS_STOP_EVICTION_STATUS</code></td>
        </tr>
        <tr>
            <td>enum string</td>
            <td><code>GOOD</code></td>
            <td>status on which to stop evicting pull requests</td>
        </tr>
    
        <tr>
            <td rowspan=4>Self Hosted Runners</td>
            <td colspan=3><code>CACHE_EVICTION_SCHEDULING_SELF_HOSTED_RUNNERS</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>20 */5 * * * ?</code></td>
            <td>schedule on which to evict self hosted runners</td>
        </tr>
    
        <tr>
            <td colspan=3><code>CACHE_EVICTION_SELF_HOSTED_RUNNERS_STOP_EVICTION_STATUS</code></td>
        </tr>
        <tr>
            <td>enum string</td>
            <td><code>CONCERNING</code></td>
            <td>status on which to stop evicting self hosted runners</td>
        </tr>
    
        <tr>
            <td rowspan=4>Repository Count</td>
            <td colspan=3><code>CACHE_EVICTION_SCHEDULING_REPOSITORY_COUNT</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>0 0 * * * ?</code></td>
            <td>schedule on which to evict repository count</td>
        </tr>
    
        <tr>
            <td colspan=3><code>CACHE_EVICTION_REPOSITORY_COUNT_STOP_EVICTION_STATUS</code></td>
        </tr>
        <tr>
            <td>enum string</td>
            <td><code>GOOD</code></td>
            <td>status on which to stop evicting repository count</td>
        </tr>
    </tbody>
</table>


## Rate Limiter

First we have some basic variables that allow for some minor configurations. If
you are not intending to get really into opimizing the service there should be
no need to touch these.

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
            <td rowspan=2>State Control Schedule</td>
            <td colspan=3><code>APP_GITHUB_RATELIMITING_STATE_CONTROL_CHECK_SCHEDULE</code></td>
        </tr>
        <tr>
            <td>java cron string</td>
            <td><code>0 */5 * * * ?</code></td>
            <td>how often the rate limiter will check if it can start making requests again if it happens to have gotten itself stuck </td>
        </tr>
        <tr>
            <td rowspan=2>State Recalculation Timing</td>
            <td colspan=3><code>APP_GITHUB_RATELIMITING_SECONDS_BETWEEN_STATE_RECALCULATIONS</code></td>
        </tr>
        <tr>
            <td>integer (seconds)</td>
            <td><code>60</code></td>
            <td>Time that passes between each reevaluation of how many requests are being made, shorter time will make the rate limiter more sensitive to bursts and longer the inverse</td>
        </tr> 
        <tr>
            <td rowspan=2>Ratelimit Buffer</td>
            <td colspan=3><code>APP_GHITHUB_RATELIMIT_BUFFER</code></td>
        </tr>
        <tr>
            <td>float</td>
            <td><code>0.9</code></td>
            <td>Percentage of the total limit that will be used. Allows for a bit of a buffer in case other applications are also using the same rate limit.</td>
        </tr> 
    </tbody>
</table>

### Status Limits

If you want to change at which point the rate limiter changes to which status 
change these values. The rate limiter waits a number of seconds specified by
`APP_GITHUB_RATELIMITING_SECONDS_BETWEEN_STATE_RECALCULATIONS` after which it
calculates how many requests per second (req/s) have been made in that time. This
number is then compared to a ideal req/s, calculate through rate limiting headers
which then in turn determines the status.

```
if (ideal * GOOD_LIMIT >= actual) {
    "OK"
} else if ...
...
} else {
    "CRITICAL"
}
```

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
            <td rowspan=2>GOOD</td>
            <td colspan=3><code>APP_GITHUB_RATELIMITING_GOOD_LIMIT</code></td>
        </tr>
        <tr>
            <td>float</td>
            <td><code>0.5</code></td>
            <td>Percentage of the ideal req/s that need to be used for the status to minimally turn to "GOOD"</td>
        </tr>
        <tr>
            <td rowspan=2>CONCERNING</td>
            <td colspan=3><code>APP_GITHUB_RATELIMITING_CONCERNING_LIMIT</code></td>
        </tr>
        <tr>
            <td>float</td>
            <td><code>0.7</code></td>
            <td>Percentage of the ideal req/s that need to be used for the status to minimally turn to "CONCERNING"</td>
        </tr> 
        <tr>
            <td rowspan=2>WARNING</td>
            <td colspan=3><code>APP_GITHUB_RATELIMITING_WARNING_LIMIT</code></td>
        </tr>
        <tr>
            <td>float</td>
            <td><code>0.9</code></td>
            <td>Percentage of the ideal req/s that need to be used for the status to minimally turn to "WARNING"</td>
        </tr> 
        <tr>
            <td rowspan=2>CRITICAL</td>
            <td colspan=3><code>APP_GITHUB_RATELIMITING_CRITICAL_LIMIT</code></td>
        </tr>
        <tr>
            <td>float</td>
            <td><code>1.2</code></td>
            <td>Percentage of the ideal req/s that need to be used for the status to minimally turn to "CONCERNING"</td>
        </tr> 
    </tbody>
</table>


## Feature Toggles

With these toggles you can decide what metrics will be exported by Github Metrics.
Although turning off features will reduce the number of requests made the relationship
is not linear. The feature toggles will not directly stop certain endpoints from
being requested they will just stop the exporter from asking for that data. As
an example, Repositories will always be requested if any other feature is active
since for exaples to fetch WorkflowRuns we need to know what repositories are 
available.

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
