# Overview

Since there are a lot of transformations happening from the github api to our internal `domain` model, here is a quick overview of it. The intermediary layer in the `github-adapter` serves to map the response json directly to java classes which then know how to transform themselves into actual `domain` classes. Aside from the whole classes, we are also mapping things like possible statuses. We do this because it reduces the cardinality of the resulting prometheus metric. Look below for more details.

<p align="center">
   <img src="https://github.com/github-insights/github-metrics/assets/57358338/3b1d7ea5-553c-4b4a-8ef2-41c7b624865d" />
</p>


# Workflow Run

## Status

### Grouping
```java
"completed", "success"                                                                -> WorkflowRunStatus.DONE;
"action_required", "cancelled", "failure", "neutral", "skipped", "stale", "timed_out" -> WorkflowRunStatus.FAILED;
"in_progress"                                                                         -> WorkflowRunStatus.IN_PROGRESS;
"queued", "requested", "waiting", "pending"                                           -> WorkflowRunStatus.PENDING;
```

## Build Times

# Jobs

## Status

### Grouping
```java
"requested", "queued", "pending" -> JobStatus.PENDING;
"in_progress"                    -> JobStatus.IN_PROGRESS;
"completed"                      -> JobStatus.DONE;
```

## Conclusion

### Grouping
```java
"success"                                              -> Job.JobConclusion.SUCCESS;
"neutral", "skipped", "null"                           -> Job.JobConclusion.NEUTRAL;
"action_required"                                      -> Job.JobConclusion.ACTION_REQUIRED;
"failure", "cancelled", "timed_out", "action_required" -> Job.JobConclusion.FAILURE;
```

# Pull Request
# Repository
# Self Hosted Runner
