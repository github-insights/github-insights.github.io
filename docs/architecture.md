# General

We are using the Hexagonal arhitecture pattern but with a slight twist "screaming" which takes the concepts of hexagonal but flips the organization a little to make it more clear from the top level what each module is responsible for/does.

# Overview

Before diving into the details of the architecture here is a quick overview of the current architecture. This drawing omits some details of the actual architecture but is still correct for the most part. 

If not already clear from the drawing the general gist of it is the following. In the `domain` module a few different schedulers are defined. These schedulers call the the `github-adapter` through a caching layer and then export that data to `prometheus-exporter`. Both of these interactions between modules are managed through dependency injections allowing easy swapping of implementations. Additionally this is also true for the scheduling system.

<p align="center">
    <img src="https://github.com/github-insights/github-metrics/assets/57358338/627b6f87-393a-471c-99fc-aecf75a1101d">
    <em>Img 1</em>
</p>

# Scheduling System

As of this moment the scheduling is done through implementing [SchedulingConfigurer](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/SchedulingConfigurer.html). The schedule time is configured via environment variables.

<p align="center">
    <img src="https://github.com/github-insights/github-metrics/assets/57358338/60edeee2-9fee-46a3-b4c1-1bdc47e73b7a">
<em>Img 2</em>
</p>

# Caching

Caching is done very simply through the `@Cachable` annotation of spring. This annotation is inserted on the implementation of the different providers (marked green in the images) and then caches the return of the function call under the specified key. The caching was initially implemented to avoid fetching the list of repositories too often but then ended up coming in handy for other things as well. When fetching jobs for example the workflow runs also need to be fetched, but since the workflow runs might have just been fetched that call is cached and will not cause an actual re-fetch, but we use the annotation @CacheEvict on the actual workflowRunsUseCase so that that it always grabs a fresh and accurate request.

# Domain
# Prometheus Exporter
# Github Adapter



