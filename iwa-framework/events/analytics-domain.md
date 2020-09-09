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

## set\_super\_properties

### Stories addressed <a id="stories-addressed-1"></a>

* Set or reset the list of properties that should be automatically added to any analytics event. 

### URL <a id="url-1"></a>

```text
liquidstate://analytics/set_super_properties?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-1"></a>

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| properties | Object | Yes | JSON object \(pairs of keys and values\) |

### Response <a id="response-data-1"></a>

No response, the native app will internally update its list of super properties to be used for future analytics events.

## add\_super\_properties

### Stories addressed <a id="stories-addressed-1"></a>

* Augment the list of properties that should be automatically added to any analytics event. 

### URL <a id="url-1"></a>

```text
liquidstate://analytics/add_super_properties?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-1"></a>

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| properties | Object | Yes | JSON object \(pairs of keys and values\) |

### Response <a id="response-data-1"></a>

No response, the native app will internally update its list of super properties to be used for future analytics events.

## remove\_super\_properties

### Stories addressed <a id="stories-addressed-1"></a>

* Remove the use of some properties that should be automatically added to any analytics event. 

### URL <a id="url-1"></a>

```text
liquidstate://analytics/remove_super_properties?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-1"></a>

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| properties | Object | Yes | JSON object \(pairs of keys and values\) |

### Response <a id="response-data-1"></a>

No response, the native app will internally update its list of super properties to be used for future analytics events.

## post

### Stories addressed <a id="stories-addressed-1"></a>

* Track an analytics event

### URL <a id="url-1"></a>

```text
liquidstate://analytics/post?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-1"></a>

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| name | String | Yes | The name of the event |
| properties | Object | Yes | JSON object \(pairs of keys and values\) |

### Response <a id="response-data-1"></a>

No response, the native app will internally update its list of super properties to be used for future analytics events.

