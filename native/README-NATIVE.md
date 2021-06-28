# WKND Native App

## UI Design

![](design.png)

## Changes to WKND

1. Added `App` CFM.
2. Added `tags` property to `Adventure` CFM.
3. Updated `Downhill Skiing in Jackson Hole` CF with `wknd:customer-journey/attract` tag to support featured query.

## GraphQL Queries

### Screen 1

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

### Screen 3: Detail Page

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

{
  "apath": "/content/dam/wknd/en/adventures/downhill-skiing-wyoming/downhill-skiing-wyoming"
}
```

TODO:
* [ ] Event List

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
