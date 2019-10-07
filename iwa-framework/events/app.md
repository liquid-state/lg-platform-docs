---
description: List of event types part of the "app" domain.
---

# app domain

## reset

### Stories addressed

* IWA decides that the entire app should be reload, including all its configuration and navigation contexts.

### URL

```text
liquidstate://app/reset?request=URLENCODED_REQUEST_OBJECT
```

### Request data

None

### Response data

No response.

## online\_status

### Stories addressed

* IWA or other part of the app needs to know if the device/app is currently online or offline

### URL

```text
liquidstate://app/online_status?request=URLENCODED_REQUEST_OBJECT
```

### Request data

None

### Response data

| Property name | Type | Required |
| :--- | :--- | :--- |
| status | Boolean | Yes |

#### Example response

```javascript
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "online_status",
    "response_data": {
        "status": true
    }
})
```

## open\_file

### Stories addressed

* IWA or other part of the app needs to open an arbitrary file in the platform-default way.

### Description

On mobile platforms, the file must be at a location accessible to the app.

If the path to the file is a relative one \(starts with “./”\), the native must compute the absolute path according to the following: - if an IWA is sending the event, the absolute path is relative to the entrypoint file of the IWA - if the native document reading view is sesnding the event, the absolute path is relative to the HTML file being read - if another native part of the app is sending the event, the absolute path is relative to the app bundle’s root

On iOS, the native would open the file in the iOS default documetn viewer \(calling NSApplication:openURL\). On Android, the native would trigger an intent and let the OS handle opening the file from that intent.

### URL

```text
liquidstate://app/open_file?request=URLENCODED_REQUEST_OBJECT
```

### Request data

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Required</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">path</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">
        <ul>
          <li>a HTTP URL (path starts with &#x201C;HTTP://&#x201D; or &#x201C;HTTPS://&#x201D;)</li>
          <li>a relative location (path starts with &#x201C;./&#x201D;)</li>
          <li>an absolute location (path starts &#x201C;<a href="file:///">file:///</a>&#x201D;)</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>#### Example request data

```javascript
{
    "path": "./files/my_sample_file"
}
```

### Response

No response.

## set\_authentication\_status

### Stories addressed

* IWA logged the user in or out and lets the native app know.

### URL

```text
liquidstate://app/set_authentication_status?request=URLENCODED_REQUEST_OBJECT
```

### Request data

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| is\_authenticated | Boolean | Yes |  |

**Example request data**

```javascript
{
    "is_authenticated": true
}
```

### Response

No response.

## set\_notification\_presentation\_status

### Stories addressed

* Login IWA needs to let the native app know that a user is ready to have notifications presesented to them.

### Description

The user might not be logged oput and therefore not in a state where they should have notifications,with actions that require being authenticated presented to them.

Similarly, the application logic or UI might be busy and in a state where it isn’t desirable to present the user with notifications.

By default, the native app is in a _not ready_ status and therefore won’t present any notification to the user until this call is made.

### URL

```text
liquidstate://app/set_notification_presentation_status?request=URLENCODED_REQUEST_OBJECT
```

### Request data

| Property name | Type | Required |
| :--- | :--- | :--- |
| is\_ready | Boolean | Yes |

#### Example request

```javascript
{
    "is_ready": true
}
```

### Response

No response

## set\_back\_override

### Stories addressed <a id="stories-addressed-3"></a>

* IWA wants to interrupt native back navigation for the current route when the user triggers it.

### URL <a id="url-3"></a>

```text
liquidstate://app/set_back_override?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-3"></a>

| Property name | Type | Required |
| :--- | :--- | :--- |
| is\_enabled | Boolean | Yes |

#### Example request <a id="example-request"></a>

```javascript
{
    "is_enabled": true
}
```

### Response <a id="response-2"></a>

No response. When the user triggers native back navigation, the native app will send the following window.communicate event to the IWA:

```javascript
window.communicate({
    "purpose": "lifecycle",
    "id": "back"
})
```

## user\_location

### Stories addressed

* IWA needs to know the devices current geographic location

### URL

```text
liquidstate://app/user_location?request=URLENCODED_REQUEST_OBJECT
```

### Request data

None

### Response

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
      <td style="text-align:left">type</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">
        <p>one of &#x201C;fine&#x201D;, &#x201C;coarse&#x201D;, &#x201C;unknown&#x201D;,
          &#x201C;none&#x201D;</p>
        <p></p>
        <p>&#x201C;fine&#x201D; and &#x201C;coarse&#x201D; match android terminology
          where:</p>
        <ul>
          <li>fine = gps location,</li>
          <li>coarse = wifi</li>
        </ul>
        <p>&#x201C;unknown&#x201D; means that there is a location but it is unclear
          how accurate it is (e.g. the platform may not report the accuracy. An example
          may be the web client or iOS which does not report how it obtained the
          location)</p>
        <p>&#x201C;none&#x201D; means no location could be returned .. see &#x201C;error&#x201D;
          for details</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">location</td>
      <td style="text-align:left">Object</td>
      <td style="text-align:left">true if &#x201C;type&#x201D; is NOT &#x201C;none&#x201D;. If &#x201C;type&#x201D;
        IS &#x201C;none&#x201D;, this property will notbe present.</td>
      <td style="text-align:left">
        <ul>
          <li>latitude
            <ul>
              <li>required : true</li>
              <li>value : numeric string</li>
            </ul>
          </li>
          <li>longitude
            <ul>
              <li>required : true</li>
              <li>value : numeric string</li>
            </ul>
          </li>
          <li>accuracy
            <ul>
              <li>required : false</li>
              <li>value : numeric string</li>
              <li>comment : accuracy of the location as a radius, units=metres. Confidence:
                68% on android, unknown on iOS. Web?</li>
            </ul>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">error</td>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">true if &#x201C;type&#x201D; is &#x201C;none&#x201D; otherwise not present</td>
      <td
      style="text-align:left">
        <ul>
          <li>reason
            <ul>
              <li>required : true</li>
              <li>value : one of &#x201C;nopermission&#x201D;, &#x201C;disabled&#x201D;,
                &#x201C;nolocation&#x201D;, &#x201C;unknown&#x201D;</li>
            </ul>
          </li>
          <li>comment :
            <ul>
              <li>&#x201C;nopermission&#x201D; means the user has specifically refused location
                permission for this app (this has a higher priority than disabled)</li>
              <li>&#x201C;disabled&#x201D; means that location services are turned off on
                the device</li>
              <li>&#x201C;nolocation&#x201D; means that this device cannot access location
                information(not expected in practice)</li>
              <li>&#x201C;unknown&#x201D; means the system could not access location information
                due to a system or app error. More descriptive information should be available
                in the &#x201C;message&#x201D; property.</li>
            </ul>
          </li>
          <li>message
            <ul>
              <li>required : true</li>
              <li>value : string</li>
              <li>comment : (hopefully) localised message describing the error</li>
            </ul>
          </li>
        </ul>
        </td>
    </tr>
  </tbody>
</table>#### Example response data

```javascript
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "user_location",
    "response_data": {
        "type": "fine",
        "location" : {
            "latitude" : "-27.502520099999998"",
            "longitude" : "153.0462454",
            "accuracy" : "7.21"
        }
    }
})

window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "user_location",
    "response_data": {
        "type": "unknown",
        "location" : {
            "latitude" : "-27.502520099999998"",
            "longitude" : "153.0462454",
            "accuracy" : "21.34"
        }
    }
})
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "user_location",
    "response_data": {
        "type": "none",
        "error" : {
            "reason" : "nopermission",
            "message" :  "You have not granted location permissions for this app. Please goto settings/security and grant location permission for this app"
        }
    }
})
```

## feature\_status

### Stories addressed <a id="stories-addressed-3"></a>

* IWA needs to know whether a device feature is available and any qualifying details that may be relevant

### URL <a id="url-3"></a>

```text
liquidstate://app/feature_status?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-3"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Required</th>
      <th style="text-align:left">Possible values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">feature</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">
        <ul>
          <li>biometrics</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>#### Example request <a id="example-request"></a>

```javascript
{
    "feature": "biometrics"
}
```

### Response

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
      <td style="text-align:left">status</td>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">
        <p>Possible values:</p>
        <ul>
          <li>&quot;unknown&quot; means the requested feature is name not handled by
            this event</li>
          <li>&quot;not_present&quot; means the current device cannot provide this feature
            (ever)</li>
          <li>&quot;disabled&quot; means the feature is present on the device but the
            user has either disabled it e.g. biometrics are never available if a passcode
            has not been set</li>
          <li>&quot;not_configured&quot; means the feature is present and is not disabled,
            but the user must take some steps to make it usable</li>
          <li>&quot;available&quot; means the feature is present and configured and
            may be used</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">details</td>
      <td style="text-align:left">Object</td>
      <td style="text-align:left">Yes is status is not &quot;unknown&quot;</td>
      <td style="text-align:left">This object will contain feature specific details &#x2013; See &quot;Feature
        Details&quot; below</td>
    </tr>
  </tbody>
</table>#### Feature details for biometrics

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
      <td style="text-align:left">feature</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">The name of the feature that was requested.</td>
    </tr>
    <tr>
      <td style="text-align:left">status</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">
        <p>Possible values:</p>
        <ul>
          <li>&quot;not_present&quot; means the current device does not have fingerprint
            or face-id authentication capability</li>
          <li>&quot;disabled&quot; means that there is no passcode registered for the
            device or that biometrics have been explicitly disabled by the user</li>
          <li>&quot;not_enrolled&quot; means biometrics are not disabled and a passcode
            has been set but that no fingerprints or face-id have been enrolled</li>
          <li>&quot;available&quot; at least one type of biometric authentication is
            available and fully configured</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">type</td>
      <td style="text-align:left">Array of String</td>
      <td style="text-align:left">Yes if status is &quot;available&quot;</td>
      <td style="text-align:left">
        <p>This lists the available biometric authentication methods. At the time
          of writing iOS devices will return a single item list containing either
          &quot;face&quot; or &quot;touch&quot;.</p>
        <p>Possible values:</p>
        <ul>
          <li>&quot;touch&quot;</li>
          <li>&quot;face&quot;</li>
          <li>&quot;iris&quot;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <ul>
          <li></li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>#### Example response

```javascript
// device does not have biometrics capability
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "feature_status",
    "response_data": {
        "status": "not_present",
        "detail": {
            feature: "biometrics",
            status: "not_present"
        }
    }
})

// biometrics disabled or passcode/pin not set
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "feature_status",
    "response_data": {
        "status": "disabled",
        "detail": {
            feature: "biometrics",
            status: "disabled"
        }
    }
})

// biometrics enabled but no fingerprints/face-id enrolled
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "feature_status",
    "response_data": {
        "status": "not_configured",
        "detail": {
            feature: "biometrics",
            status: "not_enrolled"
        }
    }
})

// biometrics available on iPhone X
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "feature_status",
    "response_data": {
        "status": "available",
        "detail": {
            feature: "biometrics",
            status: "available"
            type: ["face"]
        }
    }
})

// biometrics available on iPhone 6s
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "feature_status",
    "response_data": {
        "status": "available",
        "detail": {
            feature: "biometrics",
            status: "available"
            type: ["touch"]
        }
    }
})


// biometrics available on Samsung S8 (with nothing disabled or un-configured)
window.communicate({
    "purpose": "response",
    "request_id": "UUID",
    "event_type": "feature_status",
    "response_data": {
        "status": "available",
        "detail": {
            feature: "biometrics",
            status: "available"
            type: ["touch","face","iris"]
        }
    }
})
```

## clearall

### Stories addressed

* IWA or other part of the app wants to reset the app to its initial launch condition \(but not clearing authentication state\).

### URL

```text
liquidstate://app/clearall?request=URLENCODED_REQUEST_OBJECT
```

### Request data

None

### Response

No response

## switch\_tab

### Stories addressed

* IWA or other part of the app wants to change the current displayed tab \(only actioned in a tabbed app\)

### Request data <a id="request-data-3"></a>

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
      <td style="text-align:left">id</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">A tab id declared in the app configuration.</td>
    </tr>
    <tr>
      <td style="text-align:left">reset</td>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">False</td>
      <td style="text-align:left">
        <p>If true, the tab should be reset to it&#x2019;s initial launch state before
          any route is applied.</p>
        <p>Default value: true</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">route</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">
        <p>The name of a route to navigate to in the destination tab. If not present,
          no extra route navigation will be performed.</p>
        <p>Default value: null</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">params</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">A JSON object of arbitrary parameters to be passed onto the tab that is
        switched to.</td>
    </tr>
  </tbody>
</table>#### Example request <a id="example-request"></a>

```javascript
{
    "id": "t1",
    "reset": true,
    "route": "/page1"
}
```

### Response <a id="response-2"></a>

No response, but...

#### Message for tab being switched to

The native app will issue a window.communicate call to the tab being switched to, optionally including the "params" property specified in the original event.

```javascript
window.communicate({
    "purpose": "switch_tab",
    "params": {
        "foo1": "bar1",
        "foo2": "bar2"
    }
})
```

## set\_tab\_appearance

### Stories addressed

* IWA or other part of the app wants to modifiy the appearance \(icon and/or text\) of a tab.
* Although the configuration of the list of tabs and the IWAs loaded in them never changes, this can be used by IWAs to simulate the display of different tabs to different users.

### Request data <a id="request-data-3"></a>

Not all tabs have to be specified in this event and it is possible to update the appearance of one or more tabs at a time.

The request data is an object which has as properties the "id" of the tabs to modify, as configured in the app data.

The value for each of these tab ids is an object with the following properties:

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
      <td style="text-align:left">title</td>
      <td style="text-align:left">String or Object</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">The text to be displayed on the tab. This can be a simple string, or an
        object to support translations (please refer to the app data configuration
        documentation).</td>
    </tr>
    <tr>
      <td style="text-align:left">icon_name</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Yes if you want to change the icon and &quot;icon_font_name&quot; and
        &quot;icon_code_point&quot; are not specified.
        <br />No otherwise.</td>
      <td style="text-align:left">The name of the icon to be used from the default set of app icons.</td>
    </tr>
    <tr>
      <td style="text-align:left">icon_font_name</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">
        <p>Yes if you want to change the icon and &quot;icon_name&quot; is not specified.</p>
        <p>No otherwise.</p>
      </td>
      <td style="text-align:left">The name of the font in the custom fonts configuration.</td>
    </tr>
    <tr>
      <td style="text-align:left">icon_code_point</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">
        <p>Yes if you want to change the icon and &quot;icon_name&quot; is not specified.</p>
        <p>No otherwise.</p>
      </td>
      <td style="text-align:left">The code point of the glyph in the custom font file.</td>
    </tr>
    <tr>
      <td style="text-align:left">hidden</td>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">Specify whether this particular tab should be displayed or ommitted from
        the tab bar.</td>
    </tr>
  </tbody>
</table>#### Example request <a id="example-request"></a>

```javascript
{
    "tab1": {
        "title": "Favourites",
        "icon_name": "star"
    },
    "tab3": {
        "title": {
            "key": "favourites.tab-title",
            "translations": {
                "en": "Favourite",
                "fr": "Favoris"
            }
        },
        "icon_font_name": "custom",
        "icon_code_point": "\U234"
    }
}
```

### Response <a id="response-2"></a>

No response

