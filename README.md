![Foxy Image Logo](https://aspekt-media.b-cdn.net/logo.png)

# @foxyimg/nuxt-svg-icon

[![npm version][npm-version-src]][npm-version-href]
[![npm downloads][npm-downloads-src]][npm-downloads-href]
[![license][license-src]][license-href]

`@foxyimg/nuxt-svg-icon` is a Nuxt module to load optimized SVG files as Vue components.

This is a fork of [nuxt-svgo](https://github.com/cpsoinos/nuxt-svgo) by [Corey Psoinos](https://github.com/cpsoinos) with some modifications, mostly removing unnecessary CSS classes.

## Install

```sh
npx nuxi@latest module add @foxyimg/nuxt-svg-icon
```

## Usage

Use the [default configuration](https://github.com/cpsoinos/nuxt-svgo/blob/b30c65b5e3d034a1cb198bf351355de34d02ac92/src/module.ts#L14-L26) by adding `'@foxyimg/nuxt-svg-icon'` to the `modules` section of your Nuxt config.

```typescript
// nuxt.config.ts
import { defineNuxtConfig } from 'nuxt'

export default defineNuxtConfig({
  modules: ['@foxyimg/nuxt-svg-icon'],
})
```

Then, in any `.vue` file, import your asset and use it as a component:

```vue
<template>
  <div>
    <IconHome class="text-xl" />
  </div>
</template>

<script setup lang="ts">
  import IconHome from '~/assets/icon-home.svg'
</script>
```

Or, if you use **vite**, in any `.vue` file, simply use your icon's name with `svg-icon` prefix as component name:

```vue
<template>
  <div>
    <SvgIconHome class="text-xl" />
    <!-- Or -->
    <svg-icon-home class="text-xl" />
  </div>
</template>
```

It automatically imports your icons from `assets/icons/` folder by default. you can configure this by passing `autoImportPath` in your config:

```typescript
// nuxt.config.ts
import { defineNuxtConfig } from 'nuxt'

export default defineNuxtConfig({
  modules: ['@foxyimg/nuxt-svg-icon'],
  svgo: {
    autoImportPath: './assets/other-icons/',
  },
})
```

If you want to use auto import but you don't want to use the `nuxt-icon` component (used by default), You can do so by using `defaultImport: 'component'`:

```typescript
// nuxt.config.ts
import { defineNuxtConfig } from 'nuxt'

export default defineNuxtConfig({
  modules: ['@foxyimg/nuxt-svg-icon'],
  svgo: {
    defaultImport: 'component',
  },
})
```

You can also use your own custom component instead of the built-in `nuxt-icon` component using the `customComponent` option.
This custom component must have `icon` property, just like the `nuxt-icon` component [provided by @foxyimg/nuxt-svg-icon](https://github.com/jawngee/nuxt-svg-icon/blob/main/src/runtime/components/nuxt-icon.vue).

Example:

```typescript
// nuxt.config.ts
import { defineNuxtConfig } from 'nuxt'

export default defineNuxtConfig({
  modules: ['@foxyimg/nuxt-svg-icon'],
  svgo: {
    customComponent: 'YourComponent',
  },
})
```

By default module registers all icons inside `autoImportPath` globally. This may be unwanted behavior as it generates chunks for each icon to be used globally, which will result in huge amount of files if you have many icons. If you want to disable global registration simply use `global: false` in module options:

```typescript
// nuxt.config.ts
import { defineNuxtConfig } from 'nuxt'

export default defineNuxtConfig({
  modules: ['@foxyimg/nuxt-svg-icon'],
  svgo: {
    global: false,
  },
})
```

to disable auto importing, simply set `autoImportPath` to `false`:

```typescript
// nuxt.config.ts
import { defineNuxtConfig } from 'nuxt'

export default defineNuxtConfig({
  modules: ['@foxyimg/nuxt-svg-icon'],
  svgo: {
    autoImportPath: false,
  },
})
```

### Subfolders

The icons's component name will follow Nuxt's component prefix convention. Therefore, if prefix is turned on for your components, the component name for `assets/icons/admin/badge.svg`, for example, will be `svg-icon-admin-badge`:

```html
<svg-icon-admin-badge />
```

### `componentPrefix`

You can change the default prefix (`svg-icon`) to your custom prefix using `componentPrefix` option:

```typescript
// nuxt.config.ts
import { defineNuxtConfig } from 'nuxt'

export default defineNuxtConfig({
  modules: ['@foxyimg/nuxt-svg-icon'],
  svgo: {
    componentPrefix: 'i',
  },
})
```

```jsx
// in your template
<template>
  <div>
    <i-home />
  </div>
</template>
```

## How it works

### Vite

If your Nuxt app uses Vite, this module adds [vite-svg-loader](https://github.com/jpkleemans/vite-svg-loader) to the underlying Vite configuration. All due credit for `vite-svg-loader` to its author, [@jpkleemans](https://github.com/jpkleemans).

We use a modified copy of this vite plugin for auto loading icons with extra control using a `nuxt-icon` component.

### Webpack

If your Nuxt app uses Webpack, this module adds [vue-svg-loader](https://github.com/damianstasik/vue-svg-loader) and [svgo-loader](https://github.com/svg/svgo-loader) to the underlying Webpack configuration. As discussed in [this issue](https://github.com/damianstasik/vue-svg-loader#156), `vue-svg-loader` uses version 1 of SVGO. `vue-svg-loader` looks to be unmaintained, with the latest beta release more than 2 years old. We disable the SVGO functionality of `vue-svg-loader`, instead relying on `svgo-loader` to perform optimizations, essentially making `vue-svg-loader` wrap the svg content in `<template></template>` tags.

All due credit for `vue-svg-loader` to its author, [@damianstasik](https://github.com/damianstasik).
All due credit for `svgo-loader` to its author, [@svg](https://github.com/svg).

Make sure peer dependencies of this module (`vue-svg-loader`,`svgo-loader`, `vue-loader`) are installed if you are using webpack.

## Configuration

Use your own custom SVGO options:

```typescript
// nuxt.config.ts
import { defineNuxtConfig } from 'nuxt'

export default defineNuxtConfig({
  modules: ['@foxyimg/nuxt-svg-icon'],
  svgo: {
    svgoConfig: {
      multipass: true,
      plugins: [
        {
          name: 'preset-default',
          params: {
            overrides: {
              // customize default plugin options
              inlineStyles: {
                onlyMatchedOnce: false,
              },

              // or disable plugins
              removeDoctype: false,
              removeViewBox: false,
            },
          },
        },
      ],
    },
  },
})
```

Disable SVGO entirely:

```typescript
// nuxt.config.ts
import { defineNuxtConfig } from 'nuxt'

export default defineNuxtConfig({
  modules: ['@foxyimg/nuxt-svg-icon'],
  svgo: {
    svgo: false,
  },
})
```

## Import queries (Vite.js only)

Here are the possible queries when importing a SVG file:

- `url_encode`: loads optimized svg as data uri (uses svgo + `mini-svg-data-uri`)
- `raw`: loads contents as text
- `raw_optimized`: loads optimized svg as text
- `skipsvgo`: loads contents as a component (unoptimized, without `nuxt-icon`)
- `component`: loads optimized svg as a component
- `componentext`: loads optimized svg with `nuxt-icon` component

for example:

```vue
<template>
  <div>
    <IconHome />
  </div>
</template>

<script setup lang="ts">
  import IconHome from '~/assets/icon-home.svg?componentext' // the default
</script>
```

## Important note for `url_encode` query

`xmlns="http://www.w3.org/2000/svg"` attribute is required for uri data to work. in some rare cases, it may not be there. make sure it exists when using `url_encode` query or the image will not be shown.

## Usage with TypeScript

When importing a SVG component in TypeScript, you will get a "Cannot find module" error. In order to fix thix, you need to provide a type declaration to tell TypeScript how to handle SVG components. Here's an example, using a `custom.d.ts` file at the application's root:

```ts
// custom.d.ts
declare module '*.svg' {
  import type { DefineComponent } from 'vue'
  const component: DefineComponent
  export default component
}
```

## `nuxt-icon` component

Originally copied over from the [nuxt-icons module](https://github.com/gitFoxCode/nuxt-icons/blob/89e53649e5868c31fc97869918ede96504ae1a04/src/runtime/components/nuxt-icon.vue), but later heavily modified to support tree shaking and SSR. This is not intended to be used directly. However, you can import your icons directly and pass them to the component using the `icon` prop.

### Component props

- `icon`: the component that `nuxt-icon` will render as. this is used internally to provide control over the icon.
- `width`: Any valid CSS width value.  If width is provided but height is not, height will be set to auto.
- `height`: Any valid CSS height value.  If height is provided but width is not, width will be set to auto.
- `fill`: Any valid CSS color value.
- `stroke`: Any valid CSS color value.

The `width`, `height`, `fill`, and `stroke` props are optional and should not be used in favor of CSS classes or using something utility framework like Tailwind or UnoCSS.  They are provided for convenience.


## Development

- Run `pnpm dev:prepare` to generate type stubs.
- Use `pnpm dev` to start [playground](playground) in development mode.

## Authors

**Corey Psoinos**

- Github: [@cpsoinos](https://github.com/cpsoinos)

**Javad Mnjd**

- Github: [@jd1378](https://github.com/jd1378)

**Jon Gilkison**

- Github: [@jawngee](https://github.com/jawngee)

## Show your support

Give a ⭐️ if this project helped you!

## 📝 License

Original `nuxt-svgo` code copyright © 2024 [Corey Psoinos](https://github.com/cpsoinos).

Copyright © 2024 [Jon Gilkison](https://github.com/jawngee).

This project is [MIT](https://github.com/cpsoinos/nuxt-svgo/blob/main/LICENSE) licensed.

<!-- Badges -->

[npm-version-src]: https://img.shields.io/npm/v/@foxyimg/url-builder/latest.svg?style=flat-square
[npm-version-href]: https://npmjs.com/package/@foxyimg/url-builder
[npm-downloads-src]: https://img.shields.io/npm/dt/@foxyimg/url-builder.svg?style=flat-square
[npm-downloads-href]: https://npmjs.com/package/@foxyimg/url-builder
[license-src]: https://img.shields.io/npm/l/@foxyimg/url-builder.svg?style=flat-square
[license-href]: https://github.com/cpsoinos/@foxyimg/url-builder/blob/main/LICENSE
