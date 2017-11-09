# Introducing the Service Worker

### Get the project into the sample state

```
git reset --hard
git checkout task-register-sw
git checkout log-requests
```

### Register service worker

```javascript
if (!navigator.serviceWorker)
  return

navigator.serviceWorker.register('/sw.js').then(function() {
  console.log('Service worker registered')
})
```
