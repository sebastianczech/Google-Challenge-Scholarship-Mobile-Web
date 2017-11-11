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
git checkout task-cache-response
git checkout task-handling-updates
git checkout task-update-notify
git checkout task-update-reload
git checkout task-page-skeleton
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

### Install simple cache

```javascript
self.addEventListener('install', function(event) {
  var urlsToCache = [
    '/',
    'js/main.js',
    'css/main.css',
    'imgs/icon.png',
    'https://fonts.gstatic.com/s/roboto/v15/2UX7WLTfW3W8TclTUvlFyQ.woff',
    'https://fonts.gstatic.com/s/roboto/v15/d-6IYplOFocCacKzxwXSOD8E0i7KZn-EPnyo3HZu7kw.woff'
  ];

  event.waitUntil(
    caches.open('wittr-static-v1').then(function(cache) {
        return cache.addAll(urlsToCache);
    })
  );
});
```

### Return response from cache, if it is available

```javascript
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request).then(function(response) {
      if (response) return response;
      return fetch(event.request);
    })
  );
});
```

### Delete existing cache

```javascript
self.addEventListener('activate', function(event) {
  event.waitUntil(
    caches.delete('wittr-static-v1')
  );
});
```

### Installing new version of cache, deleting old ones and fetching data from cache

```javascript
var staticCacheName = 'wittr-static-v2';

self.addEventListener('install', function(event) {
  event.waitUntil(
    caches.open(staticCacheName).then(function(cache) {
      return cache.addAll([
        '/',
        'js/main.js',
        'css/main.css',
        'imgs/icon.png',
        'https://fonts.gstatic.com/s/roboto/v15/2UX7WLTfW3W8TclTUvlFyQ.woff',
        'https://fonts.gstatic.com/s/roboto/v15/d-6IYplOFocCacKzxwXSOD8E0i7KZn-EPnyo3HZu7kw.woff'
      ]);
    })
  );
});

self.addEventListener('activate', function(event) {
  event.waitUntil(
    caches.keys().then(function(cacheNames) {
      return Promise.all(
        cacheNames.filter(function(cacheName) {
          return cacheName.startsWith('wittr-') &&
                 cacheName != staticCacheName;
        }).map(function(cacheName) {
          return caches.delete(cacheName);
        })
      );
    })
  );
});

self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request).then(function(response) {
      return response || fetch(event.request);
    })
  );
});
```

### Notify about service worker updates

```javascript
IndexController.prototype._registerServiceWorker = function() {
  if (!navigator.serviceWorker) return;

  var indexController = this;

  navigator.serviceWorker.register('/sw.js').then(function(reg) {
    if (!navigator.serviceWorker.controller) {
      return;
    }

    if (reg.waiting) {
      indexController._updateReady();
      return;
    }

    if (reg.instaling) {
      indexController._trackInstalling(reg.instaling);
      return;
    }

    reg.addEventListener('updatefound', function() {
      indexController._trackInstalling(reg.installing);
    });
  });
};

IndexController.prototype._trackInstalling = function(worker) {
  var indexController = this;
  worker.addEventListener('statechange', function() {
    if (worker.state == 'installed') {
      indexController._updateReady(worker);
    }
  });
};
```

### Send a message to service Worker

```javascript
IndexController.prototype._updateReady = function(worker) {
  var toast = this._toastsView.show("New version available", {
    buttons: ['refresh', 'dismiss']
  });

  toast.answer.then(function(answer) {
    if (answer != 'refresh') return;
    worker.postMessage({action: 'skipWaiting'});
  });
};
```

### Receive message in service worker and call skipWaiting()

```javascript
self.addEventListener('message', function(event) {
  if (event.data.action == 'skipWaiting') {
    self.skipWaiting();
  }
});
```

### Reload page if controller is changed

```javascript
navigator.serviceWorker.addEventListener('controllerchange', function() {
  window.location.reload()
});
```

### Respond to requests for the root page with the page skeleton from the cache

```javascript
self.addEventListener('fetch', function(event) {
  var requestUrl = new URL(event.request.url);

  if (requestUrl.origin === location.origin) {
    if (requestUrl.pathname === '/') {
      event.respondWith(caches.match('/skeleton'));
      return;
    }
  }

  event.respondWith(
    caches.match(event.request).then(function(response) {
      return response || fetch(event.request);
    })
  );
});
```
