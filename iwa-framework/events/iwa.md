# iwa domain

## trigger\_action

### Stories addressed <a id="stories-addressed-1"></a>

* An action defined by a web app is triggered within the native UI \(presumably toolbar or navabar button was tapped\)
* The current IWA wants to initiate a workflow which typically starts with an action

### URL <a id="url-1"></a>

```text
liquidstate://iwa/navigate?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-1"></a>

| Property name | Type | Required | Description | Default |
| :--- | :--- | :--- | :--- | :--- |
| id | String | Yes | The identifier of the action to be triggered. Note the id is unique per route \(see webapp.json file\). | ​ |
| params | Object | No | An object listing parameter properties and their values which should override default values. | {} |

#### Example request data <a id="example-request-data"></a>

```javascript
{
    "id": "edit",
    "params": {
        "edit-mode": "advanced"
    }
}
```

### Response data <a id="response-data-1"></a>

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| id | String | Yes | the identifier of the action being triggered |
| params | Boolean | No | An object of key / value properties. The values are the default ones from the action’s definition \(see webapp.json file\), overridden by the ones specified in the request. |

#### Example response <a id="example-response-1"></a>

```javascript
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "trigger_action",
    "response_data": {
        "id": "edit",
        "params": {
            "edit-mode": "advanced"
        }
    }
})
```

### Trigger action event initiated by the native app <a id="trigger-action-event-initiated-by-the-native-app"></a>

Most of the time, actions are not triggered by the IWA but by the user tapping/clicking on a button which was presented to them within the native UI according to the IWA’s definition.

In this case there is no request/response flow but simply a `window.communicate(...)` event from the native to the IWA, with its `purpose` set to `trigger_action`.

**Comm event data**

Similar to the `response_data` above but at the root of the event object.

**Example comm event**

```javascript
window.communicate({
    "purpose": "trigger_action",
    "id": "edit",
    "params": {
        "edit-mode": "advanced"
    }
})
```

## handle\_share <a id="handle_share"></a>

### Stories addressed <a id="stories-addressed-2"></a>

* The native app had information shared with it by the OS or another app and passes it on to its default IWA.
* This not a request/response event, so its data sits at the root of the event object.

### Comm event <a id="comm-event"></a>

**Comm event data**

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| source | String | Yes | A string identifying the source of the share \(other app id etc.\) |
| files | array of objects of type File \(see below\) | No | ​ |
| info | An object of key/value properties | No | This information is open-ended and the IWA is left to interprete it. |

File object properties:

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| path | String | Yes | ​ |
| mime\_type | String | No | ​ |
| size | null or Number | No | size of the file in bytes. Null if the size cannot be obtained. |

**Example comm event**

```javascript
window.communicate({
    "purpose": "handle_share",
    "source": "com.example.otherapp",
    "files": [
        {
            "path": "file:///path/to/local/file.png",
            "mime_type": "image/png"
        }
    ],
    "info": {
        "datetime": "2017-08-02 13:42:57"
    }
})
```



