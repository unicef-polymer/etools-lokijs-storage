# \<etools-lokijs-storage\>

Polymer element that integrates LokiJS storage in your app. Can be used for client side data caching, offline work and more.

This element warps loki functionality allowing you to manage your data collections. You can add an expire time (in milliseconds)
to each collection created using `addCollection` method. This data is stored in a list (`collectionsList` collection).
Then you can use `cleanExpiredCollections` to remove the collections that expired.

You can use the `loaded` event available to execute some actions after the loki db was loaded.

## Usage

```html
<etools-lokijs-storage id="lokiStorageEl" 
  db-name="localAppDb" 
  idb-adapter-name="localStorageAdapter"
  autosaveInterval="600000"
  on-loaded="handleLoaded"></etools-lokijs-storage>
```

The local database will be identified by `db-name` and `idb-adapter-name` and it will be a single instance for the entire app.
When the element is stamped it checks if the database was loaded earlier. If it was loaded then automatically fires the loaded event. 
If it was not loaded then trigger loki initialization and then fire db loaded event. Global `LokiJsStorage` object will contain 
loki db, idbAdapter instances and also a loaded flag. Do not use this object in your code, use instead:

```javascript
// inside your Polymer element
this.$.lokiStorageEl.getLokiDatabaseInstance();
````

Adding a collection with data and expire time:

`addCollection(collectionName, uniqueProperties, collectionIndices, ttlInterval, expire)`
 
```javascript
this.$.lokiStorageEl.addCollection('newCollection', ['id'], ['id'], 86400000, 86400000*7)
```
This new collection will have a unique index for documents id property, also the added indices will be useful for querying documents, 
`ttlInterval` defines the expire time for collection's stored documents(1d in this example) and `expire` is the expire time for the entire collection.

To clean up your expired collections use `cleanExpiredCollections()`. You will have to implement your own way of cleaning expired data 
according with your app requirements.

You can always get the list of collections you added using `getCollectionsList()`.

Insert documents into collection:
```javascript
this.$.lokiStorageEl.insertData(newCollection, [{id: 1, name: 'John Doe'}, {id: 2, name: 'Jane Doe'}]);
```
The data you insert can be an array of documents(objects) or a single document. 

Get a collection by name:
```javascript
this.$.lokiStorageEl.getCollection('newCollection');
```

Remove collection:
```javascript
this.$.lokiStorageEl.removeCollection('newCollection');
```

Save database manually:
```javascript
this.$.lokiStorageEl.saveDatabase(callback);
```

For documents insert, update, search, sorting or other actions you can use loki functionality 
see [Loki documentation](https://rawgit.com/techfort/LokiJS/master/jsdoc/index.html).

## Install
```bash
$ bower install --save etools-lokijs-storage
```

## Preview element locally
Install needed dependencies by running: `$ bower install`.
Make sure you have the [Polymer CLI](https://www.npmjs.com/package/polymer-cli) installed. Then run `$ polymer serve` to serve your element application locally.

## Running Tests

```
$ polymer test
```
