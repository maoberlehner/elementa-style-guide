# @elementa/core

[![Patreon](https://img.shields.io/badge/patreon-donate-blue.svg)](https://www.patreon.com/maoberlehner)
[![Donate](https://img.shields.io/badge/Donate-PayPal-blue.svg)](https://paypal.me/maoberlehner)
[![GitHub stars](https://img.shields.io/github/stars/elementa-style-guide/elementa.svg?style=social&label=Star)](https://github.com/elementa-style-guide/elementa)

Elementa is a Vue.js component style guide, built with Vue for Vue projects.

## Quick start

```bash
npm install --save-dev @elementa/core
```

- [Create an Elementa Element file](#elements).
- Run `npx elementa` to start the Elementa development server.

Live reloading does not work when adding new Element files, restart Elementa after adding a new Element.

## Configuration

If you want to change the default configuration, you can create an `elementa.config.js` file in the root directory of your project.

```js
// elementa.config.js
module.exports = {
  // Add custom webpack configuration.
  // See: https://github.com/vuejs/vue-cli/blob/dev/docs/webpack.md
  configureWebpack: {},
  controls: {},
  elementSuffix: 'ele',
  // Define your navigation schema.
  navigationSchema: [
    {
      slug: 'components',
      title: 'Components',
      children: [
        // If you want to add an element as child of `App`
        // you must define its parent as `components.app`.
        {
          slug: 'app',
          title: 'App',
        },
      ],
    },
  ],
  paths: {
    root: process.cwd(),
    // The `<rootDir>` placeholder is automatically
    // replaced by the path you've specified above.
    src: '<rootDir>/src',
  },
};
```

## Elements

Elements are the pages of your style guide. Every Element describes one, or a group of similar components.

```bash
src
├── App.vue
└── components
    ├── AppButton.ele.vue
    └── AppButton.vue
```

Every file inside your `src` directory which ends with `.ele.vue` (this can be changed in the configuration) is automatically detected as an Element and added to the style guide.

```html
<template>
  <elementa-element>
    <template slot="title">Button</template>

    <p>Bootstrap includes several predefined button styles, each serving its
    own semantic purpose, with a few extras thrown in for more control.</p>

    <elementa-demo :controls="controls">
      <app-button
        :size="controls.size.value"
        :type="controls.type.value"
      >
        {{ controls.text.value }}
      </app-button>
    </elementa-demo>
  </elementa-element>
</template>

<script>
/* elementa-metadata
parent: components
slug: button
title: Button
*/

import ElementaDemo from '@elementa/core/src/components/ElementaDemo.vue';
import ElementaElement from '@elementa/core/src/components/ElementaElement.vue';

import AppButton from './AppButton.vue';

export default {
  name: 'Button',
  components: {
    AppButton,
    ElementaDemo,
    ElementaElement,
  },
  data() {
    return {
      // Controls make it possible to manipulate the data
      // of your component directly in the style guide.
      controls: {
        text: {
          label: 'Text',
          type: 'input',
          value: 'Button',
        },
        size: {
          label: 'Size',
          type: 'select',
          value: 'm',
          options: ['m', 'l', 'xl'],
        },
        type: {
          label: 'Type',
          type: 'select',
          value: 'primary',
          options: ['primary', 'success', 'danger'],
        },
      },
    };
  },
};
</script>
```

### Metadata

Use a metadata block at the beginning of the JavaScript section of your component, to define to which `parent` the Element belongs, which `slug` to use for the URL and which `title` to use in the navigation. You must provide all three properties. If you don't define a slug, the Element is used as homepage.

```js
// Default
/* elementa-metadata
parent: components
slug: button
title: Button
*/

// Nested parent
/* elementa-metadata
parent: components.app
slug: button
title: Button
*/

// Homepage
/* elementa-metadata
title: Homepage
*/
```

Do not forget to add the new parent item to your `navigationSchema` property in the configuration when defining a new nested parent in a metadata block.

You have to restart Elementa after modifying the metadata of an Element for the changes to take effect.

### Demo

![Elementa Element with Controls](https://raw.githubusercontent.com/elementa-style-guide/elementa/master/assets/elementa-element-controls-demo.gif)

## Controls

By default Elementa provides two basic control types: `input` and `select`. There will be more control types in the future.

Controls are regular Vue.js components. You can provide your own control types via the configuration.

```js
// elementa.config.js
module.exports = {
  controls: {
    myControl: 'src/components/MyControl.vue',
  },
};
```

You can take a look at the implementations of [the input control](https://github.com/elementa-style-guide/elementa/blob/master/packages/core/src/components/ElementaControlInput.vue) or the [select](https://github.com/elementa-style-guide/elementa/blob/master/packages/core/src/components/ElementaControlSelect.vue) to see how to build your own custom controls.

## Theming

Elementa themes are basically regular Vue components which contain all the CSS styles of the style guide app. If you want to change the layout or the style of your style guide, you can create your own Elementa theme and tell Elementa, in the `elementa.config.js` configuration file, to use your custom theme instead of the default one.

```js
// elementa.config.js
module.exports = {
  theme: './src/components/MyElementaTheme.vue',
};
```

### Theme template

Here you can see a minimal implementation of an Elementa custom theme. If you want to only make minor changes to the default theme, you can also copy [the default theme](https://github.com/elementa-style-guide/elementa/blob/master/packages/core/src/components/ElementaTheme.vue) and work your way from there.

```html
<template>
  <div class="MyElementaTheme">
    <div class="MyElementaTheme__sidebar">
      <elementa-nav></elementa-nav>
    </div>
    <main class="MyElementaTheme__main">
      <slot></slot>
    </main>
  </div>
</template>

<script>
import ElementaNav from '@elementa/core/src/components/ElementaNav.vue';

export default {
  name: 'MyElementaTheme',
  components: {
    ElementaNav,
  },
};
</script>

<style>
/* This is the place for your custom styles. */
</style>
```

## About

### Author

Markus Oberlehner  
Website: https://markus.oberlehner.net  
Twitter: https://twitter.com/MaOberlehner  
PayPal.me: https://paypal.me/maoberlehner  
Patreon: https://www.patreon.com/maoberlehner

### License

MIT
