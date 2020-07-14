# iwa domain

## set\_ready

### Stories addressed <a id="stories-addressed-1"></a>

* The IWA has finished loading its core code and is declaring to the native app that it is ready to handle window.communicate events.

### URL <a id="url-1"></a>

```text
liquidstate://iwa/set_ready?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-1"></a>

None

### Response <a id="response-data-1"></a>

No response

## navigate <a id="navigate"></a>

### Stories addressed <a id="stories-addressed-1"></a>

* Navigate from an IWA to another IWA, with a route specified
* Navigate to a different route in the same IWA
* Navigate from a native view to an IWA
* Optionally: change tab before navigation \(ignored for linear apps\)

### URL <a id="url-1"></a>

```text
liquidstate://iwa/navigate?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-1"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Required</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">webapp_id</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">Default value: current IWA&apos;s webapp_id</td>
    </tr>
    <tr>
      <td style="text-align:left">entrypoint</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">Default value: &quot;default&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left">route</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">Default value: &quot;/&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left">transition</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">
        <p>Possible values:</p>
        <ul>
          <li>push</li>
          <li>replace</li>
          <li>modal</li>
        </ul>
        <p>Default value: &quot;push&quot;</p>
        <p>Specifies how the next view or activity should be presented. The default
          is &#x201C;push&#x201D;, which is equivalent to the platform&#x2019;s default
          (e.g. &#x201C;slide from the right hand side on iOS&#x201D;).</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">latest</td>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">
        <p>Default value:</p>
        <ul>
          <li>true if navigate to other webapp id or no local copy of the webapp</li>
          <li>false if navigating to the same webapp id</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">tab_id</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">
        <p>Indicates that the equivalent of a switch_tab event should be executed
          prior to this navigate event.</p>
        <p>Not applicable to linear apps.</p>
        <p>The value must be a tab defined in the app config at launch.</p>
      </td>
    </tr>
  </tbody>
</table>

#### Example request data <a id="example-request-data"></a>

```javascript
{
    "webapp_id": "login",
    "entrypoint": "default",
    "route": "/",
    "replace": true,
    "latest": true,
}
```

### Response <a id="response-data-1"></a>

No response. Second web app will get window.communicate call with purpose = navigate.

## navigate\_back <a id="navigate_back"></a>

### Stories addressed

* Navigate from a route back to another route on the navigation stack, with a route id specified
* Navigate from a route back to the previous route

### URL <a id="url-1"></a>

```text
liquidstate://iwa/navigate_back?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-1"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Required</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">webapp_id</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">Default value: null (equivalent to &quot;routes belonging to any IWA&quot;)</td>
    </tr>
    <tr>
      <td style="text-align:left">route_id</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">
        <p>Default value: null</p>
        <p>skip all other routes until a route with a matching id is found. The default
          is null, meaning any route id&quot;.</p>
      </td>
    </tr>
  </tbody>
</table>

A route must match both webapp\_id and route\_id to be navigated to.

#### Example request data <a id="example-request-data"></a>

```javascript
{
    "webapp_id": "myapp",
    "route_id": "home"
}
```

### Response <a id="response-data-1"></a>

No response. When displaying the mathcing route again, the native app will send the original navigate event to the IWA, with all its data, for example:

```javascript
window.communicate({
    "purpose": "navigate",
    "route": "/",
    "params": {
        "querystringparam1": "value"
    },
    "context": {
        "product_id": "com.example.app123.doc1",
        "page_slug": "page-1"
    }
})
```

## trigger\_action <a id="trigger_action"></a>

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

## handle\_share \(deprecated\) <a id="handle_share"></a>

### Note

handle\_share events are deprecated in favour of the upcoming Service IWA feature

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



