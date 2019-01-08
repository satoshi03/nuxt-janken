# nuxt-janken

> Browser based janken app in JavaScript with NuxtJS.

![demo](https://media.giphy.com/media/OSaQf631QsSJmZ81f0/giphy.gif)

## Config Setup

This app uses Firebase.
So, please setup [Firebase](https://firebase.google.com/) at first, if you have not used it.

Create env file to connect Firebase.

```
$ vim .local.env
```

```.local.env
'use strict'

module.exports = {
  apiKey: "xxxxxxxxxxxxxxx",
  authDomain: "xxxxxxxxxxxxxxx",
  databaseURL: "xxxxxxxxxxxxxxx",
  projectId: "xxxxxxxxxxxxxxx",
  storageBucket: "xxxxxxxxxxxxxxx",
  messagingSenderId: "xxxxxxxxxxxxxxx"
}

```

## Build Setup

``` bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn run dev

# build for production and launch server
$ yarn run build
$ yarn start

# generate static project
$ yarn run generate
```

For detailed explanation on how things work, checkout [Nuxt.js docs](https://nuxtjs.org).
