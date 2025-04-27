An object can have both strong references and/or weak references. The garbage collector only looks for strong references when deciding reachability of an object. Normally any reference created for an object is a strong one.
```js
const some_obj = {...}; // 1 strong reference to the object
const another_obj = some_obj; // 2 strong references to the same object
some_obj = null; // only 1 strong reference left
another_obj = null; // no strong references left, and object is marked for garbage collection
```
A weak reference can be explicitly created using `WeakRef`
To access a weakly referenced object, use the `.deref()` function on the WeakRef.
```js
const some_obj = {...};
const another_obj = WeakRef(some_obj);
const ref = another_obj.deref();
alert(some_obj === ref) // true
```

We do not need to explicitly remove weak references to mark the referenced object for GC as the garbage collector only checks for strong references.
```js
const some_obj = {...}; // 1 strong reference to the object
const another_obj = WeakRef(some_obj); // 1 strong and 1 weak reference to the same object
some_obj = null; // only 1 weak reference and no strong reference, hence object is garbage

// usage of another_obj will possibly throw an error as the object it reference is garbage collected.
```
# Finalization registry
The purpose of a finalization registry is to provide the ability to perform additional operations, related to some object, after it has been finally deleted from memory.

Each finalization registry object has a cleanupFunction which is passed in its constructor when creating a finalization registry object.
Objects can be registered with finalization registries, whenever any registered object is cleaned from memory, the cleanupFunction corresponding to the finalization registry is run.

```javascript
function cleanupCallback(heldValue) {
  // cleanup callback code
}

const registry = new FinalizationRegistry(cleanupCallback);
```

`heldValue` is the value that will be passed to cleanupCallback when the object is garbage collected, when registering objects with the finalization registry, we can specify the `heldValue` for each object.
The finalization registry keeps a strong reference to the `heldValue` and a weak reference to the object.

To register an object, we use the `.register(object, heldValue)` method of the finalization registry, this returns an `unregisterToken` which can be used to unregister the object from the registry.
To unregister an object, we use the `.unregister(unregisterToken)` method.
```javascript
let user = { name: "John" };

const registry = new FinalizationRegistry((heldValue) => {
  console.log(`${heldValue} has been collected by the garbage collector.`);
});

registry.register(user, user.name); // user object is registered with the registry and user.name is the heldValue which will be passed to the cleanup function.
```
# Media caching: A possible use-case
If we have some resource-intensive media like images, it would be very network intensive to load the images frequently. Hence creating a cache is usually a good idea. There are other better methods to create a cache instead of using WeakRefs, but this holds as a good example nonetheless.

The cache should not hold strong references to any of the images, only the string names of the images must be strongly referenced, the values should be weak referenced.

When an object becomes unreachable by any strong references, it is not immediately garbage collected, there is a period during which the object is marked for garbage collection, but is still present in memory, during this time the object can be recovered and referenced through a WeakRef's deref() method.

Only after the image object is actually deleted, not just marked for deletion shall we redownload the image object from the network, until then we can just use WeakRef's deref() method.
Therefore our cache is just a map with keys as string names of the images and values as WeakRefs to the actual image objects in memory.

When any image object is accessed through the cache using the image's string name as key, we have three possible sitations:
1. The image object is not yet marked for garbage collection, in which case it still exists in memory and we just use the string name key on the cache map to get the WeakRef and then call .deref() on it and return the image.
2. The image is marked for garbage collection but not yet garbage collected, in which case we proceed the same as the first case.
3. The image is garbage collected, in which case we need to send another request to the network for the image and then set its string name and image object as key value pair in the cache map.

```javascript
function fetchImg() {
    // some function for downloading images
}

function weakRefCache(fetchImg) {
    const imgCache = new Map();

    return (imgName) => {
        const cachedImg = imgCache.get(imgName);

        if (cachedImg?.deref()) {
            return cachedImg?.deref();
        }

        const newImg = fetchImg(imgName);
        imgCache.set(imgName, new WeakRef(newImg));

        return newImg;
    };
}

const getCachedImg = weakRefCache(fetchImg);
```

the `imgCache` is a Map which holds image names as keys and WeakRefs to image objects as the corresponding values.
the `weakRefCache` function returns another function which takes an `imageName` and does the following:
1. uses `imageName` as key on the cache and gets the WeakRef to the cached image and stores it in `cachedImg`
2. checks if calling `.deref()` on the `cachedImg` WeakRef returns undefined or not. If it returns undefined, it means that the object that `cachedImg` WeakRef points to is already cleaned from memory, but if it doesn't return undefined, then the function simply returns the image object obtained by calling `.deref()` on the `cachedImg` WeakRef as it still exists in memory.
3. If on the other case calling `.deref()` on `cachedImg` WeakRef returned undefined, the function will not return prematurely and would continue onwards to fetch the image from the network, then create a WeakRef to the image obtained from the network and add it as value associated to the key `imgName` in the cache before returning the image object.

However, the above implementation poses a slight issue. Keys will be created in the cache map when images from network are pulled, but if an image has been cleared from the cache, then we also want the key associated with that image to be deleted from the cache, otherwise we will have more keys taking up space whose values have been long cleared from memory.

One way to handle this problem is to periodically scavenge the cache and clear out “dead” entries.
Another way is to use finalization registries. We can register the image objects with the finalization registry with their string name keys as `heldValue`, so that when the image object is garbage collected, it automatically deletes the string name key associated with the image from the cache. Since the string name key was passed as the `heldValue`, we can use it to delete its entry from the cache after the image object is garbage collected.

The above code can be modified as follows, newly added lines are marked with `(*)`:
```javascript
function fetchImg() {
  // some function for downloading images
}

function weakRefCache(fetchImg) {
  const imgCache = new Map();

  const registry = new FinalizationRegistry((imgName) => { // (*)
    const cachedImg = imgCache.get(imgName);               // (*)
    if (cachedImg && !cachedImg.deref()) imgCache.delete(imgName); // (*)
  }); // (*)

  return (imgName) => {
    const cachedImg = imgCache.get(imgName);

    if (cachedImg?.deref()) {
      return cachedImg?.deref();
    }

    const newImg = fetchImg(imgName);
    imgCache.set(imgName, new WeakRef(newImg));
    registry.register(newImg, imgName); // (*)

    return newImg;
  };
}

const getCachedImg = weakRefCache(fetchImg);
```
# WeakMap and WeakSet
A WeakMap is similar to a `Map` but it can only hold objects as keys, unlike a `Map` which can hold anything as a key, even a Toyota. The `WeakMap` holds weak references to its keys, therefore if no strong references to the object used as key in WeakMap remains, then the key will be removed from WeakMap and will be marked for GC.

`WeakMap` has only the following methods:
- [`weakMap.set(key, value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/set)
- [`weakMap.get(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/get)
- [`weakMap.delete(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/delete)
- [`weakMap.has(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/has)

WeakMap can be used in places where some additional data must be associated with objects without modifying the objects themselves. WeakMap allows this by using the said object as keys and the additionally associated data as values.

Why not use a regular Map?
A regular Map can also hold objects as keys and additional data as values. But a regular Map uses strong references for its keys. If we used a Map instead of a WeakMap, then the object key and additional data value would have a reference from the Map even if there were no other references to them. This means that the object and its additional data would have stayed in memory even when they weren't being utilized for any operation anywhere else in the code. We do not always want that. Using a WeakMap would let the garbage collector free the object and its associated additional data when it is not being used for anything else and no other strong references exist.

[`WeakSet`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet) is a `Set`-like collection that stores only objects and removes them once they become inaccessible by other means. Since the WeakSet automatically removes the objects that have no strong references remaining to them, we can use the set to check membership of certain objects based on some property and perform some operations if the object belonged to a particular set. All of this without having to manually clean up the object from the WeakSet after the object is done being used everywhere else.

WeakMaps and WeakSets do not have support for `clear()`, `size()`, `keys()`, `values()` or any form of iteration. We tradeoff these utilities for the ability of not manually having to clean up after ourselves when storing additional associated data outside object or checking memberships for particular operations.
