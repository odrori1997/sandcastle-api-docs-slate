---
title: API Reference

language_tabs:
  - shell
  - python
  - typescript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Sandcastle API
---

# Introduction

Welcome to the Sandcastle API! This API enables programmatic access to core features of the Sandcastle platform including invoking agents, configuring environments, and uploading build targets.

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

```python
import requests
headers = {'x-api-key': 'your_api_key'}
res = requests.get("https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/user-images", headers=headers)
print(res.json())
```

```typescript
const res = await fetch("https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/user-images", {
  headers: { "x-api-key": "your_api_key" }
});
const data = await res.json();
console.log(data);
```

```json
{
  "repo_name": "sandcastle-test-repository",
  "branch_name": "main",
  "commit_hash": "abc123def456",
  "create_time": 1715012345,
  "status": "SUCCEEDED"
}
```

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
```

```python
res = requests.get("https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/usage-statistics", headers=headers)
print(res.json())
```

```typescript
const res = await fetch("https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/usage-statistics", {
  headers: { "x-api-key": "your_api_key" }
});
const data = await res.json();
console.log(data);
```

```json
{
  "plan_id": "Starter",
  "minutes_used": 60,
  "remaining_minutes": 1380
}
```

### HTTP Request

`GET /usage-statistics`

### Response

`plan_id` *string*<br>
The name of the user’s current subscription plan.<br><br>

`minutes_used` *int*<br>
The number of minutes used during the current billing cycle.<br><br>

`remaining_minutes` *int*<br>
The number of minutes remaining in the current billing cycle.<br><br>

---

## Set Environment

```shell
curl -X POST "https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/set-env" \
  -H "x-api-key: your_api_key" \
  -H "Content-Type: application/json" \
  -d '{"repository_name": "odrori1997/sandcastle-test-repository", "vars": [{"name": "KEY", "value": "VALUE"}]}'
```

```python
data = {"repository_name": "odrori1997/sandcastle-test-repository", "vars": [{"name": "KEY", "value": "VALUE"}]}
res = requests.post("https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/set-env", headers=headers, json=data)
print(res.json())
```

```typescript
const payload = {
  repository_name: "odrori1997/sandcastle-test-repository",
  vars: [{ name: "KEY", value: "VALUE" }]
};
const res = await fetch("https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/set-env", {
  method: "POST",
  headers: { "x-api-key": "your_api_key", "Content-Type": "application/json" },
  body: JSON.stringify(payload)
});
const data = await res.json();
console.log(data);
```

```json
{
  "message": "Environment variables set successfully",
  "repository_name": "odrori1997/sandcastle-test-repository",
  "vars_count": 4
}
```

### HTTP Request

`POST /set-env`

### Parameters

`repository_name` *string*<br>
The name of the repository for which you set the environment variables. Takes the form of <github_id>/<repository>.
Example: `yoheinakajima/babyagi`<br><br>

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
  -d '{"query": "What\'s the weather?", "build_repository": {"repository_name": "SavDont/weather-agent"}, "script_path": "agents/weather.py"}'
```

```python
data = {
  "query": "What's the weather?",
  "build_repository": { "repository_name": "SavDont/weather-agent" },
  "script_path": "agents/weather.py"
}
res = requests.post("https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/call-agent", headers=headers, json=data)
print(res.json())
```

```typescript
const payload = {
  query: "What's the weather?",
  build_repository: { repository_name: "weather-agent" },
  script_path: "agents/weather.py"
};
const res = await fetch("https://i7zigj097a.execute-api.us-east-1.amazonaws.com/Prod/call-agent", {
  method: "POST",
  headers: { "x-api-key": "your_api_key", "Content-Type": "application/json" },
  body: JSON.stringify(payload)
});
const data = await res.json();
console.log(data);
```

```json
{
  "message": "Agent run started",
  "logs_output": "https://sandcastle.s3.amazonaws.com/example-object.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIOSFODNN7EXAMPLE%2F20250506%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250506T150000Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=abcd1234ef567890abcd1234ef567890abcd1234ef567890abcd1234ef567890",
  "job_id": 4
}
```

### HTTP Request

`POST /call-agent`

### Parameters

`query` *string*<br>
Accessible via the `QUERY` environment variable within your agent code.<br><br>

`build_repository` *object*<br>
Contains `repository_name`, which is the name of your build target that contains your agent code.
`respository_name` takes the form of <github_id>/<repository>.
Example: `yoheinakajima/babyagi`<br><br>

`script_path` *string*<br>
File path within your repository to your agent code.<br><br>

### Response

`message` *string*<br>
Human-readable message confirming the agent run has started.<br><br>

`logs_output` *string*<br>
An S3 pre-signed URL pointing to a text file containing the logs output of your agent run.<br><br>

`job_id` *int*<br>
Unique identifier for the agent run.<br><br>
