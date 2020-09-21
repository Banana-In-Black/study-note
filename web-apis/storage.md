# Storage for the Web

[[doc]](https://web.dev/storage-for-the-web/)  

## General recommendation for storing resources

- For the network resources necessary to load your app and file-based content, use the [`Cache Storage API`](https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage) (part of service workers).
- For other data, use [`IndexedDB`](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Using_IndexedDB) (with a [promises wrapper](https://www.npmjs.com/package/idb)).

They're both asynchronous, and will not block the main thread. They're accessible from the window object, web workers, and service workers.

The reason not to use other storage mechanism is because most of them are synchronous and limited on space.

## Tips

- Using the [`StorageManager API`](https://developer.mozilla.org/en-US/docs/Web/API/StorageManager/estimate) to detect the available space on device, and how much space have been used.
- Protect critical and long-term data by putting them into [persistent storage](https://web.dev/persistent-storage/).

```js
// Request persistent storage for site
if (navigator.storage && navigator.storage.persist) {
    const isPersisted = await navigator.storage.persist();
    console.log(`Persisted storage granted: ${isPersisted}`);
}
```

## Cache API [[doc]](https://web.dev/cache-api-quick-guide/) [[MDN]](https://developer.mozilla.org/en-US/docs/Web/API/Cache)

The `Cache` interface provides a storage mechanism for [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request) / [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) object pairs that are cached, for example as part of the [`ServiceWorker`](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) life cycle.

> The [`CacheStorage`](https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage) interface represents the storage for `Cache` objects.

```js
const cache = await caches.open('my-cache');
// do something with cache
// when cache using the Cache interface
// and caches using the CacheStorage interface.
```

### Libraries

- [`Workbox`](https://developers.google.com/web/tools/workbox) is a JavaScript Libraries for adding offline support to web apps. It takes full advantage of features used to build [`Progressive Web Apps`](https://web.dev/progressive-web-apps/).
