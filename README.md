# pouchdb-svelte

PouchDB-Svelte is a PouchDB adaptation for Svelte Rollup.

## Usage

```bash
npm i pouchdb-svelte
npm i -D rollup-plugin-node-polyfills
```
### Svelte
```js
//  rollup.config.js

import nodePolyfills from 'rollup-plugin-node-polyfills';

export default {
    plugins: [
        nodePolyfills(),
        ...
    ]
}
```

```js
import PouchDB from 'pouchdb-svelte';

const testDB = new PouchDB('test');
```

### Sapper
```js
//  rollup.config.js

import nodePolyfills from 'rollup-plugin-node-polyfills';

export default {
    client: {
        plugins: [
            nodePolyfills(),
            ...
        ]
    }
}

//not in the server nor in the serviceworker
```
```js
import PouchDB from 'pouchdb-svelte';

(() => {
    if(typeof process !== 'undefined') return; // stops the Sapper SSR 
    const testDB = new PouchDB('test');
})();
```

>Notes: 
>- You can put this script in any .svelte file in your app.
>- It is really important to block [Sapper SSR](https://sapper.svelte.dev/docs#Making_a_component_SSR_compatible) in this script. Because SSR will try to run pouchdb-svelte (pouchdb-browser) and attempt to create the LevelDB database on the server.
This is not a good thing for two reasons:
>1. The LevelDB adapter is not present in the [pouchdb-browser](https://pouchdb.com/custom.html) vanilla.
>2. If you want to run a LevelDB database on your server (polka / express), it is better to use [pouchdb or pouchdb-node](https://pouchdb.com/custom.html).
>
>**You could use pouchdb-svelte for the Front and [pouchdb](https://www.npmjs.com/package/pouchdb) for the Back.**
___

### Why don't use onMount ?

Example:
We have two .svelte files.
- parent.svelte
- Child.svelte

Child.svelte is a component of parent.svelte.</br>
In these two files there is an onMount function.

The svelte runtime will trigger Child.svelte onMount at the first time, then parent.svelte onMount at the second time.</br>
This is a problem if you create a pouchdb instance in parent.svelte and if you want to use it in Child.svelte.

With pouchdb-svelte you don't have to use onMount and so you can create a single instance that can be passed to components.

### API PouchDB
For full API documentation and guides on PouchDB, see [PouchDB.com](http://pouchdb.com/).