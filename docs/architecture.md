# Architecture

Before diving into the details of the architecture here is a quick overview of
the current architecture. This drawing omits some details of the actual 
architecture but is still correct for the most part. 

<p align="center">
    <img
        width="75%"
        src="../images/architecture-overview-light.png#only-light"
    />
    <img 
        width="75%"
        src="../images/architecture-overview-dark.png#only-dark"
    />
</p>

## Overview

The project is based on a Hexagonal architecture, this allows for the the **GitHub**
and **Prometheus** specific code to live in its own [adapters](#github-adapter) and 
[exporters](#prometheus-exporter) making the service easily extensible. Those two
adapter and exporter implementations are built around a `domain` module.

This also means that anything specific to the implementation of the adapter or
the exporter is only present in each respective module and does not need to be
considered from other modules. Examples would be that caching requests is done
in the `github-adapter` and not in the `domain`. 

<p align="center">
    <img
        width="100%"
        src="../images/github-metrics-light.png#only-light"
    />
    <img 
        width="100%"
        src="../images/github-metrics-dark.png#only-dark"
    />
</p>

## Domain

The domain exposes different use cases for getting the data from the adapter
implementation. This data, although [modeled after the Github apis](github/api-to-domain-mapping.md)
structure is not completly coupled to it and is already more generic then
Github's. Fetching the data happens through implementations of the respective
`...QueryPort` injected by spring.

Since much of the data depends on eachother, like Workflow Runs are part of a
Repository, the use cases call eachother to fetch the nessesary data.

## Prometheus Exporter

Through the [Micrometer dependency](https://micrometer.io/) the prometheus exporter
exposes its metrics on the `/actuator/prometheus` endpoint. Internally the exporter
calls the domain's use cases transforming the returned data into a format that 
is specific to each `Exporter` and then exposing it. 

Each exporter runs on a scheduled interval to fetch new data. This interval
can be very frequent since the exporter should not be concerned about source
details like ratelimiting. The interval is nontheless configurable, you can
find the possible configurations [here](configuration/configuration.md#exporters)

<p align="center">
    <img
        width="100%"
        src="../images/exporter-light.png#only-light"
    />
    <img 
        width="100%"
        src="../images/exporter-dark.png#only-dark"
    />
</p>

## Github Adapter

As mentioned in the [domain](#domain) section of this page, the adapter implementation
should implement all `QueryPort`'s with which the data gets fetched. Our implementation
uses [springs `RestClient`](https://docs.spring.io/spring-framework/reference/integration/rest-clients.html#rest-restclient)
to do this making all the nessesary requests to fetch the data.

<p align="center">
    <img
        width="100%"
        src="../images/adapter-light.png#only-light"
    />
    <img 
        width="100%"
        src="../images/adapter-dark.png#only-dark"
    />
</p>

### Mapping to Domain

This topic is explored more in the [**Api to Domain Mapping**](github/api-to-domain-mapping.md)
page but I thought I would mention a few things here. Each request response gets
gets mapped to a `github-adapter` specific class which then knows how to turn
itself into the relevant `domain` class. This allows the domain structure to be
somewhat decoupled from Githubs internal structure and easy extension to other
Github like Api's. For more details on this mapping go to [the above mentioned page](github/api-to-domain-mapping.md)

### Request Caching

To avoid slamming the Api with thousands of requests the adapter uses [springs `@Cachable`](https://docs.spring.io/spring-framework/reference/integration/cache/annotations.html)
annotation to cache function calls. This is especially important becuase a lot of use cases
need the same data which would lead to a lot of unnessesary requests.

#### Example

The list of repositories is needed by almost all use cases. The number of
workflow runs or the number of self hosted runners is repository specific, for this
reason all these use cases will make a call to the `RepositoriesQueryPort` and
fetch the nessesary data. But after the first call this result actually almost
never changes. Here a cached result comes in handy since it avoids the high number
of actual requests to the Api.
