<template>
  <div class="about">
    <section>
      <div class="container">
        <div class="header-search">
          <span class="fa fa-search"></span>
          <div style="box-shadow: 2px 4px 20px -4px rgba(0,0,0,.1);">  
            <input type="text" name="" id="" v-model="search" v-on:keyup="searchEntry" placeholder="Search for Photos..">
          </div>
        </div>
        
        <div class="image-container">
          <div class="wrapper">
            <div class="masonry masonry--v" >
              <div class="masonry-brick masonry-brick--v" @click="modal(image.urls.small, image.user.name)" v-for="image in images" :key="image.id">
                <img :src="image.urls.small" class="masonry-img">
                <div class="text">
                  <h1>{{image.user.name}}</h1>
                  <h3>London, Mumbai</h3>
                  <div class="loader" v-if="loader">
                    <pulse-loader v-show="loader" class="custom-class"  :color='color' :loading='loader'></pulse-loader>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- The Modal -->
      <div id="myModal" class="modal">
        <span class="close" @click="close">&times;</span>
        <img class="modal-content" id="img01">
        <div id="caption"></div>
      </div>

    </section>
  </div>
</template>
<script>
import axios from 'axios'
import '../assets/main.scss'
import { PulseLoader } from '@saeris/vue-spinners'
export default {
  components: {
    PulseLoader
  },
  name: 'list-images',
  data: function () {
    return {
      color: 'white',
      loader: false,
      search: '',
      images: []
    }
  },
  methods: {
    getImage: function () {
      this.loader = true
      const baseURI = 'https://api.unsplash.com/photos/?client_id=b43a1bc0c89846d2babed5151d8668cc80bb8ed19ccfffe0b846528d90198e10&per_page=8&order_by=latest'
      axios.get(baseURI)
      .then((result) => {
        let response = result.data
        response.forEach(element => {
          this.images.push(element)
          this.loader = false
        });
      })
    },
    modal: function (image, name) {
      let modal = document.getElementById("myModal")
      let modalImg = document.getElementById("img01")
      modal.style.display = "block";
      modalImg.src = image;
      modalImg.alt = name;
    },
    close: function () {
      let modal = document.getElementById("myModal")
      modal.style.display = 'none'
    },
    searchEntry: function () {
      this.loader = true
      let query = this.search
      const baseURI = 'https://api.unsplash.com/search/photos/?query='+`${query}`+'&client_id=b43a1bc0c89846d2babed5151d8668cc80bb8ed19ccfffe0b846528d90198e10&per_page=8&order_by=latest'
      axios.get(baseURI)
      .then((result) => {
        this.images = result.data.results
        this.loader = false
      })
    }
  },
  created () {
    this.getImage()
  },
  watch: {
    images: function () {
      if (this.images.length == 0) {
        this.getImage()
      } 
    }
  }
}
</script>