<template>
  <image-preview :urls="urls" :current="current"></image-preview>
</template>

<script>
  import ImagePreview from './components'

  export default {
    name: 'app',
    components: {
      ImagePreview
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
    mounted() {
      // this.urls = [
      //   'https://cdn4.buysellads.net/uu/1/41369/1544727205-bigstock-3.jpg',
      //   'https://i.loli.net/2018/10/11/5bbec10f722ea.png'
      // ];
    }
  }
</script>

<style>

</style>
