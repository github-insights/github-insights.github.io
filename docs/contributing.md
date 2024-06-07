# Ideas for Contributions

There is obviously the possiblity to add new [Adapter](#adapters) and [Exporter](#exporters)
but aside from those bigger features there are a number of things that could be
improved on the current codebase.

- Additional Metrics
- Repository label

    Currently all metrics exposed by Github Metrics are in no way tied to the
    repository they come from. Although it adds a lot of cardinality it could be
    interesting to add the repository as a metric on every metric.

- Repository grouping
    
    Instead of putting the single repository label on every metric we could
    also add some other kind of grouping. An example would be the tags that
    repositories can have. Add those tags as labels on the metrics.

- Webhooks
    
    All data needs to be fetched periodically which causes a lot of unnessesray
    requests. To circumvent this you could make it possible ot listen for webhooks
    and only start requesting once Github tells you to. That said this is a bit
    more complex since it would expose endpoints on the open internet.

- improve Startup Sequence

    Currently the service struggles on startup since all the caches need to be filled.
    To improve this situation, add some kind of functionality to stagger the requests
    or something else to avoid the number of requests on startup.



## New Adapters and Exporters

Seeing as Github Metrics is built in a hexagonal was it is quite easy to extend
it and add more adapters and exporters as preferred. So here is a non exhaustive
list of ideas for new adapter and exporters that could be added.

### Adapters

When it comes to adapters for other git hosting services there are a number of
different adapters that could be built, that said there probably would be some
changes needed on the `domain` side to be able to properly implement them. This
would mean generizising the domain to better fit the generic git usecase rather
then the specific Github one.

- Gitlab
- Bit Bucket
- Travis CI
- ... and more

### Exporters

On the exporters side we have a lot more choices since the data returned by
the use cases is quite generic and not rarrowed down to a very specific usecase
yet, meaning that that work is on the part of the exporter. 

- Relational Database
- Rest Api
- ... and more

