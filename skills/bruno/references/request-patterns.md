# Bruno Request Patterns

## HTTP Methods

```
get { url: {{baseUrl}}/resource; body: none; auth: inherit }
post { url: {{baseUrl}}/resource; body: json; auth: inherit }
patch { url: {{baseUrl}}/resource/{id}; body: json; auth: inherit }
delete { url: {{baseUrl}}/resource/{id}; body: none; auth: inherit }
```

## Path Variables

```
get {
  url: {{baseUrl}}/deployments/{{deployment_id}}/status
  body: none
  auth: inherit
}
```

## Common Test Assertions

```js
// Status codes
expect(res.getStatus()).to.equal(200);
expect(res.getStatus()).to.be.oneOf([200, 201]);

// Property existence
const data = res.getBody();
expect(data).to.have.property('id');
expect(data).to.have.property('status');

// Value checks
expect(data.status).to.equal('running');
expect(['pending', 'running']).to.include(data.status);
expect(data.replicas).to.be.above(0);

// Array checks
expect(data).to.be.an('array');
expect(data.length).to.be.above(0);

// Error structure (this API)
expect(data).to.have.property('error');
expect(data.error.type).to.equal('validation');
expect(data.error.type).to.equal('not_found');
```

## Passing Data Between Steps

```js
// Step 1 — find and save
test("Setup", function() {
  const data = res.getBody();
  const deployment = data.find(d => d.transport === 'streamable-http');
  bru.setVar("deployment_id", deployment.id);
  bru.setVar("deployment_name", deployment.name);
});

// Step 2 — use saved var in URL: {{deployment_id}}
// Step 2 — use in test
test("Check", function() {
  console.log("ID:", bru.getVar("deployment_id"));
});
```

## Conditional Assertions

```js
test("Flexible status check", function() {
  const data = res.getBody();
  // Accept multiple valid states (async ops)
  const validStates = ['pending', 'deploying', 'running', 'ready'];
  expect(validStates).to.include(data.status);
});
```
