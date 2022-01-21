<template>
  <article class="container">
    <section class="container__content">
      <input type="text" name="q" id="q" placeholder="2022.01.20 12:15" v-model="search" @input="searchImages">
      <section class="container__content__list">
        <div class="container__content__list__single" v-for="image in searchedImages.slice(0, limit)" v-bind:key="image.pathShort" @click="showImage">
          <span>{{ image.datetime }}</span>
          <img :src="image.pathLong" :alt="image.datetime">
        </div> 
        <div v-if="limit < searchedImages.length" class="container__content__list__single show-more" @click="limit+=360">
          Show more
        </div>
      </section>
    </section>
  </article>
</template>

<script>
import debounce from "lodash/debounce"
export default {
  name: 'ImageList',
  data() {
    return {
      image: {
        pathLong: '',
        pathShort: '',
        date: '',
        time: '',
        datetime: '',
      },
      search: '',
      images: [],
      searchedImages: [],
      limit: 360,
      alreadyPreview: false,
    }
  },
  mounted() {
    this.importImages(require.context('../assets/', true, /\.jpg$/));
  },
  methods: {
    importImages(destination) {
      destination.keys().forEach(key => (
        this.images.push({
           pathLong: destination(key), 
           pathShort: key, 
           date: key.replace('./', '').split('_')[0], 
           time: key.replace('./', '').split('_')[1].replace('.jpg', '').replace(';', ':'),
           datetime: key.replace('./', '').split('_')[0] + ' ' + key.replace('./', '').split('_')[1].replace('.jpg', '').replace(';', ':') 
        })
      ));
      this.setImages()
    },
    setImages() {
      this.searchedImages = this.images
    },
    searchImages: debounce(function() {
      this.searchedImages = this.images.filter((item) => item.datetime.includes(this.search))
    }, 1000),
    showImage(e) {
      if (!this.alreadyPreview) {
        e.path[1].classList.add('preview')
        this.alreadyPreview = true
      } else {
        document.querySelectorAll('.preview').forEach(i => {
          i.classList.remove('preview')
        })
        this.alreadyPreview = false
      }
    }
  }
}
</script>

<style scoped>
.container {
  width: 100%;
  padding-bottom: 50px;
}

.container__content {
  padding: 0 30px;
  margin: 0 auto;
  text-align: center;
}

.container__content input {
  display: inline-block;
  background-color: transparent;
  outline: none;
  border: 0;
  border-bottom: 1px solid #ffffffaf;
  color: #FFF;
  text-transform: uppercase;
  font-size: 1.5rem;
  text-align: center;
  margin-bottom: 40px;
  transition: border-bottom .6s;
}

.container__content input:focus {
  border-bottom: 1px solid #FFF;
}

.container__content__list {
  display: flex;
  width: 100%;
  flex-wrap: wrap;
  gap: 15px;
  align-items: center;
  justify-content: center;
}

.container__content__list__single {
  position: relative;
  width: 440px;
  height: 300px;
}

.preview {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(2);
  z-index: 100;
}

.show-more {
  width: 440px;
  height: 300px;
  background-color: #1b5a33;
  font-size: 2rem;
  font-weight: lighter;
  display: flex;
  justify-content: center;
  align-items: center;
  text-transform: uppercase;
  transition: .6s background-color;
  cursor: pointer;
}

.show-more:hover {
  background-color: #1f6138;
}

.container__content__list__single span {
  position: absolute;
  bottom: 0;
  right: 0;
  background-color: rgba(51, 51, 51, 0.5);
  padding: 3px 5px;
}

.container__content__list__single img {
  width: 100%;
  height: 100%;
}
</style>