pouchdb-adapters-rn [![npm version](https://badge.fury.io/js/pouchdb-adapters-rn.svg)](https://badge.fury.io/js/pouchdb-adapters-rn)
======
PouchDB adapter for ReactNative using HTTP (e.g. a remote CouchDB or CouchDB-like database) or SQLite as its data store.

This package has made changes in pouchdb-adapter-websql and pouch-adapter-http for compatibility with react-native. 

It supports PouchDB till version **7.0.0**

## Prerequisites

- A SQLite module
  - [react-native-sqlite-2](https://github.com/noradaiko/react-native-sqlite-2)
- [rn-fetch-blob](https://github.com/kkbhav/react-native-fetch-blob/tree/work)

## Installation
```bash
npm install --save https://github.com/nkhs007/pouchdb-adapters-rn.git
npm install --save-exact react-native-sqlite-2@1.6.0
npm install --save https://github.com/nkhs007/react-native-fetch-blob.git
react-native link rn-fetch-blob
react-native link react-native-sqlite-2
```
#### - Extra step for iOS

In Xcode, add `libsqlite3.tbd` to your project's `Build Phases` ➜ `Link Binary With Libraries`.

## Usage

```js
import PouchDB from 'pouchdb-adapters-rn';

var db = new PouchDB('mydb', { adapter: 'pouchdb-adapters-rn'});
// or
var remote = new PouchDB('http://127.0.0.1:5984/mydb');
// or
var remote1 = new PouchDB('http://127.0.0.1:5984/mydb', { adapter: 'pouchdb-adapters-rn-http'});
```

### - Updated API For RemoteDB

1. **All API's calls will return attachment as base64(default) string or a file-uri**

2. **remote.getAttachment(docId, attachmentId, options, callback)**
    - extra options values(optional)
        - path: string, Path to save the file(optional, if not passed base64 string will be returned)
        - name: name of the file(optional, only used when file parameter is passed)

3. **remote.get(docId, options, callback)**
    - extra options values(optional)
        - path: string, Path to save the file(optional, if not passed base64 string will be returned)

4. **remote.putAttachment(docId, attachmentId, [rev], attachment, type, [callback])**
    - attachment: file-uri or base64(cannot process blob types)
    
5. **remote.put(doc)** or **remote.bulkDocs(docs)**
    - accepts file-uri or base64 in attachment data,  
    Example doc
    
    ```js
    const doc = {
     _id: 'somethings',
     _attachments: {
       'photo': {
         'content-type': 'image/jpg',
         'data': '/Users/pouch/image.jpg',
       },
     }
    }
    remote.put(doc);
    ```

### - Updated API For LocalDB

1. **All API's calls will return attachment as base64(default) string or a file-uri**

2. **db.getAttachment(docId, attachmentId, options, callback)**
    - extra options values(optional)
        - path: boolean to return attachment as fileUri(optional, if not passed base64 string will be returned)

3. **db.allDocs(options)**
    - extra options values(optional)
        - path: boolean to return attachment as fileUri(optional, if not passed base64 string will be returned)
        
4. **db.get(docId)**
    - will return attachment as base64 if requested

5. **db.putAttachment(docId, attachmentId, [rev], attachment, type, [callback])**
    - attachment: file-uri or base64(cannot process blob types)
    
6. **db.bulkDocs(docs)**
    - accepts path in attachment data,
    Example docs
    
    ```js
    const docs = [{
     _id: 'somethings',
     _attachments: {
       'photo': {
         'content-type': 'image/jpg',
         'data': '/Users/pouch/image.jpg',
       },
     }
    }]
    db.bulkDocs(docs);
    ```
    
7. **db.put(doc)**
    - accepts path in attachment data,
    Example docs
    
    ```js
    const doc = {
     _id: 'somethings',
     _attachments: {
       'photo': {
         'content-type': 'image/jpg',
         'data': '/Users/pouch/image.jpg',
       },
     }
    }
    db.put(doc);
    ```






For full API documentation and guides on PouchDB, see [PouchDB.com](http://pouchdb.com/). For details on PouchDB sub-packages, see the [Custom Builds documentation](http://pouchdb.com/custom.html).

Supports PouchDB till version 7.0.0
