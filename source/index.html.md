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

> The above command returns JSON structured like this:

```json
[
  {
    "repo_name": "sandcastle-test-repository-1",
    "branch_name": "main",
    "commit_hash": "abc123",
    "create_time": 1746561268,
    "status": "SUCCEEDED",
  },
  {
    "repo_name": "sandcastle-test-repository-2",
    "branch_name": "main",
    "commit_hash": "abc123",
    "create_time": 1746561270,
    "status": "IN PROGRESS",
  },
]
```

Retrieves uploaded user images.

### HTTP Request

`GET /user-images`

---

## Get Usage Statistics

```shell
curl "https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/usage-statistics" \
  -H "x-api-key: your_api_key"
```

> The above command returns JSON structured like this:

```json
{
  "plan_id": "Starter",
  "minutes_used": 60,
  "remaining_minutes": 1380,
}
```

Retrieves the current API usage statistics for the authenticated user.

### HTTP Request

`GET /usage-statistics`

---

## Set Environment

```shell
curl -X POST "https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/set-env" \
  -H "x-api-key: your_api_key" \
  -H "Content-Type: application/json" \
  -d '{}'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Environment variables set successfully",
  "repository_name": "sandcastle-test-repository",
  "vars_count": 4,
}
```

Sets the environment configuration for the user.

### HTTP Request

`POST /set-env`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
repository_name | string | The name of the repository for which you set the environment variables. 
vars | [ { name, value } ] | An array of name, value pairs that represent the name and value for the environment variables. 

<aside class="success">
Remember to include your API key in the header!
</aside>

---

## Call Agent

```shell
curl -X POST "https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/call-agent" \
  -H "x-api-key: your_api_key" \
  -H "Content-Type: application/json" \
  -d '{}'
```

> The above command returns JSON structured like this:

```json
{
  "message": "Fargate task started",
  "logs_output": "https://sandcastle.s3.amazonaws.com/example-object.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIOSFODNN7EXAMPLE%2F20250506%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250506T150000Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=abcd1234ef567890abcd1234ef567890abcd1234ef567890abcd1234ef567890",  // s3 presigned url, a text file containing the logs output of your agent run
  "job_id": 4,
}
```

Calls an AI agent with a provided input payload.

### HTTP Request

`POST /call-agent`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
query | string | Accessible via QUERY environment variable within your agent code. 
build_repository | { repository_name } | The name of your build target that contains your agent code. 
script_path | string | File path within your repository to your agent code. 

<aside class="success">
Remember to include your API key in the header!
</aside>