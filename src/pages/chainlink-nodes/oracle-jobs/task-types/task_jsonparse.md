---
layout: ../../../../layouts/MainLayout.astro
section: nodeOperator
date: Last Modified
title: "JSON Parse Task"
permalink: "docs/jobs/task-types/jsonparse/"
---

JSON Parse tasks parse a JSON payload and extract a value at a given keypath.

**Parameters**

- `data`: the JSON string. Can be:
  - string
  - byte array
- `path`: the keypath to extract. Must be a comma-delimited list of keys, or specify a custom `separator` alternative.
- `separator`: (optional) custom `path` key separator. Defaults to comma (`,`).
- `lax` (optional): if false (or omitted), and the keypath doesn't exist, the task will error. If true, the task will return `nil` to the next task.

**Outputs**

The value at the provided keypath.

**Example**

```toml
my_json_task [type="jsonparse"
              data="$(http_fetch_result)"
              path="data,0,price"]
```

This task returns `123.45` (float64) when given the following example `data` value:

```json
{
  "data": [{ "price": 123.45 }, { "price": 678.9 }]
}
```
