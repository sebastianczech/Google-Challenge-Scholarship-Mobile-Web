# Introducing the Service Worker

### Resets the index and working tree

```
git reset --hard
```

### Get the project into the sample state

```
git checkout task-register-sw
git checkout log-requests
git checkout task-custom-response
git checkout gif-response
git checkout error-handling
git checkout task-install
```

### Show all branchces

```
git show-branch -a
```

### Register service worker

```javascript
IndexController.prototype._registerServiceWorker = function() {
  if (!navigator.serviceWorker) return;

  navigator.serviceWorker.register('/sw.js').then(function() {
    console.log('Registration worked!');
  }).catch(function() {
    console.log('Registration failed!');
  });
};
```

### Hijacking request - custom HTTP response from service worker

```javascript
self.addEventListener('fetch', function(event) {
  event.respondWith(
    new Response('<p class="a-winner-is-me">Custom HTML response</p>', {
      headers: { 'Content-Type': 'text/html' }
    })
  );
});
```

### Hijacking request - custom HTTP response only for URL ending in ".jpg"

```javascript
self.addEventListener('fetch', function(event) {
  if (event.request.url.endsWith('.jpg')) {
    event.respondWith(
      fetch('/imgs/dr-evil.gif')
    );
  }
});
```

### Hijacking request - custom HTTP response only when status for request is 404

```javascript
self.addEventListener('fetch', function(event) {
  event.respondWith(
    fetch(event.request).then(function(response) {
      if (response.status === 404) {
        return fetch('/imgs/dr-evil.gif')
      }
      return response;
    }).catch(function() {
      return new Response("Uh oh, that totally failed!");
    })
  );
});
```
