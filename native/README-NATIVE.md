# WKND Native App

## UI Design

![](design.png)

## Changes to WKND

1. Added `App` CFM.
2. Added `tags` property to `Adventure` CFM.
3. Updated `Downhill Skiing in Jackson Hole` CF with `wknd:customer-journey/attract` tag to support featured query.

## GraphQL Queries

### Screen 1 - Home

The home screen consists of a single GraphQL query that searches across two Content Fragment Models:

1. App model
2. Adventure model

```
{
  appByPath(_path: "/content/dam/wknd/en/app/wknd-adventures") {
    item {
      _path
      appTitle
      appHeroImage {
        ... on ImageRef {
          _path
          width
          height
        }
      }
    }
  }
  adventureList(
    filter: {tags: {_expressions: [{value: "wknd:customer-journey/attract", _operator: EQUALS}]}}
  ) {
    items {
      _path
      adventureTitle
      adventurePrimaryImage {
        ... on ImageRef {
          _path
          width
          height
        }
      }
    }
  }
}
```

### Screen 2 - Adventure List

```
{
  adventureList {
    items {
      _path
      adventureTitle
      adventureDescription {
        html
      }
      adventurePrimaryImage {
        ... on ImageRef {
          _path
          width
          height
        }
      }
    }
  }
}
```

**Assumptions:**
* Page title, _WKND Adventures_, is re-used from the home page query (i.e `app.appTitle`)
* Hero image is re-used from the home page query (i.e. `app.appHeroImage`)
* Sub heading, _Our Adventures_, is hard coded in the app.

### Screen 3 - Detail Page

The detail page uses a _ByPath_ query and supports a path variable.

```
query findAdventureByPath($apath: String!) {

  adventureByPath(
    _path: $apath
  ) {
    item {
      _path
      adventureTitle
      adventureDescription {
        html
      }
      adventurePrimaryImage {
        ... on ImageRef {
          _path
          width
          height
        }
      }
    }
  }
}
```

**Parameters**
```
{
  "apath": "/content/dam/wknd/en/adventures/downhill-skiing-wyoming/downhill-skiing-wyoming"
}
```

## Persisted Queries

The following section contains the Persisted Queries. At the time of this writing, the Persisted Queries are identical to the corresponding GraphQL queries above.

### Screen 1 - Home

```
$ curl -s -u admin:admin http://localhost:4502/graphql/execute.json/wknd/native-app-home 
```

### Screen 2 - Adventure List

```
$ curl -s -u admin:admin http://localhost:4502/graphql/execute.json/wknd/native-app-adventures
```

### Screen 3 - Detail Page

**Note: The `apath` parameter defines the Adventure path.**

```
$ curl -s -u admin:admin http://localhost:4502/graphql/execute.json/wknd/native-app-adventure%3bapath=/content/dam/wknd/en/adventures/downhill-skiing-wyoming/downhill-skiing-wyoming
```
