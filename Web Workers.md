# Web Workers API
* Web Workers makes it possible to run a script operation in a background thread separate from the main execution thread of a web application. The advantage of this is that laborious processing can be performed in a separate thread, allowing the main (usually the UI) thread to run without being blocked/slowed down.

* A worker is an object created using a constructor (e.g. Worker()) that runs a named JavaScript file — this file contains the code that will run in the worker thread.

- Any JavaScript code can run in the Worker. Except those ones not present in its `WorkerGlobalScope(self)`, in contrast to the normal `GlobalScope(window)`
    - This means, not all properties of the `GlobalScope(window)` is available in a `WorkerGlobalScope(self)`
    - Also, not all Web APIs are available in a *Worker context*.
    - Infact, some code are only available for a specific type of worker. e.g `postMessage` is only present for Decicated Workers

- Three types of Workers:
    1. Decicated Worker - This is the normal `Worker()`. It can only be used by a single context's main thread script.
    2. Shared Worker - `SharedWorker()`. It can be used by multiple context's main thread scripts.
    3. Service Worker - `ServiceWorker()`. It act as a proxy server between the Application, Browser and Server
    - `self` in any WorkerGlobalScope, returns a reference to itself. So you can call its properties and methods with or without it, as you see fit.
    - The same way APIs are accessed in the browser(`window`), is the same way you can access it in a worker(`self`).
        - **Browser:** `window.navigator` as **Worker:** `self.navigator`
        - **Browser:** `navigator` as **Worker:** `navigator`
    - All worker types have their own event loops

---
**Dedicated Worker**
```js
// main.js
const dedWorker = new Worker("./dedWorker.js")
dedWorker.postMessage(message: JS_Object, transfer?: transferable_objects[]) // sends message to the worker thread
// any transfferable object included will be rendered unusable from where it is transferred whether main or worker thread
dedWorker.terminate() // terminates the worker
// Events: message, messageerror, error, rejectionhandled, unhandledjection
```
**DedicatedWorkerGlobalScope**
```js
// dedWorker.js
name // dedWorker.js
postMessage(message, transferrables[]) // sends message to the main thread
close()
// Events: onmessage(e.data), messageerror
```
---

**Shared Worker**
```js
// main.js - for separate html files
const shdWorker = new SharedWorker("./shdWorker.js")
shdWorker.port.start()
shdWorker.port.postMessage()
// Events: port.onmessage(e.data)
```
**SharedWorkerGlobalScope**
```js 
// shdWorker.js - one shared by all html files
name // shdWorker.js
port.postMessage(message, transferrables[]) // sends message to all main threads
port.close()
// Events: onconnect(e.ports), port.onmessage(e.data)
```

---
- **WorkerGlobalScope**
    - Features available in all workers
    ```js
    // someWorker.js
    importScripts(...fileList) // think of importing a module in main thread
    otherMethods() // available in a WorkerGlobalScope
    otherProperties // available in a WorkerGlobalScope
    // Events: online, offline
    ```