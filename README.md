# todo

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## Add Firebase settings

Add config/local.env.(Load config/dev.env.js)

```config/local.env
'use strict'
module.exports = {
  FIRE_BASE: {
    API_KEY: '"......"',
    AUTH_DOMAIN: '"......"',
    DATABASE_URL: '"......"',
    PROJECT_ID: '"......"',
    STORAGE_BUCKET: '"......"',
    MESSAGING_SENDERID: '"......"',
  }
}
```
