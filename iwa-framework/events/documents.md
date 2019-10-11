# documents domain

## open

### Stories addressed <a id="stories-addressed-1"></a>

- Navigate from an IWA to open/reading documents

Note: currently, native apps will first check whether a document is locally present on the device and up to date before trying to open it for reading. If it is not, the native app will download the document, presenting to the user a native download progress UI. In an upcoming version of native apps, this will be removed and additional IWA events will be supproted in the "documents" domain to deal with the downloading/updating of documents.

### URL <a id="url-1"></a>

```text
liquidstate://documents/open?request=URLENCODED_REQUEST_OBJECT
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
      <td style="text-align:left">product_id</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">The unique identifier (a.k.a. "product id") of the document to open</td>
    </tr>
    <tr>
      <td style="text-align:left">page_slug</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">Default value: first page of the document</td>
    </tr>
  </tbody>
</table>

#### Example request data <a id="example-request-data"></a>

```javascript
{
    "product_id": "com.mycompany.awesomedoc",
    "page_slug": "page-4"
}
```

### Response <a id="response-data-1"></a>

No response
