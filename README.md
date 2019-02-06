<div align="center">
  <img src="/logo/VueProductSpinner.svg" width="550" align="center">
</div>

<br />
<br />

<p align="center"> The product spinner that Vue.js was missing. </p>

<p align="center">
  <a href="https://travis-ci.org/micheleriva/vue-product-spinner" target="_blank">
    <img src="https://img.shields.io/travis/micheleriva/vue-product-spinner.svg?style=for-the-badge" alt="Build Status" />
  </a>
  <a href="https://prettier.io/" target="_blank">
    <img src="https://img.shields.io/badge/Code%20Style-Prettier-ff69b4.svg?style=for-the-badge">
  </a>
  <a href="https://opensource.org/licenses/MIT" target="_blank">
    <img src="https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge" alt="MIT License" />
  </a>
</p>

# Installation

**npm**
```sh
npm i vue-product-spinner
```

**yarn**
```sh
yarn add vue-product-spinner
```

**UMD build**
```html
<script src="https://cdn.jsdelivr.net/npm/vue-product-spinner@latest/dist/VueProductSpinner.umd.min.js" type="text/javascript"></script>
```

# Usage

**VueProductSpinner** will allow you to add any component in order to handle the image prefetch time. <br />
You can use it as follows:

```html
<template>
  <VueProductSpinner 
    :imgs="imgs" 
    :slider="true"
  >
    <PreloadSpinnerComponent />
  </VueProductSpinner>
</template>

<script>
  import VueProductSpinner from 'vue-product-spinner'
  import PreloadSpinnerComponent from 'some-library'

  export default {
    components: {
      VueProductSpinner
    },
    data() {
      return {
        images: [
          'img1.jpg',
          'img2.jpg',
          'img3.jpg'
        ]
      }
    }
  }
</script>

```

# Props

| Prop Name | Type | Is Required | Default Value | Description |
|-|-|-|-|-|
| images | `string[]` | `required` | `[]` | An array of images |
| infinite | `Boolean` | optional | `true` | Infinite loop |
| touchDrag | `Boolean` | optional | `true` | Handle touch events |
| mouseWheel | `Boolean` | optional | `true` | Handle mouse wheel events |
| mouseDrag | `Boolean` | optional | `true` | Handle mouse drag events |
| slider | `Boolean` | optional | `false` | Show slider input |
| sliderClass | `String` | optional | | Custom slider CSS class |


# Roadmap

- [x] Add image preloader
- [ ] Add auto spin
- [ ] Add hooks
- [ ] Solve basic bugs
- [x] Add "mouse move" support
- [x] Add "mouse scroll" support
- [x] Add slider support
- [x] Add touchscreen support

# Buy Me a Beer!

[![Beerpay](https://beerpay.io/micheleriva/vue-product-spinner/badge.svg?style=beer)](https://beerpay.io/micheleriva/vue-product-spinner)

Please help me maintain my projects with a little and simple donation! I need beer to work! 😃

# License
This project is licensed under the MIT License - see the [LICENSE.md]((/LICENSE.md)) file for details.
