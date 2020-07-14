# analytics domain

## set\_analytics\_enabled

### Stories addressed <a id="stories-addressed-1"></a>

* Enables or disables the use of analytics. The default state of native apps is to have analytics disabled. 
* Users need to be uniquely identified for analytics purposes. Note that user identifiers must be not represent or be tied to personal information.

#### Treatment of analytics events before set\_analytics\_enabled is called

* Native apps should "queue" analytics events \(for example app launch events\) for sending and retain them during the current session until `set_analytics_enabled` is called with an `is_enabled` value of `true`, at which point sending the queued analytics events to backend servers is triggered.

### URL <a id="url-1"></a>

```text
liquidstate://analytics/set_analytics_enabled?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-1"></a>

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| is\_enabled | Boolean | Yes | Determines whether native apps should initiate analytics event tracking |
| user\_id | String | Yes | Sets the unique identifier to use for analytics purposes for the current user. |

### Response <a id="response-data-1"></a>

No response, the native app will simply enable or disable the sending of analytics events to backend servers.

