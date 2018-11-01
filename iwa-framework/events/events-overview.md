# Events overview

## Structure of requests

```text
SCHEME (liquidstate)
    DOMAIN (e.g. iwa, iab, kv, user_storage, config, documents)
        EVENT_TYPE (e.g. navigate, get_item, set_item, containers, containers/get)
            DATA = {
                "request_id": "UUID",
                "data": { object depending on event type }
```

Request from webview \(IAB, IWA or document page - eventually\)

```text
liquidstate://DOMAIN/EVENT_TYPE?request=URLENCODED_JSON_DATA
where URLENCODED_JSON_DATA = {
    "id": "REQUEST_ID",
    "data": DATA_OBJECT
}
```

## Structure of responses

```javascript
window.communicate({
    "purpose": "PURPOSE"
    // ... additional properties
})
```

Expected values for PURPOSE are:

* navigate
* response

Additional properties depend on the specified purpose. For example, when PURPOSES is “response”, the additional properties are the following:

* request\_id
* event\_type
* response\_data

Response from webview \(IAB, IWA or document page - eventually\)

```javascript
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "get_item",
    "response_data": {
        //...
    }
})
```

Example of ad-hoc communication from native app to IWA to tell it to navigate to a specific route:

```javascript
window.communicate({
    "purpose": "navigate",
    "route": "/page-2",
    "params": {
        "querystringparam1": "value"
    },
    "context": {
        "product_id": "com.example.app123.doc1",
        "page_slug": "page-1"
    }
})
```

## Availability of event domains

Event may or may not be supported depending on the type of the view currently displayed in the app.

Each type of view in the app, or rather each type of area in the app defines which event domains it supports.

When an app area supports a domain, it is assumed that it supports all events in this domain.



| Event domain | IWA views | IAB views | Document library views |
| :--- | :---: | :---: | :---: |
| app | Yes | Yes | Yes |
| config | Yes | No | Yes |
| kv | Yes | No | Yes |
| iwa | Yes | No | No |
| iab | No | Yes | No |
| documents | No | No | Yes |
| launch | Yes | Yes | Yes |
| userfiles | Yes | No | No |

Note 1: `kv` and `config` domains are not useful when used from actions \(i.e. tapping a button that triggers a “get me this config value” event isn’t very useful\).

Note 2: the document library views don’t make use of events from the `kv` and `config` domains but the existing document library code is already able to use the key/value store and app config, it just do this through the event framework.

