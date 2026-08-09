+++
title = 'Node.js Features Worth Knowing'
slug = 'nodejs-features-worth-knowing'
date = 2026-07-03T00:00:00+05:30
draft = false
tags = ['nodejs']
+++

## Loading Env files

Node.js can now load an env file either by specifying it in the command line (`--env-file`) or programmatically through `process.loadEnvFile` API.
<!--more-->

This now removes the need to use `dotenv` package for simple use cases.

Refer [this guide](https://nodejs.org/learn/command-line/how-to-read-environment-variables-from-nodejs) for more details.

## Request context with AsyncLocalStorage

`AsyncLocalStorage` lets you store data scoped to an async execution chain — like thread-local storage, but for Node's single threaded event loop. A plain variable or a global variable can't safely hold per-request data — while one request is suspended at an `await`, another request can run and overwrite it. `AsyncLocalStorage` avoids this by tying a store object to a specific async execution branch, so it stays correctly isolated across `await`, timers, and callbacks, even under concurrency.

```ts
import express from "express";
import { AsyncLocalStorage } from "node:async_hooks";
import { randomUUID } from "node:crypto";

const app = express();
export const asyncLocalStorage = new AsyncLocalStorage<{ requestId: string }>();

app.use((req, res, next) => {
  asyncLocalStorage.run({ requestId: randomUUID() }, next);
});

// anywhere downstream, no plumbing required
export function log(message: string) {
  console.log(`[${asyncLocalStorage.getStore()?.requestId}] ${message}`);
}
```

You create a single instance for the app's lifetime and export it from a shared module — every request just calls `.run()` on it with a fresh store. Node tracks each async branch internally, so concurrent `.run()` calls never clobber each other.

There's usually no manual teardown either. The store is a regular JS object, so once its request finishes and nothing references it anymore, it's garbage collected normally. You can still leak it by capturing the store in something long-lived (an interval, a retained listener, a cache), and `.enterWith()` is especially easy to get wrong here.

The place this quietly breaks is **long-lived async resources**. Context follows the resource that was created at the time of the operation, so a callback on a pooled DB connection, a socket, or a worker inherits the context from when _that resource_ was created — usually app startup, not your request. `EventEmitter` is a common source of confusion for a related reason: `emit()` is synchronous, so a listener runs in the context of whoever called `emit`, no matter where it was registered. Wrapping the callback with `AsyncResource.bind()` at the point you register it, or re-running each unit of work inside its own `.run()`, fixes both cases.

Refer [the Node.js documentation](https://nodejs.org/api/async_context.html) for more details.
