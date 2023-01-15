# Servce Worker API
* Four step process to get stuff done
# Step 1: Registration
In the main thread the service worker is registered.
```js
navigator.serviceWorker.register("./sw.js", {scope: location})
// scope: the parent from which you want the service worker to function
```
If you want to send message to the worker after it is registered.
```js
navigator.serviceWorker.ready.then((registration) => {
    registration.active.postMessage(message)
})
```

# Step 2: Install
This is where the serviceWorker - or a new version - is installed and **all you want the serviceWorker to do in terms of storage** is implemented. Here you can: *Put stuffs into the cache* or *Save things in indexedDB* with the APIs available to workers, you should have an idea of where to do things.

After installation, assuming there is an old serviceWorker version, *the just installed serviceWorker waits for the opened pages using the old version to be closed, before it then activates the new version to take control of the pages.*

But, we can skip this wait process calling `self.skipWaiting()`, when this is called, the new version activates and takes control.
```js
self.addEventListener('install', (ev) => {
    ev.waitUntil(
        // set up things you want to do with this worker
    )
    ev.skipWaiting() // as explained above
})
```

# Step 3: Activate
This is where the serviceWorker - or a new version - is activated to take control. This is also where you perform cleanups to what you did in in the install phase, like deleting old cache(s).

BUT, after this new version is activated, it will only have control over pages opened after, not on ones that are currently opened - _this is the default behaviour, that pages makes use of the SWs they see after they've been loaded and makes use of that one throughout their lifetime, unless they are reloaded and then encounter the new version SWs._

To override this default behaviour, we can call `Clients.claim()` to take over currently opened pages, without having to reload them. _This is due to the fact that we have decided not to allow currently opened pages to be closed before we activate the new serviceWorker using `skipWaiting()` in the install phase._ So we have to find a way to take control over the opened ones.

```js
self.addEventListener('activate', (ev) => {
    ev.waitUntil(
        Clients.claim()
        // clean up what you have done in the old service worker
    )
})
```

# Step 4: Fetch
This is the stage where you intercept, GET requests to the server, and check to see if a request like that has been cached, if yes, you send a response back to the client with `ev.respondWith(cachedData)`, this is useful where the client is offline and there is a cache available for his request. You can even let this happen **iff** the client is offline.

Now if there is no cache found for the request, fetch again the request by yourself from the network and `ev.respondWith(networkResponse)` the response.

Basically, since the SW has to `ev.respondWith(something)`, you can pack the whole of your logic inside the argument.