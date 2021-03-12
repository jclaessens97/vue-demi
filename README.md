<p align="center">
<img src="https://github.com/vueuse/vue-demi/blob/master/assets/banner.png?raw=true" width="600"/>
<br>
<a href='https://www.npmjs.com/package/vue-demi'><img src='https://img.shields.io/npm/v/vue-demi?color=42b883' alt='npm'></a>
</p>

<p align="center">
<b>Vue Demi</b> (<i>half</i> in French) is a developing utility<br> allows you to write <b>Universal Vue Libraries</b> for Vue 2 & 3<br>
<i>See more details in <a href='https://antfu.me/posts/make-libraries-working-with-vue-2-and-3'>this blog post</a></i>
</p>

<br>

<br>

## Usage

Install this as your plugin's dependency:

```bash
npm i vue-demi
# or
yarn add vue-demi
```

Add `vue` and `@vue/composition-api` to your plugin's peer dependencies to specify what versions you support.

```jsonc
{
  "dependencies": {
    "vue-demi": "latest"
  },
  "peerDependencies": {
    "@vue/composition-api": "^1.0.0-beta.1",
    "vue": "^2.0.0 || >=3.0.0-rc.0"
  },
  "peerDependenciesMeta": {
    "@vue/composition-api": {
      "optional": true
    }
  },
  "devDependencies": {
    "vue": "^3.0.0" // or "^2.6.0" base on your preferred working environment
  },
}
```

Import everything related to Vue from it, it will redirect to `vue@2` + `@vue/composition-api` or `vue@3` based on users' environments.

```ts
import { ref, reactive, defineComponent } from 'vue-demi'
```

Publish your plugin and all is done!

### Extra APIs

`Vue Demi` provides extra APIs to help distinguishing users' environments and to do some version-specific logics.

### `isVue2` `isVue3`

```ts
import { isVue2, isVue3 } from 'vue-demi'

if (isVue2) {
  // Vue 2 only
} else {
  // Vue 3 only
}
```

### `Vue2`

To avoid bringing in all the tree-shakable modules, we provide a `Vue2` export to support access to Vue 2's global API. (See [#41](https://github.com/vueuse/vue-demi/issues/41).)

```ts
import { Vue2 } from 'vue-demi'

if (Vue2) {
  Vue2.config.ignoredElements.push('x-foo')
}
```

### `install()`

Composition API in Vue 2 is provided as a plugin and need to install to Vue instance before using. Normally, `vue-demi` will try to install it automatically. For some usages that you might need to ensure the plugin get installed correctly, the `install()` API is exposed to as a safe version of `Vue.use(CompositionAPI)`. `install()` in Vue 3 environment will be an empty function (no-op).

```ts
import { install } from 'vue-demi'

install()
```

## CLI

### Manually Switch Versions

To explicitly switch the redirecting version, you can use these commands in your project's root.

```bash
npx vue-demi-switch 2
# or
npx vue-demi-switch 3
```

### Package Aliasing

If you would like to import `vue` under an alias, you can use the following command 

```bash
npx vue-demi-switch 2 vue2
# or
npx vue-demi-switch 3 vue3
```

Then `vue-demi` will redirect APIs from the alias name you specified, for example:

```ts
import * as Vue from 'vue3'

var isVue2 = false
var isVue3 = true
var Vue2 = undefined

export * from 'vue3'
export {
  Vue,
  Vue2,
  isVue2,
  isVue3,
}
```

### Auto Fix

If the `postinstall` hook doesn't get triggered or you have updated the Vue version, try to run the following command to resolve the redirecting.

```bash
npx vue-demi-fix
```

### Isomorphic Testings

You can support testing for both versions by adding npm alias in your dev dependencies. For example:

```json
{
  "scripts": {
    "test:2": "vue-demi-switch 2 vue2 && jest",
    "test:3": "vue-demi-switch 3 && jest",
  },
  "devDependencies": {
    "vue": "^3.0.0",
    "vue2": "npm:vue@2"
  },
}
```

or

```json
{
  "scripts": {
    "test:2": "vue-demi-switch 2 && jest",
    "test:3": "vue-demi-switch 3 vue3 && jest",
  },
  "devDependencies": {
    "vue": "^2.6.0",
    "vue3": "npm:vue@3"
  },
}
```

## Examples

See [examples](./examples).

## Who are using this?

- [VueUse](https://github.com/antfu/vueuse) - Collection of Composition API utils
- [@vue/apollo-composable](https://github.com/vuejs/vue-apollo/tree/v4/packages/vue-apollo-composable) - Apollo GraphQL functions for Vue Composition API
- [vuelidate](https://github.com/vuelidate/vuelidate) - Simple, lightweight model-based validation
- [vue-composition-test-utils](https://github.com/ariesjia/vue-composition-test-utils) - Simple vue composition api unit test utilities
- [vue-use-stripe](https://github.com/frandiox/vue-use-stripe) - Stripe Elements wrapper for Vue.js
- [@opd/g2plot-vue](https://github.com/open-data-plan/g2plot-vue) - G2plot for vue
- [vue-echarts](https://github.com/ecomfe/vue-echarts) - Vue.js component for Apache ECharts.

> open a PR to add your library ;)

## Underhood

See [the blog post](https://antfu.me/posts/make-libraries-working-with-vue-2-and-3/#-introducing-vue-demi).

## License

MIT License © 2020 [Anthony Fu](https://github.com/antfu)
