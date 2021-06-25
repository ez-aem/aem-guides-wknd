# WKND Native App

## UI Design

![](design.png)


## GraphQL Queries

### Screen 1

TODO

### Screen 2 - Adventure List

TODO

### Screen 3

TODO

## Persisted Queries

### Screen 1

TODO

### Screen 2 - Adventure List

An out of the box PQ is included that contains some of the required page data. 

```
$ curl -u admin:admin http://localhost:4502/graphql/execute.json/wknd/adventures-all
```

The following fields are missing:

1. Page title: WKND Adventures
2. Hero image
3. Sub heading: Our Adventures
4. Adventure description

To help close the gaps the following PQ is used.

```
TODO
```

### Screen 3 - Adventure Title

TODO
