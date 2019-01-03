---
home: true
heroImage: /hero.png
actionText: Getting started →
actionLink: ./guide/
features:
  - title: Original work
    details: Preserved the full functionality, the same API and is up-to-date with the latest version of APlayer.
  - title: Vue powered
    details: This is not a simple wrapper. All logic is rewritten using Vue, and all properties are responsive.
  - title: TypeScript Support
    details: Provides full TypeScript type definition support and is very user friendly for users who prefer to use JSX.
footer: MIT Licensed | Copyright © 2018-present MoePlayer
---

### Installation

```bash
yarn add @moefe/vue-aplayer # OR npm install @moefe/vue-aplayer --save
```

### Usage

```js
import Vue from 'vue';
import APlayer from '@moefe/vue-aplayer';

Vue.use(APlayer, {
  defaultCover: 'https://github.com/u3u.png', // Set the player default cover image
  productionTip: false, // Output version information in the console
});
```

::: warning Compatibility note
vue-aplayer requires Vue.js >= 2.2.0
:::
