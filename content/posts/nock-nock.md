---
title: How to use nock to assert a network call is not made.
date: "2021-04-03"
---

[nockjs](https://github.com/nock/nock) is a library used to mock out the network calls being made by your web application. It's commonly used in integration testing to ensure that all our software modules and functions does what we expect it to without considering the behavior of any external dependencies. I've often found that these are the most useful tests to have.

For example, suppose we have an application that tells us an exchange rate using [ratesapi.io](https://ratesapi.io).

```typescript
import axios from "axios";

const getExchangeRate = async () => {
  try {
    const response = await axios.get("https://api.ratesapi.io/api/latest");
    const res = response.data;
    return res;
  } catch (errors) {
    console.error(errors);
  }
};
```

## Asserting a network call

We can then use nock to assert that the function makes a network call to the intended site.

```typescript

nock("https://api.ratesapi.io")
  .get("/api/latest")
  .reply(201, () => ({ rate: "1.00 CAD = 0.79 USD" }));

const res = await getExchangeRate();
console.log(res.data);  // { rate: "1.00 CAD = 0.79 USD" }

```

## Asserting no network call

Today I learned that you can also use the nock object's `.pendingMocks()` to ensure that a network call is not being made.

It's a two step process. First you setup the nocks:

```typescript
const usedMock = nock("https://api.ratesapi.io")
    .get("/api/latest")
    .reply(201, () => ({ rate: "1.00 CAD = 0.79 USD" }));

const unusedMock = nock("https://google.com")
    .post("/subscriptions")
    .reply(201, () => "");
```

Then you can check if they were called.

```typescript
const nockWasCalled = (networkMock: nock.Scope): boolean =>
    networkMock.pendingMocks().length == 0;

console.log(nockWasCalled(usedMock));  // false
console.log(nockWasCalled(unusedMock));  // false

const res = await getExchangeRate();
console.log(res);  // { rate: "1.00 CAD = 0.79 USD" }

console.log(nockWasCalled(usedMock));  // true
console.log(nockWasCalled(unusedMock));  // false
```

You can find a copy of the entire code [here](https://gist.github.com/viktree/7003a9f99062cdaf441a6c25b1818467).
