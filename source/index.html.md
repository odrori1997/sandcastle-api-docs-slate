---
title: API Reference

language_tabs:
  - shell
  - python
  - typescript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Sandcastle API
---

# Introduction

Welcome to the Sandcastle API! This API enables programmatic access to core features of the Sandcastle system including invoking agents, configuring environments, and uploading build targets.

All requests must be authenticated with an API key using the `x-api-key` header.

Base URL:  
`https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod`

---

# Authentication

> Example API Key usage:

```shell
curl "https://..." -H "x-api-key: your_api_key"
```

```python
import requests
headers = {'x-api-key': 'your_api_key'}
requests.get("https://...", headers=headers)
```

```typescript
fetch("https://...", { headers: { "x-api-key": "your_api_key" } })
```

<aside class="notice">
Replace <code>your_api_key</code> with your actual API key.
</aside>

---

# Endpoints

## Get User Images

```shell
curl "https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/user-images" \
  -H "x-api-key: your_api_key"
```

Retrieves uploaded builds of your agent code.

### HTTP Request

`GET /user-images`

### Response
`repo_name` *string*<br>
Name of the repository that contains your agent code.<br><br>

`branch_name` *string*<br>
Only `main` is supported at this time.<br><br>

`commit_hash` *string*<br>
Latest commit that was included in the build.<br><br>

`create_time` *int*<br>
Time of the latest build.<br><br>

`status` *SUCCEEDED | IN PROGRESS | FAILED*<br>
Status of the build.<br><br>

---

## Get Usage Statistics

```shell
curl "https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/usage-statistics" \
  -H "x-api-key: your_api_key"

# The above command returns JSON structured like this:
```
```json
{
  "plan_id": "Starter",
  "minutes_used": 60,
  "remaining_minutes": 1380
}
```

Retrieves the current API usage statistics for the authenticated user.

### HTTP Request

`GET /usage-statistics`

### Response

`plan_id` *string*<br>
The name of the user’s current subscription plan.<br><br>
Example: `"Starter"`<br><br>

`minutes_used` *int*<br>
The number of minutes used during the current billing cycle.<br><br>
Example: `60`<br><br>

`remaining_minutes` *int*<br>
The number of minutes remaining in the current billing cycle.<br><br>
Example: `1380`<br><br>


---

## Set Environment

```shell
curl -X POST "https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/set-env" \
  -H "x-api-key: your_api_key" \
  -H "Content-Type: application/json" \
  -d '{}'
```

Sets the environment configuration for the user.

### HTTP Request

`POST /set-env`

### Query Parameters

`repository_name` *string*<br>
The name of the repository for which you set the environment variables.<br><br>

`vars` *array of objects*<br>
An array of `{ name, value }` pairs that represent the name and value for the environment variables.<br><br>


### Response

`message` *string*<br>
Human-readable message confirming the operation.<br><br>

`repository_name` *string*<br>
Name of the repository where environment variables were set.<br><br>

`vars_count` *int*<br>
Number of environment variables that were successfully set.<br><br>

---

## Call Agent

```shell
curl -X POST "https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/call-agent" \
  -H "x-api-key: your_api_key" \
  -H "Content-Type: application/json" \
  -d '{}'
```

Calls an AI agent with a provided input payload.

### HTTP Request

`POST /call-agent`

### Query Parameters

`query` *string*<br>
Accessible via the `QUERY` environment variable within your agent code.<br><br>

`build_repository` *object*<br>
Contains `repository_name`, which is the name of your build target that contains your agent code.<br><br>

`script_path` *string*<br>
File path within your repository to your agent code.<br><br>


<aside class="success">
Remember to include your API key in the header!
</aside>

### Response

`message` *string*<br>
Human-readable message confirming the agent run has started.<br><br>

`logs_output` *string*<br>
An S3 pre-signed URL pointing to a text file containing the logs output of your agent run.<br><br>
Example:  
`https://sandcastle.s3.amazonaws.com/example-object.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIOSFODNN7EXAMPLE%2F20250506%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250506T150000Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=abcd1234ef567890abcd1234ef567890abcd1234ef567890abcd1234ef567890`<br><br>

`job_id` *int*<br>
Unique identifier for the agent run.<br><br>