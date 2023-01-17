# Notifications API

# Step 1: User Permission
First step is to request the permission of the user with `Notification.requestPermission()`, which resolves to a `permission` value: `granted`, `denied`, or `default`.\
Note: The permission request must be triggered by a user gesture.

# Step 2: Create new Notificaton
The next thing is to create a new notification object using the `Notification()` contstructor.
```js
new Notification(title: String, options: Object)
// options: this is basically all the information for a notification.
// badge, body, tag, icon, image, data, vibrate, renotify, requireInteraction, actions, silent, dir, lang
```
Props & Methods(Static): 
* `Notification.permission`
* `Notification.requestPermission()`

Props & Methods(Instance)
* `Notification.[a property in the option argument]`
* `Notification.close()`

Events
* `click, close, error, show`

# Service Worker Additions
You can also trigger notifications even when the app is offline.

```js
ServiceWorkerRegistration.showNotification(title, options) // works exactly like the `new Notification(title, options)`.
ServiceWorkerRegistration.getNotifications(title, options)

addEventListener('notificationclick', (NotificationEvent) => {})

NotificationEvent.notification // returns a Notification object for the notification that was clicked
NotificationEvent.action // Returns the string ID of the notification button the user clicked. If the notification has an action button or more.

// NotificationEvent is an extentable event
```