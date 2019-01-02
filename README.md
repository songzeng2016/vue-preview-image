# vue-preview-image

— A vue picture preview component.

## Demo

## Installation

```
npm install vue-preview-image --save
```

## Usage

### Install plugin

```
import VuePreviewImage from 'vue-preview-image';

export default {
  components: {
    VuePreviewImage,
  },
}
```

### Examples

```
<template>
  <vue-preview-image :urls="urls" :current="current"></vue-preview-image>
</template>

<script>
  import VuePreviewImage from 'vue-preview-image';

  export default {
    components: {
      VuePreviewImage,
    },
    data() {
      return {
        urls: [
          'https://cdn4.buysellads.net/uu/1/41369/1544727205-bigstock-3.jpg',
          'https://i.loli.net/2018/10/11/5bbec10f722ea.png'
        ],
        current: '',
      };
    },
  }
</script>
```

### Prop

| Property | Description |
| ------ | ------ |
| urls | preivew list |
| current | current image |

## License

[MIT](https://github.com/songzeng2016/vue-preview-image/blob/master/LICENSE) Copyright (c) 2019-present Song Zeng
