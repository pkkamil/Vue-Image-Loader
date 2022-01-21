<template>
  <h1><span>V</span>ue <span>I</span>mage <span>L</span>oader</h1>
  <ImageList />
</template>

<script>
import ImageList from './components/ImageList.vue'

export default {
  name: 'App',
  components: {
    ImageList,
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400&display=swap');

body {
  margin: 0;
  padding: 0;
  background-color: #333;
  color: #FFF;
  font-family: Roboto;
}

h1 {
  text-align: center;
  font-weight: 300;
  margin-top: 50px;
  margin-bottom: 50px;
  font-size: 2.2rem;
}

h1 span {
  color: #257945;
  font-weight: 400;
}
</style>
