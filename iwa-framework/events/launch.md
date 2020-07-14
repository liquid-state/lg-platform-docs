# launch domain

## iwa

### Stories addressed <a id="stories-addressed-1"></a>

* Open a new instance of an Integrated Web App, optionally at a particular route

### URL <a id="url-1"></a>

```text
liquidstate://launch/iwa?request=URLENCODED_REQUEST_OBJECT
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
      <td style="text-align:left">Default value: current webapp id (or null when currently in native view)</td>
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
          <li>&quot;push&quot;</li>
          <li>&quot;replace&quot;</li>
        </ul>
        <p>Specifies how the next view or activity should be presented. The default
          is &quot;push&quot;, which is equivalent to the platform&apos;s default
          (e.g. &quot;slide from the right hand side on iOS&quot;).</p>
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
  </tbody>
</table>

### Response <a id="response-data-1"></a>

No response, the native app will simply launch the IWA with the specified transition.

## document

### Stories addressed <a id="stories-addressed-2"></a>

* Navigate from an IWA to open/reading documents

Note: currently, native apps will first check whether a document is locally present on the device and up to date before trying to open it for reading. If it is not, the native app will download the document, presenting to the user a native download progress UI. In an upcoming version of native apps, this will be removed and additional IWA events will be supproted in the "documents" domain to deal with the downloading/updating of documents.

### URL <a id="url-2"></a>

```text
liquidstate://launch/document?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-2"></a>

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| product\_id | String | Yes | The unique identifier \(a.k.a. "product id"\) of the document to open |
| page\_slug | String | No | Default value: first page of the document |

#### Example request data <a id="example-request-data-2"></a>

```javascript
{
    "product_id": "com.mycompany.awesomedoc",
    "page_slug": "page-4"
}
```

### Response <a id="response-data-2"></a>

No response, the native app will simply launch the document viewer \(as noted above it may first download the document if needed\).

## email

### Stories addressed <a id="stories-addressed-3"></a>

* Open the device's built-in email composing client, optionally pre-populating fields.

### URL <a id="url-3"></a>

```text
liquidstate://launch/email?request=URLENCODED_REQUEST_OBJECT
```

### Request data <a id="request-data-3"></a>

| Property name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| to | String | No | A comma-separated list of email addresses |
| cc | String | No | A comma-separated list of email addresses |
| bcc | String | No | A comma-separated list of email addresses |
| subject | String | No | One line of text to pre-populate the subject of the email |
| body | String | No | One or more lines of text to pre-populate the body of the email |

#### Example request data <a id="example-request-data-3"></a>

```javascript
{
    "to": "contact@example.com",
    "subject": "Could you help me out?",
    "body": "Great mobile app!\n\nI just have a few questions..."
}
```

### Response <a id="response-data-3"></a>

No response, the native app will simply launch the device's standard email composing interface. The user should get back to the app after successfully sending an email or dismissing this interface.

