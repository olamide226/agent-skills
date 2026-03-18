---
name: bruno
description: Run Bruno CLI API integration tests. Use when running bru collections, debugging API test failures, setting up environments, or managing auth tokens in Bruno test suites.
---

# Bruno CLI Integration Testing

## Quick Start

```bash
# Run a folder within a collection
bru run "Folder Name" --env EnvName --env-var key=value

# Run entire collection
bru run --env EnvName --env-var key=value

# Run single file
bru run path/to/request.bru --env EnvName
```

## Common Pattern (Bearer Token Auth)

```bash
TOKEN=$(cat access_token | tr -d '\n\r')
cd "api-tests/My Collection"
bru run "Feature Tests" --env Local --env-var authToken="$TOKEN"
```

## Collection Auth Setup

Root `collection.bru` should use env var, not hardcoded token:
```
auth {
  mode: bearer
}
auth:bearer {
  token: {{authToken}}
}
```

## Environment File

`environments/Local.bru`:
```
vars {
  baseUrl: http://localhost:8080
}
vars:secret [
  authToken
]
```

## .bru File Structure

```
meta { name: My Request; type: http; seq: 1 }

post {
  url: {{baseUrl}}/v1/resource
  body: json
  auth: inherit
}

body:json {
  { "key": "value" }
}

tests {
  test("Status is 200", function() {
    expect(res.getStatus()).to.equal(200);
  });
  test("Has id", function() {
    const data = res.getBody();
    expect(data).to.have.property('id');
  });
}
```

## Key Test APIs

| API | Description |
|-----|-------------|
| `res.getStatus()` | HTTP status code |
| `res.getBody()` | Parsed response body |
| `bru.setVar("key", val)` | Set collection variable (pass between steps) |
| `bru.getVar("key")` | Get collection variable |
| `bru.setEnvVar("key", val)` | Set environment variable |

## Verify Auth Token Before Running

```bash
TOKEN=$(cat access_token | tr -d '\n\r')
curl -s -o /dev/null -w "%{http_code}" \
  -H "Authorization: Bearer $TOKEN" http://localhost:8080/healthz
# 200 = valid, 401 = expired — refresh token before running suite
```

## Folder Sequencing

`folder.bru` controls folder metadata:
```
meta { name: My Folder; seq: 3 }
auth { mode: inherit }
```

Files run in `seq` order defined in each file's `meta` block.

## Debugging

- `console.log(res.getBody())` inside a `test()` block prints to Bruno output
- Use `bru.setVar()` in early steps to pass resource IDs to later steps
- Multi-step flows: use a setup step (seq: 1) to find/create resources and `setVar`, then reference `{{varName}}` in URLs of subsequent steps

## See Also

- [Request patterns](references/request-patterns.md) — common request types and assertions
