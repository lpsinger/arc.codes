---
title: '@http'
category: app.arc
description: Define HTTP routes
---

Define HTTP routes in API Gateway with Lambda handler functions.

## Syntax

Each route is made up of two parts: HTTP verb and a route path.

- HTTP Verb
  - `delete`
  - `get`
  - `head`
  - `options`
  - `patch`
  - `post`
  - `put`
  - `any`<sup>1</sup>

- Route Path
  - Dashes and underscores are not allowed
  - Must begin with a letter
  - Advised maximum of 100 characters for paths
  - URL parameters are defined with a leading colon (`:`)
  - A trailing asterisk (`*`) denotes a "catchall"

Routes can use more verbose configuration to allow for [custom source paths](../../guides/developer-experience/custom-source-paths) in your project. Provide a  `method` and `src` for each route:

- `method` - HTTP verb
- `src` - path to the function source

1. While not an HTTP verb, functions declared with `any` will be called for any valid HTTP method directed at that route.

## Example

These configuration examples show how to define `@http` routes:

<arc-viewer default-tab=arc>
<div slot=contents>

<arc-tab label=arc>
<h5>arc</h5>
<div slot=content>

```arc
@app
myapp

@http
get /
get /pages
get /pages/:dateID
get /contact
post /contact
get /widgets/* # catch all unmatched routes
# verbose custom source:
/weather
  method get
  src custom/source
```
</div>
</arc-tab>

<arc-tab label=json>
<h5>json</h5>
<div slot=content>

```json
{
  "app": "myapp",
  "http": [
    ["get", "/"],
    ["get", "/pages"],
    ["get", "/pages/:dateID"],
    ["get", "/contact"],
    ["post", "/contact"],
    ["get", "/widgets/*"],
    {
      "/weather": {
        "method": "get",
        "src": "custom/source",
      }
    },
  ]
}
```
</div>
</arc-tab>

<arc-tab label=yaml>
<h5>yaml</h5>
<div slot=content>

```yaml
---
app: myapp
http:
- get: "/"
- get: "/pages"
- get: "/pages/:dateID"
- get: "/contact"
- post: "/contact"
- get: "/widgets/*"
# verbose custom source:
- "/weather":
    method: get
    src: "custom/source"
```
</div>
</arc-tab>

<arc-tab label=toml>
<h5>toml</h5>
<div slot=content>

```toml
app="myapp"
http=[
  ["get", "/"],
  ["get", "/pages"],
  ["get", "/pages/:dateID"],
  ["get", "/contact"],
  ["post", "/contact"]
  ["get", "/widgets/*"],
]

# TOML doesn't allow mixed types in an array.
# Theoretically a "table" entry with a custom source would look like:
[[http]]
[http."/weather"]
method = "get"
src = "custom/source"
```
</div>
</arc-tab>

</div>
</arc-viewer>

Which utilizes the following project directory structure:

```bash
/
├── custom
│   └── source/
├── src
│   └── http
│       ├── get-index/
│       ├── get-pages/
│       ├── get-pages-000dateID/
│       ├── get-contact/
│       ├── get-widgets-catchall/
│       └── post-contact/
├── app.arc
└── package.json
```

> ⚠️  Handlers generated from routes with URL parameters i.e. `/pages/:dateID`, substitute `:` for `000`.  
> This is a deliberate convention to ensure valid directory names that correspond with your defined parameterized route.
